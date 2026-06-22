---
type: architecture
scope: pattern
platform: common
status: verified
tags: [frontend, architecture, fsd]
source: raw/kakaopay/fsd-architecture.md, https://feature-sliced.design/
created: 2026-06-19
updated: 2026-06-22
project_specific: false
adoption: optional
---

# FSD 폴더구조

## 한 줄 요약
프론트엔드 코드를 **레이어(수직) → 슬라이스(수평) → 세그먼트(목적)** 3차원으로 분리해, 재사용 범위와 의존 방향을 명확히 만드는 아키텍처 패턴. 상위: [[프론트엔드 아키텍처 개요]]

## 이 문서의 범위
이 문서는 FSD를 설명하는 패턴 문서다. 특정 프로젝트가 반드시 FSD를 따라야 한다는 팀 규칙 문서가 아니다.

프로젝트에서 FSD를 채택할지, 어떤 레이어를 생략할지, import 규칙을 어떻게 강제할지는 각 프로젝트의 `docs/`에서 결정한다. Obsidian은 그 결정을 더 잘 쓰기 위한 참고 지식으로 사용한다.

## 핵심 원칙
1. **레이어**로 재사용 범위·추상화 수준을 나눈다. 의존성은 위→아래 **단방향**.
2. **슬라이스**는 비즈니스 도메인 단위. 같은 레이어의 다른 슬라이스끼리 직접 import 금지.
3. 슬라이스는 **public API(`index.ts`)** 로만 외부 노출.
4. **세그먼트**는 코드의 *성격*이 아닌 *목적*으로 이름 짓는다 (`components`/`hooks` ❌ → `ui`/`api`/`model` ✅).
5. 모든 레이어를 다 쓸 필요 없다 — **필요한 레이어만 선택**.

## 1. 레이어 (Layers)
재사용 범위·의존성 규칙에 따른 수직 계층. 의존 방향: **app > pages > widgets > features > entities > shared** (상위가 하위를 import, 역방향 불가).

| 레이어 | 역할 | 슬라이스 | 비고 |
|---|---|---|---|
| `app` | 앱 전체 영향 — 라우터·진입점·전역 상태·전역 스타일·프로바이더 | 없음 (segment만) | |
| ~~`processes`~~ | 페이지 간 복잡 시나리오 | — | **deprecated, 사용 안 함** |
| `pages` | 전체 페이지 / 중첩 라우팅의 주요 부분 | 1페이지 ≈ 1슬라이스 | 유사·동일 도메인 페이지는 묶기 가능 |
| `widgets` | 독립 작동, 여러 페이지 재사용되는 대규모 UI/기능 블록 | 도메인/기능 단위 | 보통 하나의 완전한 기능 |
| `features` | 사용자에게 비즈니스 가치를 주는 재사용 기능 (**동사적**) | 기능 단위 | `add-to-cart`, `auth-by-email` |
| `entities` | 프로젝트가 다루는 비즈니스 데이터 (**명사적**) | 도메인 단위 | `user`, `product`, `partner` |
| `shared` | 비즈니스와 분리된 기본 구성 요소 | 없음 (segment만) | UI kit·lib·config·api 클라이언트 |

- **단방향 의존 예시**: `features/benefit/ui/A.tsx` → `shared/ui/B.tsx` import ✅ / 역방향 ❌.
- `app`·`shared`는 슬라이스 없이 **세그먼트로만** 구성.

## 2. 슬라이스 (Slices)
레이어 내부를 **비즈니스 도메인별**로 분할. 이름은 표준화되어 있지 않고 도메인에 따라 결정 (`benefit`, `coupon`, `user-profile`). 슬라이스 내부 구성은 팀 자유. 연관 높은 슬라이스는 **그룹핑** 가능.

```
✅ user-profile   auth-form   benefit   coupon
❌ profile (도메인 모호)   UserProfile   utils (역할 모호)
```

**같은 레이어 내 다른 슬라이스 참조 금지** → 왜 응집도↑·결합도↓?
- **응집도↑**: 한 도메인 코드가 슬라이스에 모임 → 관련 기능이 함께.
- **결합도↓**: 옆 슬라이스를 못 부르니 각자 독립 작동 → 영향 범위 격리.
- 슬라이스 간 공유가 필요하면 → **하위 레이어(entities·shared)로 내려서** 공유.

## 3. 세그먼트 (Segments)
슬라이스 내부를 **기술적 목적**으로 구분. 이름은 목적을 설명.

| 세그먼트 | 내용 |
|---|---|
| `ui` | UI 관련 전부 — 컴포넌트, 날짜 포맷터, 스타일 |
| `api` | 백엔드 통신·데이터 로직 — request 함수, 데이터 타입, mapper |
| `model` | 도메인 모델 — 스키마, 인터페이스, 스토어, 비즈니스 로직 (Zustand/Redux 액션·리듀서·셀렉터) |
| `lib` | 슬라이스 내에서만 쓰는 라이브러리 코드 |
| `config` | 설정·feature-flag |

**레이어별 세그먼트 활용** (같은 세그먼트라도 레이어마다 역할 다름):

| | `pages` | `widgets` | `features` | `entities` | `shared` |
|---|---|---|---|---|---|
| **ui** | 재사용 안 되는 UI·로딩·에러 | 여러 페이지 재사용 큰 UI | Form 등 상호작용 UI | 도메인 재사용 UI | 비즈니스 로직 제외 공통 UI |
| **api** | 데이터 조회/변경 요청 | — | 기능 관련 요청 | 해당 Entity 요청 | 공통 요청 코드 |
| **model** | — | — | 검증·내부 상태 | 데이터 상태·검증 스키마 | — |
| **config** | — | — | feature flag | — | 환경변수·전역 feature flag |

