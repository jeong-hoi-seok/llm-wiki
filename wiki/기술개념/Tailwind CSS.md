---
type: concept
platform: web
status: verified
tags: [frontend, styling, tailwind]
source: https://tailwindcss.com/
created: 2026-06-19
updated: 2026-06-19
---

# Tailwind CSS

> 유틸리티 클래스로 마크업에서 직접 스타일링. 상위: [[index]]

## TL;DR
미리 정의된 **유틸리티 클래스**(`flex`, `p-4`, `text-lg`)를 `className`에 나열해 스타일링한다. 별도 CSS 파일·클래스 작명이 거의 없다. 클래스 조합·variant는 **[[cn & cva]]** 로 관리.

## 왜 쓰나
| 이점 | 설명 |
|---|---|
| 작명 제거 | `.card-title-wrapper` 같은 이름을 안 지어도 됨 |
| 디자인 토큰 일관성 | 간격·색·폰트가 정해진 스케일(`p-2`/`p-4`, `text-sm`/`text-lg`) |
| 작은 번들 | JIT — 실제 쓴 클래스만 생성 |
| 빠른 파악 | 마크업만 봐도 스타일 보임 (단, 길어지면 역효과 → [[cn & cva]]) |

## 기본 문법
```tsx
<div className="flex items-center gap-2 p-4 rounded-lg bg-white dark:bg-zinc-900">
```
| 종류 | 접두 | 예 |
|---|---|---|
| **반응형** | `sm: md: lg: xl: 2xl:` | `md:flex-row` |
| **상태** | `hover: focus: active: disabled:` | `hover:bg-blue-600` |
| **다크모드** | `dark:` | `dark:text-zinc-100` |
| **그룹/형제** | `group-hover: peer-checked:` | `group-hover:opacity-100` |

## 설정 핵심
- `tailwind.config.js`의 `content`에 스캔 경로 지정 → 거기 있는 클래스만 생성.
- `theme.extend`로 색·간격·폰트 토큰 확장 (디자인 시스템 연결 → [[헤드리스 컴포넌트]]).
- v4부터는 CSS 우선 설정(`@theme`)도 지원.

## 클래스가 길어질 때 → [[cn & cva]]
- 조건부·외부 className 병합 → **`cn`** (clsx + tailwind-merge)
- variant(intent/size) 선언 → **`cva`**
- 자세히: [[cn & cva]]

## 안티패턴
| ❌ | ✅ 대안 |
|---|---|
| 동적 클래스 `` `text-${size}` `` | 정적 리터럴 / `cva` variant ([[cn & cva]]) |
| 거대 className 한 줄 방치 | variant는 `cva`, 반복은 컴포넌트로 추출 |
| `@apply` 남발로 CSS 회귀 | 유틸 우선 유지, 공통은 컴포넌트화 |

## 생태계 연결
- **RN**: [[NativeWind]] — Tailwind `className`을 React Native에서.
- **컴포넌트**: [[shadcn-ui]] — Radix + Tailwind + [[cn & cva]] 스택.
- **프레임워크**: [[Next.js]]·[[React]]에서 표준 조합.

## 관련 문서
[[cn & cva]] · [[NativeWind]] · [[shadcn-ui]] · [[가독성]]

## 출처
- Tailwind CSS: https://tailwindcss.com/
