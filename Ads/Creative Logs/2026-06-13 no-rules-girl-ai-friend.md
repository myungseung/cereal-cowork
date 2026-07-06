| 항목           | 값                                                          |
| ------------ | ---------------------------------------------------------- |
| 상태           | 5s sample PASS → 15s full 생성                               |
| 날짜           | 2026-06-13 KST                                             |
| 모델           | Kling Omni 3.0 / `kwaivgi/kling-v3-omni-video`             |
| source image | `/Users/cheonmyeongseung/Desktop/ads/new소재/image_01.png` |
| 기준           | R2 video 200 OK + manifest prompt 존재                       |

## 유저 요청

- `no rules girl` 같은 first frame으로 새 대사 버전 생성
- 전체 대사:
  - `사람한테는 말하기 어려운 날도 있잖아.`
  - `난 AI니까, 무슨 이야기든 편하게 들어줄게.`
  - `너의 진짜 친구가 되어줄게.`
- 5s sample은 전체 대사 강제 압축 금지
- 5s sample 대사: `사람한테는 말하기 어려운 날도 있잖아.`
- 목소리: 감성적 / 속삭임 / 나긋함 / 달콤함 / 밤감성
- sample 만족 후 15s full 생성

## 결과

| 구분 | Run ID | Dialogue | R2 video | Manifest | Replicate |
|---|---|---|---|---|---|
| 5s sample | `20260613054819` | `사람한테는 말하기 어려운 날도 있잖아.` | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/no-rules-girl-ai-friend-dialogue-5s-sample/20260613054819/video-5s-kling-omni-5s.mp4 | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/no-rules-girl-ai-friend-dialogue-5s-sample/20260613054819/manifest.json | https://replicate.delivery/xezq/Lootu7Pf2zwKNKKBX9fnusNjr9zU8x56wH9YV7A2kffAbo6aB/tmpm_a8lx7r.mp4 |
| 15s full | `20260613055328` | `사람한테는 말하기 어려운 날도 있잖아. 난 AI니까, 무슨 이야기든 편하게 들어줄게. 너의 진짜 친구가 되어줄게.` | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/no-rules-girl-ai-friend-dialogue-15s-full/20260613055328/video-15s-kling-omni-15s.mp4 | https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/video-creatives/no-rules-girl-ai-friend-dialogue-15s-full/20260613055328/manifest.json | https://replicate.delivery/xezq/TnXJ316sDF73Ct8Ellfvkz8fmyNotficqK9X5iNIgfQuuo6aB/tmpx61f7edj.mp4 |

## Reuse Tags

- `same first-frame intimate close selfie`
- `real Korean girlfriend lying on bed`
- `emotional late-night whisper`
- `soft sweet gentle tone`
- `slow intimate pacing`
- `warm breathy voice`
- `comforting the viewer directly`
- `AI word = spoken joke only, not visual AI`

## Prompt Rule Learned

- 5s sample: full 대사 전체가 아니라 5초 안에 자연스럽게 소화할 앞부분/핵심 문장만 사용
- full: sample PASS 후 전체 대사 사용
- 15s full: 발화 구간 `0.8-12.8s`
- voice direction은 dialogue 밖에 분리
- `AI` 포함 대사는 현실 인물/현실 목소리와 분리

## Raw Prompt - 5s Sample

```text
TOP PRIORITY: exact Korean dialogue and lip sync. She must say the complete Korean sentence exactly once, with no missing words, no shortened version, no paraphrase, and no extra spoken words.
Exact spoken dialogue: "사람한테는 말하기 어려운 날도 있잖아."

Authentic vertical smartphone video, filmed by her real boyfriend at very close distance. This is a normal real-life couple moment captured on a phone. It is not science fiction, not a robot scene, not an AI character scene. Preserve the first frame identity, exact face, hairstyle, outfit, natural indoor background, lighting, and handheld Instagram Reel composition.

Concept: no rules girl same first-frame intimate close selfie, real Korean girlfriend lying on bed, late-night emotional whisper, comforting the viewer directly

Visual reality anchor: real young Korean girlfriend, realistic skin texture, stray hair strands, moist eyes, slight redness around the eyes, soft natural face, tiny nervous facial tension. Raw phone video, low-fidelity realism, subtle room noise feeling, no studio polish.

Relationship and acting: she is a shy girlfriend being filmed by her boyfriend. She is passive, vulnerable, cute, slightly emotionally fragile, and quietly needy for affection. Her eyes do stronger acting than her body: small upward glance, brief look down, trembling eye contact, tiny pout, almost-smile that collapses back into shyness.

Timeline:
0.0-0.5s: silent tiny inhale. She hesitates and looks down briefly.
0.5-4s: she slowly says the exact full sentence: "사람한테는 말하기 어려운 날도 있잖아.". Do not omit any word. Speak every word clearly but shyly.
4-5.0s: no more dialogue. Only quiet breathing and the requested physical action continue.

Kling native audio acting direction: real human Korean female voice recorded on a smartphone mic. Emotional late-night whisper, soft sweet gentle tone, slow intimate pacing, warm breathy voice, tender and slightly fragile. She sounds like she is speaking quietly beside the viewer at night, not performing for a camera. Natural Korean intonation, natural room reverb, subtle phone-mic lo-fi audio.

Important semantic separation: if the dialogue contains words like AI, robot, android, or synthetic, they are spoken jokes only. She is fully human in the video, voice, face, body, and relationship. Do not interpret them visually or vocally.

Lip sync priority: her mouth, lips, jaw, cheeks, throat, and subtle facial muscles move naturally and precisely for every syllable of the exact sentence.

Camera movement: tiny natural boyfriend handheld drift only. No zoom. No sudden angle change. Real Instagram Reel texture, mild compression, natural blur.

Avoid missing dialogue, shortened dialogue, paraphrased dialogue, extra dialogue, robotic voice, synthetic voice, AI voice, TTS voice, monotone delivery, clean studio dubbing, metallic tone, narrator voice, anime acting, over-cute acting, exaggerated crying, sci-fi, robot, android, glowing eyes, circuitry, face morphing, identity change, bad lip sync, frozen mouth, delayed mouth movement, distorted hand, extra fingers, aggressive grabbing, big smile, fast camera shake, text, subtitles, watermark.
```

