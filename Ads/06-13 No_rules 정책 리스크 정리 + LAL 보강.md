| 항목  | 값                                    |
| --- | ------------------------------------ |
| 상태  | 실행 완료 + 대기 (06-13)                   |
| 기준  | 06-06~06-12 마감 7d, Meta API + AARRR2 |

## 결정

1. `KR_AOS_Winner_Interests` → **PAUSED**
   - adset id: `120248269167930520`
   - 근거: ROI -37.6% / 단일윈도우 ROI<-30%
2. LAL rejected `No_rules_girl` → **삭제 안 함, PAUSED 유지**
   - ad id: `120248296735080520`
   - 사유: 삭제해도 정책 이력 제거 근거 없음 / 성과 근거 보존
3. `No_rules` 복구 → **기존 shared creative 수정 금지**
   - rejected creative id `1657282095533363`
   - ACTIVE 공유: `KR_iOS_Winner_Interests`, `KR_All_OS_Broad`
4. Preheat 안전 버전 테스트
   - body-only 테스트 `KR_0613_No_rules_safe_AI_mate` → PAUSED
   - 잘못 만든 `KR_0613_No_rules_mild_video` → PAUSED
   - 현재 테스트 `KR_0613_No_rules_mild_friend` → ACTIVE / PENDING_REVIEW
5. LAL 소재 공백 보강
   - `Video-AI_pet (preheat winner) - LAL` 추가
   - ad id: `120249107706220520`
   - 수동 수정 후 creative/post 단절 확인
6. Preheat 정리
   - `[KR] Pre-heat - AI_pet (challenger)` → PAUSED
   - ad id: `120248724984310520`
7. 증액 룰 변경
   - CBO 증액: 72h window / +20%
   - 다음 후보: `₩230k -> ₩276k`
   - 가능 시점: 06-14 14:09 KST 이후

## 근거

| 대상 | Cost | Pay | Revenue | ROI | 판단 |
|---|---:|---:|---:|---:|---|
| 전체 광고 실제 | ₩1,038,695 | 77 | ₩2,823,480 | +171.8% | 기준 |
| LAL > No_rules 제거 후 | ₩744,364 | 59 | ₩2,049,080 | +175.3% | ROI 영향 작음 |
| LAL > No_rules | ₩294,331 | 18 | ₩774,400 | +163.1% | 매출 볼륨 큼 |
| KR_AOS_Winner_Interests 전체 | ₩124,572 | 3 | ₩74,700 | -40.0% | OFF |
| AOS 내 No_rules | ₩48,018 | 2 | ₩49,800 | +3.7% | 낮음 |

## No_rules adset별 7d

| Adset | Cost | Pay | Revenue | Rev share | ROI |
|---|---:|---:|---:|---:|---:|
| KR_All OS_Value_LAL | ₩294,331 | 18 | ₩774,400 | 63.4% | +163.1% |
| KR_Pre-heat_Broad | ₩49,541 | 7 | ₩291,800 | 23.9% | +489.0% |
| KR_All_OS_Broad | ₩21,265 | 3 | ₩83,600 | 6.8% | +293.1% |
| KR_AOS_Winner_Interests | ₩48,018 | 2 | ₩49,800 | 4.1% | +3.7% |
| KR_Pre-heat_DirectChat | ₩18,828 | 1 | ₩22,000 | 1.8% | +16.8% |

## 정책 운영 룰

- rejected ad: pause + evidence 보존. 삭제는 Account Quality 신호 확인 후.
- review 요청 남발 금지.
- shared creative 수정 전: 해당 creative를 쓰는 ACTIVE ad 전부 확인.
- `필터 없음`, `검열 없음`, `규칙 없음`, `no rules`, `AI 여자친구`, `여자친구`, `뭐든 해줄게`, `어떤 옷`, `사진 보내줄까`, `비밀 이야기`, `현실에선 말 못 할` 금지.
- 영상 음성/자막도 심사 대상.
- 새 영상 광고 업로드 전 9:16 확인 필수. 9:16 아니면 업로드 금지.
- Ads Manager crop/placement customization/text edit/video replacement는 새 creative/post 생성 가능. 진행 전 post id 단절 경고 필수.
- Scaling CBO에는 preheat 승인 + 24~48h delivery 확인 후 투입.
- 다계정은 국가/브랜드/결제 분리용만. 정책 회피용 금지.

## 현재 대기

| 항목 | 상태 | 다음 |
|---|---|---|
| `KR_0613_No_rules_mild_friend` | PENDING_REVIEW | 승인 + 24~48h delivery 확인 |
| `Video-AI_pet (preheat winner) - LAL` | PENDING_REVIEW | 수동 crop 후 post id 변경 확인됨 |
| CBO 증액 | 대기 | 06-14 14:09 KST 이후 `₩276k` 검토 |
| Preheat Direct | 실험계약 | 06-15 Visit→Pay 게이트 |

## 기록 보정 — 06-17 확인

| 항목 | 값 |
|---|---|
| 대상 | `KR_0613_No_rules_mild_friend` |
| ad id | `120249105820700520` |
| 현재 상태 | `PAUSED` |
| pause/update 시각 | `2026-06-15 13:20 KST` |
| 06-13~06-17 Meta spend | ₩98,259 |
| 06-13~06-17 Meta purchase | 3 |
| 기록 상태 | pause 의사결정 출처 불명확 |

- 기존 기록은 `ACTIVE / PENDING_REVIEW`까지만 존재.
- `mild_friend`는 “성과 없음”이 아니라 “AARRR 실매출/ROI 확인 전 보류”.
- 재활성화 전 ad id 기준 AARRR 실매출 확인 필요.

## 단절 확인

| 항목 | 이전 | 현재 |
|---|---|---|
| AI_pet LAL creative | `4308460432750108` | `2101189654150554` |
| AI_pet LAL post id | `642236832301451_122187157694837790` | `642236832301451_122187852914837790` |
| 원인 | 기존 creative 재사용 | 문구 수정 + 9:16 crop / placement customization |

## 근거 경로

- `cereal-ai/.agents/skills/meta-setup/SKILL.md`
- `cereal-ai/.agents/skills/meta-optimize/scripts/ad-judge.mjs`
- Ads Manager / Meta API 확인: 06-13 KST
