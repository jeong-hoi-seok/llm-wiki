---
type: source-summary
status: summarized
tags: [frontend, architecture, fsd]
source: raw/kakaopay/fsd-architecture.md
created: 2026-06-19
updated: 2026-06-19
---

# 카카오페이 FSD 도입기 요약

## 원문 정보
- 제목: FSD 아키텍처 도입기
- 작성자: 마쉬 (카카오페이 FE)
- URL: `[검증 필요]` (tech.kakaopay.com 추정)
- 원본: `raw/kakaopay/fsd-architecture.md`

## 핵심 요약
사장님 플러스(ceo-plus) 레포가 역할 중심 디렉토리(components/apis/pages…)로 시작 → 서비스 확장(혜택·쿠폰·멤버십)으로 기능별 그룹핑 추가 → **코드 위치 모호·사이드이펙트·기존 작업자 의존** 문제. FDD 시도 실패 후 **FSD 도입**으로 재사용 레벨을 코드 구조로 강제. 운영 중 서비스라 **상향식(bottom-up) 점진 마이그레이션**.

## 주요 주장 (실전 의사결정)
1. **widgets 레이어 미사용** — 재사용 독립 UI 비중 낮음. 재사용 UI도 `features` 안에서 통합 관리. → FSD는 필요한 레이어만 선택 가능.
2. **Slice Grouping 허용** — 동일 도메인의 여러 페이지(목록·상세·등록·수정)를 페이지별 슬라이스로 두면 슬라이스 폭증. 동일 도메인 데이터 페이지를 `pages/benefit/{benefitList,benefitDetail,…}`로 그룹핑. (공식 가이드 1페이지=1슬라이스의 허용 변형)
3. **API 3계층 배치 기준** — 재사용 범위로 결정:
   - 한 슬라이스 전용 → `pages/[slice]/api`
   - 슬라이스 그룹 내 재사용(기능) → `features/[slice]/api` (Features = slice group 단위)
   - 여러 그룹 재사용(데이터) → `entities/[domain]/api` (Entities = DB 개념 도메인)
   - ⚠️ 모든 API를 Entities에 몰면 Entity 비대화 — 안티패턴.
4. **상향식 이동 규칙** — pages에서 시작 → 재사용 생기면 위 레이어로 승격(pages→features→entities→shared). "이 코드가 어디까지 재사용되나?" 질문으로 자연 체득.

## 실무에서 가져갈 점
- FSD는 정석 그대로보다 **프로젝트 특성에 맞춘 변형**(widgets 생략, slice grouping)이 현실적.
- API/코드 배치는 "재사용 범위"가 단일 판단축.
- 운영 서비스는 빅뱅 전환 대신 bottom-up 점진 적용.

## 관련 개념
- [[FSD 폴더구조]] — 이 요약의 실전 결정들이 개념 문서에 "실전 적용 패턴"으로 반영됨.

## 검증 필요 사항
- 원문 URL `[검증 필요]`.
- Processes 레이어 deprecated, Pages/Widgets 정의는 FSD 공식문서 버전에 따라 변동 가능 → [[공식 문서 참조]] 또는 feature-sliced.design 재확인.
