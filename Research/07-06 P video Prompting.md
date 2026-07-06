

## Goal

Build a video asset generation pipeline for a character chat platform where the character appears continuously alive inside a room.

The product playback system already supports continuous playback and smooth sequencing.  
This handoff only defines the video assets that need to be generated.

---

## Core Principle

Every video asset must start from and return to the same shared neutral frame.

This shared frame is called:

```text
idle_start
```

`idle_start` is the universal connection point between all character videos.

All generated videos must follow this rule:

```text
idle_start → motion/emotion → return_to_idle_start
```

The final frame of every completed asset must visually match `idle_start`.

---

## Asset Duration Rules

Because the video model only supports second-based generation, use fixed segment durations.

|Asset Type|Core Segment|Return Segment|Total|
|---|--:|--:|--:|
|Idle|2s|1s|3s|
|Basic Emotion|5s|2s|7s|
|Special Situation|5–8s|2s|7–10s|

---

## Required Source Images

### 1. `idle_start`

The original neutral character image.

Purpose:

```text
Universal first frame and universal return frame.
```

All videos must start from this frame or a frame visually identical to it.

### 2. `idle_peak`

A tiny micro-motion variation of `idle_start`.

Purpose:

```text
Used as the endpoint for the idle core segment.
```

This should not look like a new pose.  
It should only contain very subtle life-like changes.

Prompt:

```text
Same woman, same room, same composition.
Keep the original pose and same distance from camera.
Soft forward gaze, gentle shy smile.
Slight breathing lift, soft blink or tiny smile change, slight hair shift.
No pose change, no camera change, no background change.
```

---

# Video Asset Structure

## 1. Idle Video

Idle is the default always-playing neutral loop.

### Structure

```text
idle_start → idle_peak → idle_start
```

### Segments

|Segment|Duration|Description|
|---|--:|---|
|`idle_core`|2s|Natural micro-motion|
|`idle_return`|1s|Return to original idle frame|

### Output

```text
idle_loop.mp4
```

### Prompt: `idle_core_2s`

```text
Same woman, same room, same composition.
Keep original pose and same distance from camera.
Soft forward gaze, gentle shy smile.
Subtle breathing, blinking, tiny smile change, slight finger movement, slight hair sway.
Locked camera, no pose change, no moving closer.
End in a subtle natural idle variation.
```

### Prompt: `idle_return_1s`

```text
Same woman, same room, same composition.
Return gently to the original first frame.
Soft forward gaze, gentle shy smile.
Only tiny breathing settling, smile settling, finger and hair settling.
Locked camera, no pose change, no moving closer.
End exactly on the original idle frame.
```

---

## 2. Basic Emotion Videos

Basic emotion videos are used after user messages.

Each emotion video must start from `idle_start`, show the emotional reaction, then return to `idle_start`.

### Structure

```text
idle_start → emotion_peak → idle_start
```

### Duration

```text
5s core + 2s return = 7s total
```

### Required Basic Emotions

|ID|Emotion|Use Case|
|---|---|---|
|`happy`|Happy smile|Praise, positive reply|
|`shy`|Shy / embarrassed|Flirting, affection|
|`curious`|Curious|User asks something|
|`thinking`|Thinking|LLM response generation|
|`sad`|Sad / hurt|User is cold or negative|
|`jealous`|Jealous / pouty|User mentions another girl|
|`comfort`|Caring / worried|User says they are tired or sad|
|`affection`|Warm affection|High intimacy moment|

### Prompt Template: `emotion_core_5s`

```text
Same woman, same room, same composition.
Start from the original idle frame.
Keep the same body position and same distance from camera.
Show a natural [EMOTION] reaction while staying in the same pose.
Small facial change, subtle breathing, blinking, slight hand and hair movement.
Locked camera, no pose change, no moving closer.
End in the peak [EMOTION] expression.
```

### Prompt Template: `emotion_return_2s`

```text
Same woman, same room, same composition.
Gently return from [EMOTION] to the original neutral idle frame.
Expression settles back to a soft gentle smile.
Only subtle breathing, blinking, hair and hand settling.
Locked camera, no pose change, no moving closer.
End exactly on the original idle frame.
```

---

## 3. Special Situation Videos

Special videos are stronger relationship moments.

They should still start and end on `idle_start`.

### Duration

```text
5–8s core + 2s return
```

### MVP Special Set

