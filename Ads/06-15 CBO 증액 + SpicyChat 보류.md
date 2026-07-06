| 항목 | 값 |
|---|---|
| 상태 | 실행 완료 + 보류 |
| 기준 | 2026-06-08~2026-06-14 마감 7d, `ad-judge.mjs` |

## 결정

1. `[KR] Cereal_Scaling_CBO` 증액 완료
   - campaign id: `120248197964370520`
   - 예산: `₩230,000 -> ₩276,000`
   - 룰: 72h window / +20%
   - 확인: `effective_status ACTIVE`
2. SpicyChat iOS 증액 보류
   - adset: `캐릭터챗_남성 - iOS mobile only`
   - adset id: `120242167475440520`
   - 판정: `SCALE후보`
   - 사유: CBO 증액 당일 추가 예산 변경 회피
   - 다음: 72h 후 ROI +38% 이상이면 `₩40,000 -> ₩48,000`

## 근거

| 대상 | Spend | Devices | Pay | ROI | Age | 판정 |
|---|---:|---:|---:|---:|---:|---|
| KR_All OS_Value_LAL | ₩145,984 | 513 | 18 | +451.4% | 13d | SCALE후보 |
| KR_All_OS_Broad | ₩609,407 | 751 | 25 | +66.7% | 13d | KEEP |
| KR_iOS_Winner_Interests | ₩98,277 | 127 | 8 | +347.7% | 13d | SCALE후보 |
| KR_Pre-heat_Broad | ₩66,887 | 655 | 22 | +1040.6% | 8d | SCALE후보 |
| KR_Pre-heat_DirectChat | ₩103,294 | 204 | 3 | +27.8% | 4d | 실험계약 |
| 캐릭터챗_남성 - iOS mobile only | ₩214,035 | 632 | 9 | +104.1% | 119d | SCALE후보 |

## 운영 룰

- 안정성 우선: 같은 날 복수 증액 금지.
- 증액일: 소재/문구/placement 수정 금지.
- `IN_PROCESS`: 30~60분 후 재확인.
- `WITH_ISSUES`/장시간 `IN_PROCESS`: 추가 변경 중단.
- SpicyChat은 Meta 귀속 ROAS 단독 판정 금지. AARRR2 결합 ROI 기준.

## 재검토

| 항목 | 기준 | 액션 |
|---|---|---|
| CBO | 증액 후 48h ROI +38% 미만 | 롤백 검토 |
| SpicyChat | 72h 후 ROI +38% 이상 | +20% 증액 |
| SpicyChat | 72h 후 ROI +38% 미만 | 유지 |

## 근거 경로

- `cereal-ai/.agents/skills/meta-optimize/scripts/ad-judge.mjs`
- Meta API 확인: 2026-06-15 KST
