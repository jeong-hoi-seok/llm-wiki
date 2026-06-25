---
type: source-summary
platform: web
status: summarized
tags: [frontend, testing, msw, storybook, jest]
source: "https://tech.kakaoent.com/front-end/2022/220317-integrate-msw-storybook-jest/"
created: 2026-06-22
updated: 2026-06-22
---

# 카카오엔터 MSW 모킹 재사용 요약

## 한 줄 요약
카카오엔터테인먼트 MSW 모킹 재사용 글은 Storybook과 Jest/Testing Library에서 같은 API 시나리오를 중복 작성하지 않고 공유하는 방향을 설명한다.

## 핵심 내용
- 글의 문제의식은 Storybook Story에는 이미 MSW handler가 있는데, Jest + Testing Library 테스트에서 `server.use(handler)`를 한 번 더 써야 하는 중복이다.
- `msw-storybook-addon`은 Storybook에서 `parameters.msw.handlers`를 읽어 MSW handler를 적용한다.
- `@storybook/testing-react`의 `composeStories`를 쓰면 Storybook Story를 테스트 컴포넌트로 변환할 수 있다.
- 하지만 `composeStories`만으로는 Story의 MSW handler가 Jest에 자동 적용되지 않는다.
- 해결책은 Testing Library의 `render`를 custom render로 감싸고, 렌더링할 Story의 `story.type.parameters?.msw?.handlers`를 읽어 `server.use(...)`에 등록하는 방식이다.
- 테스트 사이드 이펙트는 `beforeAll(server.listen)`, `afterEach(server.resetHandlers)`, `afterAll(server.close)`로 정리한다.
- Node.js 환경 MSW는 절대경로 매칭 이슈가 있다. 원문은 tagged template literal로 Node일 때만 `http://localhost` prefix를 붙이는 `testUrl` 방식을 제안한다.
- 핵심은 “Storybook에서 정의한 API 시나리오를 Jest 테스트에서 다시 쓰되, 테스트 본문에는 모킹 준비 코드가 보이지 않게 하는 것”이다.

## 위키 반영
- [[테스트 Mock·Fixture 재사용]] — handler 재사용, fixture 중복 제거, 시나리오 자산화

## 출처
- 카카오엔터테인먼트 테크블로그, MSW 모킹 코드 재사용하기 feat. Storybook, Jest: https://tech.kakaoent.com/front-end/2022/220317-integrate-msw-storybook-jest/
- 로컬 원문: `raw/testing-sources/카카오엔터 MSW 모킹 코드 재사용 원문.txt`
