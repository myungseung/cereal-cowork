# 06-12 video onboarding spec

| 항목 | 값 |
|---|---|
| Goal | 온보딩을 lovescape 스타일 비디오 경험으로 재설계 — 옵션 비디오 카드 + recap 시퀀스 + 최초 캐릭터 비디오 |
| 스코프 | 온보딩만. **챗은 불가침** (video chat은 실험영역 제외) |
| 배포 형태 | 독립 프로토타입 라우트 (경험 검증 후 A/B 전환) |
| 레퍼런스 | lovescape (`~/Desktop/app_reference_capture/lovescape`), `codex/video-chat-ui` worktree |
| 상태 | spec 작성 |
| 기준일 | 2026-06-12 |

## 확정 결정 (grill 8문답)

| # | 결정 | 선택 |
|---|---|---|
| 1 | 결과물 | 자유 조합 커스텀 캐릭터 유지 (preset 수렴 아님) |
| 2 | 생성 지점 | 옵션 카드 = 사전 제작, 실시간 생성은 최초 캐릭터 1회만 |
| 3 | 해금 모델 | 월간 구독으로 잠김요소 전체 해제. 크레딧은 테스트 대상 아님 |
| 4 | 배포 | 별도 프로토타입 라우트 → 검증 후 A/B |
| 5 | 비디오 범위 | 비주얼 3카테고리(얼굴/헤어/몸매 ~30개)만 비디오 카드. 관계/비밀은 2열 정사각 이미지 grid |
| 6 | latency 처리 | recap 시퀀스가 생성 대기시간 마스크. 실패 시 이미지 fallback |
| 7 | 생성 모델 | Replicate `prunaai/p-video` — 5초 720p ≈ 10초 생성, $0.10/개 (draft $0.025) |
| 8 | recap 형태 | 선택 비디오 3개 풀스크린 순차 자동재생(~15초) + 비비주얼 태그 오버레이 → 생성 비디오로 crossfade |

## UX 플로우

```
스텝 1-4 (비주얼): 비디오 카드 — 중앙 포커스 캐러셀, 자동재생 루프
  얼굴 → 나이(슬라이더) → 헤어 → 몸매
스텝 5-6 (이미지): 2열 정사각 이미지 grid, 위아래 스크롤
  관계 → 비밀
  └ 백그라운드: 마지막 비주얼 선택 시점부터 preview 이미지 생성 시작
recap: 선택 비디오 3개 순차 풀스크린 재생 + 태그 오버레이
  └ 백그라운드: 이미지 완료 → p-video i2v 시작 (~10초)
클라이맥스: recap 종료 = 생성 완료 → "당신의 캐릭터" 비디오 crossfade 등장
CTA: 채팅 시작 → 기존 챗으로 이동 (챗 변경 없음)
```

- 생성 실패/지연: 현행 preview 이미지로 자연 강등 (현행과 동일 경험)

## 06-13 보충 결정

| 항목 | 결정 |
|---|---|
| 관계/비밀 UI | swipe 아님. 구버전 `/create` 첫 얼굴 선택처럼 2열 정사각 이미지 grid |
| handoff 테스트 | blur tap이 아니라 `blur/teaser tap → start button 노출 → start tap` 구간 테스트 |
| DB 최소 | session, selections, assets, generation job, event log만 필요 |
| API 최소 | session 생성, selection 저장, asset 조회, reveal job 생성/조회, event 기록 |
| video 생성 | option loop는 얼굴/헤어/몸매만. reveal video는 유저 선택값 기반 1회 생성 |

## 06-13 비디오 생성 파이프라인 확정

| Gate | 단계 | 산출물 | 승인 |
|---:|---|---|---|
| 0 | 유저 입력 기반 새 캐릭터 설계 | character design prompt/spec | founder 승인 |
| 1 | 승인된 설계로 캐릭터 이미지 재생성 | start image | founder 승인 |
| 2 | 승인된 이미지로 p-video 생성 | reveal video | founder 승인 |

- 기존 승인 옵션 이미지를 바로 i2v 하지 않는다.
- reveal video는 반드시 유저 선택값을 종합한 새 캐릭터 이미지에서 시작한다.
- 각 단계는 다음 단계 실행 전 founder gate를 통과해야 한다.

## 06-14 이미지 파이프라인 완료

