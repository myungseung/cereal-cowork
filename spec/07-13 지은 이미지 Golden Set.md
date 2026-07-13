## 목표

- 제품 Chat 3턴의 맥락 이미지 Golden Set
- 판정: 2026-07-13 사용자
- 결과: 3/3 PASS

## 판정

- 🟢 PASS
- 🔴 FAIL

## 실험 데이터

| 턴 | 대화 맥락 | 장면 | Raw prompt | 결과 | 판정 |
|---:|---|---|---|---|---|
| 1 | 질투 확인 후 방으로 부름 | 문을 잠근 뒤 돌아보는 over-shoulder | [[#^raw-prompt-1\|보기]] | ![1턴 이미지](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-chat-gallery-t2i-turn-test/1783841664038-product-chat-3turn-gallery-t2i/turn-1/image.jpg)<br>[manifest](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-chat-gallery-t2i-turn-test/1783841664038-product-chat-3turn-gallery-t2i/turn-1/manifest.json) | 🟢 PASS |
| 2 | 거리 두는 답변에 질투·분노 | 20도 dutch angle / 오른쪽 1/3 | [[#^raw-prompt-2\|보기]] | ![2턴 이미지](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-chat-gallery-t2i-turn-test/1783841664038-product-chat-3turn-gallery-t2i/turn-2/image.jpg)<br>[manifest](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-chat-gallery-t2i-turn-test/1783841664038-product-chat-3turn-gallery-t2i/turn-2/manifest.json) | 🟢 PASS |
| 3 | 들킨 불안과 긴장 | 얼굴 중심 close-up / 손끝 긴장 | [[#^raw-prompt-3\|보기]] | ![3턴 이미지](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-chat-gallery-t2i-turn-test/1783841664038-product-chat-3turn-gallery-t2i/turn-3/image.jpg)<br>[manifest](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-chat-gallery-t2i-turn-test/1783841664038-product-chat-3turn-gallery-t2i/turn-3/manifest.json) | 🟢 PASS |

## Raw prompts

> [!info]- 1턴 Raw prompt
> `A beautiful adult 20-year-old Korean woman with a refined Korean actress and off-duty idol face, soft dark brown eyes, glossy natural lips, long black ponytail with loose flyaway strands, fit hourglass figure, wearing a deep navy fitted ribbed sleeveless knit top and a high-waisted ivory tailored skirt, alone inside an empty daycare classroom in the late afternoon with no children present. Invisible iPhone RAW candid POV with the camera behind her, over her shoulder, as she has just locked the classroom door and turned back to make purposeful eye contact with the camera, completing the motion. Relationship-hook tension conveyed only through confident expression and posture, asymmetrical silhouette, strong clothing contrast, warm sunset rim light outlining her hair and shoulders, natural classroom details, realistic skin texture, subtle motion in loose hair, premium cinematic realism.`
^raw-prompt-1

> [!info]- 2턴 Raw prompt
> `A beautiful adult 20-year-old Korean woman with a refined Korean actress and off-duty idol face, soft dark brown eyes, glossy natural lips, long black ponytail with loose flyaway strands, fit hourglass figure, wearing a deep navy fitted ribbed sleeveless knit top and a high-waisted ivory tailored skirt, alone in an empty daycare classroom during late afternoon with no children present. Invisible iPhone RAW candid POV using a dynamic 20-degree Dutch angle, her face positioned on the right third of the frame with strong diagonal composition, captured immediately after she steps closer and fixes purposeful eye contact with the camera. Relationship-hook atmosphere, asymmetrical silhouette, striking navy and ivory outfit contrast, glowing sunset rim light, natural classroom environment, lifelike texture, cinematic candid realism.`
^raw-prompt-2

> [!info]- 3턴 Raw prompt
> `Face-dominant close-up of a beautiful adult 20-year-old Korean woman with a refined Korean actress and off-duty idol face, soft dark brown eyes, glossy natural lips, long black ponytail with delicate flyaway strands, fit hourglass figure, wearing a deep navy fitted ribbed sleeveless knit top and a high-waisted ivory tailored skirt, alone inside an empty daycare classroom in the late afternoon with no children present. Invisible iPhone RAW candid POV capturing the completed moment after she quietly raises her fingertips into view, emphasizing subtle fingertip tension while maintaining purposeful eye contact with the camera. Relationship-hook feeling, asymmetrical framing, elegant outfit contrast, warm sunset rim light across her hair and profile, highly realistic skin detail, cinematic candid photography.`
^raw-prompt-3