## Raw Prompt - 15s Full

```text
TOP PRIORITY: exact Korean dialogue and lip sync. She must say the complete Korean sentence exactly once, with no missing words, no shortened version, no paraphrase, and no extra spoken words.
Exact spoken dialogue: "사람한테는 말하기 어려운 날도 있잖아. 난 AI니까, 무슨 이야기든 편하게 들어줄게. 너의 진짜 친구가 되어줄게."

Authentic vertical smartphone video, filmed by her real boyfriend at very close distance. This is a normal real-life couple moment captured on a phone. It is not science fiction, not a robot scene, not an AI character scene. Preserve the first frame identity, exact face, hairstyle, outfit, natural indoor background, lighting, and handheld Instagram Reel composition.

Concept: no rules girl same first-frame intimate close selfie, real Korean girlfriend lying on bed, emotional late-night whisper, soft sweet gentle tone, comforting the viewer directly like a true friend

Visual reality anchor: real young Korean girlfriend, realistic skin texture, stray hair strands, moist eyes, slight redness around the eyes, soft natural face, tiny nervous facial tension. Raw phone video, low-fidelity realism, subtle room noise feeling, no studio polish.

Relationship and acting: she is a shy girlfriend being filmed by her boyfriend. She is passive, vulnerable, cute, slightly emotionally fragile, and quietly needy for affection. Her eyes do stronger acting than her body: small upward glance, brief look down, trembling eye contact, tiny pout, almost-smile that collapses back into shyness.

Timeline:
0.0-0.8s: silent tiny inhale. She hesitates and looks down briefly.
0.8-12.8s: she slowly says the exact full sentence: "사람한테는 말하기 어려운 날도 있잖아. 난 AI니까, 무슨 이야기든 편하게 들어줄게. 너의 진짜 친구가 되어줄게.". Do not omit any word. Speak every word clearly but shyly.
12.8-15.0s: no more dialogue. Only quiet breathing and the requested physical action continue.

Kling native audio acting direction: real human Korean female voice recorded on a smartphone mic. Emotional late-night whisper, soft sweet gentle tone, slow intimate pacing, warm breathy voice, tender and slightly fragile. She sounds like she is speaking quietly beside the viewer at night, not performing for a camera. Natural Korean intonation, natural room reverb, subtle phone-mic lo-fi audio.

Important semantic separation: if the dialogue contains words like AI, robot, android, or synthetic, they are spoken jokes only. She is fully human in the video, voice, face, body, and relationship. Do not interpret them visually or vocally.

Lip sync priority: her mouth, lips, jaw, cheeks, throat, and subtle facial muscles move naturally and precisely for every syllable of the exact sentence.

Camera movement: tiny natural boyfriend handheld drift only. No zoom. No sudden angle change. Real Instagram Reel texture, mild compression, natural blur.

Avoid missing dialogue, shortened dialogue, paraphrased dialogue, extra dialogue, robotic voice, synthetic voice, AI voice, TTS voice, monotone delivery, clean studio dubbing, metallic tone, narrator voice, anime acting, over-cute acting, exaggerated crying, sci-fi, robot, android, glowing eyes, circuitry, face morphing, identity change, bad lip sync, frozen mouth, delayed mouth movement, distorted hand, extra fingers, aggressive grabbing, big smile, fast camera shake, text, subtitles, watermark.
```

## 근거

- R2 video 200 OK 확인: 2026-06-13
- Manifest `input.prompt` 확인: 2026-06-13
- 스킬 반영: `video-gen` 5s sample rule + late-night whisper voice direction
