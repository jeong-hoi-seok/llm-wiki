---
type: concept
platform: web
status: draft
tags: [frontend, testing, msw, fixture, storybook, scenario]
source:
  - "[[카카오엔터 MSW 모킹 재사용 요약]]"
  - "[[29CM Storybook 운영 방식 요약]]"
created: 2026-06-22
updated: 2026-06-23
---

# 테스트 Mock·Fixture 재사용

> API mock·fixture·시나리오를 테스트마다 흩뿌리지 말고 **제품 상태를 표현하는 재사용 자산**으로 관리. Storybook·테스트·PR Preview가 같은 시나리오를 공유한다.

## Storybook과 테스트에서 MSW 핸들러 재사용
API 응답 상태(성공·빈·오류·지연)는 Storybook에도 테스트에도 필요하다. 각각 다시 작성하면 실제 제품 상태와 멀어진다. MSW handler를 공통 모듈로 관리하면 하나의 시나리오를 Storybook·RTL·Jest/Vitest·개발 mock 환경에서 함께 쓴다.

카카오엔터가 짚는 구체 문제: Story에 이미 `parameters.msw.handlers`가 있는데 Jest 테스트에서 다시 `server.use(handler)`를 호출하는 중복. `@storybook/testing-react`의 `composeStories`로 Story를 재사용해도 MSW handler는 자동 재사용되지 않는다.

**해결** — Testing Library `render`를 custom render로 감싸 렌더링할 Story의 `story.type.parameters?.msw?.handlers`를 읽고 `server.use(...)`로 등록. 결과: 테스트 본문에서 `server.use`가 사라지고, Story의 mock 시나리오를 테스트가 그대로 쓰며, 테스트는 "무엇을 검증하는지"에 집중.

필수 cleanup — `beforeAll(server.listen)` / `afterEach(server.resetHandlers)` / `afterAll(server.close)`.

**Node.js URL 주의** — Node 테스트에서 MSW가 절대경로를 요구할 수 있다. 앱·테스트 모두 `testUrl\`/api/foo\`` 같은 tagged template을 써서 브라우저는 상대경로, Node는 `http://localhost` prefix를 붙이는 식으로 해결.

## API Fixture 중복 제거
중복 fixture는 시간이 지나면 서로 다른 사실을 말한다(Storybook은 실패 응답인데 테스트는 성공만, schema 변경이 한쪽만 반영).

- fixture는 endpoint·domain·scenario 기준으로 나누고, 성공·빈·실패·지연을 명시적으로 둔다.
- 테스트마다 필요한 값만 override. 복잡하면 builder 함수. schema 변경 시 fixture도 갱신.
- Story의 `parameters.msw.handlers`를 테스트에서 재사용 → 본문에는 mock 준비보다 사용자 행동·assertion이 먼저 보이게.
- **나쁜 신호** — 같은 JSON이 여러 파일에 복사 / Storybook과 테스트의 mock이 다름 / `mockData1`·`testData` 같은 무의미한 이름 / 실패 케이스 fixture 없음 / Story handler를 테스트에서 또 `server.use`로 등록.

## 테스트 시나리오를 자산으로 관리하기
좋은 시나리오는 테스트·Storybook·QA·문서에서 반복 재사용된다. 코드에 숨기지 말고 이름·구조를 갖춰 관리한다. 29CM은 Storybook을 컴포넌트 명세서로 쓰고, 카카오엔터는 그 Story handler를 Jest에서 재사용한다 → 시나리오가 세 영역을 잇는 자산이 된다: Storybook의 눈으로 보는 UI 상태 / 자동화 테스트가 검증하는 상태 / PR Preview에서 공유하는 상태.

관리 대상 — 성공·빈·오류·권한 없음·긴 콘텐츠·느린 응답·서버 상태 변경·주요 사용자 흐름.

- 시나리오 이름은 제품 언어로. fixture·handler는 시나리오 단위로 찾게 둔다.
- Story 이름과 테스트 케이스 이름을 가깝게 유지. 장애 케이스는 재발 방지 시나리오로 추가.
- PR 설명·리뷰 코멘트에 관련 Story URL 첨부. 신규 입사자가 Storybook만 봐도 상태를 이해하게.

## 관련 문서
- [[MSW 통합 테스트]] · [[Storybook 기반 컴포넌트 개발]] · [[통합 테스트]]

## 출처
- [[카카오엔터 MSW 모킹 재사용 요약]] · [[29CM Storybook 운영 방식 요약]]
