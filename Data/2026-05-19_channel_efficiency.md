# 2026-05-19 채널 효율

- 기준일: 2026-05-19 KST
- 범위: 최근 7일, 2026-05-13 ~ 2026-05-19
- 기준: `/admin/aarrr2?period=7` 직접 조회
- YouTube 비용: 사용자 제공 Google Ads 스크린샷

## UTM 라벨
| 라벨 | 의미 |
|---|---|
| `instagram_organic` | Arin Instagram Secret story -> `/create` |
| `instagram_dm` | Arin Instagram VVIP story -> `/preset_arin` |
| `Unknown` | UTM 누락 / 직접 방문 / 앱 전환 중 추적 끊김 가능 |
| `120239990728680520` | Meta Ads 유일 캠페인 |
| `youtube` | YouTube 광고 |

## 채널 효율
| 채널 | Visit | MSG user | Visit -> MSG | Paywall | Pay | App Pay | Paywall -> Pay | Revenue | Cost | ROI | 핵심 |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---|
| Arin Secret `/create` | 4,130 | 488 | 11.8% | 429 | 19 | 13 | 4.4% | ₩481,640 | ₩0 | N/A | 유입 큼, 대화 시작 낮음 |
| Arin VVIP `/preset_arin` | 2,688 | 647 | 24.1% | 547 | 17 | 12 | 3.1% | ₩388,740 | ₩0 | N/A | 대화 시작 2.0x |
| Arin Unknown | 641 | 197 | 30.7% | 112 | 22 | 14 | 19.6% | ₩365,850 | ₩0 | N/A | 작은 모수, 결제 강함 |
| Meta Ads | 1,080 | 420 | 38.9% | 309 | 19 | 15 | 6.1% | ₩515,740 | ₩405,838 | 27.1% | paid 승자 |
| YouTube | 240 | 30 | 12.5% | 17 | 3 | 3 | 17.6% | ₩90,800 | ₩104,341 | -13.0% | CPC 낮음, 내부 전환 약함 |

## Unknown 해석
- iOS 유저는 결제 전 앱 설치 필요.
- 앱 설치 후 새 device id가 생기면 기존 Instagram UTM이 끊길 수 있음.
- Unknown 결제 22건 중 App Pay 14건.
- App Pay 14건 중 iOS App Store 13건.
- 따라서 `Unknown = 앱 전환 중 UTM 유실` 가능성 높음.
- 단, Secret / VVIP / Meta도 App Pay 비중 높음.
- 결론: Unknown 고효율은 `앱 설치 유저 + UTM 유실 + 재방문/직접 유입`이 섞인 하단 퍼널 유저로 해석.

## 판정
- Arin 직접 링크는 `/create`보다 MSG user 효율 높음.
- Unknown은 버리면 안 됨. 추적 누락 가능성 있음.
- Meta는 paid 중 유지 가능.
- YouTube는 현재 중단/축소 판단.

## 근거
- Cereal: 2026-05-19 `/admin/aarrr2?period=7` 직접 조회.
- Instagram: 2026-05-19 사용자 제공 screenshot.
- YouTube: 2026-05-19 사용자 제공 Google Ads screenshot.
