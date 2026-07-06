| 항목 | 값 |
|---|---|
| 상태 | 실행 완료 + 심사 대기 |
| 기준 | 2026-06-10~2026-06-16 마감 7d, `ad-judge.mjs` + Meta API |

## 결정

1. `Hannam_river_dating` CBO Broad 승격
   - 신규 ad: `Hannam_river_dating - CBO Broad`
   - ad id: `120249374958320520`
   - campaign: `[KR] Cereal_Scaling_CBO` (`120248197964370520`)
   - adset: `KR_All_OS_Broad` (`120248271672680520`)
   - creative id: `1707994646920445`
   - post id: `642236832301451_122187162128837790`
   - status: `ACTIVE`
   - effective_status: `PENDING_REVIEW`
2. CBO 예산 증액 안 함
   - 유지: `₩276,000/day`
   - 사유: 안정 운영 우선. 소재 승격과 예산 증액 동시 진행 금지.
3. 전체 adset 동시 투입 안 함
   - 투입: `KR_All_OS_Broad` 1곳만
   - 사유: 한 번에 한 변수. LAL/iOS는 broad delivery 확인 후 검토.

## 근거

| 대상 | Spend | Devices | Pay | ROI | Age | 판정 |
|---|---:|---:|---:|---:|---:|---|
| KR_Pre-heat_Broad | ₩137,720 | 612 | 19 | +617.5% | 10d | SCALE후보 |
| Hannam_river_dating | ₩93,760 | 136 | 5 | +233.6% | 9d | SCALE후보 |
| KR_All_OS_Broad | ₩601,266 | 827 | 27 | +135.0% | 15d | SCALE후보 |

## 기록 보정

- `KR_0613_No_rules_mild_friend` pause 기록 누락 확인.
- 현재 상태: `PAUSED`, pause/update 시각 `2026-06-15 13:20 KST`.
- Meta 06-13~06-17 기준: spend `₩98,259`, Meta purchase `3`.
- 기존 기록에는 `ACTIVE / PENDING_REVIEW`까지만 남음.
- pause 의사결정 출처 불명확. 실행/기록 누락 가능성 있음.
- 후속 판단: AARRR 실매출/ROI ad id 기준 확인 전 재활성화 금지.

## 재검토

| 항목 | 기준 | 액션 |
|---|---|---|
| Hannam CBO Broad | 승인 + 24~48h delivery | 유지/확장 판단 |
| Hannam CBO Broad | `WITH_ISSUES` 또는 장시간 `IN_PROCESS` | 추가 변경 중단 |
| mild_friend | AARRR 실매출 확인 | 재개 여부 판단 |

## 근거 경로

- `cereal-ai/.agents/skills/meta-optimize/scripts/ad-judge.mjs`
- Meta API 확인: 2026-06-17 KST
