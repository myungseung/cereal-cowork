# 06-17 Ji-eun Seo — Grok Image Pipeline

| 항목 | 값 |
|------|-----|
| 캐릭터 | 서지은 (Ji-eun Seo) |
| 나이 | 25 |
| 직업 | 인스타그램 인플루언서 (180K followers) |
| 거주지 | 서울 성수동 오피스텔 |
| 매니저 | 김민준 (32세, 2년차) |
| 생성 모델 | `prunaai/z-image-turbo-lora` + NiceGirls 0.5 |
| 프롬프트 생성 | `x-ai/grok-4.20` + P015 패턴 템플릿 |

---

## Storyboard — 5 Scene Arc

| Scene | 각도                              | 설명                                      | 이미지                                                                                                                           | 참조 Prompt ID |
| ----- | ------------------------------- | --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | -------------- |
| **1** | 정면 POV — 그의 무릎 위에 올라탄 그녀        | 긴 하루 후 와인 한잔, 그의 무릎 위에 올라타 "오빠였으면 좋겠어"  | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617113900-ji-eun-seo/image.jpg)        | — |
| **2** | 수평 사이드 프로필                      | 안아서 침대로 옮긴 후, 그가 그녀 위로 내려오는 순간          | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617114021-ji-eun-seo-scene2/image.jpg) | — |
| **3** | 뒤에서 over-the-shoulder — 네발기기 자세 | 침대 위 네발기기, 뒤에서 삽입, 그녀가 어깨 너머로 뒤돌아봄      | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617114448-ji-eun-seo-scene3/image.jpg) | Ref #2 (all fours, rustic kitchen) |
| **4** | 낮은 앵글 아래에서 — 카우걸 자세             | 그가 아래에서 바라보는 POV, 그녀가 위에서 타고 있음, 정면 반대쪽 | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617114451-ji-eun-seo-scene4/image.jpg) | **S001** (reverse cowgirl, LED bedroom) |
| **5** | 위에서 내려다보는 클로즈업 — 선교사 자세         | 정점 순간, 그녀의 얼굴 클로즈업, 완전한 몰입              | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617114455-ji-eun-seo-scene5/image.jpg) | — |
| **6** | 뒤에서 극클로즈업 — 엉덩이/보지 집중           | 정점 후, 뒤에서 extreme close-up, 보지와 애널 디테일  | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617115051-ji-eun-seo-scene6/image.jpg) | Ref #3 (spread, extreme detail) |

### Ref #1 — 22 caucasian supermodel, dorm bedroom, cowgirl reverse POV

```
realistic candid shot of 22 year old caucasian supermodel-looking and stunning young woman. She has hazel-colored eyes. Her face has a a delicate bone structure with slightly sunken cheeks and a softly sloping brow, giving her an approachable yet mysterious air. her jawline is subtly square, lending a hint of strength to her otherwise ethereal appearance. She has a a small, slightly upturned nose with a barely visible bump on the bridge, and full lips that curve into a natural, almost pensive pout. a constellation of tiny, barely-there freckles dusts her nose and upper cheeks, adding to her unique charm. her skin is a canvas of realistic imperfections – a smattering of open pores, barely visible vellus hairs along her hairline, and subtle variations in tone around her eyes and mouth, giving the impression of a genuine, lived-in complexion. the fine lines around her eyes hint at a youthful vitality that feels both fresh and authentic. She wears a natural makeup with a touch of rosy blush on the apples of her cheeks and a subtle shimmer on her eyelids, emphasizing her natural beauty without being overbearing..Her hair is long, wavy hair with loose curls cascading down her back, creating a messy, undone look. the texture is slightly frizzy, adding volume and a sense of carefree abandon.. Color of her hair is light brown with subtle golden highlights catching the light.. Her body is slender with a gentle curve to her waist and a long, lithe frame, giving off a sense of both fragility and inner strength.. She is wearing a collar on her neck. a black velvet choker, softly embracing the neck, lined with delicate silver studs. She is fully naked. She looks directly at the viewer with cute smile. POV shot from man's perspective. Man is lying on his back while she is straddling a man's pelvis. Her buttocks rised up and spread, revealing her anus and vagina during penetration. Her pussy is fully visible for viewer. She is facing away from camera, looking back over her shoulder. Her hands are resting on her buttocks. Man's torso and arms are visible at foreground framing the scene. He has a fair skin and muscular body. The man's erect penis is visible, penetrating her vagina. The scene is set in a dimly lit, cozy dorm bedroom bathed in the soft glow of a single bedside lamp. a plush, velvet-covered futon, covered in a silk throw. Background is faded posters of indie bands and abstract art, vintage vinyl records on a shelf, sandalwood incense, a single candle on a chipped nightstand. Image represents a photograph macro lens for extreme detail, shallow depth of field, shot on a modern digital camera with slight cinematic color grading. Lighting is harsh overhead fluorescent from the ceiling, mixed with warm diffused glow from a laptop screen, slightly grainy. Color scheme is a mix of pale blues, pinks, and purples reflecting the laptop screen colors, desaturated, muted aesthetic for realism. <lora:Mystic-XXX-ZIT-V5:1> <lora:nicegirls_Zimage:0.3>
```

