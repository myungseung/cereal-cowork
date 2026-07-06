# Spec 14: Card Engine

## Context

| Phase | Status |
|-------|--------|
| 1. CharV3 파서 + lorebook + prompt-builder | DONE |
| 2. Oshimai 카드 테스트 (OpenRouter) | DONE |
| **3. 기존 캐릭터 → 카드 변환** | **NOW** |

핵심 발견: Direct Gemini API는 `HarmBlockThreshold.OFF`해도 모델 레벨 필터 별도 동작.
OpenRouter 경유 시 우회됨 → v9 전체를 OpenRouter로.

v6 코드 무변경. 모든 작업은 card-engine worktree에서.

---

## Phase 3: 기존 캐릭터 → CharV3 카드 변환

### 변환 대상

v6 캐릭터 데이터가 3곳에 분산됨 → CharV3 JSON 1개로 통합.

```
v6 (현재)                              CharV3 카드 (목표)
┌─────────────────────────┐           ┌─────────────────────────┐
│ characters 테이블        │           │ cards/{story_id}.json   │
│  ├ detailed_description  │──────────▶│  ├ description          │
│  ├ adult_config (JSON)   │──────────▶│  ├ personality          │
│  ├ prologue              │──────────▶│  ├ first_mes            │
│  └ title                 │──────────▶│  └ name                 │
├─────────────────────────┤           ├─────────────────────────┤
│ dm.prompt.ts             │           │                         │
│  ├ SYSTEM_RULE           │──────────▶│  ├ system_prompt        │
│  ├ CONSISTENCY_RULE      │──────────▶│  │ (통합)               │
│  ├ RESPONSE_INSTRUCTION  │──────────▶│  ├ post_history_instr   │
│  └ IMAGE_SEDUCTION_*     │──────────▶│  │                      │
├─────────────────────────┤           │                         │
│ PERSONALITY_EXAMPLE_DIALOGS          │                         │
│  └ {personality}: 8 pairs│──────────▶│  └ character_book       │
│    (queen, tsundere, …)  │           │     (constant lorebook) │
└─────────────────────────┘           └─────────────────────────┘
```

### Data Model: v6 → CharV3 매핑

| v6 소스 | v6 필드 | CharV3 필드 | 변환 |
|---------|---------|-------------|------|
| `characters` DB | `title` | `name` | 그대로 |
| `characters` DB | `detailed_description` | `description` | 그대로 |
| `characters` DB | `prologue` | `first_mes` | `{{char}}` 변수화 |
| `adult_config` JSON | `personality` | CharV3 `personality` | key → 설명문 |
| `adult_config` JSON | `age, bodyType, hairStyle…` | `description` 하단 | 외모 블록 |
| `adult_config` JSON | `secret` | `character_book` entry | constant lorebook |
| `dm.prompt.ts` | `SYSTEM_RULE` | `system_prompt` | DM 톤 유지 |
| `dm.prompt.ts` | `RESPONSE_INSTRUCTION` | `post_history_instructions` | 포맷 룰 |
| `dm.prompt.ts` | `CONSISTENCY_RULE` | `system_prompt` 하단 | 에스컬레이션 |
| `dm.prompt.ts` | `IMAGE_SEDUCTION_*` | `post_history_instructions` | 이미지 커맨드 |
| `dm.prompt.ts` | `PERSONALITY_EXAMPLE_DIALOGS[key]` | `character_book` entries | constant, few-shot |

### CharV3 출력 구조

```json
{
  "spec": "chara_card_v3",
  "data": {
    "name": "수빈",
    "description": "[detailed_description]\n\n## Appearance\n[adult_config → 외모]",
    "personality": "[adult_config.personality key → 성격 설명]",
    "scenario": "[adult_config.relationship + 상황]",
    "first_mes": "[prologue, {{char}}/{{user}} 변수화]",
    "system_prompt": "[SYSTEM_RULE + CONFIGURATION + CONSISTENCY_RULE]",
    "post_history_instructions": "[RESPONSE_INSTRUCTION + IMAGE rules]",
    "mes_example": "",
    "character_book": {
      "scan_depth": 4,
      "token_budget": 2048,
      "entries": [
        { "keys": [], "content": "[secret 행동 패턴]", "constant": true },
        { "keys": [], "content": "[few-shot example pairs]", "constant": true }
      ]
    }
  }
}
```

### Flow

```
1. DB에서 characters 목록 조회 (is_adult=1)
         │
         ▼
2. 각 캐릭터: characters row + adult_config 파싱
         │
         ▼
3. personality key로 PERSONALITY_EXAMPLE_DIALOGS 매핑
         │
         ▼
4. dm.prompt.ts 상수 → system_prompt / post_history_instructions 조립
         │
         ▼
5. CharV3 JSON 생성 → lib/card-engine/cards/{story_id}.json
         │
         ▼
6. v9 dm.ts에서 story_id로 카드 로드 (현재 oshimai 하드코딩 → 동적 로드)
         │
         ▼
7. card-test에서 검증 → golden-test v6 vs v9 비교
```

### 파일 변경

```
lib/card-engine/
  ├ types.ts              # 변경 없음
  ├ parser.ts             # 변경 없음
  ├ lorebook.ts           # 변경 없음
  ├ prompt-builder.ts     # 변경 없음 (DM용 post_history에 포맷 룰 이미 포함)
  ├ cards/                # NEW — 변환된 캐릭터 JSON 디렉토리
  │   ├ subin.json        # NEW
  │   └ {story_id}.json   # NEW (각 캐릭터)
  └ converter.ts          # NEW — v6 DB row → CharV3 JSON 변환기

lib/chat/v9/dm.ts         # EDIT — oshimai 하드코딩 → story_id 기반 동적 카드 로드

app/admin/card-test/page.tsx  # EDIT — 캐릭터 선택 드롭다운 추가
```

### DM 포맷 제약 (CharV3에 포함)

v6 DM은 인스타그램 DM 스타일. 카드 변환 시 유지해야 할 것:

| 제약 | 위치 |
|------|------|
| 1-3줄 짧은 메시지 | `post_history_instructions` |
| 따옴표("") 금지 | `post_history_instructions` |
| `\n` 줄바꿈으로 멀티 메시지 | `post_history_instructions` |
| 이미지 생성 트리거 | `post_history_instructions` 또는 별도 핸들러 |
| 한국어 | `post_history_instructions` |

---

## 참조

| 파일 | 내용 |
|------|------|
| `payload.md` | RisuAI 20-message payload 구조 |
| `payload-jailbreak.md` | Jailbreak 버전 payload |
| `NSFW.json` | NSFW 요청/응답 비교 |
| `lib/chat/v6/dm.prompt.ts` | v6 DM 프롬프트 상수 (변환 소스) |
| `lib/chat/v3/dm.prompt.ts` | v3 원본 상수 (v6이 re-export) |
