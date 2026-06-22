<!--
SOURCE: 카카오페이 기술 블로그 — "FSD 아키텍처 도입기" (저자: 마쉬, FE)
URL: [검증 필요] (tech.kakaopay.com 추정)
INGESTED: 2026-06-19
NOTE: 원본 보존용. LLM 수정 금지. 정리본은 wiki/sources/kakaopay-fsd-summary.md
-->

# FSD 아키텍처 도입기 (카카오페이, 마쉬)

요약: 레포지토리의 복잡한 구조적 문제를 해결하기 위해 FSD(Feature-Sliced Design) 아키텍처를 도입한 경험과 그 과정에서 했던 고민들을 공유합니다.

## 리뷰어 한줄평
- pebble.stone: 아키텍처에 정답은 없지만, 팀이 직면한 pain point를 명확히 진단하고 FSD를 도입하여 '지속 가능한 서비스 구조'를 구축해 나가는 과정이 인상적입니다.
- teemo.c: 프로젝트 규모가 커지며 구조적 복잡도나 사이드 이펙트로 고민 중이라면, 마쉬의 실전 경험이 담긴 FSD 적용 사례를 통해 명확한 방향성을 얻을 수 있을 거예요.

## 시작하며
카카오페이 FE 마쉬. 새 기능 추가·수정 때마다 "이 코드 어디 넣지", "얽힌 의존성 어떻게 풀지" 고민. 프로젝트 규모 커질수록 명확한 구조 없으면 괴로움. 이 구조적 한계 해결 위해 FSD 도입. 기존 프로젝트 구조적 한계 극복 + 확장성·유지보수성 향상 목적.

## FSD 아키텍처란?
공식문서 기준 FSD = Feature-Sliced Design(기능 분할 설계). 프론트엔드 애플리케이션 구조를 위한 아키텍처 방법론. 컴포넌트 중심 UI 환경에서 복잡한 UI 애플리케이션 구조를 체계적으로 조직. 초기 버전 단점을 계속 보완하는 살아있는 방법론.
핵심: 코드를 3가지 차원으로 분리하여 재사용 가능한 로직 통제, 높은 응집도·낮은 결합도 유지. 3차원 = 수직 레이어(Layers), 수평 슬라이스(Slices), 기술적 목적 세그먼트(Segments).

### 핵심 개념
- 레이어(Layers): 재사용 범위·의존성(import) 규칙에 따라 수직적으로 나눈 계층. 상위→하위 단방향 의존 (app > pages > widgets > features > entities > shared).
- 슬라이스(Slices): 레이어 내 코드를 비즈니스 도메인별 분할. 같은 레이어 내 다른 슬라이스 참조 불가 (높은 응집도, 낮은 결합도).
- 세그먼트(Segments): 슬라이스 내 코드를 기술적 목적에 따라 구분. 이름은 ui/api/model 등 목적 설명 (components/hooks 같은 성격 지양).

### 1. Layers
가장 큰 범위. 어떤 기능 담당, 다른 코드에 얼마나 의존하는지 기준. 모든 레이어 사용 필요 없고 필요한 것만 추가.
- App: 앱 전체 영향 코드(라우터, 진입점, 전역 상태, 전역 스타일, 프로바이더). slice 없이 segment로만.
- Processes: 페이지 간 복잡 시나리오. 더 이상 사용 안 됨(deprecated).
- Pages: 전체 페이지 또는 중첩 라우팅의 주요 부분. 대부분 페이지 1개 = 슬라이스 1개(유사 페이지는 묶기 가능).
- Widgets: 독립 작동, 여러 페이지 재사용되는 대규모 기능/UI 컴포넌트. 보통 하나의 완전한 기능.
- Features: 제품 전반 재사용되는 기능 구현체. 사용자에게 실질 비즈니스 가치 제공 동작(동사적 개념).
- Entities: 프로젝트가 다루는 비즈니스 데이터(명사적 개념).
- Shared: 앱 기본 구성 요소. 재사용 가능, 비즈니스 특성과 분리. slice 없이 segment로만.
레이어는 위→아래 단방향 의존. 예) features/benefit/ui/A.tsx는 shared/ui/B.tsx import 가능, 역방향 불가.

