## 목표

| 항목 | 값 |
|---|---|
| 작업 | 온보딩용 비디오 생성 |
| 사용 skill | `/Users/cheonmyeongseung/cereal-ai/.agents/skills/video-gen/SKILL.md` |
| 실행 방식 | `/video-gen` 규칙 준수 |
| 현재 SOT | `CEREAL_Cowork/spec/06-12 video onboarding.md` |
| 코드 branch | `video-onboarding` |
| 금지 | first image 임의 생성, 모델 비교, local mp4 저장, chat UI 변경 |

## 입력 상태

| 항목 | 값 |
|---|---|
| 테스트 캐릭터 | 야쿠자 보스 아내 x 잠입 스파이 |
| 승인 first image | `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-onboarding/characters/yakuza-boss-wife-spy-romance/20260613232858/first-frame.jpg` |
| 기존 p-video 테스트 | `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-onboarding/videos/yakuza-boss-wife-spy-romance-p-video/20260614003347/video-5s-eye-scan-living-still.mp4` |
| 레퍼런스 | `/Users/cheonmyeongseung/Desktop/app_reference_capture/toptoonchat/ScreenRecording_06-14-2026 09-05-15_1.MP4` |

## 핵심 판단

| 항목 | 결정 |
|---|---|
| 비디오 형식 | 긴 영상 1개 아님 |
| 목표 | Toptoonchat식 프롤로그 |
| 구성 | living-still 컷 묶음 + 텍스트 + 선택지 |
| 컷 목적 | 컷 1개 = 매력 1개 |
| 동작 | 눈, 숨, 표정, 고개, 손/소품 최소 움직임 |
| 시점 | viewer-POV |
| 서사 | 텍스트/선택지가 전달 |
| prompt 금지 | 과한 스토리, 큰 동작, 복잡한 안무 |

## 생성할 비디오

| 순서 | 컷 | 목적 | start image | dialogue |
|---:|---|---|---|---|
| 1 | eye scan living-still | 캐릭터 첫 매력 검증 | 승인 first image | 없음 |
| 2 | object/hand detail | 위험한 관계 암시 | 별도 first image 필요 | 없음 |
| 3 | face close-up | 시선/긴장감 | 별도 first image 필요 | 없음 |

## 이번 job 범위

| 단계 | 작업 | 산출물 |
|---:|---|---|
| 1 | 승인 first image로 5s sample 생성 | R2 video + manifest + Replicate URL |
| 2 | 움직임/identity/손/얼굴 gate 판정 | PASS/FAIL |
| 3 | PASS면 같은 컷 10s full 생성 | R2 video + manifest + Replicate URL |
| 4 | FAIL이면 prompt만 1회 수정 후 5s 재생성 | 비교 URL |
| 5 | 결과를 old spec에 URL로 기록 | `06-12 video onboarding.md` |

## video-gen 실행 규칙

| Rule | 값 |
|---|---|
| model | `kwaivgi/kling-v3-omni-video` |
| sample | 5s 먼저 |
| full | sample PASS 후 10s |
| storage | R2 only |
| prefix | `ads/video-creatives/` |
| manifest | 필수 |
| return | R2 video URL, R2 manifest URL, Replicate output URL |

## Prompt 방향

```text
Authentic vertical smartphone living-still video, viewer POV.
Preserve the first frame identity, face, hairstyle, outfit, background, lighting.
Small motion only: eyes slowly scan toward the viewer, subtle breathing,
tiny head movement, slight expression change.
No dialogue. No subtitles. No text. No watermark.
No big body movement. No camera spin. No face morph. No hand distortion.
The clip should feel like one seductive Toptoonchat prologue cut,
not a full story scene.
```

## PASS 기준

| Gate | 기준 |
|---|---|
| identity | first image 얼굴/헤어/의상 유지 |
| motion | living-still 수준, 작고 자연스러움 |
| face | morph 없음 |
| hands | 손/손가락 깨짐 없음 |
| framing | vertical mobile, viewer-POV 유지 |
| story | 한 컷이 한 매력만 전달 |
| R2 | video + manifest 둘 다 200 OK |

## 중단 조건

| 조건 | 행동 |
|---|---|
| first image 미승인 | 생성 금지 |
| R2 upload 실패 | 재시도 후 실패 기록 |
| identity drift | full 생성 금지 |
| 큰 동작/서사 과잉 | prompt 축소 후 sample 재생성 |
| 손/얼굴 붕괴 | FAIL 기록 |

## 완료 보고

| 항목 | 값 |
|---|---|
| sample URL |  |
| full URL |  |
| manifest URL |  |
| Replicate URL |  |
| PASS/FAIL |  |
| 다음 컷 필요 여부 |  |

