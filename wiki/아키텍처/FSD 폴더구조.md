---
type: architecture
scope: fsd-architecture-pattern
platform: common
status: verified
tags: [frontend, architecture, fsd]
source:
  - raw/feature-sliced-design/kr/docs/get-started/overview.mdx
  - raw/feature-sliced-design/kr/docs/reference/layers.mdx
  - raw/feature-sliced-design/kr/docs/reference/slices-segments.mdx
  - raw/feature-sliced-design/kr/docs/reference/public-api.mdx
  - raw/feature-sliced-design/kr/docs/reference/slice-groups.mdx
  - raw/feature-sliced-design/kr/docs/guides/tech/with-nextjs.mdx
  - raw/feature-sliced-design/kr/docs/guides/issues/cross-imports.mdx
  - raw/feature-sliced-design/kr/docs/guides/issues/excessive-entities.mdx
created: 2026-06-19
updated: 2026-06-23
project_specific: false
adoption: optional
---

# FSD 폴더구조

## 한 줄 요약

FSD(Feature-Sliced Design)는 프론트엔드 애플리케이션 코드를 **Layer → Slice → Segment**로 나누고, 의존 방향과 공개 경로를 제한해 구조가 커져도 변경 범위를 예측하기 쉽게 만드는 아키텍처 방법론이다.

## 이 문서의 범위

이 문서는 공식 FSD 문서 기준의 구조 요약이다. 특정 프로젝트가 반드시 FSD를 따라야 한다는 규칙이 아니다.

FSD는 단순 라이브러리보다 **애플리케이션**에 적합하다. 지금 구조에 문제가 없다면 굳이 바꿀 필요는 없다. 프로젝트가 커지면서 폴더 구조가 얽히고, 새 팀원이 구조를 이해하기 어렵고, 변경 영향 범위를 예측하기 어려울 때 도입을 검토한다.

## 기본 구조

```txt
src/
  app/
  pages/
  widgets/
  features/
  entities/
  shared/
```

이 최상위 폴더가 **Layer**다. Layer 안에는 비즈니스 도메인 단위의 **Slice**가 있고, Slice 안에는 기술 목적별 **Segment**가 있다.

```txt
src/
  app/
    routes/
    styles/
  pages/
    home/
      ui/
      api/
    settings/
      ui/
  features/
    auth-by-email/
      ui/
      api/
      model/
  entities/
    user/
      api/
      model/
      ui/
  shared/
    api/
    ui/
    lib/
    config/
```

## 핵심 규칙

1. Layer는 표준 이름을 사용한다.
2. 하위 Layer는 상위 Layer를 import하지 않는다.
3. 같은 Layer 안의 다른 Slice를 직접 import하지 않는다.
4. 외부에서는 Slice 내부 파일이 아니라 Public API를 통해 접근한다.
5. Segment 이름은 파일 종류가 아니라 목적을 드러내야 한다.
6. 모든 Layer를 반드시 만들 필요는 없다.

## Layer

Layer는 코드의 책임과 의존 범위를 나누는 가장 큰 단위다. 아래로 내려갈수록 더 범용적이고 덜 의존적인 코드가 온다.

| Layer | 역할 | Slice |
|---|---|---|
| `app` | 라우터, 엔트리포인트, 전역 스타일, provider, 전역 설정 | 없음 |
| `processes` | 과거의 여러 페이지 플로우 처리 layer | deprecated |
| `pages` | route 기준 화면 또는 activity | 있음 |
| `widgets` | 독립적으로 동작하는 큰 UI 블록 | 있음 |
| `features` | 사용자가 수행하는 주요 기능, use case | 있음 |
| `entities` | 핵심 비즈니스 개념과 재사용 도메인 로직 | 있음 |
| `shared` | 비즈니스와 분리된 공통 기반 코드 | 없음 |

`app`과 `shared`는 Slice 없이 Segment로만 구성한다. 두 Layer는 각각 앱 전체 조정자, 비즈니스 독립 기반 코드라는 성격이 강해서 내부 Segment끼리 자유롭게 import할 수 있다.

