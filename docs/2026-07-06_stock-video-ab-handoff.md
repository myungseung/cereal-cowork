# Handoff — 2026-07-06 stock video A/B

## Current State
- Branch: `main`
- Worktree: `/Users/cheonmyeongseung/cereal-ai`
- Dirty state: `?? pnpm-workspace.yaml` existing unrelated
- Lab history: `/Users/cheonmyeongseung/.codex/skills/video-chat-lab/tmp/video-chat-lab/runs.jsonl`
- Table doc: `/Users/cheonmyeongseung/Documents/Obsidian_Vault/CEREAL_Cowork/Research/07-04 동영상 비디오 idle 프롬프팅.md`
- Idle reference URL: `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/manual/idle_next_reference_a_first_f2857afde2.png`

## Todo
- [x] `idle` accepted
- [x] `typing` accepted
- [x] `reaction_flattered_smile` accepted
- [ ] `reaction_comfort_soft` user review
  - [ ] Review Kling final below
  - [ ] If pass, update Obsidian table row to PASS
  - [ ] If fail, user provides next A/B prompts or different video 소재
- [ ] Continue remaining video keys
  - [ ] `reaction_laugh`
  - [ ] `reaction_surprised`
  - [ ] `action_wave`
  - [ ] `action_approach_kiss`
  - [ ] `action_eat_together`
  - [ ] `action_lie_bed`
  - [ ] `action_clap`
- [ ] Revisit `reaction_nod_listening` only if user asks; current verdict: 동작 애매함

## What Works
- Final structure: `A action + B return` concat.
- A first frame must always be idle reference URL.
- B first frame must be extracted A last frame PNG URL.
- B last/end image must always be idle reference URL.
- User provides all A prompts and all return prompts. Do not write/rewrite prompts.
- p-video is first attempt. If user says `fail`, regenerate failed item with Kling pro backfill.
- For Kling backfill, use raw Replicate payload, not the existing `run-kling-omni-video.mjs` wrapper, because that wrapper wraps prompts.

## Accepted Finals
| key | final |
|---|---|
| `idle` | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/2026-07-04T05-54-32-169Z-idle_ab_manual_simple_concat-8fa12ff1/idle_ab_manual_simple_concat.mp4 |
| `typing` | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/2026-07-04T06-45-53-045Z-typing_manual_a_user_b_final_v2-e27efccb/typing_manual_a_user_b_final_v2.mp4 |
| `reaction_flattered_smile` | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/2026-07-04T06-23-34-835Z-reaction_flattered_smile_manual_a_user_b_final-5ff96371/reaction_flattered_smile_manual_a_user_b_final.mp4 |

## Pending Review
| key | status | final | notes |
|---|---|---|---|
| `reaction_comfort_soft` | Kling backfill generated, not user-reviewed | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/2026-07-04T08-43-30-854Z-reaction_comfort_soft_kling_final-6dddcb3f/reaction_comfort_soft_kling_final.mp4 | p-video failed. Kling A/B generated and final concat succeeded. |

## Failed / Parked
| key | final | verdict |
|---|---|---|
| `reaction_nod_listening` p-video | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/2026-07-04T06-51-30-548Z-reaction_nod_listening_final-0fc2b5e7/reaction_nod_listening_final.mp4 | fail |
| `reaction_nod_listening` Kling | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/2026-07-04T07-00-47-722Z-reaction_nod_listening_kling_final-3a0f9af7/reaction_nod_listening_kling_final.mp4 | 동작 자체가 애매함; park |

## Current Prompts
- `reaction_comfort_soft` A prompt: `/Users/cheonmyeongseung/cereal-ai/tmp/video-assets/reaction_comfort_soft-action.prompt.txt`
- `reaction_comfort_soft` return prompt: `/Users/cheonmyeongseung/cereal-ai/tmp/video-assets/reaction_comfort_soft-return.prompt.txt`
- `reaction_nod_listening` A prompt: `/Users/cheonmyeongseung/cereal-ai/tmp/video-assets/reaction_nod_listening-action.prompt.txt`
- `reaction_nod_listening` return prompt: `/Users/cheonmyeongseung/cereal-ai/tmp/video-assets/reaction_nod_listening-return.prompt.txt`
- Legacy fixed p-video B prompt file, now not universal if user provides return prompt: `/Users/cheonmyeongseung/cereal-ai/tmp/video-assets/return-to-idle-3s.prompt.txt`

## Generation Contracts
### p-video A
```json
{
  "model": "prunaai/p-video",
  "input": {
    "image": "idle reference URL",
    "prompt": "user provided A prompt",
    "duration": 4,
    "resolution": "1080p",
    "fps": 24,
    "draft": false,
    "prompt_upsampling": false,
    "disable_safety_filter": true
  }
}
```

### p-video B
```json
{
  "model": "prunaai/p-video",
  "input": {
    "image": "A last frame PNG URL",
    "last_frame_image": "idle reference URL",
    "prompt": "user provided return prompt or fixed B prompt",
    "duration": 2,
    "resolution": "1080p",
    "fps": 24,
    "draft": false,
    "prompt_upsampling": false,
    "disable_safety_filter": true
  }
}
```

### Kling A/B Backfill
Use raw Replicate model `kwaivgi/kling-v3-omni-video`.
- A input: `start_image=idle reference`, `duration=4`, `mode=pro`, `aspect_ratio=9:16`, `generate_audio=false`, `keep_original_sound=false`, `video_reference_type=feature`.
- B input: `start_image=A last PNG`, `end_image=idle reference`, `duration=3`, same Kling flags.

## Next Step
1. Show/review `reaction_comfort_soft` Kling final.
2. If user says pass, update Obsidian table row to PASS with embedded video.
3. If user says fail, ask for next video 소재 or prompt; do not keep iterating comfort blindly.
4. Continue with `reaction_laugh` or `reaction_surprised` after user picks.

## Key Files
| File | Why |
|---|---|
| `/Users/cheonmyeongseung/Documents/Obsidian_Vault/CEREAL_Cowork/Research/07-04 동영상 비디오 idle 프롬프팅.md` | Single table SOT for keys/status/video embeds |
| `/Users/cheonmyeongseung/cereal-ai/scripts/generate-pvideo-asset.mjs` | p-video A/B generation + R2 + lab history |
| `/Users/cheonmyeongseung/cereal-ai/scripts/merge-video-chat-lab-assets.mjs` | A+B concat + R2 + lab history |
| `/Users/cheonmyeongseung/.codex/skills/video-chat-lab/tmp/video-chat-lab/runs.jsonl` | Complete run history |
| `/Users/cheonmyeongseung/cereal-ai/tmp/video-assets/` | Prompt files and extracted frame working files |

## DO NOT
- Do not rewrite user prompts.
- Do not use old hair-holding/old idle first frame.
- Do not use `run-kling-omni-video.mjs` for these raw prompt backfills unless patched to disable prompt wrapping.
- Do not mark a video PASS without explicit user pass.
- Do not update product/app registration until the Obsidian table row is PASS.
