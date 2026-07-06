구현 확인: [[docs/06-24 character-chat-v16 admin API handoff]]

| 항목 | 값 |
| --- | --- |
| Test | `profile_image_live_vs_v16` |
| Type | spec-driven XY test cases |
| Stage | character image |
| Status | passed / live A/B 연결 대기 |
| 범위 | 캐릭터 프로필 이미지 |
| 제외 | 첫 메시지 / 대화 / 대화 맥락 이미지 |

## Goal

- 같은 유저 input으로 live와 v16 전체 체인 비교.
- 비교 단위: 유저 input -> prompt 변환 -> image payload -> final image.
- 한 row 안에서 control/treatment XY 비교.

## Arms

| Arm | 내용 |
| --- | --- |
| control | 현재 live `/api/preview-image` |
| treatment | v16 character card -> `imageBasePrompt` -> LoRA image |

## Test Rule

- frozen baseline 먼저 고정.
- 새 실험은 오른쪽 column/row로 추가.
- 모델 변경, LoRA 변경, prompt 변경은 각각 별도 독립변수.
- 실제 이미지 URL 없는 row는 pass 금지.
- chat context 결과를 이 실험에 섞지 않음.

## 검증 기준

| 단계 | control | treatment |
| --- | --- | --- |
| 유저 input | 동일 | 동일 |
| prompt 변환 | live prompt builder 결과 | v16 character card + `imageBasePrompt` |
| image payload | live model payload | v16 LoRA payload |
| final image | live image URL | v16 image URL |

## 근거

- caseId: `character_image_profile_image_live_vs_v16_1782122738818`
- baseline run: `character_image_profile_image_live_vs_v16_1782122738818_baseline_1782130552700.json`
- variant run: `character_image_profile_image_live_vs_v16_1782122738818_variant_1782130552754.json`
- user verified: 10-row live vs v16 profile image test passed.
- admin API 확인: `POST /api/admin/character-chat-v16/character` 200.
- admin API 확인: `POST /api/admin/character-chat-v16/image` 200.
