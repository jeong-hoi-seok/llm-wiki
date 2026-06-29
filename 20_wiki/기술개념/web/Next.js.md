---
type: concept
platform: web
status: verified
tags: [frontend, react, nextjs]
source:
  - https://nextjs.org/docs/app
  - https://nextjs.org/docs/pages
  - https://nextjs.org/blog/next-12
  - https://nextjs.org/blog/next-13
  - https://nextjs.org/blog/next-13-4
  - https://nextjs.org/blog/next-14
  - https://nextjs.org/blog/next-15
  - https://nextjs.org/blog/next-16
  - https://nextjs.org/blog/next-16-2
  - https://www.npmjs.com/package/next
created: 2026-06-19
updated: 2026-06-23
---

# Next.js

> [[React]] 풀스택 프레임워크. 파일 기반 라우팅 + 서버 렌더링. 상위: [[index]]

## TL;DR

[[React]] 위에 **라우팅·렌더링·데이터 패칭·캐싱·번들링**을 얹은 웹 프레임워크.

현재 신규 프로젝트 기준은 **App Router**다. Pages Router는 여전히 지원되지만, React Server Components·Server Actions·최신 캐싱 모델을 쓰려면 App Router를 기준으로 설계한다.

기준일: 2026-06-23. npm `latest` 확인 기준 최신 안정 버전은 `16.2.9`.

## Router 선택 기준

| 구분 | App Router | Pages Router |
|---|---|---|
| 위치 | `app/` 또는 `src/app/` | `pages/` 또는 `src/pages/` |
| 상태 | 현재 권장 라우터 | 지원되는 레거시 라우터 |
| 기본 모델 | Server Component 기본, 필요한 곳만 Client Component | page 컴포넌트 중심, 클라이언트 모델에 가까움 |
| 레이아웃 | 중첩 `layout.tsx`, route group, loading/error 파일 | `_app`, `_document`, 페이지별 커스텀 레이아웃 패턴 |
| 데이터 패칭 | Server Component `await`, 확장 `fetch`, cache/revalidate, Server Actions | `getStaticProps`, `getServerSideProps`, `getStaticPaths`, API Routes |
| API | `app/**/route.ts` Route Handler, Web `Request`/`Response` | `pages/api/*`, Node `req`/`res` |
| 마이그레이션 | route 단위 점진 도입 가능 | 기존 코드 유지, 새 기능은 제한적 |

### 권장안

- 새 프로젝트는 `src/app` 기반 App Router로 시작한다.
- 기존 Pages Router 프로젝트는 바로 전체 교체하지 말고 route 단위로 `app/`을 병행 도입한다.
- `pages/`와 `app/`을 함께 쓰는 것은 가능하지만, 같은 URL을 두 라우터에 동시에 만들지 않는다.
- Pages Router는 유지보수·보안 패치 대상이지만, 새 기능 학습과 설계 기준은 App Router로 둔다.

## App Router 권장 구조

```txt
src/
  app/
    layout.tsx          # 전역 provider, metadata, shell
    page.tsx            # 홈 route
    dashboard/
      page.tsx
      loading.tsx
      error.tsx
      _components/      # 해당 route 전용 UI
      _lib/             # 해당 route 전용 로직
    api/
      health/
        route.ts
  features/
  entities/
  shared/
```

기준:

- `page.tsx`는 route entry다. 데이터 조회·조립은 가능하지만, 거대한 화면 로직을 모두 넣지 않는다.
- Server Component를 기본값으로 둔다. 상태, 이벤트, 브라우저 API가 필요한 leaf만 `'use client'`로 분리한다.
- route 전용 코드는 `app/**/_components`, `app/**/_lib`에 둔다. 여러 route에서 재사용되면 `features/entities/shared`로 이동한다.
- mutation은 우선 Server Actions를 검토한다. 외부 클라이언트나 명확한 HTTP API가 필요하면 `route.ts`를 쓴다.
- [[FSD 폴더구조]]를 쓸 때는 `src/app`이 라우팅과 FSD app layer 역할을 흡수하고, 재사용 레이어만 `features/entities/shared`로 둔다.

## App Router 핵심 파일

| 파일 | 역할 |
|---|---|
| `page.tsx` | route UI. 해당 segment의 공개 화면 |
| `layout.tsx` | 중첩 레이아웃. 하위 route 이동 시 유지 |
| `loading.tsx` | Suspense 기반 로딩 UI |
| `error.tsx` | route segment 에러 경계. Client Component 필요 |
| `not-found.tsx` | 404 UI |
| `route.ts` | HTTP route handler |
| `template.tsx` | navigation마다 새 인스턴스가 필요한 레이아웃 |

## 서버 컴포넌트 vs 클라이언트 컴포넌트

