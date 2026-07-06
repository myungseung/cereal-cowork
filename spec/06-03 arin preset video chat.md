# 06-03 arin preset video chat spec

| 항목 | 값 |
|---|---|
| Goal | Arin 캐릭터로 preset video chat 만들기 |
| Parent | [[Experiments/06-03 preset video chat 결제전환 2배]] |
| Source | `/Users/cheonmyeongseung/Desktop/ads/new소재/preset_arin` |
| 상태 | spec 작성 |
| 기준일 | 2026-06-03 |

## 작업 순서
| 순서 | 작업 | 산출물 |
|---:|---|---|
| 1 | 6개 선택지 중 `Undress` 1개 먼저 확정 | 테스트 선택지 1개 |
| 2 | `Undress` 비디오 1개 생성 | `choice_undress_01` 비디오 파일 |
| 3 | 유저 검사 | 통과/수정/폐기 판정 |
| 4 | 성공 시 나머지 5개 선택지 확정 | 선택지 문구 / 무료·잠금 구분 / 결제 트리거 |
| 5 | 성공 시 나머지 5개 비디오 생성 | `choice_02~06` 비디오 파일 |
| 6 | 첫 화면 + 선택지 overlay 구성 | Arin video chat 첫 화면 |
| 7 | paywall / 이벤트 연결 | payment funnel 측정 |

## 목표
- Meta 광고 Arin 클릭 -> `/create` 대신 Arin preset video chat으로 직접 진입.
- 첫 화면에서 Arin 영상이 주 콘텐츠.
- Chat UI는 영상 위 overlay.
- 결제 이유: `다음 반응 / 다음 장면 / 더 깊은 선택지 unlock`.

## Source Assets
| 파일 | 확인 내용 |
|---|---|
| `image_2.png` | Arin 캐릭터 기준 이미지. 침대/창가/화이트 톤. 카메라 정면 응시. |
| `video_2.mp4` | 9.33초. 1280×1616. 30fps. 오디오 포함. |
| `voice_2.wav` | 15.76초. 24kHz mono. Whisper tiny 전사 품질 낮음. |

## Arin Visual
| 항목 | 내용 |
|---|---|
| 외형 | 검은 머리, 묶은 머리, 앞머리, 흰 크롭 탱크톱, 흰 하의 |
| 배경 | 밝은 침대, 큰 창문, 자연광, 화이트 룸 |
| 분위기 | 부드럽고 가까운 연인/아침 대화 느낌 |
| 포즈 | 침대에 기대 앉아 카메라를 직접 봄 |
| 강점 | 얼굴/눈맞춤이 강함. 광고 클릭 후 동일 캐릭터 연결에 적합 |
| 리스크 | 노출은 과하지 않지만 몸 라인이 강조됨. Meta/앱스토어 문구는 과격하게 쓰지 않기 |

## Video Flow
| 시간 | 화면 | 사용 방향 |
|---:|---|---|
| 0.0~1.5s | Arin이 침대 위에서 카메라를 봄. 상체/얼굴 중심 | chat open 첫 화면 |
| 2.0~3.5s | 입을 열고 말하는 듯한 표정. 눈맞춤 유지 | 첫 메시지 재생 구간 |
| 4.0~5.5s | 미소/부드러운 표정. 얼굴이 더 가까워짐 | 첫 선택지 노출 구간 |
| 6.0~7.5s | 계속 말하는 듯한 입모양. 가까운 거리 | 무료 interaction 후 반응 |
| 8.0~9.0s | 눈맞춤 + 미소. 마무리 느낌 | paywall 직전 preview 종료 |

## Voice
| 항목 | 내용 |
|---|---|
| 원본 | `voice_2.wav` |
| 길이 | 15.76초 |
| 전사 | `너는 많은 걸 바라지 않더랑 친구들 주도 안고 / 너는 그냥 219 싶은 거야 / 그게 좋더라 / 다 지금 듣고 있어` |
| 판정 | 전사 품질 낮음. 실제 대사 SOT 재확인 필요 |
| 사용 | 현재는 말투/길이 참고만. 문구 근거로 사용 금지 |