### Ref #2 — 20 european kawaii, rustic kitchen, all fours

```
amateur snapshot shot of 20 year old european kawaii and playful young woman. She has hazel-colored eyes. Her face has a delicate bone structure with a slightly pointed chin and softly rounded cheekbones. a gentle, almost doll-like quality. She has a a small, slightly upturned nose with subtle freckles scattered across the bridge. her lips are full and naturally rosy, with a hint of a dimple on the left side when she smiles. eyes are wide and innocent. her skin has a realistic texture with visible pores, especially around the nose and forehead. the texture appears smooth and supple overall, with a slight peach fuzz visible upon close inspection. She wears a natural makeup with subtle blush applied to the apples of her cheeks, a light touch of eyeshadow in warm brown tones, and a clear lip gloss to enhance her natural color..Her hair is long, flowing waves with a slightly messy, effortless appearance. the texture is soft and slightly tousled.. Color of her hair is light auburn. Her body is slender but not overly thin, with a gentle curve to her waist and soft, rounded hips.. She is wearing a choker on her neck. twisted hemp cord adorned with miniature pressed forget-me-nots, lending a rustic charm. She wears a creamy flax linen bonnet tied with a faded rose ribbon, delicately stitched with tiny wildflower embroidery. Her lower body is dressed in a high-waisted, off-white denim shorts, faded and gently distressed at the hem. Her perky puffy breasts are naked, with tiny and light-pink areola and puffy and slightly erected nipples. The scene is set in an intimate rustic wooden kitchen, the air thick with the earthy scent of drying herbs. She is positioned on all fours, arching her back slightly, revealing the subtle curve of her spine and the delicate hollow of her lower back. she leans forward, peering intently towards a point beyond the camera's view. Her facial expression is intense focus. Background is open shelves displaying various clay pots, copper pans, and cast iron skillets, bundles of dried lavender, rosemary, and thyme hang from the rafters, a crackling fireplace radiates warmth and light. At foreground: a single, flickering candle sits on the edge of the wooden table. Image represents a photograph vintage 35mm film with a slight grain and shallow depth of field. Lighting is soft, diffused natural light streaming in from a nearby window, slightly overexposed with a warm color temperature, creating long shadows cast from the hanging herbs, simulating an amateur snapshot aesthetic. Color scheme is a muted, earthy palette of warm browns, creams, and soft greens. <lora:Mystic-XXX-ZIT-V5:1> <lora:nicegirls_Zimage:0.3>
```

### Ref #3 — 23 caucasian sexy, xerox room, cowgirl reverse spread

