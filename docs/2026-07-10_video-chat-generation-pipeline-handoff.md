> 상태: 실행 전 handoff
> 작성: 2026-07-10
> 관련: [[06-30 동영상 대화로 애정 만들기]], [[2026-07-10_video-chat-lab-handoff]]

## 목표

- 제품과 독립된 검증 스크립트로 `실제 채팅 → 장면 결정 → 영상 생성 → 품질·비용·시간 판정`을 재현한다.
- 3개 대화 상황에서 만족스러운 p-video 결과를 안정적으로 얻는 최소 구조를 찾는다.
- 제품 UI, 채팅 로직, 실험 배정, 크레딧 차감은 다루지 않는다.

## 현재 상태

| 항목 | 상태 |
|---|---|
| Glass 채팅 UI | 별도 에이전트 담당 |
| 테스트 캐릭터 | 지은, `user_1783675965445_yqodif` |
| 채팅 미리보기 | `http://127.0.0.1:3035/tmp-video-chat-ui-preview.html?...` |
| 실제 3개 채팅 로그 | 미수집 |
| 3개 기대 영상 명세 | 초안만 있음, 로그 수집 후 동결 필요 |
| 독립 검증 스크립트 | 미구현 |
| 신규 영상·비용·시간 측정 | 미실행 |

## 완료 기준

- 실제 채팅 모델로 3개 상황을 진행한다.
- 각 케이스의 raw 요청, raw 응답, 전체 history를 저장한다.
- 저장된 최종 에이전트 응답과 history만 영상 파이프 입력으로 사용한다.
- 케이스당 영상 1개를 생성한다. 첫 검증에서 변형 생성은 하지 않는다.
- 각 생성은 기존 `runs.jsonl`에도 기록한다.
- 결과 표에 품질 PASS/FAIL, 총 생성시간, provider 시간, 실제/추정 비용, R2 URL을 남긴다.
- 사용자가 3개 영상을 직접 보고 최종 품질을 판정한다.

## 실행 순서

- [ ] 1. 실제 채팅 로그 3개 수집
  - [ ] 케이스별 고유 `deviceId` 사용
  - [ ] raw request/response/history 저장
  - [ ] 모델 응답을 수정·요약·재작성하지 않고 SOT로 동결
- [ ] 2. 기대 영상 명세 동결
  - [ ] 로그의 실제 발화와 감정 맥락에 맞춰 `expected.json` 확정
  - [ ] 정체성, 표정, 동작, 구도, 금지사항 기록
- [ ] 3. 독립 검증 스크립트 구현
  - [ ] 기존 video-chat-lab runner와 기록 함수를 재사용
  - [ ] 단계별 실행과 단일 케이스 재실행 지원
- [ ] 4. p-video 모션 영상 3개 생성
  - [ ] 720p, draft OFF, 케이스당 1개
  - [ ] raw provider payload/response와 결과 URL 저장
- [ ] 5. 품질·비용·속도 보고
  - [ ] 자동 수치와 사람 품질 판정을 분리
  - [ ] FAIL 원인을 케이스별 1개 주원인으로 기록
- [ ] 6. 모션 3개 PASS 후 음성·립싱크 게이트 시작

## 대화 케이스

| ID | 유저 입력 | 감정 목적 | 고유 deviceId |
|---|---|---|---|
| `jealousy_claim` | `직장 동료야. 왜, 신경 쓰였어?` | 절제된 질투와 관계 주장 | `video_eval_jealousy_<runId>` |
| `comfort_after_bad_day` | `오늘 너무 힘들었어. 그냥 네 목소리 듣고 싶었어.` | 걱정과 정서적 돌봄 | `video_eval_comfort_<runId>` |
| `intimate_approach` | `나도 보고 싶었어. 조금 더 가까이 와줄래?` | 친밀감 상승과 다음 기대 | `video_eval_approach_<runId>` |

규칙:

- 각 케이스는 새 대화 상태에서 시작한다.
- 캐릭터의 실제 첫 시퀀스 이후 위 입력까지 자연스럽게 진행한다.
- 중간 대화와 최종 에이전트 응답도 전부 history에 포함한다.
- 예상과 다른 응답이 나와도 응답을 고치지 않는다. 기대 영상 명세를 실제 맥락에 맞춘다.

## 기대 영상 초안

| ID | 보여야 할 것 | 실패 |
|---|---|---|
| `jealousy_claim` | 시선 회피 후 재접촉, 눌린 질투, 고정된 미디엄 클로즈업 | 밝은 미소, 과장된 분노, 얼굴 변화, 타인 등장 |
| `comfort_after_bad_day` | 부드러워진 눈, 작은 끄덕임, 약한 상체 접근 | 과장된 울음, 무관한 미소, 카메라 이동, 정체성 변화 |
| `intimate_approach` | 시선 유지, 미세한 미소, 프레임 안의 절제된 접근 | 급격한 줌, 카메라 키스, 손·신체 난입, 구도 붕괴 |

공통:

- 한 인물만 등장한다.
- 얼굴, 헤어, 의상, 배경을 입력 이미지와 일치시킨다.
- 텍스트 없이도 목표 감정이 읽혀야 한다.
- 동작은 초반부터 시작하고 물리적으로 자연스러워야 한다.
- provider prompt는 수동 작성하지 않는다. 현재 장면 결정 체인의 실제 출력을 저장·사용한다.