## First Screen
| Layer | 내용 |
|---|---|
| Background | `video_2.mp4` autoplay, muted 또는 voice 포함 여부 미확인 |
| Top | Arin 이름, 잔여 재화, Upgrade CTA |
| Center | 영상 unobstructed. 얼굴/상체 가리지 않기 |
| Bottom | 첫 메시지 카드 + 6개 선택지 |
| CTA | 첫 선택 1개 무료, 다음 반응 unlock 유도 |

## Preset Choices
|  순서 | 선택지       | 상태     | 목적               | 비디오 산출물             |
| --: | --------- | ------ | ---------------- | ------------------- |
|   1 | `Undress` | 우선 테스트 | 첫 비디오 생성 + 유저 검사 | `choice_undress_01` |
|   2 | 미확정       | 대기     | 관계감 형성           | `choice_02`         |
|   3 | 미확정       | 대기     | 영상 반응 unlock     | `choice_03`         |
|   4 | 미확정       | 대기     | 독점감              | `choice_04`         |
|   5 | 미확정       | 대기     | 다음 장면 unlock     | `choice_05`         |
|   6 | 미확정       | 대기     | paywall 연결       | `choice_06`         |

## Video Generation Method
| 단계 | 방법 | 산출물 |
|---:|---|---|
| 1 | `image_2.png`를 Arin reference image로 사용 | 캐릭터 일관성 |
| 2 | `video_2.mp4`에서 얼굴/카메라 거리/침대/창가 톤을 motion reference로 사용 | 톤 일관성 |
| 3 | `Undress`용 5~8초 video prompt 작성 | 생성 프롬프트 |
| 4 | image-to-video로 1개 생성 | `choice_undress_01.mp4` |
| 5 | 생성본을 유저가 직접 검사 | 통과/수정/폐기 |
| 6 | 통과 시 같은 방식으로 나머지 5개 생성 | `choice_02~06.mp4` |

### Generation Tool Status
| 항목 | 상태 |
|---|---|
| 선택 도구 | ComfyUI |
| Replicate | 제외 |
| Replicate 제외 이유 | 검열이 강해 `Undress` 영상을 만들 수 없음 |
| 로컬 ComfyUI CLI | 미설치 |
| 로컬 ComfyUI server | 실행 안 됨 |
| 사용 가능 도구 | `ffmpeg`, `ffprobe`, `python3` |
| Comfy Cloud | 비용 낮음. `Undress` 1차 검증 workflow 후보 |
| 진행 방향 | ComfyUI에서 `image_2.png` + `video_2.mp4` reference 기반 video generation |

### Cost / Speed Comparison
| Tool | 비용 기준 | 5초 1개 | 8초 1개 | 속도/운영 | 판정 |
|---|---|---:|---:|---|---|
| Comfy Cloud Standard | $20 / ~380 5s videos | ~$0.053 | ~$0.084 | workflow 준비 필요. 구독 최소 $20 | 진행 |
| Replicate Wan 480p | $0.09 / output sec | $0.45 | $0.72 | API 즉시 실행. 오디오 input 가능 | 제외: 검열 |
| Replicate Wan 720p | $0.25 / output sec | $1.25 | $2.00 | API 즉시 실행. 고해상도 | 제외: 검열 |
| Kling Motion Control | credit SOT 미확인 | 미확인 | 미확인 | 원본 재현율/해상도 보정 | 보정 후보. SOT 미확인 |

- 비용: Comfy Cloud가 Replicate Wan 480p보다 약 8.5배 저렴.
- 비용: Comfy Cloud가 Replicate Wan 720p보다 약 23.8배 저렴.
- 정책: Replicate은 `Undress` 생성에 부적합.
- 진행: ComfyUI로 `Undress` 1개 먼저 생성 -> 유저 검사 -> 통과 시 나머지 5개 생성.