```
35mm film shot of 23 year old caucasian sexy and daring young woman. She has hazel-colored eyes. Her face has a a strong, yet angular jawline coupled with prominent cheekbones that create an intriguing contrast. her chin is slightly cleft, adding a touch of unconventional charm. She has a a small, upturned nose with barely visible pores lends a youthful appearance. her lips are full and pouty, with natural pink hues. a scattering of fine, barely noticeable freckles across her nose and cheeks add character. one eyebrow has a slight arch, creating a subtle asymmetry. pores are visible but not exaggerated, giving the skin a lived-in feel. the complexion is a warm ivory, with subtle variations in tone. She wears a natural makeup with subtle blush on the apples of her cheeks creates a hint of flush, while a touch of brown eyeshadow in the crease of her eyes provides definition without being overpowering. her lips are coated with a sheer gloss that enhances their natural fullness..Her hair is long, wavy layers cascading down her back, with a slight natural curl. the texture is slightly frizzy, providing movement and dimension.. Color of her hair is honey brown. Her body is athletic and toned, with lean muscle definition. her shoulders are slightly broad, lending a balanced and powerful silhouette.. She is wearing a chain on her neck. fine silver chain, crafted from shimmering metal, loops around her neck. She is fully naked. She looks directly at the viewer with innocent expression. POV shot from man's perspective. Man is lying on his back while she is straddling a man's pelvis. Her butt cheeks rised up and spread wide, exposing her anus and beautiful vagina during penetration. Her anal and vagina is visible in frame. She is facing away from camera, looking back over her shoulder. Her hands are resting on her buttocks. Man's torso and arms are visible at foreground framing the scene. He has a fair skin and muscular body. The man's erect penis with veins and pinkish glans is visible, penetrating her vagina. The scene is set in a clandestine xerox room, damp with the scent of toner and old paper. a worn, oversized copy machine, its buttons illuminated by a faint, pulsating blue light. Background is stacks of discarded documents and blueprints litter the space, a single, flickering fluorescent light bulb overhead casts a pallid, unnatural glow. Image represents a photograph 35mm film, slight film grain, shallow depth of field. Lighting is low-key, cool-toned lighting from a blue fluorescent ceiling fixture combined with the soft glow of a distant window creating a cinematic chiaroscuro effect. Color scheme is dominant cool tones of blue and gray with accents of warm skin tones. <lora:Mystic-XXX-ZIT-V5:1> <lora:nicegirls_Zimage:0.3>
```

### S001 — 19 east-asian, teen bedroom LED, reverse cowgirl (Grok generated)

```
realistic candid shot of 19 year old east-asian petite and seductive young woman. She has dark almond-shaped eyes with long natural lashes. Her face has a soft heart-shaped structure with high cheekbones, a gently rounded jawline and a small delicate chin giving her an innocent yet alluring look. She has a tiny straight nose with a subtle bridge, full naturally glossy lips in a soft cupid's bow, and a light dusting of faint freckles across her nose. her skin is a smooth warm ivory with realistic texture – visible fine pores especially on the nose and cheeks, soft peach fuzz along the hairline, subtle tonal variations and a few tiny beauty marks adding lived-in authenticity. She wears natural makeup with a hint of dewy peach blush, soft brown eyeliner on the upper lids and a glossy nude lip tint. Her hair is shoulder-length silky black hair with subtle messy layers and soft bangs framing her face, slightly tousled with natural shine. Her body is slim and delicate with small perky breasts, narrow waist and gently flared hips, smooth toned legs. She is fully naked except for a thin black leather collar with a small silver ring on her neck. POV shot from man's perspective. Man is sitting on the edge of the bed while she is reverse cowgirl on his lap facing away, leaning forward with her hands braced on his knees. Her ass is lifted high and cheeks spread wide by her own fingers, fully exposing her tight pink anus and glistening shaved vagina during deep penetration. The man's muscular torso and veiny erect penis are visible in the foreground, his cock buried inside her pussy with creamy wetness visible at the entrance. She is looking back over her shoulder directly at the viewer with a naughty playful smirk. The scene is set in a messy teenage girl's bedroom at night, soft pink LED strip lights and fairy lights draped over the headboard creating an intimate glow. Background shows scattered clothes, plushies, an open laptop with anime stickers, posters of J-pop idols on pastel walls and a large mirror reflecting part of the scene. Image represents a photograph macro lens for extreme detail, shallow depth of field, slight film grain, shot on a modern digital camera with soft cinematic grading. Lighting is warm low-key pink and purple LED ambiance mixed with cool blue light from the laptop screen. Color scheme of picture is soft pastel pinks, lavenders and warm skin tones with cool blue accents, slightly desaturated for realism.
```

