상태: idle loop / action loop 제작 요령 handoff

| 항목 | 값 |
|---|---|
| 기준일 | 2026-07-04 |
| 목적 | action video가 끝난 뒤 idle first frame으로 자연 복귀 |
| 핵심 | 마지막 프레임은 PNG로 추출, return video 생성, 최종 concat은 1회만 고품질 인코딩 |

## 결론

- 마지막 프레임은 JPG 금지. `png` 사용.
- return video 입력은 `action_last_frame.png -> idle_first_frame.jpg`.
- 최종 앱 등록 파일은 `action + return_to_idle` mp4.
- 압축은 마지막 concat 때 1번만.
- re-encode 필요 시 `CRF 16~18`, `preset slow`, 원본 해상도 유지.

## last frame 추출

정확한 마지막 decoded frame:

```bash
frames=$(ffprobe -v error -select_streams v:0 -count_frames \
  -show_entries stream=nb_read_frames -of csv=p=0 action.mp4)

last=$((frames - 1))

ffmpeg -v error -i action.mp4 \
  -vf "select=eq(n\\,$last)" \
  -vsync vfr -frames:v 1 action_last_frame.png
```

빠른 확인용:

```bash
ffmpeg -sseof -0.08 -i action.mp4 -frames:v 1 action_last_frame_fast.png
```

주의:

- `fast` 방식은 마지막 근처 프레임. 최종 return 입력에는 정확 추출 방식 우선.
- `action_last_frame.png`는 R2에 같이 저장.
- return 생성 후에도 `final_last_frame.png`를 다시 뽑아 first frame과 비교.

## return-to-idle 생성

입력:

| 입력 | 값 |
|---|---|
| start image | `action_last_frame.png` |
| end image | `idle_first_frame.jpg` |
| duration | 3s |
| audio | false |
| mode | pro 권장 |

prompt 골격:

```text
Preserve the same identity, face, hairstyle, outfit, room, lighting, color balance, and candid iPhone texture. Create a short silent return-to-idle transition from the start image back to the idle first frame. The motion should be subtle and physically natural. End as close as possible to the provided idle first frame. No dialogue, no subtitles, no text, no watermark. Avoid face morphing, body morphing, hand distortion, outfit changes, background changes, camera zoom, or exaggerated gestures.
```

## concat / 압축 품질

가능하면 stream copy:

```bash
printf "file '%s'\nfile '%s'\n" action.mp4 return.mp4 > concat.txt
ffmpeg -f concat -safe 0 -i concat.txt -c copy final.mp4
```

copy가 깨지면 1회만 re-encode:

```bash
ffmpeg -i action.mp4 -i return.mp4 \
  -filter_complex "[0:v][1:v]concat=n=2:v=1:a=0[v]" \
  -map "[v]" \
  -c:v libx264 -crf 16 -preset slow -pix_fmt yuv420p \
  -movflags +faststart -an final_hq.mp4
```

품질 기준:

| 항목 | 권장 |
|---|---|
| CRF | 16~18 |
| preset | `slow` |
| pix_fmt | `yuv420p` |
| audio | `-an` |
| scaling | 하지 않음 |
| 중간 파일 | 재압축 금지 |

금지:

- `-crf 23` 이상으로 최종본 만들기.
- JPG last frame을 return start로 쓰기.
- action -> concat -> return처럼 이미 압축된 concat에서 last frame 뽑기.
- 같은 파일을 여러 번 재인코딩.
- 해상도 임의 변경.

## 검증

```bash
ffprobe -v error -select_streams v:0 \
  -show_entries stream=width,height,avg_frame_rate,duration,bit_rate \
  -of default=nw=1 final_hq.mp4

ffmpeg -sseof -0.08 -i final_hq.mp4 -frames:v 1 final_last_frame.png
```

통과 기준:

- final duration = action + return.
- final 해상도 = source action 해상도.
- final last frame이 idle first frame과 시각적으로 가까움.
- loop 재생 시 `first -> action -> return -> first`에서 튐이 작음.
- 앱 등록 URL은 final mp4만 사용.

## R2 기록 필수

| 필드 | 저장 |
|---|---|
| `firstFrameUrl` | idle first frame |
| `actionVideoUrl` | action 원본 |
| `actionLastFrameUrl` | PNG |
| `returnVideoUrl` | return 원본 |
| `finalVideoUrl` | 앱 등록 mp4 |
| `finalLastFrameUrl` | 검증용 PNG |
| `manifestUrl` | 전체 기록 |

## 다음 agent 시작점

- [ ] `reaction_flattered_smile` action 생성
- [ ] 위 명령으로 `action_last_frame.png` 추출
- [ ] return-to-idle 생성
- [ ] `final_hq.mp4` 생성
- [ ] R2 업로드 + manifest 기록
- [ ] preview에서 loop 확인
