| 항목 | 값 |
|---|---|
| 기준 | 2026-06-23~2026-07-06 KST |
| 질문 | CPD 악화가 Meta 문제인지 제품 문제인지 |
| 판정 | 주원인 Meta delivery / mix, 보조원인 Message -> Handoff 하락 |
| SOT | `/admin/aarrr2`, `cost_daily.meta` |
| 상태 | 1차 원인 분해 완료 |

## 결론

| 원인 | 비중 | 근거 |
|---|---:|---|
| Meta delivery / mix | 55~65% | CPM +39%, Meta-attributed CPD +69%, Value_LAL 확대, cheap adset 제거 |
| Product / handoff 구간 | 35~45% | Message -> Handoff -12.2%p |
| Payment/paywall | 낮음 | Handoff -> Pay 유지 |
| Device -> Message | 낮음 | 33.4% -> 33.0%, 거의 동일 |

## 현상

| Metric | Base 06-23~06-28 | Bad 07-02~07-05 | 변화 |
|---|---:|---:|---:|
| Cost / Device | ₩477 | ₩692 | +45% |
| Cost / Pay | ₩14,870 | ₩29,137 | +96% |
| Device -> Message | 31.4% | 28.3% | -3.1%p |
| Message -> Handoff | 41.2% | 30.3% | -10.9%p |
| Handoff -> Pay | 24.8% | 27.7% | +2.9%p |
| Device -> Pay | 3.21% | 2.38% | -26% |
| New User ROI | 178.4% | 44.9% | -133.5%p |

## Meta delivery 분해

Meta-attributed adsets 기준.

| Metric | Base | Bad | 변화 |
|---|---:|---:|---:|
| Spend | ₩2.12M | ₩1.48M | -30% |
| Impressions | 69,451 | 34,828 | -50% |
| Devices | 4,144 | 1,705 | -59% |
| CPM | ₩30,548 | ₩42,462 | +39% |
| Devices / 1k imp | 59.7 | 49.0 | -18% |
| Cost / Device | ₩512 | ₩867 | +69% |

수학 분해:

| 항목 | CPD 기여 |
|---|---:|
| CPM 상승 | +₩200 |
| Impression -> Device yield 하락 | +₩155 |
| 합계 | +₩355 |

판정:

- CPD 상승은 제품 post-funnel이 아니라 Meta delivery / mix에서 먼저 발생.
- CPM 상승만으로 CPD 상승의 약 56%.
- 나머지 약 44%는 노출 대비 device 생성률 하락.

## Product funnel 분해

Meta-attributed adsets 기준.

| Funnel | Base | Bad | 변화 |
|---|---:|---:|---:|
| Device -> Message | 33.4% | 33.0% | -0.4%p |
| Message -> Handoff | 44.9% | 32.7% | -12.2%p |
| Handoff -> Pay | 23.3% | 23.9% | +0.6%p |
| Device -> Pay | 3.50% | 2.58% | -26% |

판정:

- 제품 전체 장애 가능성 낮음.
- `Device -> Message` 유지.
- `Handoff -> Pay` 유지.
- 문제 구간은 `Message -> Handoff`.
- 해석: handoff UX 또는 handoff-sensitive traffic quality 문제.

## Adset 원인

| Adset | Base CPD | Bad CPD | 판정 |
|---|---:|---:|---|
| KR_All_OS_Broad | ₩586 | ₩963 | 최대 악화 |
| KR_All OS_Value_LAL | ₩567 | ₩1,168 | spend 확대 + 고가 |
| KR_iOS_Winner_Interests | ₩487 | ₩1,150 | pay 0 |
| KR_Pre-heat_Broad | ₩533 | ₩533 | 원인 아님 |
| 캐릭터챗_남성 - iOS mobile only | ₩298 | ₩0 spend | cheap traffic 제거 |

## Ad 원인

| Ad | Bad Cost | Bad CPD | ROI | 판정 |
|---|---:|---:|---:|---|
| Hannam_river_dating - CBO Broad | ₩784,639 | ₩1,419 | -25.7% | 큰 손실 |
| Video-You_are_so_kind - Copy 2 | ₩403,847 | ₩1,346 | -11.6% | Value_LAL 고가 |
| Video-You_are_so_kind - Copy | ₩308,416 | ₩1,371 | -33.6% | 낮은 pay |
| [KR] Pre-heat - No_rules_girl (ctrl) | ₩388,145 | ₩717 | 66.6% | 상대적으로 유지 |

## 다음 확인

- `07-02`, `07-06` ad_breakdowns 비어 있음 -> ad-level 클릭/CTR 판정 제외.
- adset-level spend/impression은 사용 가능.
- 오늘 회복 여부는 마감 후 재확인.
- handoff 구간은 `Message -> Handoff`만 별도 OS/adset split 재확인.

## 근거

- `/admin/aarrr2`, `period=custom`, `2026-06-23~2026-07-06`, `main`, `localhost:3000`.
- `cost_daily.source='meta_ads'`, `meta.campaign_breakdowns`, `meta.ad_breakdowns`.
- 조회 시각: 2026-07-06 KST.
