| 항목 | 값 |
|---|---|
| 상태 | 판정 기록 / 실행 없음 |
| 기준 | 2026-06-06~2026-07-05 30d + 2026-06-29~2026-07-05 7d |
| SOT | Meta API live spend + AARRR2 DB 실측 Pay/Revenue |

## 결정

1. `Video-AI_pet (preheat winner) - LAL` 단독 재활성화 보류
   - Meta `per purchase`는 좋게 보이나 실제 DB Pay와 불일치.
   - 30d 보정 ROI `-1.8%`.
   - 최근 7d는 PAUSED라 현재 효율 판단 불가.

2. 재검증 조건
   - LAL 단독 ON 금지.
   - 재테스트 시 eligible active adset 전체에 동일 소재 세트 적용.
   - 소재 제거도 전체 adset 동일 적용.

3. Pre-heat 승격
   - 현재 승격 후보 없음.
   - `Hannam_river`는 DISAPPROVED / 추가 금지.

## 근거

| 대상 | 기간 | Meta purchase | DB Pay | Spend | Revenue | ROI | 판정 |
|---|---|---:|---:|---:|---:|---:|---|
| `AI_pet - LAL` | 30d | 264 | 17 | ₩734,824 | ₩721,340 | -1.8% | ON 보류 |
| `AI_pet - Broad` | 30d | 360 | 32 | ₩990,198 | ₩1,367,200 | +38.1% | 손익선 근접 |
| `AI_pet - LAL` | 7d | 0 | 0 | ₩0 | ₩0 | - | 데이터 없음 |
| `You_are_so_kind - LAL` | 7d | 112 | 9 | ₩861,805 | ₩469,700 | -45.5% | 현재 LAL 부진 |

## 재검토

| 항목 | 기준 | 액션 |
|---|---|---|
| AI_pet LAL | 단독 ON 요청 | 보류 |
| AI_pet 재테스트 | 전체 eligible active adset 동일 소재 적용 가능 | 소액 재검증 검토 |
| Meta purchase | DB Pay와 10배 이상 차이 | 판정 근거 제외 |
| AARRR2 ad cost | `cost_daily` ad breakdown 일부 누락 | Meta live spend로 보정 |

## 근거 경로

- Meta Marketing API live 확인: 2026-07-06 KST
- DB 실측: AARRR2 기준 `utm_content=ad_id`
- 비교 ad id: `120249107706220520`, `120248958998840520`, `120248296728760520`