## Layer Import Rule

하나의 Slice 안에서 작성된 코드는 **자신이 속한 Layer보다 아래 Layer의 다른 Slice**만 import할 수 있다.

예:

```txt
features/auth-by-email/api/request.ts
```

이 파일은 다음처럼 접근한다.

- 같은 Slice 내부 `features/auth-by-email/model/*` import 가능
- 하위 Layer `entities/*`, `shared/*` import 가능
- 같은 Layer의 다른 Slice `features/profile/*` import 금지
- 상위 Layer `widgets/*`, `pages/*`, `app/*` import 금지

## Slice

Slice는 Layer 내부를 제품, 비즈니스, 애플리케이션 관점에서 서로 관련 있는 코드로 묶는 단위다.

```txt
features/
  auth-by-email/
  add-to-cart/
  delete-comment/

entities/
  user/
  product/
  order/
```

Slice 이름은 고정된 표준이 없다. 프로젝트의 비즈니스 도메인과 use case에 맞춘다.

중요한 점은 **Slice끼리 독립적이어야 한다**는 것이다. 같은 Layer 안의 Slice가 서로 직접 import하기 시작하면 변경 영향 범위가 넓어지고, 책임 경계가 흐려진다.

## Segment

Segment는 Slice 안에서 코드를 기술 목적별로 나누는 단위다.

| Segment | 역할 |
|---|---|
| `ui` | UI 컴포넌트, 스타일, 표시용 formatter |
| `api` | 요청 함수, 데이터 타입, mapper 등 백엔드 통신 |
| `model` | 상태, selector, schema, 비즈니스 로직 |
| `lib` | 해당 Slice 안에서 함께 쓰는 내부 라이브러리 코드 |
| `config` | 설정, feature flag |

`components`, `hooks`, `types`처럼 파일 종류만 드러내는 이름은 피한다. Segment 이름은 **이 폴더가 무엇을 위해 존재하는지**를 설명해야 한다.

## Public API

Slice는 외부에서 사용할 공식 진입점인 Public API를 둔다. 보통 `index.ts`에서 필요한 것만 re-export한다.

```ts
// features/auth-by-email/index.ts
export { LoginForm } from "./ui/login-form";
export { loginByEmail } from "./api/login-by-email";
```

외부 코드는 내부 경로를 직접 import하지 않는다.

```ts
// ✅
import { LoginForm } from "@/features/auth-by-email";

// ❌
import { LoginForm } from "@/features/auth-by-email/ui/login-form";
```

좋은 Public API는 필요한 기능만 노출하고, 내부 폴더 구조 변경이 외부 코드에 영향을 주지 않게 만든다. `export *`를 남발하면 내부 구현이 밖으로 새고, tree-shaking이나 순환 참조 문제가 생기기 쉽다.

## Slice Group

Slice Group은 같은 Layer 안에서 관련 Slice를 가까이 배치하기 위한 탐색용 폴더다.

```txt
pages/
  order/
    list/
      ui/
    detail/
      ui/
    create/
      ui/
```

주의할 점:

- Slice Group은 Slice가 아니다.
- `ui`, `api`, `model`, `index.ts`를 직접 갖지 않는다.
- 그룹 안에 공용 코드를 두지 않는다.
- 같은 그룹에 있어도 Slice 간 import 규칙은 그대로 유지된다.

Slice 수가 많아져 flat 구조만으로 탐색이 어려울 때만 도입한다. 그룹 기준이 자연스럽지 않거나 Slice가 적으면 만들지 않는 편이 낫다.

## Cross-import

Cross-import는 같은 Layer 안의 서로 다른 Slice가 import하는 것이다. 기본적으로 code smell로 본다.

대안:

- 실제로 항상 함께 바뀌는 Slice라면 하나로 합친다.
- 여러 Feature에서 공유하는 도메인 로직은 `entities`로 내린다.
- 여러 Feature/Widget을 함께 보여줘야 하면 상위 Layer인 `pages`나 `app`에서 조립한다.

