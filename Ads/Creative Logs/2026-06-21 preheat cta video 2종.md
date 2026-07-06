| 항목 | 값 |
|---|---|
| 상태 | Meta preheat 2종 ACTIVE 등록 |
| 날짜 | 2026-06-21 KST |
| 캠페인 | `[KR] Pre-heat_Video_Test` |
| 애드셋 | `KR_Pre-heat_Broad` |
| 기준 | 9:16 검증 + Meta 생성 ID 확인 |

## 실행

| 소재 | R2 video | 길이 | 광고명 | ad id | creative id | status |
|---|---|---:|---|---|---|---|
| 기모노 플로럴 CTA | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/no-rules-kimono-floral-girlfriend-slow-lipsync-12s-cta-fixed/20260621051125/video-overlay-c61b54f8ba.mp4 | 12.04s | `KR_Preheat_Kimono_Floral_SlowLipsync_CTA_0621` | `120249661286130520` | `1285581723650589` | `PENDING_REVIEW` |
| 루프탑 CTA | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/no-rules-rooftop-longline-cta-glass-pro-clean/20260621043755/video-overlay-9d82592a34.mp4 | 10.04s | `KR_Preheat_Rooftop_Twilight_CTA_0621` | `120249661294220520` | `4478537765715370` | `IN_PROCESS` |

## 카피

| 소재 | title | message | description |
|---|---|---|---|
| 기모노 | 플로럴 기모노 느낌 | 이런 캐릭터가 조용히 네 편 들어준다 하면… 진짜 말 걸어보게 됨 | 익명으로 바로 시작 |
| 루프탑 | 루프탑 밤공기 느낌 | 이런 캐릭터가 밤공기 속에서 네 얘기 들어준다 하면… 진짜 계속 말 걸게 됨 | 익명으로 바로 시작 |

## 근거

- 권한 audit: `RESULT PASS | meta setup automation ready`
- campaign id: `120248667608460520`
- adset id: `120248667608550520`
- link: `https://cereal-ai.com/ko/create`
- UTM: `utm_source={{campaign.id}}&utm_campaign={{adset.id}}&utm_content={{ad.id}}`
- 정책 회피: 카피에서 `no rules`, `AI 여자친구`, `비밀 이야기` 미사용
- CTA 노하우 기록: `.agents/skills/video-text-overlay/SKILL.md`
