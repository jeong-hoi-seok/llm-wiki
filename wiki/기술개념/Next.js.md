---
type: concept
platform: web
status: verified
tags: [frontend, react, nextjs]
source: https://nextjs.org/docs
created: 2026-06-19
updated: 2026-06-19
---

# Next.js

> [[React]] 풀스택 프레임워크. 파일 기반 라우팅 + 서버 렌더링. 상위: [[index]]

## TL;DR
[[React]] 위에 **라우팅·렌더링·데이터 패칭·번들링**을 얹은 프레임워크. App Router(서버 컴포넌트 기반)가 현재 표준.

## App Router (현재 표준)
- **파일 기반 라우팅**: `app/` 폴더 구조 = URL. (`app/blog/[slug]/page.tsx`)
- 특수 파일: `page` · `layout`(중첩 레이아웃) · `loading` · `error` · `route`(API).
- 프로젝트 구조는 `src/app/`에 두고 나머지는 FSD로 → [[FSD 폴더구조]].

## 서버 컴포넌트 vs 클라이언트 컴포넌트
| | Server Component (기본) | Client Component (`'use client'`) |
|---|---|---|
| 실행 | 서버에서 렌더 | 브라우저에서 렌더 |
| 용도 | 데이터 패칭·정적 표현 | 상태·이벤트·브라우저 API |
| 번들 | JS 0 (서버) | 클라 번들 포함 |
- 기본은 서버 컴포넌트, 상호작용 필요한 부분만 `'use client'`로 경계.

## 렌더링 전략
| 전략 | 시점 | 쓰임 |
|---|---|---|
| **SSG** | 빌드 타임 정적 생성 | 거의 안 바뀌는 페이지 |
| **ISR** | 빌드 후 주기적 재생성 | 가끔 바뀜 |
| **SSR** | 요청마다 서버 렌더 | 요청별 동적 |
| **Streaming/RSC** | 서버 컴포넌트 점진 전송 | 부분 로딩(Suspense) |

## 데이터 패칭
- 서버 컴포넌트에서 직접 `await fetch(...)` → `fetch` 캐싱·재검증(`revalidate`) 활용.
- 클라 데이터는 React Query/SWR 등.

## 생태계
- 스타일: [[Tailwind CSS]] · [[shadcn-ui]] (Next + shadcn 흔한 조합)
- 구조: [[FSD 폴더구조]] (`src/app` + features/entities/shared)

## 관련 문서
[[React]] · [[Tailwind CSS]] · [[FSD 폴더구조]] · [[shadcn-ui]]

## 출처
- Next.js: https://nextjs.org/docs
