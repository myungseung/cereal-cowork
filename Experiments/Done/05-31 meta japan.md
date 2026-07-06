---
date: 2026-05-31
date_end: 2026-06-11
category: paid-acquisition
type: experiment
status: done
kpi: JP paid volume
expected: 2x
---


ads ref jp
https://youtube.com/shorts/w4GLqz9nZGs?si=xE0AwIA1jJQ0fLH1
https://youtube.com/shorts/uZ0GbacWD5M?si=pPcTe9A8IphIs5NQ
https://youtube.com/shorts/zVqOh6f-3bU?si=503czP_vqhrWFBLP

| 항목 | 값 |
|---|---|
| 상태 | 종료 — 06-11 캠페인 OFF |
| 목표 | JP paid volume 2x |
| 현재 | 0.37x |
| 기준 | 06-07~06-10 KST, JP Devices 439 / KR Devices 1,200 |
| 문제 | Device -> Message / App claim -> Pay |
| 다음 | parking 해제 후 JP 대화 품질 job 완료 시 재개 |

## 06-10 성과

| Metric | JP | KR |
|---|---:|---:|
| Meta Spend | ₩191,931 | ₩825,330 |
| Meta Link Clicks | 471 | 1,560 |
| DB Devices | 439 | 1,200 |
| Messages | 120 | 554 |
| Handoff | 112 | 524 |
| App claim | 25 | 207 |
| New Pay 실제 | 1 | 38 |
| Revenue 실제 | ¥9,900 | ₩1,371,400 |

| Funnel | JP | KR |
|---|---:|---:|
| Device -> Message | 27.3% | 46.2% |
| Message -> Handoff | 93.3% | 94.6% |
| Handoff -> App claim | 22.3% | 39.5% |
| App claim -> Pay | 4.0% | 18.4% |
| Device -> Pay | 0.23% | 3.17% |

## 06-10 판정

- iOS 차단: 06-09 이후 Meta iOS 집행 0.
- Android 결제: 1건 / ¥9,900.
- JP 클릭/Device 단가: 유지 가능.
- JP 병목: 대화 시작 낮음 + 앱 결제 전환 낮음.
- KR 대비 약점: Device -> Message `-18.9%p`, App claim -> Pay `-14.4%p`.
- 다음: Android-only 유지, 광고 캐릭터 직접 챗 매칭으로 Device -> Message 우선 개선.

## 06-10 admin api-logs JP 필터 정정

| 항목 | 값 |
|---|---:|
| JP 7일 유입 devices | 439 |
| JP 7일 메시지 작성 devices | 153 |
| 기존 화면 오집계 | 3~4 devices |
| 원인 | `api_logs` recent-N 후 JP 필터 적용 |
| 수정 | JP 전용 `devices.utm_source -> api_logs` 조인 |

- URL: `http://localhost:3000/admin/api-logs?ab=jp`
- 기준 캠페인: `120248667534000520`
- 결론: JP 대화 샘플링은 3명이 아니라 전체 439 / 메시지 153 기준으로 봐야 함.

## 06-10 JP 대화 언어 품질

| Metric | 값 |
|---|---:|
| JP devices | 439 |
| Assistant messages | 990 |
| 한글 포함 응답 | 22 |
| 한글 포함률 | 2.22% |
| 일본어 문자 포함률 | 98.28% |

- 문제: 일부 캐릭터 응답이 한국어로 출력.
- 범위: 전체 응답 중 낮음. 단, JP 첫 경험에는 치명적.
- 다음: JP 직접 챗 매칭 전 캐릭터별 system prompt / 이름 / 첫 응답 언어 고정 확인.

## 06-10 JP 대화 질 샘플

| 구간 | 대상 | 평균 msg events | 평균 user msg | 한글 응답 | 어색한 JP 신호 |
|---|---:|---:|---:|---:|---:|
| Handoff | 10명 | 5.2 | 3.8 | 2.1% | 14.6% |
| Non-handoff | 8명 | 8.6 | 6.3 | 0% | 14.0% |
| KR Handoff | 10명 | 31.2 | 4.1 | 100% KR | 0% |

- Non-handoff 조건 충족 JP 유저: 8명뿐.
- Handoff 유저: 빠른 이미지/잠금/통화 유도. 전환 신호는 강하지만 대화는 짧음.
- Non-handoff 유저: 자기소개, 취미, 드라이브, 관계 탐색. 대화 품질은 더 자연스러움.
- JP 현지 감각 문제: `〇〇さん`, `オッパ`, 갑작스러운 통화/향수/방문 유도는 번역체 또는 업소 DM 느낌.
- KR 대비 차이: KR 유저는 바로 이미지/노골 요청. JP 유저는 초반 관계 맥락을 더 요구.
- 다음: JP 첫 3턴은 성적 급가속보다 이름/상황/관계 확인. 이미지 유도는 3~5턴 뒤.