### P015 — 22 east-asian korean, rooftop garden twilight (Grok generated)

```
A 22-year-old East Asian Korean beauty with delicate Korean features, fair Korean skin, soft dark brown eyes, plump glossy lips and subtle dewy makeup, standing gracefully on a misty rooftop garden at twilight, wearing a soft oversized cream knit cardigan over a white lace camisole and high-waisted denim shorts, gently holding a small bouquet of white cosmos flowers, soft wind-blown hair, cinematic purple and pink twilight lighting with gentle bokeh city lights in the background, golden hour glow on her face, shot in lo-fi amateur style
```

## 대화 ↔ 이미지 매핑

| Turn | 화자 | 내용 | Scene |
|------|------|------|-------|
| 1 | 유저 | "오늘 스케줄 끝났어. 왜 혼자 딴 데 쳐다보고 있어?" | — |
| 2 | 지은 | "오늘 댓글들 너무 시끄러웠어... 나도 내가 하고 싶어서 한 건데" | — |
| 3 | 유저 | "니가 진짜로 하고 싶은 게 뭔데? 카메라 꺼지고 아무도 안 보는데" | — |
| 4 | 지은 | "오늘은 오빠랑만 있고 싶어. 사람들 시선 없이" | — |
| 5 | 유저 | "너 취하면 원래 장난 심해지잖아" | — |
| 6 | 지은 | "오빠는 내가 취한 모습이 더 좋아? 멀쩡한 모습이 더 좋아?" | — |
| 7 | 유저 | "지금 눈빛이 좀 위험한데. 뭔가 하고 싶은 거 있으면 말해" | — |
| 8 | 지은 | *(무릎 위에 올라탄다)* "오늘만은 매니저 오빠 말고, 그냥 오빠였으면 좋겠어" | **Scene 1** |
| 9 | 유저 | "좋아, 오빠가 제대로 가르쳐줄게. 근데 오늘은 진짜 늦게 끝난다?" | **→ carry to bed** |
| 10 | 지은 | "그래서 더 좋은데. 나 얼마나 참았는데... 좀 세게 해도 돼?" | **Scene 2 → 3 → 4 → 5 → 6** |

---

## Scene 상세

### Scene 1 — Living Room: "그냥 오빠였으면 좋겠어"

![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617113900-ji-eun-seo/image.jpg)

| 필드 | 값 |
|------|-----|
| 장소 | 거실, 크림 소파, 성수동 오피스텔 |
| 시간 | 오후 10:30 |
| 조명 | 따뜻한 앰버 LED + 서울 야경 보케 |
| 의상 | 오버사이즈 화이트 남친 셔츠 (노브라), 블랙 레이스 팬티 |
| 헤어 | 살짝 헝클어진 긴 흑발 웨이브 |
| 메이크업 | 번진 아이라이너, 바랜 립틴트 |
| 각도 | 정면 POV (그의 시점, 그녀가 무릎 위에 올라탐) |
| 분위기 | 취약함, 진정한 연결을 갈망, 참아왔던 감정 폭발 직전 |

### Scene 2 — Bedroom: "세게 해도 돼?"

![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617114021-ji-eun-seo-scene2/image.jpg)

| 필드 | 값 |
|------|-----|
| 장소 | 침실, 흰 리넨 시트, 다크 우드 헤드보드 |
| 시간 | 오전 1:17 |
| 조명 | 차가운 블루 문라이트 + 따뜻한 2700K 침대 옆 램프 |
| 의상 | 오버사이즈 화이트 셔츠 (어깨 벗겨짐), 블랙 레이스 팬티 |
| 헤어 | 베개 위에 흩어진 헝클어진 흑발 |
| 메이크업 | 키스로 번지고 부은 입술 |
| 각도 | 수평 사이드 프로필 (두 body 모두 보이는 미디엄 샷) |
| 분위기 | 숨찬 기대, half-lidded eyes, 키스로 부은 입술 |

