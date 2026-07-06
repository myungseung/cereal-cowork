상태: stock video chat 다음 작업용 스펙

| 항목 | 값 |
|---|---|
| 기준일 | 2026-07-02 |
| 작업 브랜치 | `codex/stock-video-chat-idle` |
| worktree | `/Users/cheonmyeongseung/cereal-ai-worktrees/stock-video-chat-idle` |
| preview | `http://127.0.0.1:3040/tmp-video-chat-ui-preview.html?decoupled=manual&chat=1` |
| 최근 커밋 | `fcc12c4 Decouple stock video queue from chat replies` |
| 현재 원칙 | chat 응답 속도와 video queue 재생 속도 분리 |

## 현재 확정된 구조

- background video는 독립 FIFO queue.
- chat API 응답은 video 종료를 기다리지 않음.
- 대화 이벤트는 `video_key`를 queue에 추가만 함.
- queue는 idle loop boundary에서 action video 시작.
- action video 종료 즉시 idle로 복귀.
- standby video는 항상 first frame에서 paused.

## 남은 영상 목록

| 우선순위 | video_key | 용도 | 트리거 예시 | 상태 |
|---:|---|---|---|---|
| 0 | `idle` | 기본 배경 | 항상 | 완료 |
| 0 | `typing` | agent가 응답 생성 중 / 로딩 중 | message submit / loading | 완료 |
| 1 | `reaction_flattered_smile` | 칭찬/호감 리액션 | 예뻐, 귀여워, 좋아, 보고싶어, 사랑, ㅋㅋ | 미생성 |
| 2 | `reaction_nod_listening` | 듣고 있음 / 공감 | 그렇구나, 힘들었어, 오늘 뭐했어 | 미생성 |
| 3 | `reaction_laugh` | 웃음 | 웃겨, 장난, ㅋㅋㅋ | 미생성 |
| 4 | `reaction_comfort_soft` | 위로 / 걱정 | 힘들어, 우울해, 보고싶어 | 미생성 |
| 5 | `reaction_surprised` | 놀람 / 반응 | 진짜?, 대박, 갑자기 | 미생성 |
| 6 | `action_wave` | 손 흔들기 | 손 흔들어줘, 인사해줘 | 과거 실험 있음 / stock 미완성 |
| 7 | `action_approach_kiss` | 가까이 오기 / 키스 | 뽀뽀, 키스, 가까이 와 | 과거 실험 있음 / stock 미완성 |
| 8 | `action_eat_together` | 같이 먹기 | 밥 먹자, 뭐 먹어 | 미생성 |
| 9 | `action_lie_bed` | 같이 눕기 | 같이 눕자, 누워봐 | 과거 실험 있음 / stock 미완성 |
| 10 | `action_clap` | 기쁨 / 축하 | 잘했지, 축하해줘 | 미생성 |

## 영상 생성 규칙

- 모든 action video는 idle first frame에서 시작해야 함.
- 모든 action video는 `action + return_to_idle`로 합쳐서 최종본을 만들어야 함.
- 최종본의 마지막 프레임은 idle first frame과 최대한 같아야 함.
- 앱에는 `action + return_to_idle` 최종 mp4만 등록.
- 원본 action, return, first frame, action last frame은 R2 URL로 기록.
- 새 영상은 `runs.jsonl` 기록 + Obsidian 표 업데이트.

## idle 복귀 보장 파이프라인

목표: 사람이 눈으로 확인하기 전에 파일 구조상 `first -> action -> return -> first`를 강제.

| 단계 | 입력 | 출력 | 검증 |
|---|---|---|---|
| 1 | `idle_first_frame.jpg` | action 5s mp4 | first frame 기준 시작 |
| 2 | action 5s mp4 | `action_last_frame.png` | ffmpeg extract |
| 3 | `action_last_frame.png` + `idle_first_frame.jpg` | return 3s mp4 | last -> first 복귀 |
| 4 | action mp4 + return mp4 | final 8s mp4 | concat |
| 5 | final 8s mp4 | `final_last_frame.png` | first frame과 diff 측정 |
| 6 | final mp4 + manifest | R2 permanent URL | preview 등록 |

권장 스크립트:

```bash
node scripts/build-stock-action-loop.mjs \
  --video-key reaction_flattered_smile \
  --first-frame https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/profile/preview_1782982135862/1782982140069.jpg \
  --action-prompt-file prompts/reaction_flattered_smile.txt \
  --duration-action 5 \
  --duration-return 3 \
  --mode pro
```

스크립트가 보장해야 할 것:

