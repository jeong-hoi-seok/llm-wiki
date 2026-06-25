---
type: source-summary
scope: feature-sliced-design-official-docs-kr
platform: common
status: summarized
tags: [frontend, architecture, fsd]
source: raw/feature-sliced-design/kr/docs/
created: 2026-06-23
updated: 2026-06-23
---

# Feature-Sliced Design 공식 문서 요약

## 원문 정보

- 제목: Feature-Sliced Design 공식 문서 한국어판
- 원본 위치: `raw/feature-sliced-design/kr/docs/`
- 원본 수: MDX 30개
- 주요 영역: 소개, 시작 가이드, reference, 예제, migration, framework integration, issue guide

## 핵심 요약

FSD는 프론트엔드 애플리케이션 코드를 Layer, Slice, Segment로 나누는 아키텍처 방법론이다. 목표는 요구사항이 바뀌어도 구조가 무너지지 않고, 새 기능을 추가하거나 수정할 때 변경 범위를 예측하기 쉽게 만드는 것이다.

## 핵심 개념

- **Layer**: `app`, `pages`, `widgets`, `features`, `entities`, `shared`처럼 코드 책임과 의존 범위를 나누는 최상위 구조. `processes`는 deprecated.
- **Slice**: Layer 내부를 제품·비즈니스 도메인 단위로 나누는 구조. 같은 Layer의 다른 Slice를 직접 import하지 않는다.
- **Segment**: Slice 내부를 `ui`, `api`, `model`, `lib`, `config`처럼 기술 목적별로 나누는 구조.
- **Public API**: Slice 외부에서 접근하는 공식 진입점. 보통 `index.ts`로 필요한 항목만 re-export한다.
- **Slice Group**: Slice가 많을 때 탐색을 돕기 위한 그룹 폴더. Slice가 아니며 공용 코드나 Public API를 갖지 않는다.

## 실무 적용 기준

- 구조에 문제가 없다면 FSD를 억지로 도입하지 않는다.
- 도입한다면 `app`, `shared` 기반을 정리하고, 이후 `pages/widgets/features/entities`로 점진적으로 분리한다.
- Cross-import는 code smell로 보고, 상위 Layer 조립이나 Slice 병합, 도메인 로직 하향 이동으로 해결한다.
- `entities`는 접근 범위가 넓기 때문에 처음부터 세분화하지 않는다. 단순 CRUD는 `shared/api`에 두는 편이 낫다.
- Next.js와 함께 사용할 때는 Next.js 라우터 폴더와 FSD Layer를 분리한다.

## 관련 문서

- [[FSD 폴더구조]]
- [[Next.js]]

## 출처

- `raw/feature-sliced-design/kr/docs/get-started/overview.mdx`
- `raw/feature-sliced-design/kr/docs/reference/layers.mdx`
- `raw/feature-sliced-design/kr/docs/reference/slices-segments.mdx`
- `raw/feature-sliced-design/kr/docs/reference/public-api.mdx`
- `raw/feature-sliced-design/kr/docs/reference/slice-groups.mdx`
- `raw/feature-sliced-design/kr/docs/guides/tech/with-nextjs.mdx`
- `raw/feature-sliced-design/kr/docs/guides/issues/cross-imports.mdx`
- `raw/feature-sliced-design/kr/docs/guides/issues/excessive-entities.mdx`