### Scene 3 — Doggy Style: 뒤에서

![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617114448-ji-eun-seo-scene3/image.jpg)

| 필드 | 값 |
|------|-----|
| 장소 | 침실, 흰 리넨 시트, 침대 |
| 시간 | 늦은 밤 (Scene 2에서 이어짐) |
| 조명 | 차가운 블루 문라이트 + 따뜻한 2700K 침대 옆 램프 |
| 의상 | 완전 누드 (셔츠 벗겨짐) |
| 헤어 | 헝클어진 흑발이 얼굴 위로 흘러내림 |
| 메이크업 | 완전히 망가짐 — 번지고 땀에 젖음 |
| 각도 | 뒤에서 over-the-shoulder — 네발기기 자세, 뒤돌아봄 |
| 분위기 | 격렬한 삽입, 그녀가 뒤돌아 쳐다보며 신음 |

### Scene 4 — Cowgirl: 그녀가 위에서

![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617114451-ji-eun-seo-scene4/image.jpg)

| 필드 | 값 |
|------|-----|
| 장소 | 침실, 흰 리넨 시트, 침대 |
| 시간 | 늦은 밤 (Scene 3에서 이어짐) |
| 조명 | 차가운 블루 문라이트 + 따뜻한 2700K 침대 옆 램프 |
| 의상 | 완전 누드 |
| 헤어 | 땀에 젖은 흑발이 등 위로 흘러내림 |
| 메이크업 | 완전히 망가짐 — 땀에 번짐 |
| 각도 | 낮은 앵글 아래에서 — 그녀가 위에서 타고 있음, 카메라 반대쪽 |
| 분위기 | 그녀가 주도, 고개를 뒤로 젖히며 쾌락에 빠짐 |

### Scene 5 — Missionary: 정점

![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617114455-ji-eun-seo-scene5/image.jpg)

| 필드 | 값 |
|------|-----|
| 장소 | 침실, 흰 리넨 시트, 침대 |
| 시간 | 이른 아침 시간 (Scene 4에서 이어짐) |
| 조명 | 차가운 블루 문라이트 + 따뜻한 2700K 침대 옆 램프 |
| 의상 | 완전 누드 |
| 헤어 | 땀에 젖은 흑발이 이마에 붙고 베개 위에 흩어짐 |
| 메이크업 | 완전히 망가짐 — 눈가에 눈물 흔적 |
| 각도 | 위에서 내려다보는 클로즈업 — 그의 POV, 그녀의 얼굴 + 상체 |
| 분위기 | 정점 순간 — 입 벌린 신음, half-lidded empty eyes, 완전한 몰입 |

### Scene 6 — Extreme Close-up: 뒤에서

![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617115051-ji-eun-seo-scene6/image.jpg)

| 필드 | 값 |
|------|-----|
| 장소 | 침실, 흰 리넨 시트, 침대 |
| 시간 | 이른 아침 (Scene 5 직후) |
| 조명 | 차가운 블루 문라이트 + 따뜻한 2700K 침대 옆 램프 |
| 의상 | 완전 누드 |
| 헤어 | 땀에 젖은 흑발이 등 위로 흘러내림 |
| 메이크업 | 완전히 망가짐 |
| 각도 | 뒤에서 극클로즈업 — 엉덩이/보지 집중, 네발기기 자세 |
| 분위기 | 정점 후, 모든 디테일이 보이는 raw한 intimate shot |

---

## Consistency Map (5 Scenes)

