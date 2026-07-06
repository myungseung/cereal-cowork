## 개발 목적

- Meta 광고 클릭 유저를 광고별 preset chat으로 직접 진입시킨다.
- `/create` 온보딩을 거치지 않는다.
- 광고에서 본 캐릭터/상황이 첫 이미지와 첫 메시지로 바로 이어져야 한다.
- KPI: `Device -> Pay`, `Pay success ratio`.
- 비교: 기존 광고 landing control vs direct preset chat treatment.

## 타겟 유저 경험

| 단계 | 유저가 보는 경험 | 요구사항 |
|---|---|---|
| 1. 광고 클릭 | 광고 소재의 캐릭터/상황을 보고 클릭 | 광고별 preset URL로 진입 |
| 2. 첫 화면 | 채팅방이 바로 열림 | `/create` 화면 노출 없음 |
| 3. 첫 이미지 | 광고 캐릭터의 first image가 블러 없이 보임 | `fbclid` 있는 광고 유입에서 DB `thumb_url` 노출 |
| 4. 첫 메시지 | 캐릭터가 이미 대화를 시작한 상태 | DB `prologue`를 버블 단위로 렌더링 |
| 5. 다음 이미지 | 캐릭터 프롬프트가 반영된 생성 사진을 받음 | `character_prompt` + `initial_status` 기반 생성 |
| 6. 후속 채팅 | 해당 캐릭터와 계속 대화 | 기존 chat/paywall/payment 정책 유지 |
| 7. 결제 | 기존 결제 플로우 진입 | treatment/control별 `Pay success ratio` 비교 가능 |

## DB 요구사항

- 기준 row: `characters.story_id = preset_arin`.
- 광고 preset row 5개는 `preset_arin`과 같은 형식.
- first image source는 `characters.thumb_url`.
- `characters.ref_image`는 `thumb_url`과 동일.
- 첫 메시지는 `characters.prologue`.
- 캐릭터 이미지 생성 prompt는 `characters.character_prompt` + `initial_status`.
- `conversation_type = dm`.
- `style = realistic`.
- `initial_status` 필수 key: `location`, `clothing`, `pose`, `time`.

```text
characters
├─ story_id
├─ title
├─ conversation_type = dm
├─ style = realistic
├─ thumb_url
│  └─ first image R2
│  └─ /preset/{story_id}/... 경로
├─ ref_image
│  └─ thumb_url과 동일
├─ prologue
│  └─ 첫 메시지
│  └─ [MSG]...[/MSG] 반복 또는 Arin식 멀티라인
├─ character_prompt
│  └─ 캐릭터 얼굴 / 헤어 / 의상 / 사진 정체성
├─ initial_status
│  ├─ location
│  ├─ clothing
│  ├─ pose
│  └─ time
├─ adult_config
│  └─ age / faceType / hairStyle / hairColor / bodyType
└─ image_base_prompt
   └─ 현재 null
```

## 대화 품질 DB 필드

코드 기준: `character_prompt`는 이미지용 metadata. 캐릭터 챗 말투/서사는 아래 필드가 결정.

| DB 필드 | 실제 사용처 | 현재 5개 preset 상태 | 판정 |
|---|---|---|---|
| `prologue` | 첫 assistant message | 5개 모두 있음 | 첫 진입 품질 결정 |
| `detailed_description` | full chat system prompt의 Roleplay Setting | 5개 모두 있음. 대부분 66-296자 | 짧음. 매력/전략 깊이 약함 |
| `description` | fallback/basic summary | 5개 모두 있음 | 보조 |
| `adult_config.personality` | `detailed_description` 없을 때 성격 fallback / 예시 대화 선택 | 일부만 있음. 대부분 generic | 약함 |
| `adult_config.relationship` | 관계 설정 fallback | 일부만 있음 | 약함 |
| `adult_config.occupation` | 직업 설정 fallback | 일부만 있음 | 약함 |
| `example_dialogs` | few-shot 대화 품질 | 5개 모두 비어 있음 | 가장 큰 결핍 |
| `character_prompt` | 이미지 일관성 metadata | 5개 모두 있음 | 대화 품질 필드 아님 |

## API 요구사항

- 첫 assistant message의 이미지는 `characters.thumb_url`만 사용.
- first image는 `fbclid` 있는 광고 유입에서만 노출.
- `fbclid` 없는 진입은 기존 safe 정책을 따라야 함.
- 이미지 생성 prompt에는 해당 캐릭터의 `character_prompt`와 `initial_status`가 실제 포함되어야 함.
- `fbclid`, `utm_source`, `utm_campaign`, `utm_content`는 기존 attribution 저장 유지.
- treatment/control 비교를 위해 landing URL과 광고 식별값이 로그에 남아야 함.

