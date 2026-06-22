---
type: concept
platform: web
status: draft
tags: [frontend, testing, fixture, msw]
source:
  - "[[카카오엔터 MSW 모킹 재사용 요약]]"
created: 2026-06-22
updated: 2026-06-22
---

# API Fixture 중복 제거

## 한 줄 요약
API fixture는 테스트 파일마다 흩뿌리지 말고 제품 상태를 표현하는 재사용 자산으로 관리해야 한다.

## 핵심 내용
중복 fixture는 시간이 지나면 서로 다른 사실을 말한다. Storybook에서는 실패 응답인데 테스트에서는 성공 응답만 있거나, API schema 변경이 한쪽에만 반영되는 문제가 생긴다.

카카오엔터테인먼트의 MSW 재사용 글은 fixture 중복보다 한 단계 더 구체적으로, Story에 이미 붙어 있는 MSW handler를 테스트에서 다시 등록하는 중복을 지적한다. 단순히 handler를 별도 파일로 빼서 import해도 테스트 본문에는 `server.use(handler)`가 남는다. 이 코드는 검증 의도와 직접 관련 없는 부가 정보다.

## 실무 적용 기준
- fixture는 endpoint, domain, scenario 기준으로 나눈다.
- 성공, 빈 응답, 실패, 지연 응답을 명시적으로 둔다.
- 테스트마다 필요한 값만 override할 수 있게 한다.
- fixture가 너무 복잡하면 builder 함수를 둔다.
- 실제 API schema 변경 시 fixture도 같이 갱신한다.
- Storybook Story의 `parameters.msw.handlers`를 테스트에서 재사용할 수 있게 한다.
- handler 재사용 구조가 있다면 테스트 본문에는 mock 준비 코드보다 사용자 행동과 assertion이 먼저 보이게 한다.

## 나쁜 신호
- 같은 JSON이 여러 테스트 파일에 복사되어 있다.
- Storybook과 테스트의 mock 데이터가 다르다.
- fixture 이름이 `mockData1`, `testData`처럼 의미가 없다.
- 실패 케이스 fixture가 없다.
- Story에는 handler가 있는데 테스트에서도 같은 handler를 다시 `server.use`로 등록한다.

## 관련 문서
- [[Storybook과 테스트에서 MSW 핸들러 재사용하기]]
- [[테스트 시나리오를 자산으로 관리하기]]
- [[성공·빈 응답·오류 응답 테스트]]

## 출처
- [[카카오엔터 MSW 모킹 재사용 요약]]
