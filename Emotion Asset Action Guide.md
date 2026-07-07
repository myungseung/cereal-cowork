## 원칙

- 감정명 단독 금지: `surprised`, `curious`, `embarrassed`만으로 생성하지 않음.
- 기본 단위: `대화 턴 -> 구체 동작 -> 포즈/표정/각도 -> 이미지`.
- 목표: 정지 포트레이트가 아니라 7-10초 video-chat 중간 하이라이트 프레임.
- 공간: 같은 침실 안에서 실제로 움직이는 느낌.
- 실패 패턴: 정면 standing reaction, shy/embarrassed 반복, 흰 outline/sticker/cutout.

## 네이밍 규칙

| bad | good |
|---|---|
| `surprised` | `turning_back_to_camera` |
| `shocked` | `caught_mid_step` |
| `curious` | `lean_in_listening` |
| `stupefied` | `reaching_toward_camera` |
| `laughing` | `laughing_turn_away` |

## 생성 전 질문

- 이 프레임 직전 유저가 무슨 말을 했나?
- 캐릭터가 몸으로 무엇을 하고 있나?
- 침대/창문/책상/선반 중 공간 요소가 어떻게 살아 있나?
- 11-15 shy 계열과 무엇이 다른가?
- 정면 standing 표정 변화만으로 끝나지 않는가?

## 16-20 교체 후보

| priority | asset | chat trigger | action | 차이 |
|---|---|---|---|---|
| 1 | `reaching_toward_camera` | `Wait, let me show you.` | 한 손을 카메라 쪽으로 뻗음 | video-chat depth, 화면과 직접 상호작용 |
| 2 | `lean_in_listening` | `Wait, what did you say?` | 상체를 카메라 쪽으로 기울임 | 물러남이 아니라 적극적 접근 |
| 3 | `turning_back_to_camera` | `Oh—hold on.` | 책상/창문 쪽을 보다가 어깨 너머로 돌아봄 | 몸 회전, 방이 배경이 아니라 장면 |
| 4 | `caught_mid_step` | `I'm coming back.` | 방 안에서 한 걸음 움직이다 멈춤 | stationary pose가 아니라 이동 중 포착 |
| 5 | `laughing_turn_away` | unexpected funny line | 웃으며 몸을 돌렸다가 다시 돌아오는 순간 | 표정 웃음보다 전환 동작 중심 |

## Prompt 지침

- askAgent에는 [[Ask Agent Prompt]] 형식 유지.
- 먼저 action-based asset name을 확정.
- 그 다음 P Image Edit final prompt 요청.
- Codex가 final prompt 직접 작성 금지.
- 금지: border, white edge, frame, outline, poster, picture-in-picture, text, logo, symbol, sweatdrop, emoji mark, colored mark.
- 금지: `blush`, `blushing`, cheek/face color instruction.

## 근거

- askAgent log: `/Users/cheonmyeongseung/cereal-ai/.agents/skills/askAgent/logs/2026-07-07T01-08-17-102Z-askagent.md`
- source image: https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preview/video-chat-lab/manual/idle_next_reference_a_first_f2857afde2.png
