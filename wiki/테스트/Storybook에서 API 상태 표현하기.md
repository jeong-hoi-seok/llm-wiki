---
type: concept
platform: web
status: draft
tags: [frontend, testing, storybook, msw]
source:
  - "[[29CM Storybook 운영 방식 요약]]"
  - "[[카카오엔터 MSW 모킹 재사용 요약]]"
created: 2026-06-22
updated: 2026-06-22
---

# Storybook에서 API 상태 표현하기

## 한 줄 요약
API를 호출하는 컴포넌트는 Storybook에서 MSW로 성공, 빈 상태, 오류, 지연 응답을 표현해야 실제 UI 상태를 리뷰할 수 있다.

## 핵심 내용
API가 있는 컴포넌트는 props만으로 충분하지 않다. 서버 응답에 따라 loading, empty, error, success UI가 달라진다. Storybook에서 이 상태를 표현하면 개발 중에도 실제 화면 상태를 빠르게 확인할 수 있다.

29CM 글의 예시는 상품 상세페이지 이벤트 배너 영역이다. 실제 API에 이벤트 배너가 있더라도 MSW로 빈 값을 응답하게 만들면 “이벤트 배너가 없는 상태”를 Storybook에서 확인할 수 있다. 즉 Storybook은 정상 데이터만 보여주는 카탈로그가 아니라, 서버 응답별 UI 상태를 고정해 보는 환경이다.

## 실무 적용 기준
- Story별로 MSW handler를 연결한다.
- 응답 상태는 제품 언어로 이름 짓는다.
- loading과 delay 상태를 별도 Story로 둔다.
- 긴 콘텐츠, 없는 이미지, 권한 없음 같은 경계 상태를 포함한다.
- 테스트에서 같은 handler를 재사용할 수 있게 만든다.
- API 상태가 바뀌는 컴포넌트는 Story 없이 PR을 올리지 않는 것을 기준으로 삼을 수 있다.
- Story는 디자이너, QA, 리뷰어가 이해할 수 있는 이름이어야 한다.

## Story로 남기기 좋은 API 상태
- 데이터 있음
- 데이터 없음
- 일부 필드 없음
- 이미지 없음
- 긴 텍스트
- 요청 지연
- 서버 오류
- 권한 없음

## 관련 문서
- [[Storybook과 테스트에서 MSW 핸들러 재사용하기]]
- [[성공·빈 응답·오류 응답 테스트]]
- [[컴포넌트와 Story를 함께 관리하기]]

## 출처
- [[29CM Storybook 운영 방식 요약]]
- [[카카오엔터 MSW 모킹 재사용 요약]]
