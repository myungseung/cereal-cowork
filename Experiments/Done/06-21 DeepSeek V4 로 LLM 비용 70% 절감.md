| 항목        | 값                     |
| --------- | --------------------- |
| Status    | 완료                    |
| KPI       | LLM cost / MSG user   |
| Scope     | 핵심 LLM API 전체         |
| Split     | device_id 기준 5:5      |
| Control   | Gemini 3.1 Flash Lite |
| Treatment | DeepSeek V4 Flash     |
| 기대값       | LLM 단가 0.25x          |
| Done At   | 2026-06-25 KST        |
| Success?  | No                    |

## Goal

- 전체 LLM 비용 1/4 수준 전환 가능성 확인.
- 일부 API가 아니라 핵심 LLM call 전체를 같은 variant로 전환.
- 같은 device는 chat / suggestion / image prompt 모두 같은 모델.

## 대상 API

| API | 최근 30일 LLM call 비중 |
|---|---:|
| `suggestions_v2_dm` | 36.80% |
| `dm_chat_v12` | 30.98% |
| `dm_image_prompt_v12` | 13.74% |
| `gemini_highlight` | 9.26% |
| `gemini_chat` | 3.33% |
| `preview_image_prompt` | 2.51% |
| `first_message` | 2.04% |
| `character_dialogs_v12` | 1.34% |

## 가격 기준

| 모델 | 4k input + 500 output | 현재 대비 |
|---|---:|---:|
| Gemini 3.1 Flash Lite | $0.001750 | 1.00x |
| DeepSeek V4 Flash | $0.000450 | 0.257x |

## 판정

| 구분 | 기준 |
|---|---|
| 품질 | 캐릭터 유지, 말투 자연스러움, 응답 길이 |
| suggestion | 클릭 가능성, 맥락 적합성 |
| 이미지 프롬프트 | JSON parse 성공률, 이미지 프롬프트 품질 |
| 안정성 | error rate, timeout, fallback |
| 비용 | OpenRouter usage 기준 input/output token cost |
| 제품 | message send, suggestion click, image generation success, 세션 지속 |

## 근거

- 2026-06-21 OpenRouter 가격 확인: DeepSeek V4 Flash = Gemini 3.1 Flash Lite 대비 25.7%.
- 2026-06-21 최근 30일 `api_logs` 집계: 핵심 LLM API call 비중 위 표.
- 2026-06-21 live `/preset_ai_pet` 모델 override 테스트: `dm_chat_v12` 로그에 `deepseek/deepseek-v4-flash` 기록 확인.

## 결과

판정: 실패. Winner = `control`.

| 기준 | Control | DeepSeek | 판정 |
|---|---:|---:|---|
| `first_message` users | 211 | 198 | - |
| LLM API calls | 5,893 | 4,360 | - |
| API cost | ₩5,973 | ₩1,663 | DeepSeek 0.28x |
| Revenue | ₩718,200 | ₩625,600 | DeepSeek 낮음 |
| Net / user | ₩3,375 | ₩3,151 | DeepSeek -₩224 |

## 결론

- 비용 절감은 맞음: `first_message` user 기준 API cost/user `₩28 → ₩8`.
- 사업 결과는 실패: 절감액보다 revenue/user 하락이 큼.
- 실험 config는 2026-06-22 `shipped`, `winner_variant=control`.
- DeepSeek 전체 전환 안 함.
