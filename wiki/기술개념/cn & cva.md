---
type: concept
platform: [web, app]
status: verified
tags: [frontend, styling, tailwind, cva]
source: https://cva.style/, https://github.com/dcastil/tailwind-merge
created: 2026-06-19
updated: 2026-06-19
---

# cn & cva (클래스 유틸리티)

> Tailwind 클래스를 **조건부 병합(`cn`)** + **variant 선언(`cva`)** 으로 관리. 상위: [[index]]

## TL;DR
[[Tailwind CSS|Tailwind]] 유틸리티 클래스가 길어지고 동적으로 바뀔 때 두 도구로 다룬다:

| 도구 | 한 줄 |
|---|---|
| **`cn`** | 조건부 조합 + 충돌 클래스 병합 (`clsx` + `tailwind-merge`) |
| **`cva`** | variant(`intent`/`size`…)를 선언적으로 → className |

같이 쓴다: `cn(cva(...), className)`

## `cn` = clsx + tailwind-merge
```ts
import { clsx, type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";

export const cn = (...inputs: ClassValue[]) => twMerge(clsx(inputs));
```
| 도구 | 역할 |
|---|---|
| **clsx** | 조건부 클래스 조합 — `clsx("p-2", isOn && "bg-blue-500")` |
| **tailwind-merge** | **충돌 클래스 병합** — `twMerge("p-2 p-4")` → `p-4` (뒤가 이김) |

```tsx
<button className={cn("px-4 py-2 rounded", isActive && "bg-blue-500", className)} />
```
- 외부에서 받은 `className`을 합칠 땐 **항상 `cn` 통과** → override가 깔끔하게 적용.

## `cva` = Class Variance Authority
```ts
import { cva, type VariantProps } from "class-variance-authority";

const buttonVariants = cva("rounded font-medium", {
  variants: {
    intent: { primary: "bg-blue-500 text-white", ghost: "bg-transparent" },
    size:   { sm: "px-3 py-1.5 text-sm", md: "px-4 py-2" },
  },
  defaultVariants: { intent: "primary", size: "md" },
});

type ButtonProps = VariantProps<typeof buttonVariants> & { className?: string };
```
- variant 조합을 **선언적으로** 정의 → `if`/템플릿 문자열 분기 제거.
- `VariantProps<typeof ...>`로 prop 타입 자동 추출 → [[예측 가능성|예측 가능]].

## 함께 쓰는 표준 패턴
```tsx
export const Button = ({ intent, size, className }: ButtonProps) => (
  <button className={cn(buttonVariants({ intent, size }), className)} />
);
```

## 안티패턴
- 동적 클래스 문자열 `` `text-${size}` `` ❌ — Tailwind JIT가 스캔 못 함 → `cva` variant.
- `twMerge` 없이 문자열 이어붙이기 ❌ — 충돌 클래스 잔존.

## 어디서 쓰나
- **웹**: [[Tailwind CSS]]
- **RN**: [[NativeWind]] (동일 패턴)
- **컴포넌트**: [[shadcn-ui]] (Radix + 이 스택)

## 관련 문서
[[Tailwind CSS]] · [[NativeWind]] · [[shadcn-ui]] · [[예측 가능성]]

## 출처
- CVA: https://cva.style/
- tailwind-merge: https://github.com/dcastil/tailwind-merge · clsx: https://github.com/lukeed/clsx
