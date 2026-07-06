---
date: 2026-05-29
date_end: 2026-05-29
category: organic-content
type: experiment
status: done
kpi: Shorts views
expected: 재확인 필요
---

## 완료

- 실패.
- 결과: 0x.
- 근거: YouTube 1 Shorts 조회수 12.

## 가설

`@852wa` (anime POV 숏츠) 의 인간버전 → 동등 이상 조회수.

## 근거

- 852wa = 일본어 여캐 POV, 각각 다른 성격 (츤데레/멘헤라/헌신/도발), `#animation`
- 애니메이션보다 실사 인간버전이 감정 이입 더 쉬움 = 시청 유지율 ↑
- AI 영상 생성으로 실사 POV = 차별화 + 제작비 낮음

## 타겟 채널

https://www.youtube.com/@852wa

## 분석 숏츠 (6개)

| 링크 | 캐릭터 성격 | 대사 요지 |
|---|---|---|
| https://www.youtube.com/shorts/7SET4XaLxxg | 도발/츤데레 | "쳐다보는 거 재미있어?" |
| https://www.youtube.com/shorts/w3bkA2DRLCY | 무기력/유혹 | "아무것도 안 할 거야, 그래도 돼?" |
| https://www.youtube.com/shorts/7h0jrM6CvbM | 겉강/조롱 | "무서워하는 거 귀여워!" |
| https://www.youtube.com/shorts/kg-sImcruLg | 비꼼/도발 | "또 같은 상황 만들 거야?" |
| https://www.youtube.com/shorts/dIxNr4zKW8w | 순수/헌신 | "뭐든지 할게…뜨거운 물보단 찬물이…" |
| https://www.youtube.com/shorts/y5G2M3n5FUM | 집착/멘헤라 | "어디 가는데?" |

## 실행 파이프라인

1. 대상 숏츠 → Gemini 2.5 Pro video input → 대본 + 시각 흐름 추출
2. 추출 데이터 → 인간버전 기획 (컨셉, img prompt, image gen, video prompt, video gen)
3. 영상 생성 + 업로드
4. 48h 조회수 측정

## 성공 기준

- 1개 이상 숏츠 조회수 10K+ (첫 테스트)
- 인간버전이 원본 애니메이션 버전보다 CTR/유지율 높은지 확인
