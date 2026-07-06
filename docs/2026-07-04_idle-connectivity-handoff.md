# Handoff — 2026-07-04 idle connectivity

## Current State
- Branch: `codex/postres-update`
- Worktree: `/Users/cheonmyeongseung/cereal-ai`
- Lab viewer: `http://127.0.0.1:3024/`
- Last commit: `fa35c6b Guard ad funnel ROI on Meta token failure`
- Goal: solve idle A+B loop connectivity before any other video assets.
- Overall session goal: generate 26 video assets, but execution is blocked on idle connectivity.

## Full Asset Scope (26)
- Idle/core:
  - `idle`
- Obsidian stock-video queue keys:
  - `reaction_flattered_smile`
  - `reaction_nod_listening`
  - `reaction_laugh`
  - `reaction_comfort_soft`
  - `reaction_surprised`
  - `action_wave`
  - `action_approach_kiss`
  - `action_eat_together`
  - `action_lie_bed`
  - `action_clap`
- Basic emotion set:
  - `happy`
  - `shy`
  - `curious`
  - `thinking`
  - `sad`
  - `jealous`
  - `comfort`
  - `affection`
- Special situation set:
  - `first_meeting`
  - `welcome_back`
  - `missed_you`
  - `good_night`
  - `morning`
  - `confession`
  - `comfort_deep`
  - `relationship_level_up`

## 26-Asset Execution Contract
- Generate one asset at a time.
- No retries unless user explicitly asks.
- For each asset:
  - Generate A/action part first.
  - Show in lab viewer.
  - Wait for explicit `pass`.
  - Extract A first/last PNG if needed.
  - Generate B/return only after pass.
  - Concat only after B generation.
  - Record final only after user accepts.
- All outputs go through video-chat-lab history.
- R2 URLs are primary; provider URLs are evidence only.
- No prompt rewriting by agent.
- Current blocker: idle loop must be solved before starting remaining 25.

## Todo
- [x] Upload new standing first frame to R2.
- [x] Generate idle A from new standing first frame.
- [x] Extract A first/last PNG and use those as B constraints.
- [x] Test B 1s/2s, prompt variants, frame deletion, xfade, external B.
- [ ] Find acceptable B/concat loop.
  - [ ] Continue from A v012, not older hair-holding assets.
  - [ ] Try B variants using A first/last PNG only.
  - [ ] Compare final loop in lab viewer with loop playback.
  - [ ] Only mark ready after user explicitly says pass.
- [ ] Write product manifest only after pass.
- [ ] Resume remaining 25 assets after idle pass.
  - [ ] Use the accepted idle method as template.
  - [ ] Keep per-asset A/B/final records.
  - [ ] Do not batch-generate until user approves the idle template.

## What Works
- Lab viewer defaults to today-only filter and clicked videos loop.
- R2 upload/history contract works.
- Copy concat works when A/B codecs match.
- A v012 is current base A:
  - `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/2026-07-04T05-28-16-306Z-idle_core_2s_v012_standing_first_prompt_1080p-882554e0/idle_core_2s_v012_standing_first_prompt_1080p.mp4`
- A v012 extracted frames:
  - first: `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/manual/idle_v012_a_first_f2857afde2.png`
  - last: `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/manual/idle_v012_a_last_f1155a1683.png`

## What Doesn't Work
- No final idle loop is accepted yet.
- p-video `last_frame_image` is not a hard pixel lock.
- B last frame can still distort even when `last_frame_image` is set correctly.
- Prepared still image as B target caused resolution/color mismatch; using A first/last PNG reduced that class of mismatch.
- Frame deletion and xfade did not solve the perceived seam.
- External B upload/concat was tested but not accepted.

## Current Best Inputs
- First frame source now in use:
  - `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/manual/idle_first_frame_standing_source_f8be7b0514.jpeg`
- A source:
  - `idle_core_2s_v012_standing_first_prompt_1080p`
- B should use:
  - `image = A last PNG`
  - `last_frame_image = A first PNG`

