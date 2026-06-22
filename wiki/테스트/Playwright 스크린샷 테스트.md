---
type: concept
platform: web
status: draft
tags: [frontend, testing, playwright, screenshot]
source:
  - "[[우아한형제들 시각적 회귀 테스트 요약]]"
created: 2026-06-22
updated: 2026-06-22
---

# Playwright 스크린샷 테스트

## 한 줄 요약
Playwright 스크린샷 테스트는 기준 이미지와 현재 렌더링 이미지를 비교해 UI 변화 여부를 자동으로 검출한다.

## 핵심 내용
Playwright는 브라우저에서 화면을 렌더링하고 스크린샷을 찍어 expected, actual, diff 이미지를 비교할 수 있다. 실패 시 HTML 리포트와 trace로 어느 부분이 달라졌는지 확인한다.

우아한형제들 원문은 Playwright 선택 이유로 무료 사용, 한글 지원, E2E 확장 가능성, HTML 리포트를 든다. Chromatic은 유료, BackstopJS는 한글 지원 문제로 제외했다.

## 실무 적용 흐름
1. 테스트 대상 Storybook 또는 페이지를 연다.
2. 렌더링 완료를 기다린다.
3. 스크린샷을 찍는다.
4. 기준 이미지와 비교한다.
5. 의도한 변경이면 snapshot을 갱신한다.
6. 의도하지 않은 변경이면 UI 코드를 수정한다.

## 결과물
- expected: 이전 기준 이미지
- actual: 현재 실행 결과 이미지
- diff: 두 이미지 간 차이
- HTML report: 실패 원인, 테스트 단계, 이미지 mismatch, trace 확인

## 적용 기준
- CI 같은 고정된 환경에서 실행한다.
- 브라우저, OS, font 차이를 통제한다.
- snapshot 갱신은 수동 승인 흐름을 둔다.
- diff 리포트를 MR/PR에서 바로 볼 수 있게 한다.
- 첫 실행이나 신규 컴포넌트는 기준 이미지가 없어 실패할 수 있다. 이때 생성된 이미지를 기준 이미지로 등록하는 흐름이 필요하다.
- 의도한 디자인 변경과 의도하지 않은 회귀를 리뷰에서 구분해야 한다.

## 관련 문서
- [[시각적 회귀 테스트가 필요한 이유]]
- [[스크린샷 테스트의 변동성 제어]]
- [[PR Preview 기반 UI 리뷰]]

## 출처
- [[우아한형제들 시각적 회귀 테스트 요약]]
