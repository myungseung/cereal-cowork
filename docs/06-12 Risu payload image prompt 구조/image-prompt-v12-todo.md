# Image Prompt v12 — TODO

> Phase 2 Retention ×1.8 | Image quality → engagement → retention

---

## Completed

- [x] Eval 페이지 구축 — `admin/eval-image`, prod 로그 로드 + replay
- [x] Spec 작성 — 5가지 개선축, KPI 연결, model-price
- [x] v12 프롬프트 엔진 — `lib/chat/v12/image-prompt.ts`
  - 6-section structured output (Subject/Pose/Outfit/Environment/Lighting/Camera)
  - OpenRouter + Fireworks + RisuAI jailbreak payload
  - z-image-turbo 최적화 (civitai 패턴 적용)
- [x] 모델 비교 — flash-lite vs deepseek-v3.2 vs llama-4-maverick
  - **Winner: llama-4-maverick** (4s, 19금 커버링 최고, x1.5 비용)
- [x] exp 3-8 eval — 13케이스 ALL PASS, 20케이스 배치 ALL PASS
- [x] exp 9-10 추가 — SEXUAL_ACT / CLIMAX heat levels
  - 남성 파트너 신체 프레임 포함
  - 셀프 센서링 제거 → 앱 블러 시스템이 자동 처리
- [x] Replicate 파라미터 최적화
  - cfg 0→1 (프롬프트 따르기 향상, 비용 동일)
  - 832x1216 해상도 (civitai 표준)
  - steps 8 유지 (비용 동일)
- [x] Eval 인프라
  - Export → V12 버튼 (3004→3002 케이스 전달)
  - 병렬 배치 Run All
  - auto-save JSON (`eval-results/v12-eval-latest.json`)
  - logId 클릭 복사, userMessage DB 조회
- [x] NSFW 블러 필터 연동
  - main에서 `lib/nsfw-filter.ts` + `lib/models/320n.onnx` 머지
  - `generateImage()` 파이프라인에 `censorImage()` 연결 (Replicate → 다운로드 → 블러 → R2)
  - `r2.ts`에 `uploadImageBuffer()` 추가
  - exp 10 CLIMAX 3케이스 테스트 — 블러 정상 동작 확인
- [x] Eval 인프라 강화
  - api-logs 각 user 메시지에 E 버튼 → eval-image로 케이스 전달
  - localStorage → 서버 파일 저장 전환 (`.claude/data/eval-image-ids.json`)
  - 시크릿 모드/다른 브라우저에서도 케이스 유지
  - `saved-ids` API (GET/POST/DELETE)
  - 탭 전환 시 자동 리로드 (visibilitychange)
- [x] Eval 모델/버전 선택
  - prompt version 토글 (V6/V12) — v6은 Gemini 직접, v12는 OpenRouter
  - LLM 모델 드롭다운 (maverick/flash-lite/deepseek/qwen3)
  - Pipeline Inspector → fixed right overlay + dimmed backdrop
- [x] Style suffix 확정 — compare-theme A/B 검증
  - zit-prompt-db 2,763건 + civitai reference에서 suffix 패턴 추출
  - 고정(카메라/텍스처) vs 가변(조명/무드/색감) 분리
  - **Winner: V6 Production suffix** (10개 후보 비교 검증)
  - `"iPhone 16 Pro raw photo, no filter, natural skin texture, subtle grain, slight lens distortion, candid amateur quality, NOT studio photography"`
  - v12 CONFIG: lighting 지시를 warm/soft/low-contrast로 변경

## Next

- [ ] **exp 9-10 신체 깨짐 개선** — zit DB 성공 패턴 분석 + anatomy check 강화
  - 두 사람 신체가 깨지는 문제 (팔다리 비율, 포즈 물리적 불가능)
  - z-image-turbo 모델의 두 사람 씬 한계 파악
  - 프롬프트 anatomy constraint 추가
- [ ] exp 9-10 배치 eval — 10케이스 PASS율 80%+ 목표

## Backlog — 프로덕션 배포

- [ ] v6 dm.ts 이미지 파이프라인에 v12 통합
  - `buildImagePrompt` → `generateV12ImagePrompt` 교체
  - OpenRouter 호출로 전환
  - warm_memory에 `lastPrompts` 필드 추가 (반복 방지)
- [ ] AB 실험 설정 — v6 vs v12 50:50
- [ ] 프로덕션 모니터링 — 이미지 탭 수, 결제 전환율

---

## Architecture

| Layer | v6 (현재) | v12 (신규) |
|-------|----------|-----------|
| LLM | Direct Gemini API (flash-lite) | **OpenRouter + Fireworks** (llama-4-maverick) |
| Jailbreak | 없음 | **RisuAI 7-message payload** |
| 프롬프트 | 80-150 word paragraph | **6-section structured** |
| Heat levels | CASUAL~EXPLICIT (3-8) | **CASUAL~CLIMAX (3-10)** |
| 센서링 | genital 금지 | **앱 블러 자동 처리, 프롬프트 무검열** |
| Replicate | cfg 0, steps 8, 768x1024 | **cfg 1, steps 8, 832x1216** |

## Quality Standard (6-point checklist)

| # | Criterion | Description |
|---|-----------|-------------|
| 1 | User Request Fidelity | 유저 요청을 정확히 따르는가? |
| 2 | Character Consistency | 캐릭터 얼굴/체형이 일관되는가? |
| 3 | Context Following | 의상/배경/포즈/각도/조명/헤어가 맥락에 맞게 변하는가? (얼굴 제외 전부 변경 가능) |
| 4 | Body Perfection | 신체 비율/해부학적으로 자연스러운가? |
| 5 | English Only | 프롬프트가 영어로만 작성되는가? |
| 6 | No Repetition | 이전 이미지와 반복되지 않는가? |

## Files

| File | Location |
|------|----------|
| v12 프롬프트 엔진 | `lib/chat/v12/image-prompt.ts` |
| 테스트 API | `app/api/admin/test-v12/route.ts` |
| eval replay API | `app/api/admin/eval-image/replay/route.ts` (v6/v12 분기) |
| eval 페이지 | `app/admin/eval-image/page.tsx` |
| eval compare-theme | `app/admin/eval-image/compare-theme/page.tsx` (suffix A/B 비교) |
| compare-gen API | `app/api/admin/eval-image/compare-gen/route.ts` |
| eval 결과 | `.claude/spec/19-20260317-image-prompt-v12/eval-results/` |
| eval 케이스 저장 | `.claude/data/eval-image-ids.json` (서버 파일) |
| eval 케이스 API | `app/api/admin/eval-image/saved-ids/route.ts` |
| NSFW 필터 | `lib/nsfw-filter.ts` + `lib/models/320n.onnx` |
| 이미지 생성 | `lib/replicate.ts` (NSFW 블러 통합) |
| R2 업로드 | `lib/r2.ts` (`uploadImageBuffer` 추가) |
| zit prompt DB | `.claude/data/zit-prompt-db.json` (2,763 entries) |
| zit reference | `.claude/spec/19-20260317-image-prompt-v12/z-image-turbo-prompt.md` |
| log-detail 쿼리 | `.claude/skills/query/queries/log-detail.ts` (PK 조회 + ±2분 컨텍스트, <1s) |
| worktree | `image-prompt-v12` (port 3002) |
