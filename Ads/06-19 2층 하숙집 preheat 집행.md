| 항목 | 값 |
|---|---|
| 상태 | 실행 완료 + 심사/처리 대기 |
| 기준 | 2026-06-13~2026-06-19, Meta API + `meta-optimize.mjs` |

## 결정

1. 새 preheat 영상 소재 집행
   - ad: `[KR] Pre-heat - 2F_boarding_house_girls_copyfit_0619 (challenger)`
   - ad id: `120249553310030520`
   - campaign: `[KR] Pre-heat_Video_Test` (`120248667608460520`)
   - adset: `KR_Pre-heat_Broad` (`120248667608550520`)
   - creative id: `995422043213254`
   - video id: `1201587072063459`
   - status: `ACTIVE`
   - effective_status: `IN_PROCESS`
2. 카피를 기존 preheat 우수 소재 톤으로 교체
   - 기준 소재: `[KR] Pre-heat - Hannam_river_dating (challenger)`
   - 기존 title: `한강 데이트 느낌`
   - 기존 description: `익명으로 바로 시작`
   - 새 title: `2층 하숙집 느낌`
   - 새 body: `이런 캐릭터들이 2층에서 기다린다 하면… 진짜 말 걸어보게 됨`
   - 새 description: `익명으로 바로 시작`
3. 설명문 톤 초안 pause
   - ad id: `120249553292260520`
   - 상태: `PAUSED`
   - 사유: 기존 preheat 소재 톤과 불일치
4. 월드컵 다영 preheat 집행
   - ad: `KR_Preheat_Worldcup_Dayoung_SafeArea_0619`
   - ad id: `120249555070710520`
   - campaign: `[KR] Pre-heat_Video_Test` (`120248667608460520`)
   - adset: `KR_Pre-heat_Broad` (`120248667608550520`)
   - creative id: `1799196084393473`
   - video id: `1829604711351289`
   - status: `ACTIVE`
   - effective_status: `IN_PROCESS`
   - 주의: Instagram 광고 UI safe-area 반영, 영상 내 하단 CTA 제거

## 근거

| 대상 | Spend | Devices | Pay | Revenue | ROI | 판정 |
|---|---:|---:|---:|---:|---:|---|
| `[KR] Pre-heat_Video_Test` | ₩276,156 | 884 | 25 | ₩1,120,200 | +305.6% | SCALE |
| `KR_Pre-heat_Broad` | ₩276,156 | 721 | 21 | ₩966,200 | +249.9% | SCALE |
| `Hannam_river_dating` preheat | ₩146,459 | 201 | 7 | ₩425,700 | +190.7% | KEEP |

## 소재

| 항목 | 값 |
|---|---|
| R2 video | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/boarding-house-stairs-close-bright-7s-dialogue/20260619071242/video-7s-overlay-8f16eb0fc2.mp4 |
| 규격 | 720x1280 / 7.04s / audio 있음 |
| 음성 | `2층에서 기다릴게, 올라올래?` |
| 랜딩 | `https://cereal-ai.com/ko/create` |
| UTM | `utm_source={{campaign.id}}&utm_campaign={{adset.id}}&utm_content={{ad.id}}` |

## 소재 2

| 항목 | 값 |
|---|---|
| 이름 | 월드컵 다영 safe-area poster |
| R2 video | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/worldcup-red-devils-dayoung/20260619065750-safearea-poster/video-5s-safearea-poster-831579ef85.mp4 |
| 규격 | 720x1280 / 5.04s / audio 있음 |
| 음성 | `부회장 말고... 그냥 다영이라고 불러.` |
| 랜딩 | `https://cereal-ai.com/ko/create` |
| UTM | `utm_source={{campaign.id}}&utm_campaign={{adset.id}}&utm_content={{ad.id}}` |
| 카피 | `월드컵 밤의 AI 캐릭터` / `월드컵 보다가 문득 말 걸고 싶은 캐릭터. 네 말투에 맞춰 편하게 이어지는 AI 채팅.` |

## 재검토

| 항목 | 기준 | 액션 |
|---|---|---|
| 새 preheat 소재 | 승인 + 24h delivery | spend/click/device 초기 확인 |
| 새 preheat 소재 | spend ₩50k+ / devices 100+ / 7일+ | CBO 승격 or OFF 판단 |
| 새 preheat 소재 | `WITH_ISSUES` 또는 장시간 `IN_PROCESS` | 소재/문구 추가 변경 중단 |

## 근거 경로

- `cereal-ai/.agents/skills/meta-setup/scripts/meta-setup.mjs`
- `cereal-ai/.agents/skills/meta-optimize/scripts/meta-optimize.mjs`
- Meta API 확인: 2026-06-19 KST
