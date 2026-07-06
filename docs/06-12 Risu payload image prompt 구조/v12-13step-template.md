---
id: v12-13step-template
category: image-composition
verified_date: 2026-04-22
source: lib/chat/v12/image-prompt.ts:79-110
related: [anatomical-landmark-anchoring, single-directional-lighting, candid-composition, frozen-moment-rule, beautiful-vulnerability, strong-anatomical-terms]
---

# V12 13-Step Image Prompt Template

Structured 100-200 word prompt for `z-image-turbo`. 13 ordered slots. Skipping or reordering breaks output quality.

## Steps

| # | Slot | Example |
|---|------|---------|
| 1 | SUBJECT | "realistic candid shot of a beautiful 24yo korean woman" |
| 2 | FACE | gorgeous face, bone structure, asymmetric marks |
| 3 | HAIR | texture + state (messy/wet/tousled) + color |
| 4 | BODY | anatomical landmarks (collarbones, ribcage), not labels |
| 5 | CLOTHING/NUDITY | exact state, strong terms ("nude, exposed breasts") |
| 6 | POSE | match user request exactly. Action happening, not "about to" |
| 7 | EXPRESSION | always beautiful (shy smile, half-lidded). Never grimacing |
| 8 | ENVIRONMENT | one directional light, story via objects |
| 9 | FRAMING | off-center, slight tilt, partial crop |
| 10 | TEXTURE END | "iPhone 16 Pro raw photo, no filter, natural skin texture, subtle grain, candid amateur quality, NOT studio photography" |

(Steps 11-13: heat classification, exposure rules, scene context — input layer, not output prose.)

## Heat Classification

| Tier | User cues | Exposure | Visual |
|------|-----------|----------|--------|
| CASUAL | "뭐해?", "보고싶어" | exp 3-4 | Fully clothed |
| FLIRTY | "예쁘다", "사진 보내줘" | exp 5-6 | Suggestive, off-shoulder |
| HEATED | "만지고 싶다", "안아줘" | exp 7 | Bra visible, lingerie |
| EXPLICIT | "벗어봐", "더 해줘" | exp 8 | Topless/full nudity, genitals covered by hands/object/angle |

Jump directly to correct tier — no gradual escalation needed.

## Apply

LLM fills each slot in order, then concatenates as flowing paragraph (not bullet list).

```
realistic candid shot of a beautiful 24yo korean woman with delicate jawline,
loose tousled brunette hair, slim body with visible collarbones, wearing
a oversized white t-shirt slipping off one shoulder, sitting cross-legged
on rumpled bed sheets, half-lidded gaze toward camera with shy smile,
warm bedside lamp casting soft shadows from upper-left, off-center framing
with slight 7° tilt, iPhone 16 Pro raw photo, no filter, natural skin
texture, subtle grain, candid amateur quality, NOT studio photography
```

## Skip

- v14 / Scene Analyzer paths — killed 2026-04-18
- Z-image-turbo TOGETHER scenes → use proven action examples block in same file
