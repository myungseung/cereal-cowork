


## 현재 상태
- 목표: Meta 광고 캐릭터 -> `/create` 대신 캐릭터별 preset video chat 직접 진입.
- 실험: [[Experiments/06-03 preset video chat 결제전환 2배]].
- Spec: [[Experiments/spec/06-03 arin preset video chat]].
- 첫 캐릭터: Arin.
- 첫 선택지: `Undress`.
- 현재 단계: 첫 생성본 확인 후 수정 방향 결정.

## 산출물
| 항목 | 값 |
|---|---|
| 생성본 | `/Users/cheonmyeongseung/Desktop/ads/output/ugc-runs/arin_comfyui_undress_04-20260603223853/choice_undress_01.mp4` |
| Run folder | `/Users/cheonmyeongseung/Desktop/ads/output/ugc-runs/arin_comfyui_undress_04-20260603223853` |
| Manifest | `/Users/cheonmyeongseung/Desktop/ads/output/ugc-runs/arin_comfyui_undress_04-20260603223853/manifest.json` |
| Workflow | `/Users/cheonmyeongseung/Desktop/ads/output/ugc-runs/arin_comfyui_undress_04-20260603223853/workflow_api.json` |
| 입력 이미지 | `/Users/cheonmyeongseung/Desktop/ads/new소재/preset_arin/image_2.png` |
| Prompt ID | `cf34ec0d-1be4-46ea-96f3-be1ad18b49b4` |
| 상태 | Comfy job `success`, 로컬 mp4 저장 완료 |

## 생성본 판정
| 항목 | 판정 |
|---|---|
| 길이 | 5.038초 |
| 해상도 | 858x1072 |
| FPS | 30 |
| Codec | h264 + aac |
| 용량 | 3.23MB |
| 화면 | 침대 / 흰 침구 / 큰 창문 / 자연광 유지 |
| 캐릭터 | 검은 묶은 머리 / 앞머리 / 흰 크롭 탱크톱 / 흰 하의 유지 |
| 동작 | 상의 밑단 잡기 -> 살짝 올리기 -> 미소 |
| 수위 | suggestive, non-explicit |
| 이슈 | 손/팔 관절 약간 AI 느낌. 치명적 아님 |

## Comfy workflow
1. `LoadImage`
   - `image_2.png` 업로드 후 first frame 사용.
2. `Wan2ImageToVideoApi`
   - model: `wan2.7-i2v`
   - duration: `5`
   - resolution: `720P`
   - seed: `340121880`
   - prompt_extend: `true`
   - watermark: `false`
3. `SaveVideo`
   - filename_prefix: `video/arin_undress_04_wan27`
   - format: `mp4`
   - codec: `h264`

## Prompt 방향
- 같은 Arin 얼굴/헤어/방/조명/의상 유지.
- 침대 위 vertical video chat angle.
- 상의 밑단을 천천히 잡고 살짝 들어 올림.
- 노출 전 멈추고 미소.
- suggestive but non-explicit.

## Negative prompt 방향
- nudity 금지.
- exposed breasts / nipples 금지.
- explicit sexual act 금지.
- pornographic framing 금지.
- identity drift 금지.
- distorted hands / extra fingers 금지.
- text / logo / watermark 금지.

## 사용자 피드백
- 요청 1: `Undress` 수위를 가슴 노출까지 올리고 싶음.
- 처리: 지원 불가. explicit nudity 생성/우회/프롬프트 작성 불가.
- 이유: 안전 정책 + hosted video model 검열 리스크.
- 가능한 대체: 비노출 tease, 컷 직전 멈춤, 어깨/복부/쇄골 강조.

## 오디오 방향
- 대사 없음.
- 실제 생활음 중심.
- 침대 시트 마찰음.
- 옷감 스치는 소리.
- 작은 숨소리.
- 방 안 자연 ambience.
- 잔잔한 BGM.
- vocal 없음.
- BGM low volume.

## 남은 결정
- `Undress` 생성본 사용/수정/폐기 판정.
- 비노출 tease 수위로 재생성할지 결정.
- 나머지 5개 선택지 문구 확정.
- 선택지별 무료/잠금/결제 후 반응 구분.
- Arin preset chat route / 캐릭터 ID SOT 확인.
- 측정 이벤트 SOT 확인.

## 다음 액션
- 유저가 첫 생성본 판정.
- 통과 시 나머지 5개 선택지 설계.
- 수정 시 prompt는 비노출 tease + motion only + 생활음/BGM 방향으로 변경.

## 근거
- `manifest.json` 확인: Comfy Cloud, Wan2.7 I2V, prompt_id, output spec.
- `workflow_api.json` 확인: LoadImage -> Wan2ImageToVideoApi -> SaveVideo.
- ffprobe 확인: 5.038초, 858x1072, 30fps, h264 + aac.
- 프레임 QA 확인: 0.5s / 2.5s / 4.8s.
- 사용자 요청 2026-06-03: 대사 없음, 실제 생활음 + 잔잔한 BGM.
- 사용자 요청 2026-06-03: handoff md docs 업데이트.