| 요소 | Scene 1 | Scene 2 | Scene 3 | Scene 4 | Scene 5 | Scene 6 |
|------|---------|---------|---------|---------|---------|---------|
| 공간 | 거실 소파 | 침대 | 침대 | 침대 | 침대 | 침대 |
| 조명 | 앰버 LED | 문라이트+램프 | 문라이트+램프 | 문라이트+램프 | 문라이트+램프 | 문라이트+램프 |
| 의상 | 셔츠+팬티 | 셔츠(벗겨짐)+팬티 | 누드 | 누드 | 누드 | 누드 |
| 헤어 | 살짝 헝클어짐 | 베개에 흩어짐 | 얼굴 위로 | 등 위로 흘러내림 | 이마에 붙음 | 등 위로 흘러내림 |
| 메이크업 | 번짐 | 키스 자국 | 망가짐+땀 | 망가짐+땀 | 눈물+완전붕괴 | 완전붕괴 |
| 각도 | 정면 POV | 사이드 프로필 | 뒤에서 OTS | 아래에서 위로 | 위에서 아래로 | 뒤에서 극클로즈업 |
| 페이스 앵커 | ✅ fox eyes, plump lips, heart-shaped face (모든 Scene 동일) |
| 바디 앵커 | ✅ K-cup, tiny waist, wide hips, thick thighs, milky skin (모든 Scene 동일) |
| 스킨 앵커 | ✅ fair Korean skin, milky skin (모든 Scene 동일) |

---

## Character Profile

```json
{
  "name": "Ji-eun Seo (서지은)",
  "age": 25,
  "platform": "Instagram / private fan community",
  "niche": "Fashion, beauty, lifestyle — 'premium girlfriend fantasy'",
  "followerCount": "180K Instagram, 800 private fan subscribers",
  "incomeLevel": "15-20M KRW/month",
  "seoulNeighborhood": "Seongsu-dong, Seoul",
  "apartmentType": "Luxury officetel, modern minimalist, floor-to-ceiling windows",
  "manager": "Kim Min-jun, 32, 2 years working together"
}
```

## Success Criteria (합격/불합격 기준)

본 세션에서 검증된 규칙. 아래 기준 중 하나라도 누락 시 해당 prompt는 재생성.

### A. 한국인 조건 (Mandatory)
prompt 시작 부분에 아래 4개 중 **2개 이상** 반드시 포함:
- `East Asian Korean`
- `Korean features`
- `fair Korean skin`
- `soft dark brown eyes`

| 판정 | 예 |
|------|-----|
| ✅ PASS | `A 25-year-old East Asian Korean beauty with delicate Korean features, fair Korean skin, soft dark brown eyes...` |
| ❌ FAIL | `Ji-eun Seo, 25-year-old Korean influencer...` (V1이 실패한 원인 — `Korean`만 1개, `fair Korean skin` 없음) |

### B. Beauty Anchor First 구조
Prompt는 반드시 **나이 → 한국인 → 얼굴상세 → 메이크업** 순서로 시작. 그 다음 장소/동작/의상.

```
[나이] + [한국인조건] + [얼굴형/눈/입술] + [메이크업] + [동작/장소/시간] + [의상] + [소품] + [분위기] + [조명] + [사진스타일]
```

| 판정 | 예 |
|------|-----|
| ✅ PASS | `A 25-year-old East Asian Korean beauty with delicate Korean features, fair Korean skin, soft dark brown eyes, seductive fox eyes, plump glossy lips, heart-shaped face and smudged makeup...` |
| ❌ FAIL | `Ji-eun Seo is standing in a penthouse...` (이름부터 시작, 한국인 조건 없음) |

### C. Chain of Thought 선행
Prompt 작성 전 반드시 캐릭터의 **현재 상황(context)** 을 추론할 것:

1. 캐릭터는 지금 **무슨 플랫폼**에서 활동하는가? (Instagram / OnlyFans / BJ)
2. **오늘 무슨 일**이 있었는가? (라이브 방송 악플 → 매니저에게 문자)
3. **현재 장소**는 어디인가? (거실? 침대? 몇 시?)
4. **현재 복장**은 무엇이며, 왜 그 복장인가? (방송복 → 갈아입음? 아니면 그대로?)
5. **조명/무드**는 구체적으로 무엇인가? (LED 스트립? 달빛? 램프?)

| 판정 | 예 |
|------|-----|
| ✅ PASS | `"Changed out of broadcast camisole into oversized boyfriend shirt (no bra). Hair slightly tangled from removing accessories. Makeup smudged — vulnerable and real."` |
| ❌ FAIL | `"wearing a cream silk camisole"` (broadcast outfit을 벗지 않고 그대로 — 비현실적) |