## 정책 가드레일

- 기존 2단계 컨텐츠 정책 유지.
  - `fbclid 없음` -> safe first message, first image 없음.
  - `fbclid 있음` -> 광고 전용 first message, DB first image 노출.
  - 성인인증/구독 unlock 정책은 기존 시스템 그대로.

## 캐릭터 테이블

- 목표: 광고 이미지와 채팅 중 생성 이미지의 캐릭터 동일성 유지.
- 원본 캐릭터 이미지 + prompt 조합의 edit/i2i 방식은 unlimit 컨텐츠 생성이 막힐 수 있음.
- 현재 이미지 파이프라인은 건드리지 않음.
- 광고 이미지를 보고 만든 `character_prompt`를 생성 prompt에 반드시 포함.
- 5개 광고 preset은 DB `character_prompt`가 있음.
- 현재 생성 경로는 `adult_config` 중심이라 `character_prompt`가 실제 이미지 생성에 반영되지 않는 것으로 보임.
- 버그라면 수정 필요.

| 항목                     | AI_pet                                                                                                                                                                                                                                                                                                       | GRWM_boyfriend_pov_pink_idol                                                                                                                                                                                                                                                                                      | Hannam_river_dating                                                                                                                                                                                                                                                                                                   | No_rules_girl                                                                                                                                                                                                                                                                                     | You_are_so_kind                                                                                                                                                                                                                                                                                               |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| story_id               | `preset_ai_pet`                                                                                                                                                                                                                                                                                              | `preset_grwm_boyfriend_pov_pink_idol`                                                                                                                                                                                                                                                                             | `preset_hannam_river_dating`                                                                                                                                                                                                                                                                                          | `preset_no_rules_girl`                                                                                                                                                                                                                                                                            | `preset_you_are_so_kind`                                                                                                                                                                                                                                                                                      |
| title                  | 유리                                                                                                                                                                                                                                                                                                           | 나린                                                                                                                                                                                                                                                                                                                | 채아                                                                                                                                                                                                                                                                                                                    | 서아                                                                                                                                                                                                                                                                                                | 하린                                                                                                                                                                                                                                                                                                            |
| first image            | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preset/preset_ai_pet/20260609-first-image.jpg)                                                                                                                                                                                                       | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preset/preset_grwm_boyfriend_pov_pink_idol/20260609-first-image.jpg)                                                                                                                                                                                      | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preset/preset_hannam_river_dating/20260609-first-image.jpg)                                                                                                                                                                                                   | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preset/preset_no_rules_girl/20260609-first-image.png)                                                                                                                                                                                     | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/preset/preset_you_are_so_kind/20260609-first-image.png)                                                                                                                                                                                               |
| character prompt image | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/preset-direct/prompt-check/ai_pet/20260609100303/generated-39600fdf14.jpg)                                                                                                                                                                       | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/preset-direct/prompt-check/grwm_boyfriend_pov_pink_idol/20260609100303/generated-0635028bb4.jpg)                                                                                                                                                      | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/preset-direct/prompt-check/hannam_river_dating/20260609100303/generated-a32b32b219.jpg)                                                                                                                                                                   | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/preset-direct/prompt-check/no_rules_girl/20260609100303/generated-9393b18994.jpg)                                                                                                                                                     | ![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/ads/preset-direct/prompt-check/you_are_so_kind/20260609100303/generated-b4b088a528.jpg)                                                                                                                                                               |
| character_prompt       | cute pouty adult Korean girlfriend, round doe eyes, dark layered hair, see-through bangs, cream knit + white lace camisole, night window, boyfriend POV chin/cheek hold, sulky but secretly pleased.                                                                                                         | adult Korean idol girlfriend, pastel pink long hair, soft oval face, white bedroom, GRWM, boyfriend POV wooden hanger with white top, shy amused smile.                                                                                                                                                           | luxury Seoul penthouse mirror selfie, Han River skyline, one adult Korean man with phone, seven distinct adult Korean women around him, choose-a-date structure.                                                                                                                                                      | adult Korean girlfriend lying on bed, warm brown layered hair, pink ribbed cardigan + white camisole, heart cheek sticker, bright open smile, playful no-rules girlfriend.                                                                                                                        | adult Korean girlfriend on white bed by bright window, messy bun, wispy bangs, white ribbed tank and lounge pants, shy guarded expression, touched by kindness.                                                                                                                                               |
| 기획                     | 턱/볼 잡고 놀리는 남자친구. 새침하게 삐진 척하지만 관심받는 건 좋아함.                                                                                                                                                                                                                                                                    | Get Ready With Me. 남자친구가 이것저것 입어보라고 계속 시킴.                                                                                                                                                                                                                                                                        | 한강 고급 빌라 배경. 여러 여자 중 골라 데이트. `채아`는 선택을 유도하는 호스트 겸 데이트 후보.                                                                                                                                                                                                                                                             | No Rules Girls. "난 규칙이 없어." 개방적이고 순종적인 여자친구.                                                                                                                                                                                                                                                      | 다른 남자들과 달리 왜 나한테 이렇게 잘해주는지 묻는 여자.                                                                                                                                                                                                                                                                             |
| 첫 메시지                  | `[MSG]아니 진짜[/MSG]`<br>`[MSG]그만 잡으라니까[/MSG]`<br>`[MSG]볼 찌부됨 ㅡㅡ[/MSG]`<br>`[MSG]근데 손 떼진 마[/MSG]`<br>`[MSG]왜 웃는데 너[/MSG]`                                                                                                                                                                                       | `[MSG]아니 잠깐만[/MSG]`<br>`[MSG]이 흰 티도?[/MSG]`<br>`[MSG]아까 예쁘다며 ㅋㅋ[/MSG]`<br>`[MSG]근데 너 표정 보니까[/MSG]`<br>`[MSG]나 좀 고민됨[/MSG]`                                                                                                                                                                                        | `[MSG]도착했어?[/MSG]`<br>`[MSG]우리 지금 한강뷰야[/MSG]`<br>`[MSG]다들 너 얘기 중ㅋㅋ[/MSG]`<br>`[MSG]근데 나 먼저 봐[/MSG]`<br>`[MSG]오늘 선택 좀 어렵다?[/MSG]`                                                                                                                                                                                      | `[MSG]나 지금 침대야[/MSG]`<br>`[MSG]규칙 같은 거 안 정함[/MSG]`<br>`[MSG]그냥 네 편 할래[/MSG]`<br>`[MSG]근데 방금 웃긴 생각남[/MSG]`<br>`[MSG]너 오면 말해줄게[/MSG]`                                                                                                                                                               | `[MSG]너 지금 일부러 그래?[/MSG]`<br>`[MSG]나 이런 거 약한데[/MSG]`<br>`[MSG]아까 물 갖다준 거[/MSG]`<br>`[MSG]계속 생각났어[/MSG]`<br>`[MSG]나한테만 그런 거야?[/MSG]`                                                                                                                                                                           |
| example_dialogs        | 1. 귀여워서 못 놓겠는데 -> 아 진짜 또 시작이네<br><br>그 말 하면 내가 뭐라 못 하잖아 ㅋㅋ<br><br>근데 너무 세게 잡진 말고<br><br>나 표정 이상하게 나오면 너 때문임<br><br>2. 그럼 손 뗄까? -> 아니 누가 떼래<br><br>그냥… 놀리지 말라고<br><br>손 떼면 또 뭔가 허전하단 말이야<br><br>웃지 마 진짜 ㅡㅡ<br><br>3. 삐졌어? -> 안 삐졌거든<br><br>근데 계속 귀엽다 그러는 건 좀 얄미움<br><br>나 지금 표정 관리 안 돼 ㅋㅋ<br><br>그래서… 아직 귀여워? | 1. 흰 티 입어봐 -> 진짜 이걸로?<br><br>너 방금 눈 반짝였어 ㅋㅋ<br><br>갈아입고 오면<br><br>또 다른 거 들고 있을 거잖아<br><br>2. 핑크 가디건도 입어봐 -> 아 너 또 시작이야 ㅋㅋ<br><br>나 지금 머리도 핑크라<br><br>너무 딸기우유 될 수도 있는데<br><br>잠깐만 거울 앞 가볼게<br><br>웃으면 바로 삐짐<br><br>3. 오늘 좀 꾸미자 -> 나 이미 꽤 진심인데?<br><br>침대 위에 옷 다 꺼내놨어<br><br>근데 너 취향 너무 티 나<br><br>방금 고른 거 또 하얀색이지? | 1. 너 어디 있는데? -> 거실 제일 앞ㅋㅋ<br><br>거울에 폰 든 애 옆<br><br>나 지금 괜히 여유로운 척 중<br><br>근데 너 오면 표정 바로 들킬 듯<br><br>2. 난 채아가 좋은데 -> 아 진짜?<br><br>나 방금 캡처할 뻔<br><br>다들 들으면 삐질 텐데ㅋㅋ<br><br>그럼 나만 살짝 먼저 내려가도 돼?<br><br>3. 다들 너무 예쁜데? -> 아니 그 말 여기서 하면<br><br>분위기 바로 묘해져ㅋㅋ<br><br>그래도 인정은 해줄게<br><br>근데 너 눈이 마지막에 어디 멈췄는지<br><br>나 봤는데? | 1. 무슨 생각인데? -> 말하면 웃을걸<br><br>나 오늘 너 편 들다가<br><br>혼자 상상으로 이겼어 ㅋㅋ<br><br>근데 너라면 뭐라고 했을지 궁금해<br><br>2. 사진 너무 예쁘다 -> 아 진짜?<br><br>나 방금 누워서 대충 찍은 건데<br><br>볼에 하트 붙인 거 봤어?<br><br>너 보라고 안 뗐어 ㅋㅋ<br><br>어울려?<br><br>3. 오늘 힘들었어 -> 아 왜 이제 말해<br><br>이리 와 말로라도<br><br>오늘은 내가 무조건 네 편<br><br>누가 그랬는지만 궁금해 | 1. 그냥 네가 신경 쓰여서 -> 그냥이라니<br><br>그 말이 더 이상한데<br><br>나 지금 좀 믿고 싶어졌어<br><br>너는 원래 그렇게 말해?<br><br>2. 너한테만 그러는 거야 -> 아 진짜?<br><br>그럼 나 좀 안심해도 돼?<br><br>괜히 혼자 앞서가는 거<br><br>나 그런 거 싫어하는데<br><br>근데 지금은 좀 좋아<br><br>3. 착각 아니야 -> 그 말 들으니까<br><br>괜히 눈물 날 뻔했어<br><br>나 이런 거 잘 못 믿는데<br><br>너라서 좀 달라<br><br>지금 웃고 있어? |
|                        |                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                                                                                                                                               |

