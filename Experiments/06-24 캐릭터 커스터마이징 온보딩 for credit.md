---
date: 2026-06-24
date_end: 2026-07-06
category: product-retention
type: experiment
status: active
kpi: average credit use per chat session
expected: 10x
---

| 항목      | 값                                   |
| ------- | ----------------------------------- |
| Status  | 클라이언트 뼈대 제작 중                       |
| KPI     | average credit use per chat session |
| 기대값     | 10x                                 |
| Scope   | credit-only 온보딩                     |
| 전환 flag | credit 매출 > 구독 매출                   |

## Goal
- New User의크래딧 사용량 10배

## Hypothesis
- 더 마음에 드는 캐릭터를 만들고 싶어서 크래딧을 결제할 것이다

#### 현재 문제

| Metric             |       Live |   Credit |
| ------------------ | ---------: | -------: |
| Assigned           |        513 |      411 |
| Primary revenue    | ₩1,052,600 | ₩115,500 |
| Any revenue        | ₩1,052,600 | ₩140,400 |
| Revenue / assigned |     ₩2,052 |     ₩342 |
| Payer CVR          |       4.7% |     1.9% |


#### 행동 데이터 기준 판정

기준: 신규 Meta 유저 7,856명 / 30일.

| 행동              |                       빈도 |
| --------------- | -----------------------: |
| chat 사진 요청      | 20,985건 / 1,387명 (17.7%) |
| 캐릭터 생성 + 재생성 후보 | 54,154건 / 3,750명 (47.7%) |
| 대화 이미지 생성       | 35,844건 / 2,998명 (38.2%) |
| 메시지 전송          | 89,773건 / 3,044명 (38.7%) |

판정: credit 소비 시점 1순위는 `chat 사진 요청`.

근거: 사진 요청은 명시적 credit 과금 후보. 캐릭터 생성/재생성은 빈도는 높지만 우상단 재생성 전용 이벤트가 없음.

## 근거

- 기준: 2026-06-24 사용자 제공 Live vs Credit 결과.
- 기존 SOT: [[06-15 기존제품 그대로 크레딧 전환율 확인]]
- KPI 인덱스: Next 10x / average credit use per chat session.

---

## Asset generation TODO

- 목적: credit customizer 옵션 썸네일 생성만.
- 금지: UI 수정, 기능 수정, 직접 프롬프트 작성.
- 방식: `askAgent(grok-4.20)`로 최종 영어 이미지 프롬프트 받기 → 받은 프롬프트 그대로 `512x512` 생성.
- 기준: 4열 정방형에서 1초 안에 옵션 의미가 읽혀야 함.

### Done

- 얼굴: create `approved-assets.json` 재사용.
- 헤어: create `approved-assets.json` 재사용.
- 몸매: create `approved-assets.json` 재사용.
- 비밀: 10개 초안 생성.
- 비밀 재생성 기준 정리: 인물 중심 X, 옵션 썸네일 O.
- Grok prompt log: `docs/credit-10x-summary/askagent/2026-06-26T10-13-56-574Z-askagent.md`.

### Left

- [ ] 관계 13개 생성: 인물/상황 중심, 512x512.
- [ ] 직업 12개 생성: 3:4 원본 생성, 정방형 중앙 crop, 얼굴보다 의상/소품 강조.
- [ ] 성격 10개 생성: 인물 표정/태도 중심, 512x512.
- [ ] 비밀 12개 확정: 암시형 신체/소품/action close-up, 512x512.
- [ ] 비밀 `secret_tattoo` 재생성본 확정.
- [ ] 비밀 `shower_call` 재생성본 확정.
- [ ] 비밀 나머지 10개 small-size QA.
- [ ] 생성 asset contact sheet 생성: category별 1장.
- [ ] 생성 파일명/metadata 정리.
- [ ] 승인된 asset만 `mock.html` 매핑.

### Category counts

| category | asset source | count | status |
| --- | --- | ---: | --- |
| faceType | create 재사용 | 12 | done |
| age | asset 없음 | 0 | slider |
| hairStyle | create 재사용 | 10 | done |
| bodyType | create 재사용 | 11 | done |
| relationship | 신규 생성 | 13 | left |
| occupation | 신규 생성 | 12 | left |
| personality | 신규 생성 | 10 | left |
| secret | 신규 생성 | 12 | draft |

### Final format

```text
public/uploads/credit-customizer/{category}/{category}_{option_key}_512.jpg
public/uploads/credit-customizer/{category}/{category}_{option_key}_512.json
```

```json
{
  "createdAt": "ISO_DATE",
  "category": "secret",
  "key": "secret_tattoo",
  "prompt": "GROK_FINAL_PROMPT",
  "predictionId": "replicate_prediction_id",
  "sourceUrl": "replicate_output_url",
  "width": 512,
  "height": 512,
  "askAgentLog": "ABS_PATH"
}
```
