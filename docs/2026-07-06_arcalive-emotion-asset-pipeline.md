- parent experiment: [[Experiments/06-30 동영상 대화로 애정 만들기]]
- source: 감정 에셋 프리셋 72종 JSON
- SOT: 35개 image/video asset 생성 리스트
- 기준: `p-image-edit` image 확정 -> p-video first -> fail 시 Kling backfill

## SOT

| asset | asset | asset | asset | asset |
|---|---|---|---|---|
| ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/arca-preset-first-example/playful-wink/20260706114007/p-image-edit.jpg)<br>**1. playful_wink**<br>[manifest](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/arca-preset-first-example/playful-wink/20260706114007/p-image-edit-manifest.json) | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/arca-emotion-assets/smile/20260706122102/image.jpg)<br>**2. smile**<br>[image](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/arca-emotion-assets/smile/20260706122102/image.jpg)<br>[manifest](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/arca-emotion-assets/smile/20260706122102/manifest.json) | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/2026-07-04T06-23-34-835Z-reaction_flattered_smile_manual_a_user_b_final-5ff96371/reaction_flattered_smile_manual_a_user_b_final-last-frame.jpg)<br>**3. happy_smile**<br>[video](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/2026-07-04T06-23-34-835Z-reaction_flattered_smile_manual_a_user_b_final-5ff96371/reaction_flattered_smile_manual_a_user_b_final.mp4)<br>[video manifest](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/2026-07-04T06-23-34-835Z-reaction_flattered_smile_manual_a_user_b_final-5ff96371/reaction_flattered_smile_manual_a_user_b_final-manifest.json)<br>[image manifest](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/arca-emotion-assets/happy_smile/20260706123247/manifest.json) | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/arca-emotion-assets/joyful/20260706124755/image.jpg)<br>**4. joyful**<br>[image](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/arca-emotion-assets/joyful/20260706124755/image.jpg)<br>[manifest](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/arca-emotion-assets/joyful/20260706124755/manifest.json) | image:<br>**5. giggling**<br>manifest: |
| image:<br>**6. laughing**<br>manifest: | image:<br>**7. cozy**<br>manifest: | image:<br>**8. relieved**<br>manifest: | image:<br>**9. admiring**<br>manifest: | image:<br>**10. lovestruck**<br>manifest: |
| image:<br>**11. blushing_shyly**<br>manifest: | image:<br>**12. looking_away_shyly**<br>manifest: | image:<br>**13. fidgeting_shyly**<br>manifest: | image:<br>**14. embarrassed**<br>manifest: | image:<br>**15. flustered**<br>manifest: |
| image:<br>**16. full_face_blush**<br>manifest: | image:<br>**17. surprised**<br>manifest: | image:<br>**18. shocked**<br>manifest: | image:<br>**19. stupefied**<br>manifest: | image:<br>**20. curious**<br>manifest: |
| image:<br>**21. thinking**<br>manifest: | image:<br>**22. eureka**<br>manifest: | image:<br>**23. sad**<br>manifest: | image:<br>**24. crying_with_eyes_open**<br>manifest: | image:<br>**25. crying_with_eyes_closed**<br>manifest: |
| image:<br>**26. happy_tears**<br>manifest: | image:<br>**27. worried**<br>manifest: | image:<br>**28. pleading**<br>manifest: | image:<br>**29. annoyed**<br>manifest: | image:<br>**30. pouting**<br>manifest: |
| image:<br>**31. aroused**<br>manifest: | image:<br>**32. lustful**<br>manifest: | image:<br>**33. seductive_smile**<br>manifest: | image:<br>**34. acting_coy**<br>manifest: | image:<br>**35. taunting**<br>manifest: |

## History

- 2026-07-06: T2I 테스트 폐기. 배경/구도 변경.
- 2026-07-06: `z-image-turbo-img2img` 테스트 무효. edit 모델 아님.
- 2026-07-06: `playful_wink` image B-cut 생성. video loop는 first frame 기준 오류로 미채택.
- 2026-07-06: clean first frame 기준 재생성은 `peace sign` artifact로 미채택. [manifest](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/arca-preset-first-example/playful-wink-clean/20260706123733/p-image-edit-clean-manifest.json)

## 규칙

- 72개 일괄 처리 금지.
- 1개 preset씩 `p-image-edit` 검증.
- image 성공 후 video preview가 생성되면 grid 셀은 image preview 대신 video preview 표시.
- video는 p-video first.
- Kling은 p-video fail backfill에서만 사용.
- prompt는 preset 원문 태그만 사용.
- agent prompt rewrite 금지.
- 모든 결과: R2 image/video + R2 manifest + Lab record 저장.
