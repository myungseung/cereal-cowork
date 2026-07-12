| 항목 | 값 |
|---|---|
| 목적 | 현재 제품 Chat 3턴 -> 턴별 Gallery T2I 이미지 확인 |
| 상태 | 유저 판정 대기 |
| 기준일 | 2026-07-12 KST |
| 캐릭터 | `user_1783675965445_yqodif` |
| 테스트 device | `gallery_t2i_chat_20260712_v2` |
| 제품 Chat | main `http://localhost:3000` / `contentMode=full` |
| 기준 캐릭터 manifest | [열기](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/profile/preview_1783700936593/manifest.json) |
| 이미지 모델 | `prunaai/z-image-turbo-lora` |
| 고정 seed | `4170581481` |
| LoRA | NiceGirls `0.5` + Hyper-SD `0.2` + Mystic `0.3` |
| 예상 이미지 비용 | 3장 합계 `$0.0207` |

초기 메시지: **지은:** 안녕, 나야. 재혼 가정 여동생. 뭐 해?

| 턴 | 제품 Chat 로그 | 생성 장면 / 이미지 | 유저 판정 |
|---:|---|---|---|
| 1 | **유저:** 질투하는 거야?<br><br>**지은:** 질투라니, 내가? 🙄<br>말 예쁘게 안 하면 오빠 방 가서 혼내줄 거야.<br>이따가 내 방으로 잠깐 들어올 수 있어? 🖤<br><br>`trace_1783841479335_r766xh` · Chat `1.887s` | Gallery `1484` · 문을 잠근 뒤 뒤돌아보는 over-shoulder<br><br><img src="https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-chat-gallery-t2i-turn-test/1783841664038-product-chat-3turn-gallery-t2i/turn-1/image.jpg" width="420"><br><br>[원본 이미지](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-chat-gallery-t2i-turn-test/1783841664038-product-chat-3turn-gallery-t2i/turn-1/image.jpg) · [manifest](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-chat-gallery-t2i-turn-test/1783841664038-product-chat-3turn-gallery-t2i/turn-1/manifest.json)<br>생성 `4.489s` | **미판정**<br>PASS / FAIL |
| 2 | **유저:** 그냥 아는 동생일 뿐이야. 그만해.<br><br>**지은:** 오빠 지금 나랑 거리 두는 거야?<br>그 동생이 그렇게 예뻐? 흥, 나보다 더? 😒<br>나 진짜 화나기 직전인데... 이리 와서 직접 풀어줘.<br><br>`trace_1783841480555_7h1du9` · Chat `0.915s` | Gallery `1486` · 20도 dutch angle / 오른쪽 1/3 배치<br><br><img src="https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-chat-gallery-t2i-turn-test/1783841664038-product-chat-3turn-gallery-t2i/turn-2/image.jpg" width="420"><br><br>[원본 이미지](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-chat-gallery-t2i-turn-test/1783841664038-product-chat-3turn-gallery-t2i/turn-2/image.jpg) · [manifest](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-chat-gallery-t2i-turn-test/1783841664038-product-chat-3turn-gallery-t2i/turn-2/manifest.json)<br>생성 `7.377s` | **미판정**<br>PASS / FAIL |
| 3 | **유저:** 불안해하는 네가 귀여워서 그래.<br><br>**지은:** 귀엽다고? 장난쳐 지금? 😠<br>오빠가 그러니까 더 화나는 거 알지...<br>근데... 사실 방금 오빠한테 들켜서 가슴 엄청 뛰어.<br>손끝 떨리는 거 보여? 앗, 방금 허리 쪽에 손이 스쳤는데... 😳<br><br>`trace_1783841481669_u42p4f` · Chat `0.986s` | Gallery `1485` · 얼굴 중심 close-up / 손끝 긴장<br><br><img src="https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-chat-gallery-t2i-turn-test/1783841664038-product-chat-3turn-gallery-t2i/turn-3/image.jpg" width="420"><br><br>[원본 이미지](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-chat-gallery-t2i-turn-test/1783841664038-product-chat-3turn-gallery-t2i/turn-3/image.jpg) · [manifest](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-chat-gallery-t2i-turn-test/1783841664038-product-chat-3turn-gallery-t2i/turn-3/manifest.json)<br>생성 `10.253s` | **미판정**<br>PASS / FAIL |

## 근거

- 제품 Chat raw 요청/응답: `/Users/cheonmyeongseung/cereal-ai/.agents/runtime/video-chat-gallery-t2i-turn-test/chat-log.json`
- 장면 계획: `/Users/cheonmyeongseung/cereal-ai/.agents/runtime/video-chat-gallery-t2i-turn-test/scene-plans.json`
- 생성 결과: `/Users/cheonmyeongseung/cereal-ai/.agents/runtime/video-chat-gallery-t2i-turn-test/result.json`
- AskAgent 장면 설계: [세션](http://127.0.0.1:3030/?session=2026-07-12T07-33-04-361Z-gallery-t2i-product-chat-3turns)
- 판정 권한: 유저만 PASS / FAIL 확정
