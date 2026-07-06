# Handoff — 2026-06-14

## Current State
- Branch: `main`
- Worktree: `/Users/cheonmyeongseung/cereal-ai`
- Server: none
- Last commit: `3c99e30 fix: resolve circular dependency between raw-error-ledger and turso by separating db client`
- Scope split: image pipeline done. Next session starts from prologue video.

## Todo
- [x] 확정 이미지 prompt DB 구축
  - [x] Obsidian 통과 prompt table 생성
  - [x] `Prompt string` 컬럼 추가
  - [x] 확정 노하우 기록
- [x] 온보딩 first image prompt skill/script 생성
  - [x] Gemini 3 Flash 사용
  - [x] Obsidian DB only 기본
  - [x] Gemini는 body prompt only
  - [x] fixed prefix는 코드에서 prepend
- [x] 야쿠자 보스 아내 first image 생성/R2 업로드
- [x] p-video 단일 컷 테스트
- [ ] Toptoonchat식 프롤로그 동영상 플로우 확정
  - [ ] 녹화 파일을 기준으로 프롤로그 컷 구조 정리
  - [ ] 보스 아내 컨셉의 3~5개 컷 목적 정의
  - [ ] 각 컷별 first frame 필요 여부 결정
  - [ ] 각 컷별 p-video action prompt를 Gemini에 물어보기
  - [ ] 1컷 생성 → founder gate
  - [ ] 통과 컷만 Obsidian/manifest에 기록
- [ ] API화 설계
  - [ ] prompt 생성 API
  - [ ] first image generation API
  - [ ] video generation API
  - [ ] 각 단계 founder/user gate 분리

## What Works
- Image prompt generation:
  - SOT: `/Users/cheonmyeongseung/Documents/Obsidian_Vault/CEREAL_Cowork/docs/06-14 image prompt pattern table.md`
  - Script: `/Users/cheonmyeongseung/cereal-ai/.agents/skills/onboarding-first-image-prompt/scripts/generate-prompt.mjs`
  - Flow: Obsidian accepted raw prompt table → Gemini 3 Flash body only → fixed prefix prepend.
- Passed first image:
  - `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-onboarding/characters/yakuza-boss-wife-spy-romance/20260613232858/first-frame.jpg`
  - manifest: `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-onboarding/characters/yakuza-boss-wife-spy-romance/20260613232858/manifest.json`
- p-video works:
  - model: `prunaai/p-video`
  - 5s draft execution can be ~5.7s to ~22s
  - actual output duration verified by `ffprobe`
- Best p-video so far:
  - `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-onboarding/videos/yakuza-boss-wife-spy-romance-p-video/20260614003347/video-5s-eye-scan-living-still.mp4`
  - action: eye-scan + subtle lean-in, no hand reaching

## What Doesn't Work
- Do not ask the agent to invent video action.
  - It produced unrelated `hand reaches toward viewer`.
  - Gemini judged this wrong for the boss-wife concept.
- Do not put full story arc into 5 seconds.
  - p-video should animate one prologue beat, not explain the full relationship.
- Do not show fixed image prefix to Gemini.
  - It dilutes Obsidian reference charm.
  - Current confirmed flow: Gemini body only, prefix added after.

## Pending Builds

### Toptoonchat-Style Prologue Video Flow
- Purpose: convert one approved character profile image into a prologue sequence.
- Reference: `/Users/cheonmyeongseung/Desktop/app_reference_capture/toptoonchat/ScreenRecording_06-14-2026 09-05-15_1.MP4`
- Observed structure:
  - profile card → start chat
  - place cut → object/body detail cut → face cut → user choice
  - reward/paywall after engagement
- Required output:
  - 3~5 short living-still p-video clips
  - each clip has one purpose only
  - text/choice carries story, not video motion
- For boss-wife test:
  - Cut 1: profile/first encounter
  - Cut 2: status/power detail
  - Cut 3: face/gaze suspicion
  - Cut 4: choice setup

## Next Step
1. Read this handoff and `docs/06-14 image prompt pattern table.md`.
2. Rewatch/reference Toptoonchat recording; do not invent a new format.
3. Ask Gemini: for boss-wife concept, define 3~5 Toptoon-style prologue cuts and each cut's single charm/action.
4. Generate only cut 1 first frame/video with p-video.
5. Return R2 video, manifest, durationMs, actual duration.

## Key Files
| File | Why |
|---|---|
| `/Users/cheonmyeongseung/Documents/Obsidian_Vault/CEREAL_Cowork/docs/06-14 image prompt pattern table.md` | image prompt DB + confirmed generation process |
| `/Users/cheonmyeongseung/cereal-ai/.agents/skills/onboarding-first-image-prompt/SKILL.md` | first image prompt process |
| `/Users/cheonmyeongseung/cereal-ai/.agents/skills/onboarding-first-image-prompt/scripts/generate-prompt.mjs` | reproducible prompt script |
| `/Users/cheonmyeongseung/Documents/Obsidian_Vault/CEREAL_Cowork/spec/06-12 video onboarding.md` | onboarding video product spec |
| `/Users/cheonmyeongseung/Desktop/app_reference_capture/toptoonchat/ScreenRecording_06-14-2026 09-05-15_1.MP4` | prologue reference |

## Key Learnings
- Users prefer first-person POV and clear face/eyes.
- First image can be both profile image and first video frame, but prologue cuts may need separate first frames.
- Toptoonchat does not rely on one cinematic video. It uses short living-still cuts plus text and choices.
- For character charm, ask Gemini. Do not let the agent invent action.
- For boss-wife charm, Gemini's good direction:
  - "Untouchable Sovereign"
  - charm = distance, gaze, status, suspicion, forbidden curiosity
  - action = slow eye-scan + slight lean-in
  - not hand reaching

## DO NOT
- Do not modify scripts/skills unless user explicitly says flow is confirmed.
- Do not generate image/video before gate when user asks for planning.
- Do not include `P015+` recent wins unless user asks; default prompt script uses Obsidian only.
- Do not show fixed prefix to Gemini.
- Do not make p-video tell a full story.
- Do not assume hand/approach actions are attractive; validate with Gemini against character charm.
