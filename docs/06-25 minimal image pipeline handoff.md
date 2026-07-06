| 항목     | 값                                                                  |
| ------ | ------------------------------------------------------------------ |
| 상태     | CLI 파이프라인 작동 확인                                                    |
| 목적     | create 온보딩 선택값으로 5초 이내 첫 이미지 반환                                    |
| 범위     | 온보딩 selection -> Grok prompt 변환 -> Replicate -> R2 -> manifest     |
| 제외     | 앱 화면 / DB 저장 / 채팅 / NSFW 필터                                        |
| branch | `feat/minimal-image-pipeline-cli`                                  |
| 실행 파일  | `/Users/cheonmyeongseung/cereal-ai/bin/minimal-image-pipeline.mjs` |

## 실행

```bash
pnpm image:pipeline
pnpm image:pipeline -- --json
pnpm image:pipeline -- --input ./selection.json
```

## 기본 input

create 온보딩의 label이 아니라 normalized value 사용.

```json
{
  "faceType": "insta_goddess",
  "hairStyle": "goddess_wave",
  "bodyType": "hourglass",
  "relationship": "roommate",
  "occupation": "influencer",
  "personality": "seductive",
  "secret": "secret_account"
}
```

## Selection -> Grok -> Prompt

1차 deterministic seed:

| selection | seed prompt 조각 |
|---|---|
| `faceType: insta_goddess` | `glamorous Gangnam influencer face, polished makeup, sharp eyebrows, refined Seoul beauty` |
| `hairStyle: goddess_wave` | `voluminous glossy goddess waves` |
| `bodyType: hourglass` | `hourglass figure, slim waist, wide hips` |
| `occupation: influencer` | `Korean fashion influencer, sponsored photo shoot, luxury cafe in Gangnam` |
| `relationship: roommate` | `private apartment roommate setting` |
| `personality: seductive` | `confident seductive expression` |
| `secret: secret_account` | `selfie-ready pose, phone in hand, secret social account vibe` |

Grok call:

| 항목 | 값 |
|---|---|
| provider | OpenRouter |
| model | `x-ai/grok-4.20` |
| max tokens | `180` |
| timeout | `2500ms` |
| 목적 | seed prompt를 Replicate용 최종 prompt로 압축/정리 |

최종 Replicate prompt 예시:

```text
realistic candid iPhone photo of a Korean woman, glamorous Gangnam influencer face with polished makeup, sharp eyebrows and refined Seoul beauty, voluminous glossy goddess waves cascading over shoulders, seductive hourglass figure with slim waist and wide hips, stylish luxury lounge outfit, confident seductive expression, selfie-ready pose holding phone, luxury cafe in Gangnam with private apartment roommate setting, secret social account vibe, natural skin texture, real camera imperfections, sharp focus, flattering natural light
```

## 동작 순서

1. `--input` 있으면 JSON 읽기.
2. 없으면 `gangnam_influencer` preset 사용.
3. normalized value를 seed prompt로 변환.
4. Grok `x-ai/grok-4.20` 직접 호출.
5. Grok 결과를 최종 Replicate prompt로 저장.
6. Replicate `prunaai/z-image-turbo` 호출.
7. Replicate 결과 즉시 `replicateUrl` 출력.
8. 같은 이미지를 R2 업로드.
9. R2 완료 후 `r2Url` 출력.
10. manifest 저장.

## Manifest

| 항목 | 값 |
|---|---|
| 저장 위치 | `data/minimal-image-pipeline-manifests/<id>.json` |
| git 상태 | `data/` gitignore |
| 포함 | `selection`, `seedPrompt`, `grokRequest`, `input.prompt`, `replicateUrl`, `r2Url`, `timingsMs`, `error` |

상태값:

- `starting`: 시작
- `grok_ready`: Grok prompt 변환 완료
- `r2_pending`: Replicate URL 확보, R2 업로드 중
- `done`: R2 업로드 완료
- `r2_failed`: R2 실패, Replicate URL은 보존

## 검증

| 테스트 | 결과 |
|---|---:|
| Grok prompt 변환 | `1491ms` |
| Replicate 첫 이미지 | `1341ms` |
| 첫 이미지 총합 | `2832ms` |
| R2 업로드 | `673ms` |

R2 산출물:

```text
https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/minimal-image-pipeline/20260625054308-vh5dxi.jpg
```

## 다음

- 앱 연결 전 결정 필요: CLI만 유지 vs `/api/preview-image` treatment로 연결.
- 5초 초과 시 원인 구분: Grok latency / Replicate queue 분리.
- selection field 확장 시 manifest에 원본 selection 유지.

## 근거

- 2026-06-25 CLI 실행 확인.
- `node --check bin/minimal-image-pipeline.mjs` 통과.
- `pnpm image:pipeline -- --json` 성공.
- first image: Grok `1491ms` + Replicate `1341ms` = `2832ms`.