### 2. Slices
2번째 계층. 레이어 내 코드를 비즈니스 도메인별 분할. 슬라이스 이름은 표준화 안 됨, 비즈니스 도메인에 따라 결정. 내부는 팀 자유 구성. 연관 높은 슬라이스는 그룹핑 가능. 같은 레이어 내 다른 슬라이스 참조 불가 → 높은 응집도·낮은 결합도.
- 응집도 높음 = 관련 기능 함께 모임. 도메인 코드들이 슬라이스별로 모임.
- 결합도 낮음 = 서로 독립 작동. 같은 레이어 내 다른 슬라이스 참조 불가 → 각 슬라이스 독립 기능.

### 3. Segments
마지막 계층. 기술적 목적 구분. 이름은 코드 성격(components/hooks) 아닌 목적 설명(예: components 대신 ui).
- ui: UI 관련 모든 것(컴포넌트, 날짜 포맷터, 스타일 등)
- api: 백엔드 통신·데이터 로직(request 함수, 데이터 타입, mapper 등)
- model: 도메인 모델(스키마, 인터페이스, 스토어, 비즈니스 로직 — Redux/Zustand 액션·리듀서·셀렉터)
- lib: 슬라이스 내에서만 쓰는 라이브러리 코드
- config: 설정 파일, feature-flag

레이어별 세그먼트 활용:
- ui: Pages(재사용 안 되는 UI·로딩·에러), Widgets(여러 페이지 재사용 큰 UI), Features(Form 등 상호작용 UI), Entities(도메인 관련 재사용 UI), Shared(비즈니스 로직 제외 공통 UI)
- api: Pages(데이터 조회/변경 요청), Features(기능 관련 요청), Entities(해당 Entity 요청), Shared(공통 코드)
- model: Features(검증·내부 상태), Entities(데이터 상태·검증 스키마)
- config: Features(feature flag), Shared(환경변수·전역 feature flag)

### FSD 전체 구조
```
└─ src
  ├─ app (routes, store, styles, entrypoint, ...)
  ├─ pages (benefitList{ui,api}, benefitDetail, ...)
  ├─ widgets (benefitItem{ui}, ...)
  ├─ features (benefit{config,model,api,ui}, coupon{...}, ...)
  ├─ entities (benefit{model,api,ui}, coupon{...}, ...)
  └─ shared (api, ui, lib, config, routes, i18n, ...)
```

### 장단점
장점: ① 구조 명확 → 협업·온보딩 용이. ② 추가 기능 시 사이드 이펙트 확률↓(레이어·슬라이스 구분). ③ 유지보수성↑(재사용/지역 코드 레벨 구분).
단점: 모든 작업자 FSD 학습 필요.

## FSD를 선택한 이유
기존 사장님 플러스 레포(ceo-plus). 초기 웹뷰 단일 페이지, 역할 중심 디렉토리(components, apis, pages, configs, mocks, constants, errors, helpers, hooks, styles, types).
서비스 확장(혜택·쿠폰·멤버십·매장소식) → components/benefit, apis/benefit, pages/benefit 식 기능별 그룹핑 추가.

### 직면한 문제
1. 개발 생산성 저하·탐색 비용 증가: 공통 컴포넌트·API 위치 모호. 코드 위치 즉각 파악 어려움. 신규 작업자마다 다르게 이해 → pages/***/components 같은 비일관 구조. 의사결정 비용↑.
2. 시스템 안정성 저하·기존 작업자 의존 심화: 기능·공통 로직 뒤섞임. 수정 시 예상 못한 다른 화면 영향. 모듈 경계 불분명 → 영향 범위 예측 어려움. 변경 부담↑, 기존 작업자 의존↑.

### 선택 이유
FDD(Feature-Driven Development)로 리팩토링 시도했으나 ceo-plus에 안 맞음. 여러 feature에서 쓰지만 범용은 아닌 ui/API/hooks/helper/types 너무 많음 → shared/common에 넣으면 재사용 범위 통제 못 해 의존성 다시 엉킴. 여러 feature 환경, 한 서비스 수정이 다른 서비스에 영향 안 주게 분리하고 싶었음.
FSD 공식문서 문구: "현재 구조 문제없으면 안 바꿔도 됨. 단 ① 프로젝트 커지며 구조 얽혀 개발 속도↓ ② 새 팀원이 구조 이해 어려움 → 도입 고려."
결론: ① 레이어·슬라이스로 재사용/지역 레벨 명확 구분 ② 코드 레벨 구조적 분리 가능 → ceo-plus에 가장 적합.

