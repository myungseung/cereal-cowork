# 06-05 Undress 비디오 동작 디자인

| 항목 | 값 |
|---|---|
| Parent | [[06-03 arin preset video chat]] |
| 수위 | 3-tier: 전신노출 허용. 4-tier(성기클로즈업/노골적성행위) 금지 |
| 대상 | 성인인증 유저 |

## Flow

| 구간 | 시간 | 동작 |
|---|---|---|
| 읽기 | 0.0~1.0s | Arin이 "Undress" 읽음. 카메라 응시, 미소. |
| 준비 | 1.0~2.5s | 손이 탱크탑 밑단으로. 자연스러운 속도. |
| 탈의 | 2.5~4.5s | 탱크탑 천천히 올림. 몸 드러남. 눈맞춤 유지. |
| 완료 | 4.5~5.0s | 상의 제거 완료. 탑리스. → paywall 트리거 |

## 생성

| 항목 | 값 |
|---|---|
| 도구 | RunPod RTX 5090 Wan2.2-Remix NSFW I2V |
| CLI | `scripts/wanpod run --workflow remix-i2v` |
| 입력 | `image_2.png` |
| 길이 | 5s / 16fps |
| 해상도 | 720×1280 (16-div) |

## 프롬프트

```
The same adult Korean woman from the first frame, dark messy bun with wispy bangs, white ribbed cropped tank top and white lounge pants, bright white bedroom, white bedding, large sunny window, natural daylight, seated on bed. She slowly lifts the hem of her white tank top with both hands, sliding the fabric up, revealing her bare chest. Smooth unhurried motion, intermittent eye contact, soft knowing smile. Cinematic realistic, 9:16 vertical, high quality skin texture.
```

## Paywall

- 5.0s 종료 → "이어서 보기"
- 8초판 확장 시 바지 탈의 직전 컷 → "다음 장면 unlock"
