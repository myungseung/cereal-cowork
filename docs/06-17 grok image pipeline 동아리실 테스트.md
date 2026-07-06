# 06-17 grok image pipeline — 동아리실 SFW→NSFW 5장 연속

**생성일**: 2026-06-17 14:22
**Generator**: Grok 4.20 (x-ai/grok-4.20, OpenRouter)
**Image Model**: prunaai/z-image-turbo-lora
**LoRA**: NiceGirls 0.5 + Hyper-SD 0.2
**Steps**: 9 | **Guidance**: 1 | **Resolution**: 832×1216

---

## 캐릭터

| 속성 | 값 |
|------|-----|
| 이름 | 하린 |
| 나이 | 20 |
| 얼굴 | big innocent round eyes, pale skin, small nose |
| 헤어 | short ash-brown bob, semi-perm wave |
| 체형 | petite slim cute |
| 성격 | 밝고 활발한 동아리 후배, 선배 앞에서는 부끄러움 많음 |
| 직업 | 대학교 2학년 |

## Arc: 동아리방 연속

| # | Stage | 위치 | 각도 | 의상 |
|---|-------|------|------|------|
| 1 | 동아리방 연습 | 동아리방 책상/소파 | 정면 책상 너머 | 후드티+청바지 |
| 2 | 늦은 밤 | 동아리방 바닥 | 사이드 프로필 | 상의탈의(브라) |
| 3 | 분위기 | 동아리방 소파 | 낮은 앵글 | 상의탈의 |
| 4 | 첫 키스 | 동아리방 소파 | 정면 클로즈업 | 상의탈의 |
| 5 | 동아리방에서 | 동아리방 바닥 시트 | 위에서 아래로 | 거의 누드 |

## 대화 (10턴)

```
[character] 선배! 오늘도 연습 도와주러 오신 거예요?
[user] 응, 하린이 혼자 있는 거 같길래. 문 잠그고 들어왔어.
[character] 근데 선배 앞에서는 아직도 긴장돼요. 둘만 있으니까 더 부끄러워지네요.
[user] 긴장 풀어. 우리 이제 비밀 연애 중이잖아. 형광등 좀 줄일까?
[character] 네. 하나만 켜놓을게요. 선배, 여기 바닥에 앉아서 좀 쉬실래요?
[user] 그래. 머리 좀 풀어봐.
[character] 이렇게요? 선배가 원하시는 대로... 후드티 단추도 좀 풀어도 될까요?
[user] 응, 편하게 해. 소파로 옮길까? ...눈 감아볼래?
[character] 네... 선배. ...이제 키스해 주세요. 처음으로...
[user] 하린아... 사랑해. 이제 다 벗겨도 돼? 바닥에 시트 깔아줄게...
```

## 생성 결과

### Scene 1 — 동아리방 연습
![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617142243-하린-scene1/image.jpg)
- **연속성**: 소파, 후드티 단추 풀기 시작, 형광등 1개만 켜짐
- **소요**: 3.7s
- **manifest**: `experiments/grok-pipeline/20260617142243-하린-scene1/manifest.json`

### Scene 2 — 늦은 밤
![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617142252-하린-scene2/image.jpg)
- **연속성**: 바닥 시트로 이동, 상의 완전 벗겨짐(브라만)
- **소요**: 3.9s
- **manifest**: `experiments/grok-pipeline/20260617142252-하린-scene2/manifest.json`

### Scene 3 — 분위기
![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617142300-하린-scene3/image.jpg)
- **연속성**: 바닥 시트, 깊은 키스, 신체 접촉
- **소요**: 3.7s
- **manifest**: `experiments/grok-pipeline/20260617142300-하린-scene3/manifest.json`

### Scene 4 — 첫 키스
![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617142308-하린-scene4/image.jpg)
- **연속성**: 바닥 시트, 시선 처리, 더 intimate한 구도
- **소요**: 3.7s
- **manifest**: `experiments/grok-pipeline/20260617142308-하린-scene4/manifest.json`

### Scene 5 — 동아리방에서
![](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/grok-pipeline/20260617142318-하린-scene5/image.jpg)
- **연속성**: 거의 누드, 전신, 첫 합일 직전
- **소요**: 4.0s
- **manifest**: `experiments/grok-pipeline/20260617142318-하린-scene5/manifest.json`

---

## 파이프라인 구조

```
grok-image-pipeline.mjs
├── Layer 1: Grok 4.20 — 대화 10턴 생성 (캐릭터 설정 + arc 기반)
└── [Scene loop × 5]
    ├── Layer 2: Grok 4.20 — 이전 scene context + 대화 slice → prompt
    ├── Layer 3: z-image-turbo — image generation
    └── R2 upload: image.jpg + manifest.json (pair)
```

## 핵심 패턴

| 패턴 | 설명 |
|------|------|
| 연속성 전달 | 각 scene의 Grok 호출에 이전 scene의 summary/location/outfit/angle 전달 |
| Fabric 변화 | outfitProgression 배열로 장면별 의상 상태 관리 |
| 조명 유지 | arc의 lighting 설정이 모든 scene에 일관 적용 |
| Stage 자유도 | Grok이 stage label + 대화 맥락을 종합해 scene 결정 |
| Manifest 필수 | 모든 생성에 R2 image.jpg + manifest.json 쌍 저장 |

## 현재 이슈

1. **얼굴 일관성 없음** — z-image-turbo는 LoRA 스타일 통일만 하고 face identity는 고정 안 됨
2. **Scene 1 jump** — 대화가 첫 키스로 빠르게 진행되어 scene 1에서부터 상의탈의가 시작됨
3. **Prompt length 편차** — 876~1226자, 1100자 이상이면 z-image-turbo가 일부 truncate 가능성

## 관련 링크

- `.agents/skills/askAgent/scripts/grok-image-pipeline.mjs`
- `.agents/skills/askAgent/scripts/test-clubroom.json`
- [docs/06-17 ji-eun seo grok image pipeline.md](docs/06-17%20ji-eun%20seo%20grok%20image%20pipeline.md)
