---
date: 2026-06-09
date_end: 2026-06-10
category: pricing-revenue
type: experiment
status: done
kpi: Realized LTV per customer
expected: 1.45x
---

| 항목       | 값                         |
| -------- | ------------------------- |
| 상태       | 완료 / 성공                   |
| 대상       | iOS new customers         |
| 후보       | Pricing C                 |
| 목표       | iOS 가격상향 성공 여부 판단         |
| Success? | Yes                       |
| Done At  | 2026-06-10                |
| SOT      | RevenueCat Experiment CSV |

## 기준

- Android Pricing C는 완료 / 성공.
- iOS는 별도 가격 실험 진행 중.
- 성공 기준: Realized LTV per customer 1.45x+.

## 결과

| Metric | Variant A | Variant B | 결과 |
|---|---:|---:|---:|
| Customers | 64 | 58 |  |
| Initial conversion rate | 20.31% | 20.69% | 1.02x |
| Paid customers | 13 | 12 | 0.92x |
| Refunded customers | 2 | 2 | 1.00x |
| Realized LTV / customer | ₩2,905 | ₩4,843 | 1.67x |
| Realized LTV / customer, proceeds | ₩1,849 | ₩3,082 | 1.67x |
| Realized LTV / paying customer | ₩14,300 | ₩23,408 | 1.64x |
| MRR / customer | ₩2,905 | ₩3,452 | 1.19x |
| MRR set to renew, proceeds | ₩64,531 | ₩46,669 | 0.72x |

## 판정

- 성공 유지: refund 포함 보수 기준 `proceeds / customer` 1.67x.
- 전환율 방어: 20.31% -> 20.69%.
- 환불 악화 없음: 2 -> 2.
- 가드: `set to renew MRR` 0.72x. renewal cohort 재확인 필요.
- KPI 인덱스 결과: 1.67x.

## 근거

- RevenueCat CSV: `/Users/cheonmyeongseung/Downloads/rc-1781064536667-experiment-ios 가격 올리기.csv`
- RevenueCat Experiment: https://app.revenuecat.com/projects/1589ca27/experiments/prexpfd7a86efc2
- PostHog 보조 확인, 2026-06-09 이후: iOS `rc_paywall_show` pricing_b 24명 / pricing_c 39명, `payment_start` pricing_b 8명 / pricing_c 5명, `rc_purchase_success` pricing_b 5명 / pricing_c 3명.