## Assets
- 2026-06-07: R2 No_rules_girl: https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/meta-ads/jp/1780809059350.mp4
- 2026-06-07: Meta video_id No_rules_girl: `2525641081229885`
- ⏳ You_are_so_kind: generating...

---

## JP Campaign Structure (draft 2026-06-07)

### 근거: KR 7일 (05-31~06-06) winners
| Segment | CPA | ROI | 판정 |
|---|---|---|---|
| KR_iOS_Winner_Interests | ₩8,440 | 221.3% | Best |
| KR_All_OS_Value_LAL | ₩13,407 | 114.5% | Scale |
| You_are_so_kind (creative) | ₩11,171 | 267.7% | Best 소재 |

### JP Campaign Design

**1 Campaign: JP Cereal_Launch**
- Budget: ₩150,000/day (start conservative)
- Bidding: oCPM / Lowest Cost
- Optimization: conversions (purchase)

**4 Adsets (iOS-first + test):**

| # | Adset | Targeting | Budget | 근거 |
|---|---|---|---|---|
| 1 | JP_iOS_Interest_Winner | iOS only, JP, 18-34, interests: AI chatbot, anime, dating sim | ₩60,000 | KR iOS Winner 복제 |
| 2 | JP_iOS_Broad | iOS only, JP, 18-45, no interest | ₩30,000 | Reach test |
| 3 | JP_AOS_Interest | Android only, JP, 18-34, interests: anime, mobile game | ₩30,000 | Android test (KR 별도 캠페인 참고) |
| 4 | JP_All_OS_Value_LAL | All OS, JP, 18-45, Value LAL (purchase seed) | ₩30,000 | 스케일 대비 (LAL seed 생길 때까지 대기 가능) |

**3 Ad creatives:**

| # | 소재 | R2 URL | 언어 | 근거 |
|---|---|---|---|---|
| A | JP-Copy1 | 위 R2 영상 | JP | You_are_so_kind 스타일 JP 리메이크 |
| B | JP-Copy2 | 필요 | JP | Video Ad-KR 스타일 JP 버전 |
| C | JP-No_rules | 필요 | JP | No_rules_girl 스타일 (test only, 10% budget) |

### Budget Flow

| 단계 | 조건 | 예산 | 액션 |
|---|---|---|---|
| Day 1-3 | Launch | ₩150k/day | 4개 adset all active |
| Day 4-7 | CPA ≤ ₩12k adset 발견 시 | ₩200k/day | winner adset 증액, underperformer cut |
| Day 8+ | CPA ≤ ₩10k, ROI ≥ 100% | ₩300k/day | scale winner |

### Targeting notes
- JP iOS share ~65% (higher than KR) → iOS budget 60%+
- JP users: /ja landing, full JP UI/캐릭터/프롬프트
- Age: JP younger demo works (18-34 primary)
- Interests: アニメ, AIチャット, 恋愛シミュレーション, 乙女ゲーム
- Language: Japanese (ja) only

### Next Actions
1. [ ] JP 소재 A 확인 (R2 영상 OK), B/C 제작 또는 KR 영상 JP 자막/더빙
2. [ ] Meta Ads Manager에서 JP campaign 생성 (1 campaign, 4 adset)
3. [ ] UTM: utm_source={campaign_id} / utm_campaign={adset_id} / utm_content={ad_id}
4. [ ] /admin/aarrr2 확인 설정 — JP cost는 utm_source/campaign/content 기준으로 자동 집계됨
5. [ ] Launch 후 48h 첫 체크

## 06-11 최종 판정 (7일: 06-05~06-11)

| Funnel | JP | KR CBO | 차이 |
|---|---:|---:|---:|
| Spend | ₩191,931 | ₩827,356 | - |
| Device -> Message | 27.3% | 43.5% | -16.2%p |
| Handoff -> Pay | 0.9% | 13.1% | -12.2%p |
| Pay | 1 | 54 | - |
| ROI | -94.8% | +119.0% | -213.8%p |
| CPA | ₩191,931 | ₩15,321 | 12.5x |

- 결론: **OFF**. 타게팅 문제 아님 — 트래픽 도달 정상, 대화 시작/결제 전환 동시 붕괴.
- 원인: JP 로컬라이제이션 (한글 응답 2.2%, 번역체, 첫 3턴 급가속) = 제품 영역. parking mode 중 수정 불가.
- 재개 조건: parking 해제 후 JP 대화 품질 job (system prompt 언어 고정 + 첫 3턴 관계 맥락) 완료 시.
- 근거: meta-optimize 2026-06-11, AARRR2 7d
