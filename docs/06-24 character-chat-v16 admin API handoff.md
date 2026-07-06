| 항목     | 값                                    |
| ------ | ------------------------------------ |
| 상태     | main 기준 admin API 이식 완료              |
| 목적     | 프로필 사진 A/B treatment 준비              |
| 범위     | 프로필 사진 생성                            |
| 제외     | 대화 생성 / image-prompt API / 대화 맥락 이미지 |
| branch | `codex/v16-admin-api-main`           |
| commit | `fb6b40c`                            |

## 추가 API

```txt
POST /api/admin/character-chat-v16/character
POST /api/admin/character-chat-v16/image
```

## SOT

| 구분 | 경로 |
| --- | --- |
| 원본 | `/Users/cheonmyeongseung/cereal-ai-worktrees/codex-character-chat-v16-quality/app/api/admin/character-chat-v16/character/route.ts` |
| 원본 | `/Users/cheonmyeongseung/cereal-ai-worktrees/codex-character-chat-v16-quality/app/api/admin/character-chat-v16/image/route.ts` |
| main 이식 | `/Users/cheonmyeongseung/cereal-ai-worktrees/v16-admin-api-main/app/api/admin/character-chat-v16/character/route.ts` |
| main 이식 | `/Users/cheonmyeongseung/cereal-ai-worktrees/v16-admin-api-main/app/api/admin/character-chat-v16/image/route.ts` |

## 프로필 이미지 플로우

```txt
user input (normalized selection JSON)
→ character API
→ character.imageBasePrompt
→ image API
→ z-image-turbo-lora + LoRA
→ R2 image / manifest
```

## 기준 유저 input

검증은 최종 프롬프트가 아니라 유저 input에서 시작한다.

```json
{
  "fixtureId": "preset_college_noona",
  "displayName": "수연",
  "source": "selection-json",
  "profile": {
    "gender": "female",
    "style": "realistic",
    "ethnicity": "asian Korean",
    "age": 24,
    "faceType": "first_love",
    "hairStyle": "goddess_wave",
    "hairColor": "brunette",
    "eyeColor": "brown",
    "bodyType": "average",
    "seed": 1704067201
  },
  "expanded": {
    "faceType": "pure innocent face, minimal to no makeup, porcelain white flawless skin, natural beauty, girl-next-door charm, gentle warm expression",
    "hairStyle": "voluminous glamorous brown waves, bouncy goddess curls, luxurious elegant hair",
    "hairColor": "brown",
    "bodyType": "realistic average body type, natural normal proportions, relatable everyday figure"
  }
}
```

## 검증 비교 기준

| 단계 | control | treatment |
| --- | --- | --- |
| 유저 input | 동일 normalized selection | 동일 normalized selection |
| prompt 변환 | live prompt builder | character API -> `character.imageBasePrompt` |
| image payload | live image model payload | v16 LoRA image payload |
| final image | live image URL | v16 image URL |

검증 실패:

- 최종 이미지만 비교.
- treatment prompt만 비교.
- 유저 input 없이 payload만 재실행.
- character card 중간 산출물 미기록.

## live와 다른 점

| 항목           | live current            | v16 treatment                           |
| ------------ | ----------------------- | --------------------------------------- |
| 단계           | 1-step                  | 2-step                                  |
| 중간 산출물       | 없음                      | character card                          |
| 이미지 프롬프트     | live prompt builder     | `character.imageBasePrompt`             |
| prompt model | live 설정                 | `deepseek/deepseek-v4-flash` 가능         |
| image model  | `prunaai/z-image-turbo` | `prunaai/z-image-turbo-lora:197b2db...` |
| LoRA         | 없음                      | NiceGirls + Hyper-SD + Mystic           |
| size         | 768x1024                | 832x1216                                |

## 검증

| Check | Result |
| --- | --- |
| `npx tsc --noEmit --pretty false` | pass |
| `POST /api/admin/character-chat-v16/character` | 200 |
| `POST /api/admin/character-chat-v16/image` | 200 |
| R2 image URL | 200 |
| R2 manifest URL | 200 |

## 산출물

- image: `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/character-chat-v16/20260624034204/image.jpg`
- manifest: `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/character-chat-v16/20260624034204/manifest.json`

## 다음

- live `/api/preview-image` A/B treatment 연결.
- treatment는 admin API 로직 재사용.
- control 변경 금지.
- QA force param:
  - `?imagePipelineVariant=control`
  - `?imagePipelineVariant=v16_lora`
- 같은 유저 input에서 시작해 control/treatment 전체 체인 row 비교.

## 금지

- admin API prompt 요약 재작성 금지.
- live control 교체 금지.
- chat/image-prompt/context-image 범위 혼합 금지.
- mock image pass 금지.
