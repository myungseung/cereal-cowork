## 현재 상태

- Branch: `codex/love-multimedia-chat`
- Worktree: `/Users/cheonmyeongseung/cereal-ai-worktrees/love-multimedia-chat`
- Server: `3035`
- Last commit: `47a5ec2a refactor(chat): replace v12 with Risu and golden image pipeline`
- 독립 Golden Set: 14/14 사용자 PASS
- 제품 Chat 이미지: 품질 FAIL
- 판정: 제품 파이프라인 변경 전, 실제 5턴을 독립 재현 세트로 전환

## Todo

- [x] 독립 Golden Set 14종 생성·사용자 PASS — [[spec/07-13 지은 이미지 Golden Set]]
- [x] 실제 지은 Chat 대화 품질 사용자 PASS
- [x] 제품 Chat에서 이미지 생성 실행
- [ ] api-logs에서 동일 세션 5턴 수집 ← 여기서 시작
  - [ ] 유저 대사 원문
  - [ ] 지은 대사 원문
  - [ ] 직전 이미지 SceneSpec·prompt
  - [ ] 현재 이미지 SceneSpec·prompt
  - [ ] 이미지 URL·message ID·artifact ID
- [ ] 5턴을 제품 밖 독립 재현 세트로 고정
- [ ] 각 턴의 현재 이미지와 목표 장면을 한 표에 배치
- [ ] Golden Set 방식으로 5턴 재생성
  - [ ] 마지막 턴의 감정 변화 1개 선택
  - [ ] 이전 장면의 위치·의상·카메라·동작 상태 입력
  - [ ] 이전 장면에서 유지할 것과 현재 장면에서 바꿀 것 분리
  - [ ] viewerRole·구도·표정·완결 동작·몸짓·공간 증거 확정
  - [ ] image.jpg + manifest.json 저장
- [ ] 사용자 5/5 PASS
- [ ] PASS한 5개 입력→SceneSpec 흐름을 제품 파이프라인으로 치환
- [ ] 같은 5턴을 test HTML에서 E2E 재검증

## 성공한 방식

```text
현재 대화 마지막 턴
+ 이전 장면 manifest
        |
        v
현재 감정 변화 1개 선택
        |
        v
유지: 캐릭터 / 의상 / 장소 / 시간 / identity
변경: 카메라 / 표정 / 완결 동작 / 몸짓 / 공간 배치
        |
        v
sceneSpec
emotion + viewerRole + camera + expression
+ completedAction + bodyLanguage + environmentalEvidence
+ failureConditions
        |
        v
Golden Set deterministic prompt builder
        |
        v
고정 z-image-turbo + seed + LoRA
        |
        v
R2 image.jpg + manifest.json
        |
        v
사용자 PASS/FAIL
```

## 실패한 방식

- 마지막 대화만 Flash에 전달해 SceneSpec 즉석 생성
- 이전 이미지의 실제 구도·표정·동작 상태 미입력
- 감정 단어를 generic close-up·미소로 치환
- 보이지 않는 남성 신체 접촉 지시 → 자기 몸을 만지는 포즈로 변형
- `안도`를 얼굴 close-up으로 처리 → 셀피처럼 보임
- 생성 성공을 품질 PASS로 간주
- 제품 코드를 먼저 바꾸며 품질을 동시에 검증

## 실제 5턴 시작점

| 턴 | 유저 | 지은 장면 | 기존 결과 |
|---:|---|---|---|
| 1 | 직장 동료야. 왜, 신경 쓰였어? | 셔츠 깃을 잡고 연락 여부 추궁 | [이미지](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/chat/user_1783675965445_yqodif/1783948474961.jpg) |
| 2 | 대답이 마음에 안 들면 어떻게 할 건데? | 열쇠를 넣고 문을 막으며 통제 | [이미지](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/chat/user_1783675965445_yqodif/1783948485648.jpg) |
| 3 | 내 휴대폰이라도 뒤진 거야? | 턱을 잡고 연락을 끊으라고 요구 | 생성 실패 |
| 4 | 불안해서 이러는 거야? 제발 그만 좀 해, 숨 막혀. | 불안을 인정하며 도망가지 말라고 함 | [이미지](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/chat/user_1783675965445_yqodif/1783948557974.jpg) |
| 5 | 불안해하는 네가 불쌍해서 한 번만 봐준다. | 긴장이 풀리지만 소유욕은 유지 | [이미지](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/chat/user_1783675965445_yqodif/1783948590651.jpg) |

위 표는 시작점. 다음 에이전트는 api-logs raw request/response와 artifact URL로 원문을 다시 고정해야 한다.

## 다음 실행 순서

1. `/admin/api-logs?device=glass_jieun_inline_suggestions_20260710_v1&story=user_1783675965445_yqodif`에서 5턴 raw 자료 수집
2. 제품 API를 호출하지 않는 독립 실행표 작성
3. 각 턴에 이전 장면 manifest를 입력으로 연결
4. Golden Set builder로 1장씩 생성
5. 대화·이전 장면·변경 결정·SceneSpec·이미지·manifest를 한 표로 사용자에게 제시
6. 사용자 FAIL은 장면 기획만 수정해 재생성
7. 5/5 PASS 후에만 제품 코드 변경

## Key files

| 파일 | 이유 |
|---|---|
| `spec/07-13 지은 이미지 Golden Set.md` | 성공한 14종 장면·prompt·manifest SOT |
| `lib/chat/risu/image.ts` | 현재 실패 중인 제품 SceneSpec·prompt 흐름 |
| `app/api/characters/[storyId]/chat/image/generate/route.ts` | 제품 이미지 생성 진입점 |
| `app/admin/api-logs/page.tsx` | 실제 5턴 raw 자료 확인 UI |
| `public/tmp-video-chat-ui-preview.html` | 최종 E2E 화면 |

## 작업 경계

- 현재 worktree에 미커밋 변경 존재
- 변경 파일: Risu prompt·history·image·test HTML
- 독립 5턴 5/5 PASS 전 기존 변경을 커밋하지 않는다
- 사용자 변경과 섞지 않는다

## DO NOT

- 제품 파이프라인부터 다시 수정하지 않기
- 감정 단어만으로 이미지 생성하지 않기
- 이전 장면 manifest 없이 연속 장면 만들지 않기
- 보이지 않는 남성의 손·가슴·셔츠·목을 직접 묘사하지 않기
- 셀피·generic beauty shot을 1인칭 관계 장면으로 판정하지 않기
- 생성 성공과 사용자 PASS를 혼동하지 않기
- 5/5 PASS 전 일반화하지 않기
