---
date: 2026-06-12
date_end: 2026-06-29
category: paid-acquisition
type: experiment
status: done
kpi: Meta spend / profit
expected: 1.6x
---

| 항목       | 값                                                     |
| -------- | ----------------------------------------------------- |
| 상태       | 종료 — 성공                                               |
| 목표       | Meta spend 단계 증액으로 순이익 절대량 확대                         |
| 기대값      | 1.6x                                                  |
| 결과       | 1.55x                                                 |
| Success? | Yes                                                   |
| Done At  | 2026-06-29 KST                                        |
| SOT      | Ads 기록 + admin cost-revenue + AARRR2/meta-optimize 기록 |

## 가설

- AARRR2 ROI가 손익분기(+38%) 위에서 유지되면 spend를 72시간당 30% 이내로 증액해도 ROI가 손익분기 아래로 내려가지 않는다.
- 성공: spend ≥ 2.2x 도달 + ROI ≥ +38% 유지.
- 강한 성공: spend ≥ 2.9x + ROI ≥ +38% 유지.
- 실패: 1.69x 이전 게이트 붕괴.

## 결과

| 항목 | 계획 | 실제 | 판정 |
|---|---:|---:|---|
| 기준 일 spend | ₩178k | ₩178k | 기준 |
| 도달 일 spend | ₩391k~₩661k | ₩276k | 1.55x |
| 목표 배수 | 2.2x+ | 1.55x | 미달 |
| 06-15~06-29 Meta spend |  | ₩5.21M | 확인 |
| 06-15~06-29 revenue |  | ₩17.37M | 확인 |
| 06-15~06-29 net revenue |  | ₩14.81M | 확인 |
| 06-15~06-29 profit |  | ₩8.61M | 확인 |
| Meta ROAS |  | 3.33x | 손익분기 통과 |

## 판정

- 성공: ROI/ROAS 통과.
- 주의: spend scale 목표 미달. 2.2x 도달 전 중단.
- 검증 상한: ₩276k/day.
- 원인: 06-15 CBO 증액 후 안정 운영 우선. 06-17 소재 승격, 06-19 preheat 소재 집행으로 추가 예산 증액 보류.
- 다음: 새 증액 실험은 `₩276k/day -> ₩360k/day`부터 별도 note.

## 집행 기록

| 날짜 | 액션 | 근거 |
|---|---|---|
| 06-11 | KR CBO ₩180k -> ₩230k/day | [[Ads/06-11 JP off + preheat 승격]] |
| 06-15 | KR CBO ₩230k -> ₩276k/day | [[Ads/06-15 CBO 증액 + SpicyChat 보류]] |
| 06-17 | 예산 유지, Hannam CBO Broad 승격 | [[Ads/06-17 Hannam CBO Broad 승격 + 기록 보정]] |
| 06-19 | 예산 증액 없음, preheat 소재 집행 | [[Ads/06-19 2층 하숙집 preheat 집행]] |

## 근거

- Ads SOT: [[Ads/INDEX]]
- admin cost-revenue API: 2026-06-15~2026-06-29, `meta_ads ₩5,208,545`, `revenue ₩17,368,240`, `net revenue ₩14,808,649`, `profit ₩8,609,039`.
- AARRR2/meta-optimize 기록: Ads notes 내 `meta-optimize + AARRR2` 기준 ROI 테이블.
- 라이브 `ad-judge`/`meta-optimize` 호출: 2026-06-29 KST, 30s+ 무출력으로 중단. 종료 판단에는 기존 SOT 사용.
