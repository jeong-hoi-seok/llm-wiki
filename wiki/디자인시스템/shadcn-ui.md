---
type: concept
platform: web
status: verified
tags: [frontend, design-system, shadcn, tailwind, radix, base-ui]
source:
  - "https://ui.shadcn.com/docs"
  - "https://ui.shadcn.com/docs/changelog/2026-01-base-ui"
  - "https://base-ui.com/react/overview/community"
created: 2026-06-19
updated: 2026-06-22
---

# shadcn/ui

> "컴포넌트 라이브러리가 아니라, 네 컴포넌트 라이브러리를 만드는 방법." 상위: [[index]]

## TL;DR
[[헤드리스 컴포넌트|헤드리스]] primitive(Radix 또는 [[Base UI]]) + Tailwind 스타일을 **CLI로 프로젝트에 복사**하는 모델. npm 의존성으로 설치해서 숨겨 쓰는 컴포넌트가 아니라 **코드를 소유**해서 자유롭게 수정한다.

## 핵심 철학 — copy, not install
```
일반 라이브러리:  npm install → node_modules에 잠김 → override 싸움
shadcn/ui:        npx shadcn add button → src/에 코드 복사 → 내가 소유·수정
```
- 컴포넌트 코드가 **내 레포 안**에 들어옴 → 디자인·동작을 마음대로 바꿈.
- 벤더 종속·버전 잠김 없음. 단, 업데이트는 **수동**(내 코드라서).

## 구성 스택
| 레이어 | 역할 |
|---|---|
| **Radix UI 또는 Base UI** | 로직·상태·접근성([[접근성]]) — 헤드리스 베이스 |
| **Tailwind CSS** | 스타일 |
| **CVA** (class-variance-authority) | variant → className |
| **tailwind-merge + clsx** (`cn`) | 클래스 충돌 병합 |
> 이 스택은 RN의 [[NativeWind]] variant 패턴(CVA + `cn`)과 **동일** — 웹은 Tailwind, RN은 NativeWind.

## Base UI 기반
shadcn-ui는 Radix 기반뿐 아니라 Base UI 기반 컴포넌트도 제공한다. 새 프로젝트에서 primitive 선택지가 열려 있다면 [[shadcn-ui와 Base UI 선택 기준]]을 먼저 본다.

Base UI 기반은 다음 상황에 잘 맞는다.
- 자체 디자인 시스템을 만들고 싶다.
- 스타일은 Tailwind와 design token으로 직접 관리하고 싶다.
- 접근성 동작은 검증된 unstyled primitive에 맡기고 싶다.
- 프로젝트 코드로 wrapper를 소유하고 싶다.

## 장단점
| 👍 장점 | 👎 단점 |
|---|---|
| 완전 커스터마이징 (코드 소유) | 업데이트 수동 (자동 패치 없음) |
| 의존성·버전 잠김 없음 | 컴포넌트 코드 유지보수 책임 |
| 접근성 내장 (Radix) | 초기엔 직접 추가·관리 필요 |
| 디자인 시스템 출발점으로 적합 | "라이브러리처럼 그냥 쓰기"엔 손 더 감 |

## 언제 쓰나
- 자체 디자인 시스템을 코드로 **소유·확장**하고 싶을 때.
- MUI·Antd처럼 스타일드 라이브러리의 override 싸움이 싫을 때.
- 반대로 빠른 프로토타입·내부 툴엔 스타일드가 더 빠를 수 있음 → [[헤드리스 컴포넌트]] 비교표 참고.

## 관련 문서
[[shadcn-ui와 Base UI 선택 기준]] · [[Base UI]] · [[헤드리스 컴포넌트]] · [[Tailwind CSS]] · [[cn & cva]] · [[Next.js]] · [[NativeWind]] · [[접근성]] · [[공통 컴포넌트 추출 기준]]

## 출처
- shadcn/ui: https://ui.shadcn.com/docs (확인: 2026-06-22)
- shadcn/ui Base UI changelog: https://ui.shadcn.com/docs/changelog/2026-01-base-ui (확인: 2026-06-22)
- Base UI shadcn/ui integration: https://base-ui.com/react/overview/community (확인: 2026-06-22)