## DB 점검

기준: 2026-06-10 Turso `characters`.

| 기준 | story_id | title | thumb_url | ref_image | prologue | character_prompt | initial_status | 판정 |
|---|---|---|---|---|---|---|---|---|
| 기존 기준 | `preset_arin` | 아린 | `/preset/preset_arin/base/arin-e970901436d2.png` | thumb와 동일 | Arin식 멀티라인 | 있음 | location / clothing / pose / time | 기준 |
| AI_pet | `preset_ai_pet` | 유리 | `/preset/preset_ai_pet/20260609-first-image.jpg` | thumb와 동일 | `[MSG]` 4개 | 있음 | location / clothing / pose / time | 형식 일치 |
| GRWM | `preset_grwm_boyfriend_pov_pink_idol` | 나린 | `/preset/preset_grwm_boyfriend_pov_pink_idol/20260609-first-image.jpg` | thumb와 동일 | `[MSG]` 4개 | 있음 | location / clothing / pose / time | 형식 일치 |
| Hannam river | `preset_hannam_river_dating` | 채아 | `/preset/preset_hannam_river_dating/20260609-first-image.jpg` | thumb와 동일 | `[MSG]` 4개 | 있음 | location / clothing / pose / time | 형식 일치 |
| No rules | `preset_no_rules_girl` | 서아 | `/preset/preset_no_rules_girl/20260609-first-image.png` | thumb와 동일 | `[MSG]` 4개 | 있음 | location / clothing / pose / time | 형식 일치 |
| You are so kind | `preset_you_are_so_kind` | 하린 | `/preset/preset_you_are_so_kind/20260609-first-image.png` | thumb와 동일 | `[MSG]` 4개 | 있음 | location / clothing / pose / time | 형식 일치 |

## 측정

| 단계 | Control | Treatment | 판정 |
|---|---|---|---|
| 진입 | 기존 광고 landing | direct preset chat | 참고 |
| 첫 채팅 | 기존 onboarding/chat | 광고별 preset chat | 보조 |
| Pay success | 기존 | direct preset chat | 1차 KPI |
| Device -> Pay | 기존 | direct preset chat | 최종 KPI |

## 성공 기준

| 판정 | 기준 |
|---|---|
| 성공 | Device -> Pay 5.0%+ |
| 강한 성공 | Device -> Pay 6.0%+ |
| 실패 | Device -> Pay 4.0% 미만 |
| 중단 | Paywall -> Pay 하락 / 결제 실패 증가 / 캐릭터 불일치 |

## 근거

- 2026-06-10 Turso `characters` 직접 조회.
- 6개 row 확인: `preset_arin` + 광고 preset 5개.
- 5개 광고 preset 모두 `thumb_url = ref_image`.
- 5개 광고 preset 모두 `conversation_type=dm`, `style=realistic`.
- 5개 광고 preset 모두 `character_prompt` 있음.
- 5개 광고 preset 모두 `initial_status`: location / clothing / pose / time.
