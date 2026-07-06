| 항목 | 값 |
|---|---|
| 상태 | 실행 완료 |
| 기준 | 2026-06-25~2026-07-01 7d + 2026-06-29~2026-07-01 3d, AARRR2 결합 성과 |

## 결정

1. `KR_Pre-heat_Broad` 증액 완료
   - campaign: `[KR] Pre-heat_Video_Test` (`120248667608460520`)
   - adset: `KR_Pre-heat_Broad` (`120248667608550520`)
   - 운영 방식: ABO
   - 예산: `₩60,000/day -> ₩78,000/day`
   - 변경폭: `+30%`
   - 확인: `ACTIVE`
   - updated: `2026-07-01 22:01:15 KST`

## 근거

| 대상 | 기간 | Spend | Devices | Pay | Revenue | CPA | ROI | 판정 |
|---|---|---:|---:|---:|---:|---:|---:|---|
| `[KR] Pre-heat_Video_Test` | 7d | ₩420,093 | 780 | 41 | ₩1,743,100 | ₩10,246 | +314.9% | SCALE |
| `KR_Pre-heat_Broad` | 7d | ₩420,093 | 780 | 41 | ₩1,743,100 | ₩10,246 | +314.9% | SCALE |
| `KR_Pre-heat_Broad` | 3d | ₩173,022 | 316 | 12 | ₩404,700 | ₩14,419 | +133.9% | SCALE |
| `No_rules_girl` | 3d | ₩168,121 | 299 | 12 | ₩404,700 | ₩14,010 | +140.7% | SCALE |

## 재검토

| 항목 | 기준 | 액션 |
|---|---|---|
| Pre-heat Broad | 증액 후 48h ROI +38% 미만 | 롤백 검토 |
| Pre-heat Broad | 증액 후 CPA ₩15k 초과 지속 | 추가 증액 중단 |
| CBO | Scaling 중 문제 확인 전 | 추가 증액 보류 |

## 근거 경로

- `cereal-ai/.agents/skills/meta-optimize/scripts/meta-optimize.mjs --days 7`
- `cereal-ai/.agents/skills/meta-optimize/scripts/meta-optimize.mjs --days 3`
- Meta API budget 확인: 2026-07-01 KST