- `action.mp4` 생성.
- `action_last_frame.png` 추출.
- `return.mp4` 생성: start=`action_last_frame.png`, end=`idle_first_frame`.
- `final.mp4` 생성: action + return concat.
- `final_last_frame.png` 추출.
- first/final_last diff 저장.
- R2 업로드.
- `manifest.json` 저장.
- `video-chat-lab/tmp/video-chat-lab/runs.jsonl`에 record 추가.
- 실패 시 final URL 미등록.

최소 manifest:

```json
{
  "videoKey": "reaction_flattered_smile",
  "status": "ready",
  "firstFrameUrl": "...",
  "actionVideoUrl": "...",
  "returnVideoUrl": "...",
  "finalVideoUrl": "...",
  "actionLastFrameUrl": "...",
  "finalLastFrameUrl": "...",
  "durationSec": 8,
  "provider": "kling-v3-omni-video",
  "mode": "pro",
  "createdAt": "2026-07-02T00:00:00.000Z"
}
```

## chat output param

목표: LLM/chat API가 reply와 별도로 queue 이벤트만 알려줌. chat은 video 종료를 기다리지 않음.

응답 shape:

```json
{
  "reply": "응답 텍스트",
  "video_event": {
    "key": "reaction_flattered_smile",
    "confidence": 0.74,
    "source": "assistant_reply",
    "reason": "칭찬/호감 반응"
  }
}
```

규칙:

- `video_event.key`는 optional.
- 없으면 video queue 추가 없음.
- `typing`은 API 응답 전 frontend local event로 넣어도 됨.
- `assistant_reply` 기반 event는 API 응답 도착 시 queue에 추가.
- `confidence < 0.55`면 queue에 넣지 않음.
- 여러 후보가 있어도 1개만 반환.
- chat text는 즉시 표시. video queue는 독립 실행.

## frontend 감지/queue 연결

현재 파일:

| 파일 | 역할 |
|---|---|
| `/Users/cheonmyeongseung/cereal-ai-worktrees/stock-video-chat-idle/public/tmp-video-chat-ui-preview.html` | 실제 preview |
| `/Users/cheonmyeongseung/cereal-ai-worktrees/stock-video-chat-idle/public/tmp-video-chat-lab-demo.html` | lab demo |

현재 구현:

- `queueStockVideo(name)`이 FIFO queue에 이벤트 추가.
- `stockVideoQueue` / `stockQueueRunning`으로 video 독립 실행.
- `chatBusy`는 chat 입력 잠금 전용.
- `state`는 video 상태 전용.

다음 구현:

```js
const data = await dataPromise;
if (data.video_event?.key) queueStockVideo(data.video_event.key);
```

등록 테이블:

```js
const STOCK_VIDEO_URLS = {
  typing: "https://.../video-8s-typing-plus-return-pro-hq-76a71ada94.mp4",
  reaction_flattered_smile: "https://...",
  reaction_nod_listening: "https://..."
};
```

주의:

- `queueStockVideo()`는 `await`하지 않음.
- chat reply display는 기존 속도 유지.
- video event가 밀려도 허용.
- queue 중 새 event가 들어오면 FIFO로 뒤에 붙임.
- action video가 없으면 무시하거나 `idle` 유지.

## 기존 생성 비디오 / 프롬프트 표

