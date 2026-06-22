---
type: concept
platform: web
status: draft
tags: [frontend, design-system, base-ui, headless, accessibility]
source:
  - "https://base-ui.com/"
  - "https://base-ui.com/react/overview/community"
created: 2026-06-22
updated: 2026-06-22
---

# Base UI

## 한 줄 요약
Base UI는 접근성 있는 React UI를 만들기 위한 unstyled component primitive다. 스타일을 강제하지 않아 Tailwind 기반 디자인 시스템이나 [[shadcn-ui]] wrapper와 조합하기 좋다.

## 핵심 개념
Base UI는 완성된 스타일드 컴포넌트 라이브러리가 아니다. Dialog, Tooltip, Select 같은 복잡한 UI의 동작과 접근성 기반을 제공하고, 스타일은 프로젝트가 직접 입힌다.

## 왜 중요한가
- 키보드, 포커스, ARIA 같은 접근성 동작을 직접 구현하는 부담을 줄인다.
- 스타일이 없어서 디자인 시스템 토큰과 충돌이 적다.
- [[헤드리스 컴포넌트]] 전략과 잘 맞는다.
- shadcn-ui와 통합되어 Tailwind wrapper 기반 컴포넌트 라이브러리를 만들 수 있다.

## 실무 적용 기준
- 자체 디자인 시스템을 만들 때 primitive 후보로 검토한다.
- 접근성 있는 동작이 필요한 컴포넌트부터 우선 적용한다.
- 스타일은 [[Tailwind CSS]], CSS variables, design token 등 프로젝트 표준으로 관리한다.
- 이미 Radix 기반 코드가 많으면 Base UI 전환은 점진적으로 판단한다.

## 관련 문서
- [[shadcn-ui]]
- [[shadcn-ui와 Base UI 선택 기준]]
- [[헤드리스 컴포넌트]]
- [[접근성]]
- [[Tailwind CSS]]

## 출처
- Base UI: https://base-ui.com/ (확인: 2026-06-22)
- Base UI shadcn/ui integration: https://base-ui.com/react/overview/community (확인: 2026-06-22)
