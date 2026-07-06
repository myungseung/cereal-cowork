# Image Prompt v12

> 2026-03-17 | Phase 2 Retention ×1.8 | Image = 결제 드라이버 + 리텐션 훅

---

## Reading Order

| # | File | Contents |
|---|------|----------|
| 1 | **spec.md** (this) | Problem, hypothesis, approach |
| 2 | `todo.md` | Implementation steps + eval criteria |
| 3 | `model-price.md` | Gemini + Replicate model cost comparison |

---

## KPI Link

| Driver | Current | Target | Image의 역할 |
|--------|---------|--------|-------------|
| Retention | 45% | 70% | Tap 30+ = 91.7% retention. 이미지 품질 → 탭 유도 → retention |
| ARPU | ₩11K | ₩17K | 블러 이미지 해제 = 결제 동기 #1 |
| Death Zone | 46% of users | <20% | 이미지 탭 30+ = Death Zone 탈출 조건 |

**핵심 데이터:** Tap 30+ 유저 retention = 91.7%. Image Path(Tap 30+, AI<250) = 87.5%.
→ 이미지 품질이 올라가면 탭 증가 → Death Zone 탈출 → retention 상승

---

## Problem: 현재 이미지 프롬프트의 한계

### 유저 불만 (v11 spec에서 확인)

| 증상 | 원인 | 이미지 영향 |
|------|------|-----------|
| "같은 사진만 나옴" | 포즈/구도 반복, 다양성 부재 | 탭 감소 → Death Zone |
| "말한 거랑 다름" | 유저 요청 무시, 열도 미스매치 | 신뢰 하락 → 이탈 |
| "캐릭터가 바뀜" | outfit/외모 불일관 | 몰입 파괴 |
| "지루해짐" | 동일 카메라 각도, 동일 조명 | 반복 피로 |

### 구조적 문제 (현재 v6 buildImagePrompt)

| 문제 | 현재 상태 | 결과 |
|------|---------|------|
| 포즈 다양성 | 프롬프트가 "derive from conversation"만 지시 | Gemini가 매번 비슷한 포즈 생성 |
| 카메라 다양성 | 지시 없음 | 90%+ selfie angle from above |
| 반복 감지 | 없음 | 연속 3장 같은 구도 |
| 침대 씬 | "topless OK" 정도의 가이드 | 포즈 단조, sexual tension 부재 |
| 유저 요청 파싱 | lastUserMessage만 참조 | "뒤돌아봐" → 무시되는 경우 다수 |

---

## Hypothesis

> 이미지 프롬프트에 5가지 축을 강화하면 탭 수 증가 → retention 상승

---

## 5 Axes of Improvement

### Axis 1: User Request Fidelity (유저 요청 정확 반영)

**Problem:** "사진 보내줘"와 "뒤에서 찍어줘"를 같은 FLIRTY로 처리

**Fix:**
- STEP 0 추가: 유저 메시지에서 **구체적 요청** 추출 (포즈, 각도, 의상, 장소)
- 구체적 요청 있으면 → heat 분류보다 우선 적용
- 없으면 → 기존 heat 기반 자동 결정

| User says | 현재 | v12 |
|-----------|------|-----|
| "뒤에서 찍어줘" | selfie from above | back view, looking over shoulder |
| "거울셀카" | selfie from above | mirror selfie, phone visible |
| "누워서 찍어" | selfie from above | lying down POV, camera above face |
| "전신 보여줘" | close-up selfie | full body, standing/sitting |

### Axis 2: Character Consistency (얼굴 + 체형만 고정)

**Problem:** seed 기반 얼굴 일관성은 있지만, 체형/나이/헤어 등이 프롬프트마다 흔들림

**Scope:** 얼굴 + 체형만 고정. 의상/장소/액세서리는 대화 맥락에 따라 자유 변경.

| 고정 (character identity) | 자유 (context-dependent) |
|--------------------------|------------------------|
| 나이, 얼굴형, 눈 색 | 의상 (대화에서 파생) |
| 체형, 키 | 장소 (대화에서 파생) |
| 머리색, 머리 길이/스타일 | 액세서리 |
| seed (Replicate) | 포즈, 카메라 각도, 조명 |

**Fix:**
- characterVisual을 구체적 수치로 강화 (slim → 168cm 54kg slim)
- 프롬프트 내 character identity 블록을 상단 고정 + 하단 반복 (recency bias)
- outfit/location은 `LOCKED` 제거 → warm memory의 현재 상태 참조하되, 대화에서 변경 시 즉시 반영