Entity 간 타입 참조처럼 피하기 어려운 경우에는 `@x` 형태의 교차 Public API를 제한적으로 사용할 수 있다. 단, 이는 권장 기본값이 아니라 마지막 수단에 가깝다.

```txt
entities/
  artist/
    @x/
      song.ts
    index.ts
```

## Entities 사용 기준

`entities`는 접근 범위가 넓다. `features`, `widgets`, `pages`가 모두 참조할 수 있으므로, 여기에 코드가 쌓이면 변경 영향 범위도 커진다.

기준:

- thin client라면 처음부터 `entities`를 만들지 않아도 된다.
- 재사용이 분명해지기 전에는 현재 Slice의 `model`에 둔다.
- 단순 CRUD 요청은 보통 `shared/api`에 둔다.
- 인증 토큰이나 로그인 응답처럼 인증 맥락에 묶인 데이터는 `user` entity보다 `shared/auth` 또는 `shared/api`가 더 적합할 수 있다.
- 항상 함께 움직이는 도메인은 여러 Entity로 쪼개기보다 하나의 Entity Slice로 캡슐화한다.

## Next.js와 함께 쓸 때

FSD 공식 문서는 Next.js의 라우터 폴더와 FSD Layer를 분리하는 방식을 제안한다. 이유는 Next.js의 `app`/`pages` 폴더가 라우팅 예약 폴더이고, FSD의 `app`/`pages` Layer와 의미가 다르기 때문이다.

### App Router

```txt
app/                  # Next.js route folder
  example/
    page.tsx
  api/
    get-example/
      route.ts
pages/                # 빈 폴더. src/pages를 Pages Router로 오인하지 않게 둠
  README.md
src/
  app/                # FSD app layer
    api-routes/
  pages/              # FSD pages layer
    example/
      index.ts
      ui/
        example.tsx
  widgets/
  features/
  entities/
  shared/
```

루트 `app/example/page.tsx`는 실제 페이지 구현을 갖지 않고 `src/pages/example`을 re-export한다.

```tsx
export { ExamplePage as default, metadata } from "@/pages/example";
```

### Pages Router

Pages Router도 같은 원리다. 루트 `pages`는 Next.js 라우팅용으로 두고, 실제 FSD 페이지 코드는 `src/pages`에 둔다.

```txt
pages/
  _app.tsx
  example/
    index.tsx
src/
  app/
    custom-app/
  pages/
    example/
      index.ts
      ui/
        example.tsx
  widgets/
  features/
  entities/
  shared/
```

## 점진적 도입

기존 프로젝트에는 한 번에 전체 구조를 바꾸기보다 아래 순서로 도입한다.

1. `app`, `shared` 기반을 먼저 정리한다.
2. 기존 UI를 `pages`, `widgets`로 분배한다.
3. import 위반을 하나씩 줄인다.
4. 재사용되는 도메인 로직을 `features`, `entities`로 이동한다.

도입 중에는 새 Layer를 억지로 늘리지 말고, 구조 안정화에 집중한다.

## 관련 문서

- [[Next.js]]
- [[네이밍 컨벤션]]

## 출처

- Feature-Sliced Design KR Overview: `raw/feature-sliced-design/kr/docs/get-started/overview.mdx`
- Layers: `raw/feature-sliced-design/kr/docs/reference/layers.mdx`
- Slices and Segments: `raw/feature-sliced-design/kr/docs/reference/slices-segments.mdx`
- Public API: `raw/feature-sliced-design/kr/docs/reference/public-api.mdx`
- Slice Groups: `raw/feature-sliced-design/kr/docs/reference/slice-groups.mdx`
- Usage with Next.js: `raw/feature-sliced-design/kr/docs/guides/tech/with-nextjs.mdx`
- Cross-imports: `raw/feature-sliced-design/kr/docs/guides/issues/cross-imports.mdx`
- Excessive Entities: `raw/feature-sliced-design/kr/docs/guides/issues/excessive-entities.mdx`
