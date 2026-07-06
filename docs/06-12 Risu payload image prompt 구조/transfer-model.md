# Transfer Engine — Data Model

## Pipeline

```
RisuRaw (EN, variable structure)
  → LLM Transform (single call)
    → CerealCard (KO, standardized structure)
      → cards/{story_id}.json
```

## Input: RisuRaw

Source: `data/character-cards.db` → `cards` table

```typescript
interface RisuRaw {
  // from DB row
  id: string;
  name: string;
  source: "chub" | "risu";

  // from data JSON (CharV3)
  description: string;      // EN, 200~5000 chars, free-form
  personality: string;       // EN, may be empty
  scenario: string;          // EN, may be empty
  first_mes: string;         // EN, often long-form RP (action + dialogue)
  mes_example: string;       // EN, example dialogues
  system_prompt: string;     // DISCARD — cereal uses hardcoded jailbreak
  post_history_instructions: string;  // DISCARD — cereal uses standard DM format

  character_book: {
    entries: RisuEntry[];
    scan_depth?: number;
    token_budget?: number;
  };

  tags: string[];
}

interface RisuEntry {
  name: string;
  keys: string[];
  content: string;           // EN, variable length
  constant: boolean;
  enabled: boolean;
  insertion_order: number;
  extensions?: {
    risu_depth?: number;     // @@depth decorator
  };
}
```

## Output: CerealCard

Target: `lib/card-engine/cards/{story_id}.json`

```typescript
interface CerealCard {
  spec: "chara_card_v3";
  spec_version: "3.0";
  data: {
    name: string;                // KO name
    description: string;         // KO, see Description Model below
    personality: CerealPersonality;  // key enum
    scenario: string;            // KO, DM relationship context
    first_mes: string;           // KO, DM format (short messages)
    mes_example: "";             // always empty — few-shot goes to lorebook
    system_prompt: "";           // always empty — prompt-builder hardcodes
    post_history_instructions: string;  // cereal DM standard (RESPONSE_INSTRUCTION)
    creator: "cereal-transfer";
    creator_notes: string;       // source attribution
    tags: string[];
    alternate_greetings: [];
    character_book: CerealLorebook;
    extensions: {
      source_id: string;        // original card id
      source_name: string;      // original EN name
      source_platform: "chub" | "risu";
      adult_config: AdultConfig;
    };
  };
}

type CerealPersonality =
  | "noona"       // 누나 — 연상, 리드, 여유
  | "queen"       // 여왕 — 도도, 주도적, 차가움+뜨거움
  | "seductive"   // 유혹 — 적극적, 대담, 직접적
  | "shy"         // 수줍 — 소극적이다 한번 열리면
  | "tsundere"    // 츤데레 — 겉차갑+속따뜻
  | "yandere"     // 얀데레 — 집착, 독점
  | "gentle"      // 순수 — 조용, 다정, 헌신
  | "wild";       // 자유 — 거침없음, 즉흥적

interface AdultConfig {
  faceType: string;
  age: number;
  hairStyle: string;
  bodyType: string;
  relationship: string;     // KO — "룸메이트", "직장 상사", etc.
  occupation: string;       // KO
  personality: CerealPersonality;
  secret: string;           // KO — behavior pattern name
  hairColor: string;
}
```

## Description Model

LLM이 source description + personality + scenario를 합쳐서 재구성.

```
## 외모
{source appearance — translated, condensed}

## 성격
{source personality — translated as mechanism, not adjective}
  - 원인: {why this trait exists}
  - 행동: {how it manifests}

## 배경
{source background — translated}

## 현재 상황
{scenario rewritten as DM context}
```

제약:
- 500~1000 chars
- 형용사 나열 금지 → mechanism 기술
- source에 없는 정보 생성 금지

## first_mes Model

Source first_mes는 대부분 long-form RP (행동묘사 + 대화).
Cereal은 DM 포맷 (짧은 메시지).

```
변환 규칙:
1. 행동묘사 → *이탤릭* 1줄 (10~30자)
2. 대화 → DM 메시지 2~3줄 (각 10~40자)
3. 총 길이: 50~150자
4. 큰따옴표 금지
5. (괄호) 행동묘사 금지
6. 감정 훅 1개 필수 — 긴장, 호기심, 설렘 중 하나
```

Example transform:
```
Source (EN, long-form):
"*She leans against the doorframe, arms crossed, a cigarette dangling
from her lips. Her eyes narrow as she spots you.*
'Well, well. Look who decided to show up. You know what time it is?'
*She takes a slow drag, exhaling smoke toward the ceiling.*
'I've been waiting. Don't make me wait again.'"

Output (KO, DM):
*담배를 물고 문에 기대서 있다*

어머, 이제야 오네?
몇 시인 줄 알아?
기다리게 하지 마... 다음엔 😤
```

## Scenario Model

```
관계: {relationship — KO}
직업: {occupation — KO}
대화 채널: 인스타그램 DM
```

## Transform Rules (LLM에 전달)

```
PRESERVE (인간의 창작):
- 캐릭터 성격의 원인과 행동 패턴
- lorebook의 세계관, 관계 역학, 트리거 이벤트
- Visible ↔ Hidden 모순 (있으면)
- NPC 관계 (있으면)

TRANSLATE:
- EN → KO (의역 허용, 축약 금지)
- long-form RP → DM 포맷 (first_mes only)

REPLACE:
- system_prompt → "" (prompt-builder가 주입)
- post_history_instructions → cereal DM standard
- jailbreak entries → cereal unrestricted_guidelines
- personality → CerealPersonality enum mapping

ADD:
- cereal DM lorebook entries (see lorebook-template.md)
- adult_config metadata
- DM format rules in post_history_instructions

DISCARD:
- source system_prompt (replaced by prompt-builder)
- source post_history_instructions (replaced by standard)
- source mes_example (replaced by few-shot lorebook entry)
- {{original}} placeholders
- author notes / OOC instructions
```

## LLM Prompt Structure

```
System: You are a character card translator.
        Input: CharV3 JSON (English)
        Output: Cereal CharV3 JSON (Korean)
        Rules: {transform rules above}

User: {source card JSON}
      Personality mapping hint: {closest CerealPersonality}

Assistant: {CerealCard JSON}
```

Single LLM call per card. Model: gemini-2.5-flash via OpenRouter.

## Validation (post-transform)

```typescript
interface ValidationResult {
  pass: boolean;
  checks: {
    name_ko: boolean;           // Korean characters present
    desc_length: boolean;       // 500~1000 chars
    desc_no_adjective_list: boolean;  // mechanism > adjective
    first_mes_dm_format: boolean;     // no quotes, no (brackets), short lines
    first_mes_length: boolean;        // 50~150 chars
    personality_valid: boolean;       // CerealPersonality enum
    lorebook_count: boolean;          // >= 4 entries
    lorebook_total_chars: boolean;    // >= 3000 chars
    lorebook_has_constant: boolean;   // >= 2 constant entries
    lorebook_has_keyword: boolean;    // >= 1 keyword entry (if source had any)
    scenario_dm: boolean;             // contains "인스타그램 DM"
    json_valid: boolean;              // parseable JSON
  };
}
```