## FSD 적용 과정
공식 마이그레이션 가이드 따름. 기본 1~4단계 생략, 프로젝트 특성상 추가 고민 부분 중심.
작업 전 구조: benefit/coupon/membership/shared 각각 {components, apis, pages, helpers} + configs/mocks/constants/errors/helpers/hooks/styles/types.

### 1. widgets 사용 여부 → 미사용
재사용 독립 UI 컴포넌트 비중 낮음, 대부분 특정 페이지 한정. 일부 재사용도 features 레이어 내에서 충분히 관리 가능 → widgets 레이어 미추가. 재사용 UI는 features 레이어 내 배치, features가 기능 단위 안에서 UI까지 통합 관리.

### 2. Slice Grouping 허용 + Pages 구성
동일 도메인 다양한 페이지 제공 → 페이지별 Slice 두면 너무 많은 Slice(혜택만 해도 목록·상세·등록·수정...). 작업 속도 저해. 공식 가이드: 1페이지=1슬라이스 권장하나 동일 도메인 데이터 다루는 페이지는 하나의 Slice 가능.
결론: 동일 도메인 데이터 페이지를 묶는 Slice Grouping 허용 + 레이어 간 Import Rule 준수.
허용 후: pages/benefit(slice group)/{benefitCreate, benefitDetail, benefitList}(slice)/segments.

### 3. API 배치: pages vs features vs entities
초기엔 모든 API를 Entities에 몰아넣으려다 문제 — 특정 페이지에서만 쓰는 단순 조회 API까지 전역 Entity에 섞여 Entity 비대화, 진정한 도메인 엔티티 역할 어려움.
기준 (API가 어느 도메인/범위에서 재사용되는가):
- 페이지 내 슬라이스에서만: pages/[slice]/api. ex) pages/benefit/benefitList/api (BenefitList에서만 쓰는 목록 조회)
- 페이지 슬라이스 그룹 내 다른 슬라이스 재사용(기능): features/[slice]/api. Features는 slice group 단위. ex) features/benefit/api (혜택 상세 조회 — BenefitDetail·BenefitUpdate에서 사용)
- 여러 페이지 슬라이스 그룹 재사용(데이터): entities/[domain]/api. Entities는 DB 개념 도메인. ex) entities/partner/api (매장 상세 조회)

### 4. 상향식(Bottom-Up) 채택
FSD는 점진적 도입 가능 → 운영 중 서비스에 최적. 단계:
1. 우선 pages 하위에 모두 작업
2. 여러 pages/[slice-group]/slice에서 쓰는 코드 → features/[slice-group]로 이동
3. 여러 features/[slice-group]에서 쓰는 코드 → entities/domain으로 이동
4. 전역 사용 코드 → shared
"이 코드가 현재 어느 레이어까지 재사용되는가?" 질문을 던지게 됨 → pages>features>entities>shared 흐름 자연 체득.

## FSD 적용 후기
- 명확한 코드 위치: pages(특정 slice에서만), features(특정 slice-group에서만), entities(여러 slice-group), shared(전역).
- 재사용 통제: 잘못된 의존성(상위가 하위 참조) 사전 방지.
- 비즈니스 도메인 명확화: 슬라이스 기준 묶임 → 역할·도메인 한눈에.
- 개발 생산성↑: 배치 고민 시간 사라짐 → 신규 기능 착수 빨라짐.
- 리팩토링·테스트 용이: 높은 응집도 → 영향 범위 예측 쉬움 → 배포 안정성↑.

## 마치며
학습 부담 있으나 점진적 도입 가능, 장기적으로 일관성·확장성·유지보수성 크게 향상. "이 코드 어디 넣지?"라는 근원적 질문에 확신을 주는 전략적 도구. 구조적 문제 고민 팀에 적극 추천.