|ID|Situation|Purpose|
|---|---|---|
|`first_meeting`|First encounter|First user experience|
|`welcome_back`|User returns|Retention / memory feeling|
|`missed_you`|User returns after absence|Light longing|
|`good_night`|Night conversation|Intimacy|
|`morning`|Morning greeting|Daily habit|
|`confession`|User says “I like you”|Emotional reward|
|`comfort_deep`|User shares difficulty|Attachment|
|`relationship_level_up`|Relationship milestone|Premium / reward moment|

### Prompt Template: `special_core_5s_8s`

```text
Same woman, same room, same composition.
Start from the original idle frame.
Keep the same body position and same distance from camera.
Show a natural [SITUATION] reaction with subtle emotional movement.
Facial expression changes gently, with breathing, blinking, slight hand movement, and slight hair sway.
Locked camera, no pose change, no moving closer.
End in the peak [SITUATION] expression.
```

### Prompt Template: `special_return_2s`

```text
Same woman, same room, same composition.
Gently return from the [SITUATION] expression to the original neutral idle frame.
Expression settles back to a soft gentle smile.
Only subtle breathing, blinking, hair and hand settling.
Locked camera, no pose change, no moving closer.
End exactly on the original idle frame.
```

---

# Naming Convention

Use this naming format:

```text
characterId_scene_emotion_segment_duration_version.mp4
```

Examples:

```text
girl01_bedroom_idle_core_2s_v001.mp4
girl01_bedroom_idle_return_1s_v001.mp4
girl01_bedroom_idle_loop_3s_v001.mp4

girl01_bedroom_shy_core_5s_v001.mp4
girl01_bedroom_shy_return_2s_v001.mp4
girl01_bedroom_shy_full_7s_v001.mp4
```

---

# Generated File Types

For each asset, save:

```text
/core.mp4
/return.mp4
/full.mp4
/metadata.json
/preview.gif or preview.mp4
```

Example folder:

```text
assets/girl01/bedroom/idle/
  idle_start.png
  idle_peak.png
  core_2s.mp4
  return_1s.mp4
  full_3s.mp4
  metadata.json
  preview.mp4
```

---

# Metadata Format

Each generated asset should include metadata.

```json
{
  "character_id": "girl01",
  "scene": "bedroom",
  "asset_type": "idle",
  "emotion": "neutral",
  "start_frame": "idle_start",
  "end_frame": "idle_start",
  "core_duration": 2,
  "return_duration": 1,
  "total_duration": 3,
  "prompt_core": "...",
  "prompt_return": "...",
  "status": "pending_review"
}
```

---

# Quality Requirements

Each generated video must pass these checks before approval.

|Check|Requirement|
|---|---|
|Identity consistency|Same face and same character|
|Pose consistency|No pose change|
|Camera consistency|No zoom, pan, rotation, or crop drift|
|Distance consistency|Character must not move closer to camera|
|Background consistency|Same room, no flicker|
|Hand consistency|No warped hands or extra fingers|
|Final frame match|Final frame must match `idle_start`|
|Motion quality|Alive but subtle|
|Return quality|Return should not look like an obvious rewind|
|Transition readiness|Can connect to any next video from `idle_start`|

---

# Review Flow

Use the existing Lab viewer for approval.

Pipeline:

```text
generate core segment
→ generate return segment
→ concatenate full asset
→ auto quality check
→ Lab viewer review
→ approve / reject
→ if approved, move to next asset
```

---

# MVP Production Order

Generate assets in this order.

## Phase 1: Minimum Alive Character

```text
1. idle
2. thinking
3. happy
4. shy
5. comfort
```

## Phase 2: Relationship Feeling

```text
6. welcome_back
7. missed_you
8. jealous
9. affection
10. good_night
```

## Phase 3: Monetizable Special Moments

```text
11. confession
12. relationship_level_up
13. comfort_deep
14. morning
15. first_meeting
```

---

# Final Rule

Do not generate videos as isolated animations.

Every video is part of a shared transition system.

The rule is always:

```text
Start from idle_start.
Show the motion.
Return to idle_start.
End exactly on idle_start.
```

This allows the character to feel continuously alive while still making every video asset composable.

이 문서 기준이면 Codex는 **영상 생성 자동화 / 파일 네이밍 / QA / Lab 리뷰 루프**까지 바로 잡을 수 있음.