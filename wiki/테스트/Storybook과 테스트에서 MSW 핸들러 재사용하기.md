---
type: concept
platform: web
status: draft
tags: [frontend, testing, msw, storybook]
source:
  - "[[카카오엔터 MSW 모킹 재사용 요약]]"
  - "[[29CM Storybook 운영 방식 요약]]"
created: 2026-06-22
updated: 2026-06-22
---

# Storybook과 테스트에서 MSW 핸들러 재사용하기

## 한 줄 요약
Storybook과 자동화 테스트에서 같은 MSW handler를 재사용하면 API 시나리오 중복과 fixture 불일치를 줄일 수 있다.

## 핵심 내용
API 응답 상태는 Storybook에서도 필요하고 테스트에서도 필요하다. 성공, 빈 상태, 오류, 지연 응답을 각각 다시 작성하면 실제 제품 상태와 멀어지기 쉽다.

MSW handler를 공통 모듈로 관리하면 하나의 시나리오를 다음 환경에서 함께 쓸 수 있다.

- Storybook
- React Testing Library
- Jest/Vitest
- 개발용 mock 환경

카카오엔터테인먼트 원문은 더 구체적인 문제를 다룬다. Storybook Story에 이미 `parameters.msw.handlers`가 있는데, Jest 테스트에서 다시 `server.use(handler)`를 호출해야 하는 중복이다. `@storybook/testing-react`의 `composeStories`로 Story를 테스트 컴포넌트로 재사용해도 MSW handler는 자동 재사용되지 않는다.

## 원문 해결 방식
Testing Library의 `render`를 custom render로 감싼다. custom render가 렌더링할 Story의 `story.type.parameters?.msw?.handlers`를 읽고 `server.use(...)`로 등록한다.

결과:
- 테스트 본문에서 `server.use(handler)`가 사라진다.
- Storybook에 정의한 mock 시나리오를 Jest 테스트가 그대로 쓴다.
- 테스트 코드는 “무엇을 검증하는지”에 집중한다.

필수 cleanup:
- `beforeAll(() => server.listen())`
- `afterEach(() => server.resetHandlers())`
- `afterAll(() => server.close())`

## 실무 적용 기준
- handler는 API endpoint 기준으로 관리한다.
- scenario는 제품 상태 기준으로 이름 짓는다.
- Story는 필요한 scenario handler를 조합한다.
- 테스트는 Story 또는 handler를 재사용하되 검증 의도를 숨기지 않는다.
- custom render가 너무 마법처럼 동작하지 않게 팀 문서에 동작 방식을 남긴다.
- 테스트별 handler override가 필요한 경우 Story handler와 테스트 override의 우선순위를 명확히 한다.

## Node.js URL 주의
원문은 Node.js에서 MSW가 절대경로를 요구하는 문제를 다룬다. 브라우저 앱 코드에서는 `/api/...` 같은 상대경로를 쓰고 싶은데, Node 테스트에서는 `http://localhost/api/...`처럼 절대경로가 필요할 수 있다.

해결 아이디어:
- 앱 코드와 테스트 코드 모두 `testUrl\`/api/foo\`` 같은 tagged template literal을 사용한다.
- 브라우저에서는 상대경로를 반환한다.
- Node.js에서는 `http://localhost` prefix를 붙인다.

## 관련 문서
- [[API Fixture 중복 제거]]
- [[테스트 시나리오를 자산으로 관리하기]]
- [[Storybook에서 API 상태 표현하기]]

## 출처
- [[카카오엔터 MSW 모킹 재사용 요약]]
- [[29CM Storybook 운영 방식 요약]]