### Undress Prompt Draft
```text
Arin, same woman as the reference image, Korean young woman, soft morning bedroom, white bedding, large bright window, natural daylight, white cropped tank top and white lounge pants, sitting on bed, intimate video chat angle, direct eye contact, gentle teasing smile, slowly reaches toward the hem of her white top as if about to undress, subtle shoulder movement, no nudity, no explicit exposure, cinematic realistic, vertical 9:16, smooth natural motion, 5 seconds
```

Negative:
```text
nudity, explicit sexual content, exposed nipples, distorted face, extra fingers, identity drift, different outfit color, different room, camera shake, text, watermark, low quality
```

## Paywall Trigger
| Trigger | 문구 방향 |
|---|---|
| 무료 선택지 1~2개 후 | `Arin의 다음 반응 보기` |
| 영상 8~9초 preview 종료 | `이어서 보기` |
| 잠금 선택지 tap | `이 선택에 Arin이 반응해요` |
| 재화 0 | `Upgrade하고 다음 장면 unlock` |

## Events
| Event | 의미 |
|---|---|
| `preset_chat_open` | 광고 클릭 후 Arin chat 진입 |
| `preset_video_start` | Arin 영상 시작 |
| `preset_choice_tap` | 선택지 클릭 |
| `preset_locked_choice_tap` | 잠긴 선택지 클릭 |
| `preset_video_reaction_start` | 선택지 후 영상 반응 시작 |
| `preset_paywall_view` | paywall 노출 |
| `preset_payment_start` | 결제 시작 |
| `preset_payment_success` | 결제 성공 |

## Todo
- [ ] `Undress` 선택지 1개 먼저 확정.
- [x] `Undress` 비디오 1개 생성: `/Users/cheonmyeongseung/Desktop/ads/output/ugc-runs/arin_comfyui_undress_04-20260603223853/choice_undress_01.mp4`.
- [ ] `Undress` 생성본 유저 검사.
- [ ] 성공 시 나머지 5개 선택지 확정.
- [ ] 성공 시 나머지 5개 비디오 생성.
- [ ] 선택지별 무료/잠금/결제 후 반응 구분.
- [ ] 선택지별 비디오 파일명/위치 기록.
- [ ] 첫 메시지 1개 작성.
- [ ] `video_2.mp4`를 첫 화면 background로 쓸지, 첫 메시지 후 reaction으로 쓸지 결정.
- [ ] `voice_2.wav` 실제 대사 재전사 또는 원문 SOT 확보.
- [ ] paywall 문구 2안 작성.
- [ ] Arin 광고 소재 URL / 캠페인 / 랜딩 현재값 확인.
- [ ] Arin preset chat route/entry SOT 확인.
- [ ] 이벤트명/분석 SOT 확인.
- [ ] Meta/앱스토어 리스크 문구 확인.

## 미확인
- 현재 Arin 광고 1개인지, 3개 캐릭터 중 1개인지.
- 실제 앱에서 Arin 캐릭터 ID.
- preset chat이 웹 랜딩인지 앱 deep link인지.
- 비디오/보이스 사용 권리와 최종 파일 SOT.
- 유저 챗이 동영상 반응을 실제로 바꾸는 범위.
- 나머지 5개 선택지 최종 문구.
- 사용할 video generation tool SOT.
- `Undress` 생성 비디오 SOT.
- 나머지 5개 생성 비디오 SOT.

## 근거
- 사용자 요청 2026-06-03: `goal: arin 캐릭터로 video chat 만들기`.
- 사용자 정정 2026-06-03: 작업 시작은 선택지 6개 확정 + 선택지별 비디오 6개 생성.
- 사용자 정정 2026-06-03: 첫 작업은 `Undress` 항목 비디오 1개 생성 -> 유저 검사 -> 성공 시 다른 5개 비디오 제작.
- Source folder 확인: `image_2.png`, `video_2.mp4`, `voice_2.wav`.
- ffprobe: `video_2.mp4` 9.33초, 1280×1616, 30fps.
- ffprobe: `voice_2.wav` 15.76초, 24kHz mono.
- vision 확인: image + video frames 0.5초 간격 19장.
- Whisper tiny 확인: 한국어 전사 품질 낮음, 문구 SOT로 사용 금지.
