---
type: concept
platform: web
status: draft
tags: [frontend, testing, msw, integration-test, error-state, empty-state, server-state]
source:
  - "[[카카오엔터 MSW 통합 테스트 요약]]"
created: 2026-06-22
updated: 2026-06-23
---

# MSW 통합 테스트

> MSW로 API 응답을 통제하면서 실제 요청 흐름과 UI 반응을 함께 검증. 성공뿐 아니라 빈 응답·오류·서버 상태 변경까지 분리해 검증한다.

## MSW로 요청 가로채기
MSW는 네트워크 요청을 가로채는 mock 도구다. API client 함수를 직접 바꿔치기하는 대신 요청 계층에서 응답을 제어하므로 실제 앱 흐름에 가깝다.

카카오엔터 예시: 검색어 입력 후 Enter → `/search` 호출 → 응답을 결과 리스트에 렌더링. API가 없거나 원하는 데이터를 안 주면 통합 테스트는 실패한다. MSW가 요청을 가로채 사전 준비한 응답을 내려 "입력 → API 호출 → 결과 렌더링" 흐름을 테스트 가능하게 만든다.

**기본 구성** (Jest + Testing Library)
- `src/mocks/handlers.ts` — 사용할 handler 목록
- `src/mocks/server.ts` — `setupServer(...handlers)`
- `setupTests.ts` — `server.listen` / `server.resetHandlers` / `server.close`
- `src/mocks/api/*` — API별 handler / `src/mocks/api/data/*` — 응답 fixture

handler와 data를 분리하면 API가 늘어도 setup이 한 파일에 몰리지 않는다.

**적합한 경우** — 서버 응답에 따라 화면이 달라짐 / API 실패 처리가 중요 / Storybook과 테스트에서 같은 시나리오 사용 / 개발 서버 없이 흐름 검증.

**주의** — MSW를 쓴다고 모든 테스트가 통합이 되진 않는다(여러 컴포넌트가 함께 변할 때 효과 큼) / fixture가 실제 스펙과 어긋나면 신뢰도 하락 / `server.use` handler를 초기화 안 하면 다른 테스트에 영향 / Node 환경은 URL 매칭이 브라우저와 달라 상대·절대경로 정책을 정해야 함.

## 성공·빈 응답·오류 응답
장애는 성공 흐름보다 비정상 응답에서 자주 드러난다. 데이터 없음·필드 누락·권한 없음·서버 실패에서 UI가 무너지지 않아야 한다.

검증할 상태:
- **loading** 요청 중 안내 / **success** 정상 렌더링 / **empty** 빈 상태 메시지와 다음 행동 / **error** 오류 메시지와 재시도 / **edge** 값 누락·긴 텍스트·비정상 수량.

- handler를 상태별로 분리하고, empty·error를 success 안에 섞지 않는다.
- 사용자에게 보이는 복구 경로를 검증(에러 로그보다 화면 피드백 우선).
- 404·403은 서버와 합의한 정책에 맞춰 테스트. 공통 interceptor를 쓰면 error handler 호출 여부도 별도 테스트.
- 판단 예: 빈 배열과 404는 같은 "결과 없음" UI라도 원인이 다르니 케이스를 분리한다.
- **나쁜 신호** — 성공 응답만 테스트 / 빈 응답을 눈으로만 확인 / 에러에서 안 죽는지만 보고 메시지는 미검증.

## 서버 상태가 변경되는 시나리오
실제 CRUD가 아니라 테스트 런타임에 `server.use(...)`로 응답을 동적 교체해 조건부 렌더링·후속 UI를 검증한다. 예: 검색 응답 `type`을 `rect`→`rounded`로 바꿔 카드 스타일 변화 확인.

유용한 상황 — 같은 API가 다른 shape을 줄 때 / 응답 값에 따라 variant가 바뀔 때 / 특정 테스트만 빈·오류 응답 필요 / fixture 새로 안 만들고 일부 값만 바꿀 때.

흐름: 기본 handler·fixture 준비 → 특정 테스트에서 `server.use`로 override → 사용자 행동 → UI 변화 대기 → class·text·role·fallback 검증.

**주의** — 테스트 간 서버 상태 공유 시 결과가 흔들림 / fixture 객체를 직접 mutate 말고 새 객체로 override / `afterEach(server.resetHandlers)` 필수 / 실제 mutation까지 검증할 땐 낙관적 업데이트·실패 rollback·후속 query 갱신을 별도 설계(원문 확장).

## 관련 문서
- [[통합 테스트]] · [[테스트 Mock·Fixture 재사용]] · [[Storybook 기반 컴포넌트 개발]]

## 출처
- [[카카오엔터 MSW 통합 테스트 요약]]