| 항목 | 상태 |
|---|---|
| prompt DB | `docs/06-14 image prompt pattern table.md` |
| prompt 생성 | Obsidian 통과 raw prompt table 전체 → Gemini 3 Flash body prompt |
| prefix | Gemini에는 숨김. 코드/수동으로 고정 prefix prepend |
| skill | `/Users/cheonmyeongseung/cereal-ai/.agents/skills/onboarding-first-image-prompt/SKILL.md` |
| script | `/Users/cheonmyeongseung/cereal-ai/.agents/skills/onboarding-first-image-prompt/scripts/generate-prompt.mjs` |
| gate | prompt → first image → founder 승인 후 video |

### 확정 이미지 prompt 플로우

```text
Obsidian accepted prompt table
→ Gemini 3 Flash: body prompt only
→ fixed viewer-POV prefix prepend
→ image generation
→ founder gate
```

### 다음 미완료

| 항목 | 상태 |
|---|---|
| 목표 | 캐릭터를 살리는 Toptoonchat식 프롤로그 동영상 |
| 기준 | 긴 영상 1개 아님. living-still 컷 묶음 + 텍스트 + 선택지 |
| 참고 | `/Users/cheonmyeongseung/Desktop/app_reference_capture/toptoonchat/ScreenRecording_06-14-2026 09-05-15_1.MP4` |
| 테스트 캐릭터 | 야쿠자 보스 아내 x 잠입 스파이 |
| 통과 이미지 | `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-onboarding/characters/yakuza-boss-wife-spy-romance/20260613232858/first-frame.jpg` |
| p-video 테스트 | `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/experiments/video-onboarding/videos/yakuza-boss-wife-spy-romance-p-video/20260614003347/video-5s-eye-scan-living-still.mp4` |

### Toptoonchat 학습

| 관찰 | 적용 |
|---|---|
| 영상 하나가 전체 서사를 말하지 않음 | 컷별 1개 매력만 표현 |
| 장소 → 소품/몸 → 얼굴 → 선택지 | first frame도 컷 목적별로 별도 설계 |
| 동작은 작음 | 눈, 표정, 숨, 고개, 손/소품 최소 |
| 텍스트/선택지가 서사 전달 | video prompt에 과한 스토리 금지 |
| 캐릭터가 카메라를 직접 봄 | viewer-POV 고정 |

## lovescape에서 가져올 비주얼 언어
- 중앙 포커스 캐러셀: 선택 카드 확대 + 글로우 보더, 양옆 카드 살짝 보임
- 상단 아이콘 스테퍼 (진행 단계가 카테고리 아이콘으로 보임)
- 슬라이더는 카드 아래 인라인
- 필(pill) 형태 라이트 그라데이션 CTA
- 전체 다크 배경 + 비디오가 유일한 빛

## 에셋 사전 제작
| 항목 | 내용 |
|---|---|
| 소스 | 기존 approved-assets 옵션별 승인 이미지 |
| 변환 | p-video i2v, 5초 루프, 세로형 |
| 수량 | 비주얼 옵션 ~30개 (1회성) |
| 비용 | ≈ $3 (draft로 시안 → standard로 확정) |

## 검증 (프로토타입 단계)
- 성공 기준: founder가 프로토타입 라우트에서 풀 플로우 체험 후 "비디오가 짠 나타나는 경험"이 성립하는지 판정
- 측정 포인트(차후 A/B 전환 시): 온보딩 완주율, create→chat 진입율, 생성 latency 실측(p95)
- KPI 연결: 온보딩 전환 ↑ → 구독 전환 모수 ↑ (순이익 기준)

## 작업 순서
| 순서 | 작업 | 산출물 |
|---:|---|---|
| 1 | 옵션 비디오 1카테고리(얼굴) 먼저 생성 | 비디오 카드 시안 → 유저 검사 |
| 2 | 통과 시 나머지 비주얼 옵션 전체 생성 | ~30개 에셋 |
| 3 | 프로토타입 라우트 — 비디오 캐러셀 스텝 UI | 스텝 1-4 |
| 4 | recap 시퀀스 + 백그라운드 생성 파이프라인 | recap → crossfade |
| 5 | 관계/비밀 2열 이미지 grid 연결 | swipe 제거 |
| 6 | blur/teaser → start button handoff 테스트 연결 | curiosity peak 측정 |
| 7 | 풀 플로우 연결 + 유저 검사 | 프로토타입 완성 |
| 8 | 통과 시 A/B 실험 contract 별도 작성 | 새 job |