## 검증 스크립트 계약

- 위치: `/Users/cheonmyeongseung/.codex/skills/video-chat-lab/scripts/eval-chat-to-video.mjs`
- 출력: `/Users/cheonmyeongseung/.codex/skills/video-chat-lab/tmp/chat-video-eval/<runId>/`
- 실행 단계:
  - `--capture-chat`
  - `--generate`
  - `--report`
  - `--all`
  - `--case <caseId>`
- 3개 로그와 기대값이 모두 동결되기 전 `--generate` 금지.

케이스별 저장: `chat-request.json`, `chat-response.json`, `history.json`, `expected.json`, `scene-decision.json`, `provider-payload.json`, `provider-response.json`, `result.json`.

실행 단위 저장:

```text
<runId>/
├── cases.json
├── case-<id>/...
├── report.json
└── report.md
```

## 생성 파이프 기준

- 기준 모델: `prunaai/p-video`
- 기준 설정: 720p, draft OFF, 24fps
- 현재 canonical runner 기본 길이: 7초
- 입력 이미지: 지은 프로필 원본
  - `https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/profile/preview_1783675953819/1783675958254.jpg`
- canonical 기록:
  - `/Users/cheonmyeongseung/.codex/skills/video-chat-lab/tmp/video-chat-lab/runs.jsonl`
- 필수 필드:
  - `prompt`
  - `providerPayload`
  - `resultR2Url`
  - `manifestR2Url`
  - `thumbR2Url`
  - `durationMs.total`
  - `costUsd`

## 품질 게이트

| Gate | PASS |
|---|---|
| Identity | 얼굴·헤어·의상·배경 유지 |
| Emotion | 텍스트 없이 목표 감정 식별 가능 |
| Motion | 즉시 시작, 자연스러운 표정·몸 동작 |
| Framing | 한 인물, 안정된 카메라·구도 |
| Record | raw payload/response, R2 결과, 시간·비용 저장 |

- 위 5개 중 하나라도 실패하면 해당 케이스 FAIL.
- 음성·립싱크는 별도 Gate다. 무음 p-video가 PASS여도 완성 영상 PASS로 쓰지 않는다.
- 모션 3개 PASS 전 audio-gen 또는 립싱크 provider를 연결하지 않는다.

## 비용·속도 기준

- 사용자 제공 가격: 720p Standard, base, draft OFF = `$0.02/초`.
- 7초 기준 기대 비용: `$0.14/개`, 3개 `$0.42`.
- 현재 runner 상수는 `$0.025/초`라 7초를 `$0.175`로 기록한다.
- 검증 스크립트는 아래를 함께 기록한다.
  - provider 또는 billing 실제 비용
  - runner 기록 비용
  - `$0.02/초` 환산 비용
- 가격 확인 전 canonical runner 상수를 수정하지 않는다.
- 과거 p-video 720p 기록 30건: 총시간 최소 `4.846초`, 중앙값 `8.154초`, 최대 `20.984초`.
- 신규 3개 임시 속도 기준:
  - 목표: 중앙값 `12초 이하`
  - FAIL: 개별 총시간 `20초 초과`
- 최종 속도 기준은 신규 3개 실측 후 확정한다.

## 보고 형식

| case | chat trace | expected | result | 품질 | 총시간 | provider 시간 | 실제 비용 | `$0.02/초` 비용 | 메모 |
|---|---|---|---|---|---:|---:|---:|---:|---|

## 예상 작업량

- 예상: `3~6시간`
- 구성:
  - 케이스·로그 계약: `0.5시간`
  - 스크립트·로그 수집: `1~2시간`
  - 3개 생성·판정: `1~2.5시간`
  - 보고: `0.5~1시간`
- 추가 위험: p-video가 음성·립싱크를 해결하지 못하면 별도 provider 검증 `+2시간`.

## 핵심 파일

1. `/Users/cheonmyeongseung/.codex/skills/video-chat-lab/SKILL.md`
2. `/Users/cheonmyeongseung/.codex/skills/video-chat-lab/scripts/video-chat-lab.mjs`
3. `/Users/cheonmyeongseung/.codex/skills/video-chat-lab/tmp/video-chat-lab/runs.jsonl`
4. `/Users/cheonmyeongseung/cereal-ai-worktrees/love-multimedia-chat/tmp-video-chat-ui-preview.html`
5. 이 문서

## 금지

- 제품 코드, UI, 실험 배정, 크레딧 로직 수정 금지.
- 모델 응답 또는 provider prompt 수동 보정 금지.
- 3개 로그 동결 전 영상 생성 금지.
- 한 케이스에 여러 변형을 만들어 선택하는 방식 금지.
- local/provider URL만으로 성공 보고 금지. R2와 manifest 기록 필수.
- 음성·립싱크 미검증 상태를 완성 영상 PASS로 보고 금지.

## 다음 작업 시작점

- 첫 미완료 항목 `1. 실제 채팅 로그 3개 수집`부터 시작한다.
- 추가 확인 없이 위 케이스와 저장 계약을 사용한다.
