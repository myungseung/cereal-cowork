## 목표

- Golden set: [Toptoon session 1006749](https://chat.toptoon.com/chat/session/1006749)
- 목표: 제공된 first frame으로 원본 동영상 장면을 지은 캐릭터 버전으로 재현
- 현재 상태: 원본 에셋 14개 수집 / 생성 미실행
- CoT: 재현 가능한 장면 설계 근거만 기록
- first frame url: 다른 에이전트 제공. 동영상 생성의 첫 프레임으로 사용
- 결과 video url: first frame 기반 생성 결과

## 판정

- ⚪ 판단 필요
- 🟢 성공
- 🔴 실패

## 실험 데이터

| 장면 | CoT | first frame url | 원본 url | 결과 video url | 판정 |
|---|---|---|---|---|---|
| 일상 1 · 라커룸 벤치에 앉아 카메라를 향해 상체를 살짝 숙이며 미소 짓는 하이앵글 구도 · 동영상 · 4.29초 · 832×1216 |  |  | [원본 MP4](https://showcase.chat.toptoon.com/character/290/album/51cd26b1-815a-4383-b624-8957c127fb77.mp4?key=fd237ec5dc692b8c238e056436b0dd02&time=1783931400) |  | ⚪ |
| 일상 2 · 라커룸에 서서 카메라를 올려다보며 눈을 깜빡이는 하이앵글 구도 · 동영상 · 4초 · 832×1216 |  |  | [원본 MP4](https://showcase.chat.toptoon.com/character/290/album/ed8fdab5-08af-49b2-bbf9-c9b6c4ec2424.mp4?key=9146165ba184cb9a6aedc277f77a68ae&time=1783931400) |  | ⚪ |
| 일상 3 · 허리에 손을 얹고 웃다가 카메라 앞으로 가까이 다가온 뒤 다시 물러나는 정면 구도 · 동영상 · 6.04초 · 832×1216 |  |  | [원본 MP4](https://showcase.chat.toptoon.com/character/290/album/f45c5502-5312-42ab-be73-593c8f599bb3.mp4?key=0486b11a820ee305ccc1b476dee696bb&time=1783931400) |  | ⚪ |
| 일상 4 · 라커룸에 서서 한 손으로 머리카락을 쓸어넘기며 시선을 맞추는 정면 구도 · 동영상 · 4.04초 · 832×1216 |  |  | [원본 MP4](https://showcase.chat.toptoon.com/character/290/album/86e345d3-91ce-4172-9b17-c1039963e486.mp4?key=55155f7b88d67cff92bc6b9dafb69f6d&time=1783931400) |  | ⚪ |
| 일상 5 · 라커룸 테이블에 기대어 손가락으로 OK 사인을 만들고 혀를 살짝 내미는 장난스러운 포즈 · 동영상 · 5.04초 · 832×1216 |  |  | [원본 MP4](https://showcase.chat.toptoon.com/character/290/album/877eed15-5f8d-4d01-b701-7fb5f2ab9073.mp4?key=341280c5987aede180eae1294780a8c4&time=1783931400) |  | ⚪ |
| 일상 6 · 땀에 젖은 채 라커룸 벤치에 앉아 수건으로 얼굴과 목을 닦아내는 측면 구도 · 동영상 · 6.04초 · 832×1216 |  |  | [원본 MP4](https://showcase.chat.toptoon.com/character/290/album/7a06b342-69c9-489c-bf9b-779aafb0e342.mp4?key=6af608416810a93e991207c6e27b9546&time=1783931400) |  | ⚪ |
| 프리미엄 1 · 이미지 |  |  | [원본 JPG](https://showcase.chat.toptoon.com/character/290/album/22d18d96-ddc5-4a0a-a045-37253a994200.jpg?f=webp&resize=p_4,w_1200&key=04f08cbd35585ee41bab36a4c24cf9bf&time=1783931400) |  | ⚪ |
| 프리미엄 2 · 이미지 |  |  | [원본 JPG](https://showcase.chat.toptoon.com/character/290/album/418a476f-2570-481d-9983-0a3245e5fb07.jpg?f=webp&resize=p_4,w_1200&key=351604cd71f38ee0fcac9c342e4624ee&time=1783931400) |  | ⚪ |
| 프리미엄 3 · 이미지 |  |  | [원본 JPG](https://showcase.chat.toptoon.com/character/290/album/fd9683b5-b75f-4b24-993e-b0deafd9214c.jpg?f=webp&resize=p_4,w_1200&key=859824e43a2c225b8cba64278fe583aa&time=1783931400) |  | ⚪ |
| 프리미엄 4 · 이미지 |  |  | [원본 JPG](https://showcase.chat.toptoon.com/character/290/album/092c24a4-4dda-41df-8a99-e318206d8c47.jpg?f=webp&resize=p_4,w_1200&key=060e647137cbf9f89493addd844b0687&time=1783931400) |  | ⚪ |
| 프리미엄 5 · 이미지 |  |  | [원본 JPG](https://showcase.chat.toptoon.com/character/290/album/3aa45467-e32b-4043-b640-6cbf9145f930.jpg?f=webp&resize=p_4,w_1200&key=03df87ed78703a2eb4a236d340ab205b&time=1783931400) |  | ⚪ |
| 로얄 1 · 이미지 |  |  | [원본 AVIF](https://showcase.chat.toptoon.com/character/290/album/4ccee33f-7552-43d7-86a0-c9e44d5154e8.avif?key=9cf024e8d84b86b2946948e8df6dec1a&time=1783931400) |  | ⚪ |
| 로얄 2 · 이미지 |  |  | [원본 AVIF](https://showcase.chat.toptoon.com/character/290/album/b4249279-ad6a-4681-bb23-53dc4c19f1a3.avif?key=44655ebcdbd0a26694dda2375a076fd0&time=1783931400) |  | ⚪ |
| 로얄 3 · 야구장 관중석을 배경으로 밝게 웃으며 엄지를 치켜세우는 정면 구도, 약한 카메라 패닝 · 동영상 · 5.125초 · 832×1216 · 무음 loop |  |  | [원본 MP4](https://showcase.chat.toptoon.com/character/290/video-thumbnail/03f0c84f-3536-4c92-9bd0-a7a067267451.mp4) |  | ⚪ |

## 근거

- 확인일: 2026-07-13
- 컬렉션: 14개 = 콘텐츠 동영상 7개 + 이미지 7개
- 동영상 분석: askAgent Gemini Pro
- 직접 확인: `ffmpeg` 0.5초 프레임 추출 후 contact sheet 확인
- 제외: `fx-premium.webm`, `fx-royal.webm` = 등급 전환 UI 효과
