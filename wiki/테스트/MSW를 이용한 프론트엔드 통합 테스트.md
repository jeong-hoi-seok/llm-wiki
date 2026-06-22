---
type: concept
platform: web
status: draft
tags: [frontend, testing, msw, integration-test]
source:
  - "[[카카오엔터 MSW 통합 테스트 요약]]"
created: 2026-06-22
updated: 2026-06-22
---

# MSW를 이용한 프론트엔드 통합 테스트

## 한 줄 요약
MSW를 이용하면 테스트에서 API 응답을 통제하면서도 프론트엔드의 실제 요청 흐름과 UI 반응을 함께 검증할 수 있다.

## 핵심 내용
MSW는 네트워크 요청을 가로채는 mock 도구다. 테스트 코드에서 API client 함수를 직접 바꿔치기하는 대신 요청 계층에서 응답을 제어하므로 실제 앱 흐름과 더 가깝다.

카카오엔터테인먼트 예시는 검색 페이지다. 사용자가 검색어를 입력하고 Enter를 누르면 `/search` API를 호출하고, 응답 데이터를 결과 리스트에 렌더링한다. API가 아직 없거나 원하는 데이터를 주지 않으면 통합 테스트는 실패한다. MSW는 이 요청을 가로채 사전에 준비한 응답을 내려주어 “입력 → API 호출 → 결과 렌더링” 흐름을 테스트 가능하게 만든다.

## 기본 구성
일반적인 Jest + Testing Library 환경에서는 다음 파일을 둔다.

- `src/mocks/handlers.ts` — 사용할 API handler 목록
- `src/mocks/server.ts` — `setupServer(...handlers)` 구성
- `setupTests.ts` 또는 `jest.setup.js` — `server.listen`, `server.resetHandlers`, `server.close`
- `src/mocks/api/*` — API별 handler
- `src/mocks/api/data/*` — 응답 fixture

handler와 data를 분리하면 API 종류가 늘어도 테스트 setup이 한 파일에 몰리지 않는다.

## 실무 적용 기준
- 공통 server setup을 테스트 환경에 둔다.
- API handler는 도메인·기능 단위로 분리한다.
- 응답 fixture는 별도 data 파일로 분리한다.
- 테스트별로 필요한 응답만 `server.use(...)`로 override한다.
- 성공, 조건부 렌더링, 빈 응답, 오류 응답을 분리해 검증한다.
- 테스트 후 `server.resetHandlers()`로 런타임 handler를 초기화한다.

## 적합한 경우
- 서버 응답에 따라 화면 상태가 달라진다.
- API 실패 처리가 중요하다.
- Storybook과 테스트에서 같은 응답 시나리오를 쓰고 싶다.
- 개발 서버 없이 프론트엔드 흐름을 검증해야 한다.

## 주의점
- MSW를 쓴다고 모든 테스트가 통합 테스트가 되는 것은 아니다. API 응답에 따라 여러 컴포넌트가 함께 변할 때 효과가 크다.
- fixture가 실제 API 스펙과 어긋나면 테스트 신뢰도가 떨어진다.
- 테스트마다 `server.use`로 바꾼 handler를 초기화하지 않으면 다른 테스트에 영향을 준다.
- Node.js 환경에서는 요청 URL 매칭 방식이 브라우저와 다를 수 있다. 상대경로/절대경로 정책을 팀에서 정해야 한다.

## 관련 문서
- [[컴포넌트와 API를 함께 테스트하는 방법]]
- [[Storybook과 테스트에서 MSW 핸들러 재사용하기]]
- [[성공·빈 응답·오류 응답 테스트]]

## 출처
- [[카카오엔터 MSW 통합 테스트 요약]]
