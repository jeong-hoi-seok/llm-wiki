---
type: concept
platform: web
status: draft
tags: [frontend, testing, react, rtl]
source:
  - "[[우아한형제들 단위 테스트 시리즈 요약]]"
created: 2026-06-22
updated: 2026-06-22
---

# React 컴포넌트 테스트 작성법

## 한 줄 요약
React 컴포넌트 테스트는 props, 화면, 사용자 상호작용, 접근 가능한 role/name을 기준으로 외부 동작을 검증한다.

## 핵심 내용
React Testing Library의 기본 방향은 구현 세부사항이 아니라 사용자가 화면을 사용하는 방식에 가깝게 테스트하는 것이다.

우선순위는 다음과 같다.

1. `screen.getByRole`과 accessible name으로 요소를 찾는다.
2. `userEvent`로 실제 사용자 행동에 가까운 이벤트를 발생시킨다.
3. 화면에 보이는 텍스트, role, 상태를 검증한다.
4. 비동기 렌더링은 `findBy*`, `waitFor`로 기다린다.

우아한형제들 Part 2는 실제 컴포넌트에서 loading이 사라질 때까지 기다린 뒤 input을 찾고, 사용자가 입력했을 때 글자 수 제한이 적용되는지 검증하는 식으로 테스트를 구성한다. 조건부 렌더링도 props, store 상태, 사용자 정보 같은 “노출 조건”을 명확히 둔 뒤 화면 결과를 본다.

## 실무 적용 기준
- 버튼은 `getByRole('button', { name })`으로 찾는다.
- 입력은 label, placeholder, role 기준으로 찾는다.
- 클릭, 입력, 선택은 `userEvent`를 우선한다.
- 내부 state 값보다 화면 결과를 검증한다.
- loading, empty, error 같은 상태도 별도 케이스로 둔다.
- 비동기 UI는 loading 제거, 데이터 노출, 버튼 활성화 같은 관찰 가능한 지점을 기다린다.
- callback 검증이 필요한 경우에도 먼저 사용자 행동을 발생시킨다.

## 추천 흐름
1. Given: 필요한 props, mock 응답, 초기 상태를 준비한다.
2. When: 사용자가 실제로 할 행동을 `userEvent`로 수행한다.
3. Then: 화면, callback, 접근성 상태 중 테스트 목적에 맞는 결과를 검증한다.

## 피해야 할 흐름
- state setter를 직접 호출하고 컴포넌트 결과를 건너뛴다.
- class name만으로 모든 것을 판단한다.
- `waitFor` 안에 원인 모를 assertion을 많이 넣는다.
- 사용자 행동 없이 구현 함수를 직접 호출한다.

## 관련 문서
- [[내부 구현을 테스트하면 안 되는 이유]]
- [[사용자 시나리오 기반 테스트]]
- [[Storybook에서 API 상태 표현하기]]

## 출처
- [[우아한형제들 단위 테스트 시리즈 요약]]