## 전체 구조
```
└─ src
  ├─ app        # routes, store, styles, entrypoint, providers
  ├─ pages      # benefitList{ui,api}, benefitDetail, ...
  ├─ widgets    # benefitItem{ui}, ...        (생략 가능)
  ├─ features   # benefit{config,model,api,ui}, coupon{...}
  ├─ entities   # benefit{model,api,ui}, partner{...}
  └─ shared     # api, ui, lib, config, routes, i18n
```

## 패턴 내부 import 규칙
프로젝트가 FSD를 채택했다면 보통 아래 규칙을 사용한다.

```ts
// ✅ slice 외부에서는 public API(index.ts)만
import { AuthForm } from '@/features/auth-form';
import { Button } from '@/shared/ui';

// ❌ 슬라이스 내부 파일 직접 import (캡슐화 깨짐)
import { AuthForm } from '@/features/auth-form/ui/auth-form';

// ❌ 같은 레이어 슬라이스끼리 직접 의존
//    features/auth-form → features/cart  ❌
//    공유 필요 시 entities 또는 shared로 내림
```

## 안티패턴
- `utils/`·`components/` 같은 **역할 기반 평면 분할** (FSD는 기능 기반 수직 분할).
- 슬라이스 내부 파일을 외부에서 직접 import.
- 하위 레이어가 상위를 import (의존 역방향 — `entities`가 `features` 참조 ❌).
- **모든 API를 `entities`에 몰아넣기** → Entity 비대화, 도메인 엔티티 역할 상실. (→ 아래 실전 패턴 3)
- 거대 `shared` — 도메인 코드가 shared로 새는 것.

## 실전 적용 패턴 (카카오페이 ceo-plus 사례)
> 출처: [[카카오페이 FSD 도입기 요약]] — 정석을 프로젝트 특성에 맞게 변형한 실전 결정.

1. **widgets 생략** — 재사용 독립 UI 비중이 낮으면 widgets 레이어를 빼고, 재사용 UI도 `features` 안에서 통합 관리. (필요한 레이어만 쓰는 원칙)
2. **Slice Grouping** — 동일 도메인의 페이지가 많을 때(목록·상세·등록·수정) 페이지별 슬라이스가 폭증 → 동일 도메인 데이터 페이지를 묶음:
   ```
   pages/benefit/          # slice group
     benefitList/  benefitDetail/  benefitCreate/   # slices
   ```
3. **API 배치 = 재사용 범위로 결정** (가장 자주 하는 고민):
   | 재사용 범위 | 위치 | 예 |
   |---|---|---|
   | 한 슬라이스 전용 | `pages/[slice]/api` | `pages/benefit/benefitList/api` |
   | 슬라이스 그룹 내 재사용(기능) | `features/[slice]/api` | `features/benefit/api` |
   | 여러 그룹 재사용(데이터) | `entities/[domain]/api` | `entities/partner/api` |
   - Features = slice group 단위, Entities = DB 개념 도메인.
4. **상향식(Bottom-Up) 점진 마이그레이션** — 운영 중 서비스에 적합. 빅뱅 전환 ❌:
   ```
   1) 일단 pages 하위에 작업
   2) 여러 slice가 쓰면 → features로 승격
   3) 여러 slice-group이 쓰면 → entities로 승격
   4) 전역이면 → shared
   ```
   판단 질문: **"이 코드가 지금 어디까지 재사용되나?"** → pages > features > entities > shared 흐름을 자연 체득.

## 도입 판단 기준
공식문서: 현재 구조에 문제없으면 안 바꿔도 된다. 단 아래면 도입 고려.
- 프로젝트가 커지며 구조가 얽혀 **개발 속도가 느려짐**.
- **새 팀원이 구조를 이해하기 어려움**.

## 채택 시 변형 예시
- **RN/Expo (간소화)**: `app`(라우트) / `features` / `entities` / `shared`. widgets·pages 생략. → [[앱 기술 스택]], [[Expo 프로젝트 부트스트랩]]
- alias `@/*` → `src/*`. 슬라이스 외부는 배럴(`index.ts`)만.

위 예시는 채택 사례다. 프로젝트별 실제 규칙은 프로젝트 `docs/`에 별도로 남긴다.

## 장단점
- 👍 구조 명확(협업·온보딩↑) / 사이드이펙트↓ / 유지보수성↑ / 영향 범위 예측 쉬움.
- 👎 모든 작업자 FSD 학습 필요.

## 관련 문서
- [[프론트엔드 아키텍처 개요]] — 프론트엔드 아키텍처 기본 관점
- [[레이어드 아키텍처]] — FSD의 레이어 사고 기반
- [[모듈러 아키텍처]] — FSD의 슬라이스·세그먼트 분리 기반
- [[네이밍 컨벤션]] — 슬라이스·파일 명명
- [[카카오페이 FSD 도입기 요약]] — 실전 도입 사례 원문 요약
- [[앱 기술 스택]] · [[Expo 프로젝트 부트스트랩]] — RN/Expo에서의 FSD 간소화 적용

## 출처
- [feature-sliced.design](https://feature-sliced.design/) (확인: 2026-06)
- [[카카오페이 FSD 도입기 요약]] (카카오페이 마쉬, ingest: 2026-06-19)