| | Server Component (기본) | Client Component (`'use client'`) |
|---|---|---|
| 실행 | 서버에서 렌더 | 브라우저에서 렌더 |
| 용도 | 데이터 패칭·정적 표현 | 상태·이벤트·브라우저 API |
| 번들 | JS 0 (서버) | 클라 번들 포함 |

기본은 Server Component다. Client Component는 UI 전체가 아니라 상호작용이 필요한 경계만 만든다.

## 렌더링 전략

| 전략 | 시점 | 쓰임 |
|---|---|---|
| **SSG** | 빌드 타임 정적 생성 | 거의 안 바뀌는 페이지 |
| **ISR** | 빌드 후 주기적 재생성 | 가끔 바뀜 |
| **SSR** | 요청마다 서버 렌더 | 요청별 동적 |
| **Streaming/RSC** | 서버 컴포넌트 점진 전송 | 부분 로딩(Suspense) |
| **PPR / Cache Components** | 정적 shell + 동적 부분 | Next.js 16 이후 캐싱 모델 검토 대상 |

## 데이터 패칭

- App Router에서는 Server Component에서 직접 `await fetch(...)`를 쓴다.
- Next.js 15부터 `GET` Route Handler와 Client Router Cache 기본값이 더 동적인 방향으로 바뀌었다. 캐싱은 명시적으로 opt-in한다고 생각하는 편이 안전하다.
- Next.js 16의 Cache Components는 `"use cache"`와 `cacheComponents`를 통해 캐싱을 더 명시적으로 다룬다.
- 클라 데이터는 React Query/SWR 등.

## 버전별 변화

| 버전 | 핵심 변화 | 실무 의미 |
|---|---|---|
| 12 | SWC 기반 Rust Compiler, Middleware beta, React 18 준비, RSC alpha | Pages Router 중심 시대. 빌드/개발 속도 개선이 큰 변화 |
| 13.0 | `app/` beta, Server Components, Streaming, Turbopack alpha | App Router 등장. 단, 초기는 production 기본값 아님 |
| 13.4 | App Router stable | App Router를 production에 채택할 수 있는 기준점 |
| 14 | Server Actions stable, Partial Prerendering preview, Turbopack 개선 | App Router 기반 mutation과 캐싱/재검증 흐름이 본격화 |
| 15 | React 19 정렬, async request APIs, 캐싱 기본값 변경, Turbopack dev stable | 업그레이드 시 `headers/cookies/params/searchParams` async 전환과 캐싱 변경 확인 필요 |
| 15.5 | Turbopack build beta, Node.js Middleware stable, `next lint` deprecation 경고 | 16 이전 마이그레이션 준비 성격 |
| 16 | Cache Components, `proxy.ts`, DevTools MCP, Turbopack·캐싱 아키텍처 개선 | Middleware를 Proxy로 재정의하고, 캐싱은 명시적 모델로 이동 |
| 16.2 | 성능·디버깅·Agent 개선, Turbopack 수정 다수 | 최신 16 계열 안정화 업데이트. 신규 프로젝트는 16.2.x 기준 검토 |

## 버전 업그레이드 체크

- 12→13: App Router는 beta였으므로 기존 `pages/` 유지 가능. 새 `app/`은 실험/점진 도입으로 본다.
- 13→13.4+: App Router production 채택 가능 여부를 다시 판단한다.
- 14→15: async request APIs와 캐싱 기본값 변경을 먼저 확인한다.
- 15→16: `middleware.ts`에서 `proxy.ts`로 개념·파일명 이동, Cache Components 도입 여부를 확인한다.
- major 업그레이드는 공식 codemod와 upgrade guide를 먼저 본다.

## 생태계

- 스타일: [[Tailwind CSS]] · [[shadcn-ui]] (Next + shadcn 흔한 조합)
- 구조: [[FSD 폴더구조]] (`src/app` + features/entities/shared)

## 관련 문서

[[React]] · [[Tailwind CSS]] · [[FSD 폴더구조]] · [[shadcn-ui]] · [[번들링]] · [[코드 스플리팅]]

## 출처

- Next.js App Router Docs: https://nextjs.org/docs/app
- Next.js Pages Router Docs: https://nextjs.org/docs/pages
- App Router Migration: https://nextjs.org/docs/app/guides/migrating/app-router-migration
- Next.js 12: https://nextjs.org/blog/next-12
- Next.js 13: https://nextjs.org/blog/next-13
- Next.js 13.4: https://nextjs.org/blog/next-13-4
- Next.js 14: https://nextjs.org/blog/next-14
- Next.js 15: https://nextjs.org/blog/next-15
- Next.js 16: https://nextjs.org/blog/next-16
- Next.js 16.2: https://nextjs.org/blog/next-16-2
- npm next: https://www.npmjs.com/package/next
