---
type: concept
platform: app
status: verified
tags: [app, styling, nativewind, cva]
source: https://www.nativewind.dev/docs/getting-started/installation
created: 2026-06-19
updated: 2026-06-23
---

# NativeWind 스타일링

RN UI 스타일은 **NativeWind `className`** 1순위. variant 공용 컴포넌트는 **CVA + `cn`**. 상위: [[index]]

> 웹 [[Tailwind CSS]]의 RN판. `cn`·`cva` 패턴 자체는 [[cn & cva]]에 정리(여기선 RN 적용·우선순위 위주).

## 우선순위

| 순위 | 방식 | 사용 시점 |
|---|---|---|
| **1** | NativeWind `className` | 레이아웃·색·타이포·간격·다크모드 등 **기본** |
| **1-b** | CVA + `cn` | 버튼·텍스트처럼 **variant 있는 공용 UI** |
| **2** | `StyleSheet.create` | NativeWind로 표현 어려울 때만 |
| **3** | inline `style` | 런타임 동적 값(prop 색 등) |

새 화면·컴포넌트는 `StyleSheet`부터 시작하지 않음.

## 파일·설정 위치

| 경로 | 역할 |
|---|---|
| `styles/global.css` | Tailwind 진입점 (`@tailwind`) |
| `tailwind.config.js` | `content: ["./src/**/*.{js,jsx,ts,tsx}"]` |
| `metro.config.js` | `withNativeWind(config, { input: "./styles/global.css" })` |
| `src/app/_layout.tsx` | `import "../../styles/global.css"` |
| `src/shared/lib/cn.ts` | `twMerge(clsx(...))` |
| `nativewind-env.d.ts` | `/// <reference types="nativewind/types" />` |

- CVA variant 정의는 **`src/` 아래** (Tailwind `content`가 스캔).

## `cn` 유틸
```ts
import { type ClassValue, clsx } from "clsx";
import { twMerge } from "tailwind-merge";
export const cn = (...inputs: ClassValue[]) => twMerge(clsx(inputs));
```
```tsx
<View className={cn("flex-1 p-4", isActive && "bg-blue-500", className)} />
```
- variant 결과 + 사용자 `className` 합칠 땐 **항상 `cn` 통과**.

## CVA 패턴
```tsx
import { cva, type VariantProps } from "class-variance-authority";
import { Pressable, type PressableProps } from "react-native";
import { cn } from "@/shared/lib";

const buttonVariants = cva("rounded-lg items-center justify-center", {
  variants: {
    intent: { primary: "bg-blue-500", secondary: "bg-gray-200" },
    size: { sm: "px-3 py-2", md: "px-4 py-3" },
  },
  defaultVariants: { intent: "primary", size: "md" },
});

type ButtonProps = PressableProps & VariantProps<typeof buttonVariants>;
export const Button = ({ intent, size, className, ...props }: ButtonProps) => (
  <Pressable className={cn(buttonVariants({ intent, size }), className)} {...props} />
);
```

## 화면 vs 공용 컴포넌트

| 대상 | 권장 |
|---|---|
| 화면 (`src/app/**`) | `View`·`SafeAreaView`에 `className` 직접 |
| 공용 UI (`src/shared/ui/**`) | variant 있으면 CVA, 단순하면 `className`만 |
| feature UI | 재사용·variant 늘면 `shared/ui`로 승격 |

## 다크 모드
- Tailwind `dark:` 접두사 (`dark:text-[#ECEDEE]`).
- 시스템 테마는 `useColorScheme`(`@/shared/lib`) 연동. JS 색 상수는 `shared/config/theme.ts`.

## 주의 (안티패턴)
1. **동적 클래스 문자열 금지** — `` `text-${size}` `` ❌. CVA variant·정적 리터럴만.
2. `twMerge` 없이 클래스 합치기 지양 — 충돌 클래스 잔존.
3. 3rd party 컴포넌트는 `className` 안 내려주면 동작 안 함 → `remapProps`/`cssInterop`.
4. `metro`/`babel`/`global.css` 변경 후 `pnpm exec expo start -c` (캐시 클리어).

## 관련
[[Tailwind CSS]] · [[cn & cva]] · [[React Native]] · [[Expo 프로젝트 부트스트랩]] · [[공식 문서 참조]] · [[앱 기술 스택]]