| 상태 | video_key 후보 | runId | 생성 의도 | prompt / 기록 | 최종 URL |
|---|---|---|---|---|---|
| 폐기 | `idle_test_same_end` | `stock-first-love-kling-idle-same-end-20260702081612` | same start/end idle test | prompt 원문 미기록. 기록: `stock first-love bedroom idle, same start/end image`; 결과 너무 fixed | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/first-love-bedroom-idle-fullshot/20260702081612/video-5s-kling-omni-idle.mp4 |
| 폐기 | `idle_visible_same_end` | `stock-first-love-kling-visible-idle-20260702081942` | visible idle motion, same end | prompt 원문 미기록. 기록: `standing idle with weight shift, blink, hair/curtain movement` | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/first-love-bedroom-visible-idle-motion/20260702081942/video-5s-kling-omni-visible-idle-motion.mp4 |
| 참고 | `idle_no_end` | `stock-first-love-kling-visible-idle-no-end-20260702082236` | no end image idle | prompt 원문 미기록. 기록: `standing idle without last-frame constraint` | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/first-love-bedroom-visible-idle-motion-no-end/20260702082236/video-5s-kling-omni-visible-idle-motion-no-end.mp4 |
| source | `idle` | `kling-stock-idle-silent-v2-20260702085005` | idle source 5s | `Preserve the same identity, face, hairstyle, outfit, room, lighting, color balance, and candid iPhone texture from the start image. Create a short realistic vertical smartphone video for: idle. The motion should be visible but restrained, natural, and loop-friendly. Keep her as the same real person in the same bedroom. No dialogue, no subtitles, no text, no watermark. Avoid face morphing, body morphing, hand distortion, extra fingers, outfit changes, background changes, sitting unless the clip requires it, crouching, exaggerated gestures, or camera zoom.` | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/first-love-bedroom-idle-stock-silent-v2/20260702085005/video-5s-kling-omni-idle-silent.mp4 |
| 폐기 | `idle_loop_2x` | `kling-stock-idle-silent-v2-concat-2x-20260702085005` | idle 2x seam test | ffmpeg concat copy. prompt 없음 | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/first-love-bedroom-idle-stock-silent-v2/20260702085005/video-10s-idle-loop-2x-3613f8c31f.mp4 |
| 참고 | `idle_return_v2` | `kling-stock-idle-plus-return-v2-20260702085754` | idle + return loop | return prompt 원문 미기록. 구조: idle 5s + return lastFrame -> firstFrame 3s | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/first-love-bedroom-idle-stock-silent-v2/20260702085005/video-8s-idle-plus-return-9cab4373dd.mp4 |
| 완료 | `idle` | `kling-stock-idle-pro-v3-loop-20260702091747` | FHD target idle + return | prompt 원문 미기록. 기록: `Kling pro idle regenerated, pro return, CRF18 concat`; 1188x1740, 8.08s, 5.2Mbps | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/first-love-bedroom-idle-stock-silent-v3-pro/20260702091317/video-8s-idle-plus-return-pro-hq-612acb8a25.mp4 |
| 폐기 | `typing_v1` | `kling-stock-typing-pro-v1-20260702092908` | typing source 5s | `Preserve the same identity, face, hairstyle, outfit, room, lighting, color balance, and candid iPhone texture from the start image. Create a short realistic vertical smartphone video for: typing / texting. She gently looks down at her phone, types with her thumbs with a small shy smile, then briefly returns her gaze toward the viewer. Motion should be visible but restrained, natural, and loop-friendly. Keep her as the same real person in the same bedroom. No dialogue, no subtitles, no text, no watermark. Avoid face morphing, body morphing, hand distortion, extra fingers, outfit changes, background changes, crouching, exaggerated gestures, or camera zoom.` | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/first-love-bedroom-typing-stock-silent-v1-pro/20260702092908/video-5s-kling-omni-typing-silent-pro.mp4 |
| source | `typing` | `kling-stock-typing-immediate-pro-v2-20260702094559` | typing immediate source 5s | `Preserve the same identity, face, hairstyle, outfit, room, lighting, color balance, and candid iPhone texture from the start image. Create a short realistic vertical smartphone video for: typing / texting. From the very first moment, she already has a phone in her hands at chest level and immediately starts typing with both thumbs. Do not spend time reaching for the phone or preparing. She looks down at the phone right away, types with a small shy smile, then briefly glances back toward the viewer while still holding the phone. Motion should be visible but restrained, natural, and loop-friendly. Keep her as the same real person in the same bedroom. No dialogue, no subtitles, no generated text, no watermark. Avoid face morphing, body morphing, hand distortion, extra fingers, outfit changes, background changes, crouching, exaggerated gestures, or camera zoom.` | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/first-love-bedroom-typing-stock-silent-v2-immediate-pro/20260702094559/video-5s-kling-omni-typing-immediate-silent-pro.mp4 |
| 완료 | `typing` | `kling-stock-typing-immediate-loop-v2-20260702095537` | typing + return final | 구조: typing immediate 5s + return-to-idle 3s; 1188x1740, 8.08s, 4.9Mbps | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/first-love-bedroom-typing-stock-silent-v2-immediate-pro/20260702094559/video-8s-typing-plus-return-pro-hq-76a71ada94.mp4 |

## 다음 작업 순서

- [ ] `reaction_flattered_smile` 생성
  - [ ] idle first frame 사용
  - [ ] action 5s 생성
  - [ ] action last frame 추출
  - [ ] return 3s 생성
  - [ ] final 8s concat
  - [ ] R2 final + manifest 기록
- [ ] `STOCK_VIDEO_URLS`에 `reaction_flattered_smile` 등록
- [ ] chat API output에 `video_event.key` 추가
- [ ] frontend에서 `data.video_event?.key` 감지 후 `queueStockVideo(key)`
- [ ] E2E
  - [ ] reply가 video 종료를 기다리지 않음
  - [ ] video는 queue대로 뒤늦게 재생 가능
  - [ ] action 종료 후 idle 복귀

## 금지

- video queue를 chat reply 속도와 동기화하지 않음.
- `await queueStockVideo(...)` 금지.
- action video를 return 없이 앱에 등록하지 않음.
- R2 업로드 없는 provider URL만 기록하지 않음.
- prompt 원문 없는 과거 run은 원문처럼 재구성하지 않음. `prompt 원문 미기록`으로 표기.