## Prompts
Use only user-provided prompt strings. Do not rewrite.

### A prompt
```text
Same woman, same room, same composition.
Keep the original pose and same distance from camera.
Soft forward gaze, gentle shy smile.
Subtle breathing and blinking only.
Locked camera, no pose change, no moving closer.
Seamless loop, final frame matches first frame.
```

### B settle prompt
```text
Same woman, same room, same composition.
Begin from the current subtle idle variation.
Her breathing, eyes, smile, hair, and fingers gently settle back to the original neutral resting state.
Keep the same pose, body position, face direction, and camera distance.
No new gesture, no head turn, no leaning, no zoom, no camera movement.
End in the neutral resting pose.
```

### B short prompt tested
```text
start from first frame end with last frame

Her breathing, eyes, smile, hair, and fingers gently settle back to the original neutral resting state.
```

## Recent Outputs
- v013 A first/last based B 1s final:
  - `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/2026-07-04T05-38-28-731Z-idle_loop_3s_v013_a_frames_return_copy_concat-9649798b/idle_loop_3s_v013_a_frames_return_copy_concat.mp4`
- v014 A first/last based B 2s final:
  - `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/2026-07-04T05-41-34-034Z-idle_loop_4s_v014_a_frames_return2s_copy_concat-d0a99532/idle_loop_4s_v014_a_frames_return2s_copy_concat.mp4`
- v015 short B prompt final:
  - `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/2026-07-04T05-44-30-214Z-idle_loop_4s_v015_start_first_end_last_prompt_copy_concat-6e9b0467/idle_loop_4s_v015_start_first_end_last_prompt_copy_concat.mp4`
- v016 settle prompt retry final:
  - `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/2026-07-04T05-48-04-905Z-idle_loop_4s_v016_settle_prompt_retry_copy_concat-6b7ec4be/idle_loop_4s_v016_settle_prompt_retry_copy_concat.mp4`
- External B concat final:
  - `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/2026-07-04T05-50-06-516Z-idle_loop_external_b_copy_concat-69063985/idle_loop_external_b_copy_concat.mp4`

## Next Step
1. Ask user which recent final looked least bad, if not already clear.
2. If continuing generation, keep A v012 fixed.
3. Generate only B variants from A last PNG to A first PNG.
4. Use copy concat first; avoid re-encode unless explicitly testing xfade.
5. Show final in lab viewer; wait for explicit `pass`.

## Key Files
| File | Why |
|---|---|
| `/Users/cheonmyeongseung/.codex/skills/video-chat-lab/scripts/video-chat-lab.mjs` | Lab viewer/history renderer. Today filter and loop playback live here. |
| `/Users/cheonmyeongseung/cereal-ai/scripts/generate-pvideo-asset.mjs` | Direct p-video generation + R2 + lab history helper. |
| `/Users/cheonmyeongseung/cereal-ai/scripts/merge-video-chat-lab-assets.mjs` | Final import/concat/R2/history helper. |
| `/Users/cheonmyeongseung/cereal-ai/tmp/video-assets/idle-b-settle-user.prompt.txt` | Current B settle prompt. |
| `/Users/cheonmyeongseung/cereal-ai/tmp/video-assets/idle-user-provided-simple.prompt.txt` | Current A prompt. |

## Key Learnings
- Do not call the prepared first frame "upscale" in future discussion; user means it is the updated first frame.
- Video pipeline must not apply additional upscaling.
- Use A-extracted first/last frames for B to avoid resolution/color mismatch.
- `last_frame_image` guides p-video but does not guarantee exact final pixels.
- User has no tolerance for agent-written prompt changes.

## DO NOT
- Do not mark any idle final as ready without explicit user pass.
- Do not use hair-holding old first frames.
- Do not generate smile/reaction assets; current scope is idle only.
- Do not rewrite prompts.
- Do not assume copy concat fixes model seam.
- Do not create product manifest until accepted.
- Do not start the remaining 25 assets until idle is accepted.
