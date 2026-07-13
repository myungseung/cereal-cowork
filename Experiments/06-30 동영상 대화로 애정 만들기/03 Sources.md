## 실험 배경

- KPI: 결제 유저 D1 retention `33% → 60%`, `1.8x`
- 구 스펙: [[spec/06-30 동영상 대화로 애정 만들기 - 스펙]]
- 영상 파이프 계획: [[docs/2026-07-10_video-chat-generation-pipeline-handoff]]
- 영상 Lab 기록 규칙: [[docs/2026-07-10_video-chat-lab-handoff]]

## 품질 기준

- Golden Set 원문: [[Research/07-10 Toptoon Chat 첫 대화 Golden Set Raw]]
- Golden Set 분석: [[Research/07-10 Toptoon Chat 첫 대화 Golden Set 분석]]
- 캐릭터 manifest: [열기](https://pub-7b95b4d9166a41488c47d4653ea8db54.r2.dev/profile/preview_1783700936593/manifest.json)
- 이미지 설정: Z-Image Turbo + NiceGirls `0.5` + Hyper-SD `0.2` + Mystic `0.3`
- 고정 seed: `4170581481`

## 생성 근거

- 제품 Chat 요청·응답: `/Users/cheonmyeongseung/cereal-ai/.agents/runtime/video-chat-gallery-t2i-turn-test/chat-log.json`
- 장면 계획: `/Users/cheonmyeongseung/cereal-ai/.agents/runtime/video-chat-gallery-t2i-turn-test/scene-plans.json`
- 생성 결과: `/Users/cheonmyeongseung/cereal-ai/.agents/runtime/video-chat-gallery-t2i-turn-test/result.json`
- 판정 규칙: 생성 성공과 품질 PASS 분리. 품질 판정은 사용자만 확정
