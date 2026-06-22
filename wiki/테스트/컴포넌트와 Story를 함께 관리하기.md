---
type: concept
platform: web
status: draft
tags: [frontend, testing, storybook, component]
source:
  - "[[29CM Storybook 운영 방식 요약]]"
created: 2026-06-22
updated: 2026-06-22
---

# 컴포넌트와 Story를 함께 관리하기

## 한 줄 요약
컴포넌트와 Story를 함께 관리하면 UI 상태를 코드 밖에서 확인하고, 리뷰·테스트·문서 자산으로 재사용할 수 있다.

## 핵심 내용
Story는 컴포넌트의 사용 예시이자 상태 문서다. 작은 컴포넌트부터 페이지 수준 컴포넌트까지 Story를 만들면 팀원이 기존 컴포넌트를 찾고 이해하기 쉬워진다.

29CM은 React 전환 과정에서 Storybook을 도입했고, 컴포넌트와 Story를 1:1로 맞춰 관리했다. 목적은 단순 문서화가 아니라 팀 개발 문제 해결이었다.

해결하려던 문제:
- 이미 있는 컴포넌트를 몰라 중복 생성한다.
- 기존 컴포넌트 사용법과 동작 확인에 시간이 든다.
- UI 테스트와 리뷰에 시간이 많이 든다.

## 실무 적용 기준
- 컴포넌트와 Story를 1:1로 둔다.
- 기본 상태, loading, empty, error, disabled, long content를 Story로 남긴다.
- Story 이름은 제품 상태가 드러나게 짓는다.
- Story가 실제 사용법과 멀어지지 않게 컴포넌트 변경 시 같이 갱신한다.
- 반복되는 Story boilerplate는 snippet으로 줄인다.
- 페이지 수준 컴포넌트도 핵심 상태가 있으면 Story로 남긴다.
- Storybook은 신규 입사자 온보딩 자료로도 쓰일 수 있게 관리한다.

## 관련 문서
- [[Storybook에서 API 상태 표현하기]]
- [[Component-Driven Development]]
- [[디자인 시스템 회귀 테스트]]

## 출처
- [[29CM Storybook 운영 방식 요약]]
