---
type: decision
platform: web
status: draft
tags: [frontend, design-system, shadcn, base-ui, tailwind, decision]
source:
  - "https://ui.shadcn.com/docs"
  - "https://ui.shadcn.com/docs/changelog/2026-01-base-ui"
  - "https://ui.shadcn.com/docs/changelog/2026-05-shadcn-eject"
  - "https://base-ui.com/"
  - "https://base-ui.com/react/overview/community"
created: 2026-06-22
updated: 2026-06-22
---

# shadcn-ui와 Base UI 선택 기준

## 결정
웹 프로젝트에서 자체 디자인 시스템을 빠르게 만들고 장기적으로 코드 소유권을 가져가고 싶다면 [[shadcn-ui]]를 우선 검토한다.

shadcn-ui 안에서 primitive 선택지가 필요할 때는 Base UI 기반을 우선 검토한다. 단, 이미 Radix 기반 컴포넌트가 많이 쌓인 프로젝트는 기존 선택을 유지하는 것이 더 안전할 수 있다.

## 배경
전통적인 UI 라이브러리는 npm 패키지를 설치하고 외부 컴포넌트를 import해서 쓴다. 빠르게 시작할 수 있지만, 스타일 override, 테마 확장, 컴포넌트 내부 수정, 버전 업데이트에 제약이 생길 수 있다.

shadcn-ui는 “컴포넌트 라이브러리”라기보다 “내 컴포넌트 라이브러리를 만드는 방식”에 가깝다. CLI로 컴포넌트 코드를 프로젝트에 복사하고, 팀이 그 코드를 소유한다.

Base UI는 스타일이 없는 접근성 중심 React primitive다. shadcn-ui는 Radix와 Base UI를 선택할 수 있는 방향으로 확장되었고, Base UI도 shadcn-ui 통합을 공식적으로 제공한다.

## 선택지

| 선택지 | 장점 | 단점 |
|---|---|---|
| MUI / Chakra / Ant Design | 완성형 컴포넌트 빠름 | 디자인 커스터마이징과 override 비용 |
| Radix / Base UI 직접 사용 | 접근성·동작 primitive를 직접 조합 | 스타일·API wrapper를 팀이 직접 설계 |
| shadcn-ui + Radix | 검증된 생태계와 예제 풍부 | Radix API에 맞춘 wrapper 유지 |
| shadcn-ui + Base UI | unstyled primitive + Tailwind wrapper를 함께 가져감 | Radix 대비 상대적으로 새 선택지 |

## 판단 기준
- 팀이 컴포넌트 코드를 직접 소유·수정할 준비가 있는가?
- Tailwind 기반 스타일링을 표준으로 쓸 것인가?
- 디자인 시스템을 장기적으로 키울 것인가?
- 접근성 동작을 직접 구현하지 않고 primitive에 맡길 것인가?
- 완성형 UI보다 커스터마이징 자유도가 중요한가?
- Base UI의 API와 컴포넌트 범위가 프로젝트 요구와 맞는가?

## 최종 선택
다음 조건이면 shadcn-ui를 선택한다.

- [[Tailwind CSS]] 기반 웹 프로젝트
- 자체 디자인 시스템을 만들 예정
- 컴포넌트 코드를 프로젝트 안에서 소유하고 싶음
- 완성형 라이브러리의 테마/override와 싸우고 싶지 않음
- 접근성 primitive 위에서 스타일만 팀 기준으로 입히고 싶음

다음 조건이면 shadcn-ui를 피하거나 신중히 쓴다.

- 팀이 컴포넌트 코드를 유지보수할 여력이 없음
- 빠른 프로토타입만 필요하고 디자인 커스터마이징이 중요하지 않음
- 완성형 엔터프라이즈 컴포넌트가 더 중요한 프로젝트
- Tailwind를 쓰지 않는 프로젝트

## Base UI 기반을 우선 검토하는 이유

### 1. unstyled primitive라 디자인 시스템과 충돌이 적다
Base UI는 스타일을 강제하지 않는 React UI primitive다. 팀 디자인 토큰과 Tailwind 스타일을 직접 입히기 쉽다.

### 2. 접근성 동작을 primitive에 위임할 수 있다
Dialog, Select, Tooltip 같은 인터랙션 컴포넌트는 키보드, 포커스, ARIA 처리가 어렵다. Base UI는 접근성 있는 컴포넌트 라이브러리를 만들기 위한 기반을 제공한다.

### 3. shadcn-ui와 1급 통합이 있다
Base UI 공식 문서는 shadcn-ui 통합을 제공한다고 설명한다. shadcn-ui create나 CLI에서 Base UI 기반을 선택해 Tailwind wrapper를 함께 가져갈 수 있다.

### 4. shadcn-ui의 코드 소유 모델과 잘 맞는다
Base UI는 동작 primitive, shadcn-ui는 코드 배포와 wrapper 생성 방식이다. 두 조합은 “동작은 검증된 primitive, UI는 프로젝트 코드로 소유”라는 방향에 맞다.

## Radix 대신 Base UI를 고를 때
Base UI가 항상 Radix보다 낫다는 뜻은 아니다. 아래 상황이면 Base UI를 우선 검토한다.

- 새 프로젝트라 기존 Radix wrapper가 없음
- shadcn-ui create에서 Base UI 기반으로 시작할 수 있음
- 컴포넌트 API가 더 단순하거나 팀 취향에 맞음
- Base UI의 공식 shadcn 통합을 그대로 활용하고 싶음

아래 상황이면 Radix 유지가 낫다.

- 이미 Radix 기반 shadcn 컴포넌트가 많이 존재
- 팀이 Radix API에 익숙함
- 특정 Radix 컴포넌트나 생태계 의존이 있음
- Base UI로 바꿀 명확한 비용 절감이 없음

## 포기한 것
- npm 패키지 업데이트만으로 컴포넌트가 자동 개선되는 방식은 포기한다.
- 컴포넌트 코드 유지보수 책임을 팀이 가진다.
- 완성형 디자인 시스템의 즉시 사용성은 줄어든다.
- Base UI 기반은 Radix 대비 프로젝트 사례와 레퍼런스가 적을 수 있다.

## 에이전트 적용 기준
- 프로젝트가 `shadcn-ui + Base UI`를 채택했다면 새 컴포넌트는 같은 기반을 유지한다.
- `components/ui`에 복사된 코드는 외부 라이브러리 코드가 아니라 프로젝트 소유 코드로 본다.
- 접근성 동작은 직접 구현하기 전에 Base UI primitive나 shadcn-ui 제공 컴포넌트를 먼저 확인한다.
- Radix 기반 기존 코드와 Base UI 기반 코드를 섞을 때는 컴포넌트별 점진 전환만 허용하고, 한 화면 안에서 중복 primitive가 늘어나는지 점검한다.

## 관련 문서
- [[shadcn-ui]]
- [[Base UI]]
- [[헤드리스 컴포넌트]]
- [[Tailwind CSS]]
- [[cn & cva]]
- [[공통 컴포넌트 추출 기준]]
- [[접근성]]

## 출처
- shadcn/ui Introduction: https://ui.shadcn.com/docs (확인: 2026-06-22)
- shadcn/ui Base UI changelog: https://ui.shadcn.com/docs/changelog/2026-01-base-ui (확인: 2026-06-22)
- shadcn/ui eject changelog: https://ui.shadcn.com/docs/changelog/2026-05-shadcn-eject (확인: 2026-06-22)
- Base UI: https://base-ui.com/ (확인: 2026-06-22)
- Base UI shadcn/ui integration: https://base-ui.com/react/overview/community (확인: 2026-06-22)
