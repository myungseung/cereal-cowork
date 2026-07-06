# Stock Video Chat Handoff — 2026-07-02

## Current State

- Goal: replace static profile image with reusable stock motion clips for video chat feel.
- Current approved profile image:
  - https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/profile/preview_1782981331924/1782981335966.jpg
- Matching prompt JSON:
  - `/Users/cheonmyeongseung/cereal-ai/.agents/runtime/image-profile-pipeline/2026-07-02T08-35-31-879Z-01-wide-full-body-spacious-main.json`
- Character direction:
  - adult 21-year-old East Asian Korean woman
  - first-love / clean summer bedroom / candid iPhone texture
  - full-body, wider angle, enough room around body
  - upright standing pose, no bent waist

## Todo

- [x] Find acceptable stock first frame type
- [x] Create approved wide full-body profile image
- [x] Test Kling Omni idle with same start/end image
- [x] Test Kling Omni idle without end image
- [x] Test p-image-edit for more upright first frame
- [ ] Build stock clip set
  - [ ] idle
  - [ ] typing / texting
  - [ ] approach and kiss
  - [ ] eating together
  - [ ] lying together on bed
  - [ ] clapping
  - [ ] laughing
  - [ ] sad / quiet emotional reaction
- [ ] For each stock clip, save R2 video + R2 manifest
- [ ] Build chat playback state machine
  - [ ] idle loop
  - [ ] user message selects stock clip
  - [ ] play action clip
  - [ ] return clip back to idle first frame
  - [ ] resume idle loop

## Pipeline Idea

1. Generate or approve one stable first frame image.
2. Generate `idle` video from the first frame.
3. Extract idle video's last frame.
4. Generate a return video using:
   - start image = idle last frame
   - end image = idle first frame
5. Concatenate:
   - `idle/action clip -> return-to-idle clip -> idle loop`
6. Use this to hide hard cuts when switching stock videos.

## Current Tested Assets

| Type | URL |
|---|---|
| Approved profile image | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/profile/preview_1782981331924/1782981335966.jpg |
| p-image-edit upright test | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/image-consistency/p-image-edit-pose-lab/runs/1782980967480-upright-standing-first-frame/image.jpg |
| Kling idle, same start/end | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/first-love-bedroom-idle-fullshot/20260702081612/video-5s-kling-omni-idle.mp4 |
| Kling idle, no end image | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/first-love-bedroom-visible-idle-motion-no-end/20260702082236/video-5s-kling-omni-visible-idle-motion-no-end.mp4 |

## Known Results

- Same start/end image made the video too fixed.
- Removing `end_image` allowed more movement.
- One Kling return-to-first-frame test failed with Kling internal error.
- p-image-edit can adjust pose/framing quickly, but may change identity/mood.
- The current best first frame type is wider full-body with room around the body.

## Base First Frame Prompt

```text
POV intimate candid smartphone photo of a beautiful adult 21-year-old East Asian Korean woman, delicate Korean features, fair Korean skin, soft dark brown eyes, long natural dark brown hair slightly messy and flowing around her face, fresh minimal makeup, rosy cheeks, glossy natural lips, gentle first-love smile, warm and innocent expression, looking directly at the viewer as if she is quietly noticing you in her room.

She is wearing a simple summer camisole tank top with thin spaghetti straps, soft ribbed cotton texture, light gray-white color, casual and clean, relaxed homewear styling, subtle collarbone visible, youthful but SFW. She is also wearing simple light summer bottoms that match the casual indoor outfit.

She is standing naturally in a bright cozy bedroom, shown in a full-body composition from head to feet. Use a wider angle and place the camera farther away so her entire body fits comfortably inside the frame with extra space above her head and below her feet. The framing should feel open and breathable, not cramped or tight. She stands upright with a straight waist, relaxed shoulders, and both legs naturally extended. No bent waist, no leaning forward, no crouching, no sitting. Her body faces the viewer in a calm natural stance, with one arm resting naturally by her side and the other hand lightly touching her hair or resting near her thigh. She looks comfortable, fresh, and quietly affectionate.

The room has a soft summer atmosphere: white bedsheets, pale wooden furniture, books stacked near the window, thin curtains moving slightly in the breeze, hydrangea or small flowers near the window, warm daylight pouring in from the side, soft highlights on her hair and skin, airy fresh mood, clean but lived-in Korean apartment bedroom.

Photorealistic raw iPhone snapshot, candid amateur photography, natural skin texture, visible pores, slight soft focus, shallow depth of field, gentle bokeh, mild film grain, realistic smartphone color, sunlit summer haze, no heavy retouching, no studio lighting, no artificial glamour, no text, no watermark, no logo, no nudity.

Wide-angle full-body shot, head-to-feet visible, camera positioned farther back, spacious framing, open composition, enough room around the subject, extra headroom and foot room, entire body fully inside the frame, upright standing pose only, straight waist, no forward bend, no leaning forward, no crouching, no sitting.
```

## Stock Clip List

| Clip | Motion brief |
|---|---|
| idle | standing in room, breathing, blinking, tiny weight shift, hair/curtain movement |
| typing | looks down at phone, thumbs type, small smile, returns gaze |
| approach and kiss | steps slightly closer, soft eye contact, gentle kiss toward camera |
| eating together | seated or standing near table, small bite, looks at viewer, smiles |
| lying together on bed | relaxed side-by-side POV, gentle gaze, small head movement |
| clapping | small happy clap, bright smile, still natural and non-performative |
| laughing | shy laugh, shoulders move lightly, eyes crinkle |
| sad | quiet sadness, eyes lower, small breath, restrained expression |

## Recommended Kling Prompt Pattern

```text
Preserve the same identity, face, hairstyle, outfit, room, lighting, color balance, and candid iPhone texture from the start image. Create a short realistic vertical smartphone video for: {stock_clip}. The motion should be visible but restrained, natural, and loop-friendly. Keep her as the same real person in the same bedroom. No dialogue, no subtitles, no text, no watermark. Avoid face morphing, body morphing, hand distortion, extra fingers, outfit changes, background changes, sitting unless the clip requires it, crouching, exaggerated gestures, or camera zoom.
```

## Key Files

| File | Why |
|---|---|
| `/Users/cheonmyeongseung/cereal-ai/.agents/skills/video-gen/SKILL.md` | Kling Omni + R2 manifest rules |
| `/Users/cheonmyeongseung/cereal-ai/.agents/skills/video-gen/scripts/run-kling-omni-video.mjs` | Existing video runner, but dialogue-required |
| `/Users/cheonmyeongseung/cereal-ai/.agents/skills/p-image-edit-pose-lab/SKILL.md` | First-frame pose correction path |
| `/Users/cheonmyeongseung/cereal-ai/.agents/runtime/image-profile-pipeline/2026-07-02T08-35-31-879Z-01-wide-full-body-spacious-main.json` | Current approved first-frame prompt/result |

## Do Not

- Do not mutate prompts silently. User wants execution, not hidden prompt rewriting.
- Do not use same start/end image for idle if the goal is visible motion.
- Do not use tight portrait framing for stock video; it causes bent-waist/compressed-body artifacts.
- Do not store video only as Replicate URL. Upload mp4 + manifest to R2.
- Do not make generated Korean text inside video; use overlay only if needed later.
