---
type: concept
platform: web
status: verified
tags: [frontend, design-system, base-ui, headless, accessibility]
source:
  - "https://base-ui.com/"
  - "https://base-ui.com/react/overview/about"
  - "https://base-ui.com/react/overview/community"
created: 2026-06-22
updated: 2026-06-23
---

# Base UI

> 접근성 있는 unstyled React primitive. 상위: [[index]]

## TL;DR
Base UI는 스타일을 강제하지 않는 React UI **primitive** 모음이다. Dialog·Select·Tooltip 같은 복잡한 컴포넌트의 **동작·상태·접근성**만 제공하고, 시각 표현은 프로젝트가 입힌다. Tailwind 기반 디자인 시스템이나 [[shadcn-ui]] wrapper와 조합하기 좋다. [[헤드리스 컴포넌트]] 전략의 primitive 후보.

## 누가 만들었나
Radix·Floating UI·Material UI 핵심 개발자들이 함께 만든 오픈소스다. MUI가 유지보수한다(`mui/base-ui`).

- **Colm Tuite** — Radix UI 제작자
- **James Nelson** — Floating UI
- **Michał Dudak·Marija Najdova** — Material UI
- 외 Albert Yu, Lukas Tyla, Aarón García, Jenna Smith

> Radix 제작자가 다시 참여한 "차세대 Radix" 성격 → Radix와 설계 철학이 가깝다.

## 컴포넌트 API — composable parts
완성 컴포넌트 하나가 아니라 **part를 조립**한다. 각 노드에 직접 접근 가능해 part를 더하거나 빼고 자유롭게 wrapping한다.

```jsx
<Select.Root>
  <Select.Trigger>
    <Select.Value />
    <Select.Icon />
  </Select.Trigger>
  <Select.Portal>
    <Select.Positioner>
      <Select.Popup>
        <Select.List>
          <Select.Item>
            <Select.ItemText />
            <Select.ItemIndicator />
          </Select.Item>
        </Select.List>
      </Select.Popup>
    </Select.Positioner>
  </Select.Portal>
</Select.Root>
```
- `Root`는 자체 HTML 안 그림(논리 그룹). `Trigger`=`<button>`, `Positioner`=`<div>` 식으로 part마다 렌더 요소가 정해짐.
- 스타일은 각 part에 className/토큰 직접 부여 → [[Tailwind CSS]]와 충돌 적음.

## 제공 컴포넌트 (40+)
| 분류 | 예 |
|---|---|
| 폼 | Input, Button, Checkbox, Radio, Select, Switch, Slider |
| 레이아웃·오버레이 | Accordion, Dialog, Drawer, Menu, Navigation Menu, Tabs |
| 데이터 표시 | Progress, Meter, Tooltip, Avatar |
| 유틸 | CSP Provider, Direction Provider, `mergeProps` |

## 왜 중요한가
- 키보드·포커스·ARIA 같은 [[접근성]] 동작을 직접 구현하는 부담을 줄인다.
- 스타일이 없어 design token과 충돌이 적다.
- shadcn-ui와 1급 통합 → Tailwind wrapper 컴포넌트 라이브러리를 만들 수 있다.

## Radix UI와 차이
| | Base UI | Radix Primitives |
|---|---|---|
| 계보 | Radix 제작자가 다시 만든 후속 | 원조 헤드리스 |
| API | composable parts (`Portal`/`Positioner`/`Popup` 분리) | composable parts (유사하나 part 구성 다름) |
| 성숙도·레퍼런스 | 상대적으로 새 선택지 | 사례·생태계 풍부 |
| 적합 | 새 프로젝트, primitive 자유롭게 조합 | 기존 shadcn/Radix 코드 축적된 곳 |

> 둘 중 무엇을 쓸지는 [[shadcn-ui와 Base UI 선택 기준]] 참고. "항상 Base UI가 낫다"가 아니라 신규 프로젝트·Tailwind wrapper 직접 소유 시 우선 검토.

## 실무 적용 기준
- 자체 디자인 시스템을 만들 때 primitive 후보로 검토한다.
- 접근성 동작이 필요한 컴포넌트(Dialog·Select·Tooltip)부터 우선 적용한다.
- 스타일은 [[Tailwind CSS]], CSS variables, design token 등 프로젝트 표준으로 관리한다.
- 이미 Radix 기반 코드가 많으면 컴포넌트별로 점진 전환한다.

## 관련 문서
- [[shadcn-ui]]
- [[shadcn-ui와 Base UI 선택 기준]]
- [[헤드리스 컴포넌트]]
- [[접근성]]
- [[Tailwind CSS]]
- [[cn & cva]]

## 출처
- Base UI: https://base-ui.com/ (확인: 2026-06-23)
- Base UI About(제작자·범위): https://base-ui.com/react/overview/about (확인: 2026-06-23)
- Base UI shadcn/ui integration: https://base-ui.com/react/overview/community (확인: 2026-06-23)
