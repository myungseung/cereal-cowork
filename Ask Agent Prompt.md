## 고정 요청 형식

- 목적: P Image Edit용 9:16 emotion highlight frame prompt 생성
- 사용처: `askAgent` 호출 전 원문 템플릿
- 원칙: 아래 형식 유지. 한국어 축약 요청 금지. 이미지 URL은 반드시 `-i` 첨부.

## Template

We need final P Image Edit prompts for [N] 9:16 image assets from the attached exact source image. Generate prompts for assets: [asset number + asset name list]. Each must be grounded in a specific chat turn so the emotion is explained by the conversation, not just a generic pose.

Source image constraints: preserve exact bedroom structure: same bed position, same window/curtain, same shelf, same desk, same wall/floor relationship, same room depth. Camera may shift only slightly. Preserve identity, hairstyle, outfit, lighting, proportions. Full-bleed realistic photo. No border, white edge, frame, outline, poster, picture-in-picture, text, logo, symbol, sweatdrop, emoji mark, colored mark. Avoid words 'blush' and 'blushing'. Do not redesign room.

Use realistic body language only. Prefer actions feasible from the source image and using existing outfit/room. Examples of chat triggers:
- User says: '[chat trigger 1]'
- User says: '[chat trigger 2]'
- User says: '[chat trigger 3]'

For each asset, first decide the chat turn and action, then output exactly one final English P Image Edit prompt. Labels required: [LABEL_1_PROMPT], [LABEL_2_PROMPT], [LABEL_3_PROMPT]. No extra explanation after prompts.

images:
- https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/manual/idle_next_reference_a_first_f2857afde2.png

## 성공 예시: 13-15

We need final P Image Edit prompts for three 9:16 image assets from the attached exact source image. Generate prompts for assets: 13 fidgeting_shyly, 14 embarrassed, 15 flustered. Each must be grounded in a specific chat turn so the emotion is explained by the conversation, not just a generic pose.

Source image constraints: preserve exact bedroom structure: same bed position, same window/curtain, same shelf, same desk, same wall/floor relationship, same room depth. Camera may shift only slightly. Preserve identity, hairstyle, outfit, lighting, proportions. Full-bleed realistic photo. No border, white edge, frame, outline, poster, picture-in-picture, text, logo, symbol, sweatdrop, emoji mark, colored mark. Avoid words 'blush' and 'blushing'. Do not redesign room.

Use realistic body language only. Prefer actions feasible from the source image and using existing outfit/room. Examples of chat triggers:
- User teases her: '왜 갑자기 손이 가만히 못 있어? 긴장했어?'
- User says: '방금 말 진심이야? 나한테 그렇게 말하면 설레잖아.'
- User says: '지금 당황했지? 표정 다 보이는데.'

For each asset, first decide the chat turn and action, then output exactly one final English P Image Edit prompt. Labels required: [FIDGETING_SHYLY_PROMPT], [EMBARRASSED_PROMPT], [FLUSTERED_PROMPT]. No extra explanation after prompts.

images:
- https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/manual/idle_next_reference_a_first_f2857afde2.png

## 실행 규칙

- `askAgent`에는 위 요청문을 그대로 전달.
- `-i`에 source image URL 첨부.
- 실패 이미지 분석 재요청도 같은 구조 유지.
- 최종 이미지 생성 prompt는 askAgent 출력만 사용.
- Codex가 직접 final P Image Edit prompt 작성 금지.