### Axis 3: Context-Aware Best Shot (맥락 기반 최적 장면)

**Problem:** 대화 맥락에서 최적의 "찰나"를 포착하지 못함

**Fix:**
- AI 응답 내용에서 **구체적 동작/상황** 추출
- "침대에 누워서 쉬고 있어" → lying on bed, relaxed
- "방금 샤워하고 나왔어" → wet hair, towel, bathroom steam
- 동작 + 감정 조합으로 자연스러운 순간 포착

### Axis 4: Repeat Penalty (반복 방지)

**Problem:** 연속 이미지가 같은 구도/포즈

**Fix:**
- `previousPrompts` 필드 추가 — 최근 3개 이미지 프롬프트를 입력에 포함
- 명시적 금지: "MUST NOT repeat camera angle, pose, or framing from previous prompts"
- 카메라 각도 풀 제공 (selfie above / eye level / below / side / back / mirror / full body)
- 포즈 뱅크: standing / sitting / lying / leaning / kneeling / stretching

| Turn | 현재 (반복) | v12 (다양) |
|------|-----------|-----------|
| T1 | selfie above, lying bed | selfie above, lying bed |
| T2 | selfie above, lying bed | eye level, sitting on bed edge |
| T3 | selfie above, lying bed | mirror selfie, standing |

### Axis 5: Explicit Wow — Pre-defined Sexual Poses (침대 씬 포즈 뱅크)

**Problem:** HEATED/EXPLICIT에서 포즈가 단조, "just topless standing" 수준

**Fix:**
- HEATED (exp 7) 포즈 뱅크:

| Pose | Prompt Keywords |
|------|----------------|
| 이불 속 어깨 | lying under white sheets, bare shoulders peeking out, clutching blanket to chest |
| 브라 스트랩 내림 | sitting on bed, one bra strap slipping down shoulder, looking at camera |
| 엎드려 턱 괴기 | lying prone on bed, chin resting on hands, legs bent up behind |
| 등 돌리고 어깨 | back view sitting on bed, bare back, looking over shoulder |

- EXPLICIT (exp 8) 포즈 뱅크:

| Pose | Prompt Keywords |
|------|----------------|
| 이불 흘러내림 | lying on bed, sheet barely covering lower body, one arm across chest |
| 뒤돌아 엉덩이 | back view standing, nude, looking over shoulder, bare back and butt |
| 무릎 세우기 | sitting nude on bed, knees drawn up, arms wrapped around legs |
| 옆으로 누움 | lying on side, nude, one hand between thighs, other arm under pillow |
| 거울 앞 | standing before mirror, covering with towel/hand, post-shower |

- 랜덤 선택 + previousPrompts와 중복 방지

---

## Approach

### v6 → v12 변경점

| Layer | v6 (현재) | v12 |
|-------|----------|-----|
| 유저 요청 파싱 | lastUserMessage → heat 분류만 | STEP 0: 구체적 요청 추출 → 우선 적용 |
| 캐릭터 고정 | outfit/location까지 LOCKED | 얼굴+체형만 고정, 의상/장소는 맥락 자유 |
| 맥락 포착 | conversation context만 | AI 응답에서 동작/상황 추출 |
| 반복 방지 | 없음 | previousPrompts 3개 + MUST NOT repeat |
| 침대 씬 | "topless OK" | 포즈 뱅크에서 선택 |
| 카메라 | 지시 없음 | 7가지 각도 풀 + 이전과 다른 각도 강제 |

### 변경하지 않는 것

- Gemini model (flash-lite 유지)
- Replicate model (z-image-turbo 유지)
- exposure scoring 로직 (heat → score 매핑 동일)
- R2 업로드 파이프라인
- seed 기반 캐릭터 얼굴 일관성

---

## Eval Plan

1. `admin/eval-image`에서 prod 케이스 20건 로드
2. v6 원본 vs v12 replay 비교
3. 6가지 기준 PASS/FAIL (todo.md 참조)
4. 목표: **PASS율 80%+** → 프로덕션 배포

---

## Risk

| Risk | Mitigation |
|------|-----------|
| 프롬프트 길이 증가 → 비용 | flash-lite input 저렴 ($0.10/1M), 무시 가능 |
| 포즈 뱅크가 부자연스러움 | eval에서 검증, 뱅크는 가이드일 뿐 강제 아님 |
| previousPrompts 추가 = warm_memory 변경 | visualState에 lastPrompts 필드 추가 (backward compatible) |
| Replicate seed 고정 = 포즈 달라도 얼굴 동일 | 의도한 동작 — 캐릭터 일관성 유지 |
