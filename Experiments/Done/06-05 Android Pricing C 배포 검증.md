| 항목 | 값 |
|---|---|
| 상태 | Android 완료 / iOS 진행 중 |
| 실험 | RevenueCat `prexp73342a4345` |
| 대상 | Android new customers |
| 후보 | Pricing C |
| 목표 | Android 완료 판정 + iOS 가격 실험 진행 |
| SOT | RevenueCat Experiment UI + Turso `rc_subscription_events` |

## 최종 판정

- Android 가격 실험 완료.
- 결과: 성공.
- 현재: iOS 가격 실험 진행 중.
- 근거 CSV: `/Users/cheonmyeongseung/Desktop/rc-1780974943149-experiment-android 가격 올리기.csv`

## 판정

| 질문 | 판정 | 근거 |
|---|---|---|
| Android 성공? | 성공 | Proceeds/customer A ₩3,253 -> B ₩7,767, +138.77% |
| 환불 문제? | 없음 | Refunded customers A 0 / B 0 |
| 결제전환 유지? | 유지 | Conversion 16.13% -> 18.31%, +13.52% |
| iOS 전체 적용? | 진행 중 | iOS 가격 실험 진행 중 |

## RevenueCat Full Report

| Metric | A Pricing B | B Pricing C | Change |
|---|---:|---:|---:|
| Customers | 93 | 71 | - |
| Paid customers | 15 | 13 | -13.33% |
| Conversion to paying | 16.13% | 18.31% | +13.52% |
| Refunded customers | 0 | 0 | - |
| Realized LTV proceeds | ₩302,521 | ₩551,461 | +82.29% |
| Proceeds/customer | ₩3,253 | ₩7,767 | +138.77% |
| Proceeds/paying customer | ₩20,168 | ₩42,420 | +110.33% |
| MRR/customer proceeds | ₩1,816 | ₩3,283 | +80.80% |

## Turso Cross-check

| Metric | A Pricing B | B Pricing C | Change |
|---|---:|---:|---:|
| 구매자 | 14 | 12 | -14.3% |
| Gross | ₩374,600 | ₩688,800 | +83.9% |
| Net 추정 | ₩318,410 | ₩585,480 | +83.9% |
| Net/구매자 | ₩22,744 | ₩48,790 | +114.5% |
| 취소율 | 50.0% | 33.3% | -16.7%p |
| 환불 | 0 | 0 | - |

## 다음

- [x] Android Pricing C winner 확정.
- [x] Android 가격 실험 완료.
- [ ] iOS 가격 실험 진행 확인.
- [ ] iOS guardrail: conversion, refunded customers, set-to-renew proceeds.

## 근거

- RevenueCat CSV 확인: 2026-06-09.
- RevenueCat subscriber API: `subscriber.experiment.id = prexp73342a4345`, variant 매핑 확인.
- Turso `rc_subscription_events`: product_id arm 역분류 26명 중 25명 일치, mismatch 0.
