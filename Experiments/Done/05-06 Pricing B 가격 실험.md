---
date: 2026-05-06
date_end: 2026-05-16
category: pricing-revenue
type: experiment
status: done
kpi: ARPU
expected: 2x
---

| **Success**? | Yes                                                |
| ------------ | -------------------------------------------------- |
| Impact       | ARPU 개선                                            |
| Input Metric | Paywall, Payment Start, Success, Revenue / Paywall |
| 실험           | iOS 앱 RevenueCat 가격 B 상품군                          |
| 병목           | 메시지 시작 -> 결제                                       |
| Created At   | 05-06                                              |
| Done At      | 05-16                                              |

### Result

| Metric | control | treatment_b | Delta |
|---|---:|---:|---:|
| Assigned iOS app | 43 | 35 | - |
| Paywall | 36 | 29 | - |
| Payment Start | 13 | 10 | -23.1% |
| Paywall→Start | 36.1% | 34.5% | -4.5% |
| Success | 8 | 7 | -12.5% |
| Paywall→Revenue | 22.2% | 24.1% | +8.6% |
| Revenue | ₩93,900 | ₩145,400 | +54.8% |
| Revenue / Paywall | ₩2,608 | ₩5,014 | +92.2% |
| Revenue / Assigned | ₩2,184 | ₩4,154 | +90.2% |

### Decision

- 결과: `treatment_b` 승리.
- 이유: 결제 전환율 동일권, 객단가 +54%.
- 적용: iOS 앱 B 가격군 유지.
- winner: B 가격군.

### 근거

- 2026-05-16 데이터 확인: `pricing-b-ios-app-funnel --hours 168`.
- 기준: `experiment_assignments` ∩ `platform=ios_app`.
- 제외: web / Android / iOS Safari handoff-only.
