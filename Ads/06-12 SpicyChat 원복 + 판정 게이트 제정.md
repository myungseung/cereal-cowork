| 항목  | 값                                         |
| --- | ----------------------------------------- |
| 상태  | 실행 완료 (06-12)                             |
| 기준  | 06-05~06-11 마감 7d, meta-optimize + AARRR2 |

## 결정

1. SpicyChat iOS (120242167475440520) 예산 ₩10k → **₩40k 원복** (founder 수동 실행, API 확인 완료)
2. KR_AOS_Winner_Interests OFF 제안 → **철회** (부분일 데이터 오판)
3. AI_pet CBO 조기 판정 시도 → **철회**, 원계약 D+7 (06-18) 유지
4. CBO 증액 → **06-14로 연기** (06-11 증액 후 72h 쿨다운)
5. 판정 게이트 룰셋 제정 → `cereal-ai/.agents/skills/meta-optimize/GATES.md`

## 근거 (마감 7d, 06-05~11)

| 대상 | Spend | Pay | ROI | 판정 |
|---|---:|---:|---:|---|
| KR_iOS_Winner_Interests | ₩107,932 | 10 | +345% | SCALE 대기 |
| KR_All OS_Value_LAL | ₩236,076 | 19 | +164% | SCALE 대기 |
| KR_Pre-heat_Broad | ₩97,689 | 9 | +107% | KEEP |
| SpicyChat iOS | ₩218,585 | 11 | +104% | ₩40k 원복 |
| KR_AOS_Winner_Interests | ₩134,650 | 6 | +59% | KEEP (오늘포함 -32%는 부분일 노이즈) |
| KR_All_OS_Broad | ₩462,170 | 24 | +46% | 본전권 관찰 |
| No_rules_girl (소재) | ₩348,244 | 28 | +147% | 유일 winner |
| AI_pet CBO 승격분 | ₩29,603 | 0 | -100% | 표본 부족, 06-18 판정 |

- SpicyChat 원복 사유: 06-11 감액은 Meta 귀속 ROAS(1.34x) 기반 오판. iOS는 Meta가 결제 ~30% 누락 → AARRR2 실측 +104~129% 흑자
- 순이익 손익분기: ROI +38% (순이익 = 매출×0.725 − 광고비)

## 게이트 요약 (전문은 GATES.md)

- 판정 도구: meta-optimize만. meta-roas(Meta 귀속)로 판정 금지
- 윈도우: 어제 마감 7d만. 오늘 포함 데이터 판정 무효
- 표본: spend ₩50k+ / devices 100+ / 집행 7일+ 전부 충족해야 판정
- OFF: ROI<+38% 연속 2윈도우 (단일은 ROI<-30%만)
- SCALE: ROI≥+100% → +30% step, 쿨다운 72h, 48h ROI<+38%면 롤백

## 기대 효과

- SpicyChat 원복: 주간 순이익 +₩130k 손실 차단
- 증액 스케줄 (06-14 ₩300k → 06-17 ₩390k → …): 월말 하루 수익 보수 ₩230k / 목표 ₩480k / 직접챗 적중 시 ₩950k

## 재검토 조건

- 06-13 마감: 06-11 증액분 48h ROI ≥ +38% 확인 → 06-14 ₩300k 실행
- 06-15: 직접 챗 판정 (Visit→Pay 4.3/5.0/6.0% 게이트 + msg% 병행) + DC_* 배분 왜곡 확인 (GRWM 1개만 실집행 중)
- 06-18: AI_pet CBO D+7 (<50% 제거)
- 미해결: 6월 비용 집계에 turso/vercel/novelai 누락 → 순이익 KPI 정확도 구멍, 데이터 확인 필요