### D. Scene 간 Consistency (멀티 Scene 필수)
동일 캐릭터의 여러 Scene을 생성할 때 아래 요소는 **절대 변경 금지**:

| 고정 요소 | Scene 1 | Scene 2 | Scene 3 | Scene 4 | Scene 5 |
|-----------|---------|---------|---------|---------|---------|
| 얼굴 앵커 | `heart-shaped face, seductive fox eyes, plump kiss-swollen glossy lips` | 동일 | 동일 | 동일 | 동일 |
| 헤어 앵커 | `long [tangled] jet-black wavy hair` | 동일 | 동일 | 동일 | 동일 |
| 피부 앵커 | `fair Korean skin, milky skin` | 동일 | 동일 | 동일 | 동일 |
| 바디 앵커 | `K-cup breasts, tiny waist, wide hips, thick thighs` | 동일 | 동일 | 동일 | 동일 |
| 공간 | Seongsu-dong officetel | bedroom | bedroom | bedroom | bedroom |
| 조명 시그니처 | warm amber LED | moonlight + 2700K lamp | moonlight + 2700K lamp | moonlight + 2700K lamp | moonlight + 2700K lamp |

진화는 허용되지만 **단절은 금지**:
- ✅ 의상 진화: 셔츠+팬티 → 셔츠 벗겨짐 → 누드 (자연스러움)
- ✅ 헤어 진화: 살짝 헝클어짐 → 베개에 흩어짐 → 얼굴 위로 → 등으로 → 이마에 붙음
- ✅ 메이크업 진화: 번짐 → 키스 자국 → 망가짐+땀 → 눈물
- ❌ 갑자기 의상이 다시 입혀지거나, 헤어가 다시 정리되면 안 됨

### E. 카메라 각도 — 모든 Scene 달라야 함

```
Scene 1: 정면 POV (front-facing, his lap POV)
Scene 2: 수평 사이드 프로필 (horizontal side profile)
Scene 3: 뒤에서 over-the-shoulder (from behind OTS, doggy style)
Scene 4: 낮은 앵글 위로 (low angle from below, cowgirl)
Scene 5: 위에서 아래로 클로즈업 (top-down close-up, missionary)
```

| 판정 | 예 |
|------|-----|
| ✅ PASS | Scene마다 명시적으로 다른 각도 지정 (`low angle`, `from behind`, `close-up from above` 등) |
| ❌ FAIL | 두 Scene 연속으로 같은 각도 사용 (예: Scene 3도 정면 POV) |

### F. Male Body Framing (Together Shots)
남성 신체가 프레임에 포함될 때의 규칙:

- **그의 얼굴은 절대 보이면 안 됨** (항상 `his face out of frame`)
- 그의 신체는 **프레임 가장자리**에만 위치 (`visible at frame edges`, `framing the scene`)
- 각 신체는 **별도 문장**으로 설명 (절대 "그들의" body 혼용 금지)
- `framing the scene` 키워드가 남성 신체 렌더링을 강제함

| 판정 | 예 |
|------|-----|
| ✅ PASS | `man's hands gripping her tiny waist visible at frame edges, his face out of frame` |
| ✅ PASS | `a man's torso and arms visible behind her gripping her waist` |
| ❌ FAIL | `they are having sex` (각 body 분리 안 됨) |
| ❌ FAIL | `his face visible in the background` (그의 얼굴 금지) |

### G. Photo Style — 통일
모든 Scene은 동일한 사진 스타일 유지:
- `shot in lo-fi amateur boyfriend photo style`
- iPhone snapshot feel
- visible grain
- 자연스러운 피부 결

| 판정 | 예 |
|------|-----|
| ✅ PASS | `shot in lo-fi amateur boyfriend photo style, ultra realistic skin texture` |
| ❌ FAIL | `cinematic studio lighting, professional photoshoot` (캐릭터 컨셉과 불일치) |

### H. Prompt 길이
- **최소 500자** 이상 (지나치게 짧으면 디테일 부족)
- 이상적인 범위: **800~1100자**
- 너무 길면(1200자+) 모델이 특정 디테일을 drop함

