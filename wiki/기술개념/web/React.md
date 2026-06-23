---
type: concept
platform: web
status: verified
tags: [frontend, react]
source: https://react.dev/
created: 2026-06-19
updated: 2026-06-23
---

# React

> 선언적 UI 라이브러리. 상태 → UI를 함수로 기술. 상위: [[index]]

## TL;DR
**상태(state)를 넣으면 UI가 나오는 함수**처럼 컴포넌트를 작성. 상태가 바뀌면 React가 바뀐 부분만 다시 그린다(재조정).

## 핵심 개념
| 개념 | 설명 |
|---|---|
| **컴포넌트** | UI 조각을 반환하는 함수. 조합(composition)으로 화면 구성 |
| **props** | 부모 → 자식으로 내려주는 읽기 전용 입력 |
| **state** | 컴포넌트가 가진 변하는 값. 변경 시 리렌더 |
| **재조정(Reconciliation)** | 가상 DOM 비교 → 바뀐 부분만 실제 DOM 갱신 |
| **key** | 리스트 항목 식별 → 안정적 재조정 (index를 key로 쓰지 말 것) |

## 훅 (Hooks)
| 훅 | 용도 |
|---|---|
| `useState` | 지역 상태 |
| `useEffect` | 외부 동기화(구독·요청·DOM). 의존성 배열 주의 |
| `useMemo` / `useCallback` | 계산·함수 메모이즈 (참조 안정화) |
| `useRef` | 리렌더 없는 가변 참조·DOM 접근 |
| **커스텀 훅** | `use*` 로 로직 추출·재사용 → [[응집도]] |

## 좋은 컴포넌트 (코드 품질 연결)
- 단일 책임 → [[응집도]], 비대화 경계 → [[공통 컴포넌트 추출 기준]]
- props/반환 일관 → [[예측 가능성]], 깊은 prop 전달 줄이기(Composition) → [[결합도]]
- 동작/스타일 분리 → [[헤드리스 컴포넌트]]

## 생태계
- 스타일: [[Tailwind CSS]] · [[cn & cva]]
- 프레임워크: [[Next.js]] (웹) · [[React Native]] (앱)
- 구조: [[FSD 폴더구조]]

## 관련 문서
[[Next.js]] · [[React Native]] · [[코드 품질]] · [[헤드리스 컴포넌트]]

## 출처
- React: https://react.dev/