### I. LoRA 설정 고정

| 파라미터 | 값 |
|----------|-----|
| LoRA | NiceGirls-UltraReal-v1 / nicegirls_Zimage.safetensors |
| LoRA Scale | **0.5** (0.3 = 삽입 약함, 0.7+ = 과도함) |
| Steps | 9 |
| Guidance Scale | 1 |
| Size | 832x1216 (세로) |

---

## Prompt Pattern (P015 기준)

```
A {age}-year-old East Asian Korean beauty with delicate Korean features,
fair Korean skin, soft dark brown eyes, {face details} and {makeup},
{action grounded in current moment} on {exact location} at {time},
wearing {exact outfit}, {props and environmental details},
{atmosphere and emotional tone}, {specific lighting with colors},
shot in lo-fi amateur boyfriend photo style
```

## Consistency Pattern (Face/Body Anchor)

```
{face}: heart-shaped face, seductive fox eyes, plump kiss-swollen glossy lips
{hair}: long tangled jet-black wavy hair
{body}: massive K-cup breasts, tiny waist, wide hips, thick thighs
{skin}: milky skin, fair Korean skin
{setting}: Seongsu-dong officetel bedroom, white linen sheets, floor-to-ceiling window
{lighting}: cool blue moonlight + warm amber 2700K bedside lamp
```

## Zit-Prompt DB Reference (`/api/admin/zit-prompts`)

| 검색 키워드 | 개수 | 용도 |
|------------|------|------|
| `framing the scene` | 64 | Male body 함께 있는 together shot 패턴 참고 |
| `all fours` / `doggy` | 47 | 뒤에서 / 네발기기 자세 참고 |
| `missionary` | 26 | 선교사 자세, high angle, man's POV 패턴 |
| `cowgirl` | 1 | 위에서 타는 자세 (드뭄 — 직접 작성 필요) |
| `from behind` | 258 | 뒤에서 찍은 앵글 |
| `straddling` | 79 | 올라타는 자세 (Scene 1 참고) |
| `penetration` | 91 | 삽입 장면 prompt 구조 참고 |
| `amateur snapshot shot of a sexual act` | 2 | 전형적인 together shot 오프닝 패턴 |
| `POV shot from a man's perspective` | 1 | 남성 POV 구조 참고 |

주요 zit-prompt DB 구조적 차이:
- 한국어/동양인보다 **european** / **slavic** / **russian** 위주
- Face description이 **매우 상세** (눈동자색, 코 모양, 턱선, 주근깨 등 — 50-100단어)
- 우리 P015 패턴보다 **scene/context 디테일이 적음** — 장소/조명/소품보다 얼굴 위주
- `style` 태그: `vintage`, `film`, `phone` 위주 — 우리의 `lo-fi amateur boyfriend`와 유사

## 생성 이력

| Version | Scene | Prompt Source | Preview |
|---------|-------|---------------|---------|
| V1 | 1 — Living room | Grok freeform (no pattern) | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617113526-ji-eun-seo/image.jpg) |
| V2 | 1 — Living room | P015 pattern (generic outfit) | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617113635-ji-eun-seo/image.jpg) |
| V3 | 1 — Living room | P015 + CoT (context-grounded) ✅ | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617113900-ji-eun-seo/image.jpg) |
| Scene 2 | 2 — Bedroom | P015 + CoT + side profile angle ✅ | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617114021-ji-eun-seo-scene2/image.jpg) |
| Scene 3 | 3 — Doggy style | P015 + CoT + behind OTS ✅ | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617114448-ji-eun-seo-scene3/image.jpg) |
| Scene 4 | 4 — Cowgirl | P015 + CoT + low angle ✅ | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617114451-ji-eun-seo-scene4/image.jpg) |
| Scene 5 | 5 — Missionary | P015 + CoT + top close-up ✅ | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617114455-ji-eun-seo-scene5/image.jpg) |
| Scene 6 | 6 — Extreme close-up behind | P015 + CoT + extreme close-up butt/vagina ✅ | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617115051-ji-eun-seo-scene6/image.jpg) |
