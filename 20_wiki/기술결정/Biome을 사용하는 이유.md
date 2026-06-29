---
type: decision
platform: common
status: draft
tags: [frontend, tooling, formatter, linter, biome, decision]
source:
  - "https://biomejs.dev/"
  - "https://biomejs.dev/linter/"
  - "https://biomejs.dev/guides/migrate-eslint-prettier/"
created: 2026-06-22
updated: 2026-06-22
---

# Biome을 사용하는 이유

## 결정
프론트엔드 프로젝트의 기본 포맷터·린터 후보로 ESLint + Prettier 조합 대신 Biome을 우선 검토한다.

이 문서는 모든 프로젝트에서 ESLint와 Prettier를 금지하는 문서가 아니다. 실제 채택 여부는 프로젝트 `docs/`에서 확정한다.

## 배경
기존 프론트엔드 품질 도구는 보통 Prettier가 포맷팅을 맡고 ESLint가 린팅을 맡는다. 여기에 TypeScript ESLint, React plugin, import plugin, eslint-config-prettier 같은 설정이 붙으면 도구 경계와 설정 표면이 커진다.

Biome은 포맷팅, 린팅, import 정리, 마이그레이션을 하나의 도구 체인으로 제공한다. 속도와 설정 단순성을 우선하는 프로젝트에서 유지비를 줄일 수 있다.

## 선택지

| 선택지 | 장점 | 단점 |
|---|---|---|
| ESLint + Prettier | 생태계·플러그인 풍부, 오래 검증됨 | 설정 파일과 플러그인이 많고 포맷/린트 충돌 관리 필요 |
| Biome | 빠른 포맷/린트, 단일 설정, 마이그레이션 도구 | ESLint 플러그인 생태계를 완전히 대체하지 못할 수 있음 |
| Prettier만 사용 | 단순함 | 코드 품질 규칙 부족 |

## 판단 기준
- 프로젝트가 TypeScript/JavaScript/React 중심인가?
- ESLint custom plugin이나 복잡한 rule 확장이 꼭 필요한가?
- 포맷팅과 린팅 속도가 개발 경험에 영향을 주는가?
- 설정 단순화가 팀 유지보수에 도움이 되는가?
- 기존 ESLint/Prettier 설정을 Biome으로 이식 가능한가?

## 최종 선택
다음 조건이면 Biome을 선택한다.

- 새 프로젝트
- TypeScript/React 중심 프로젝트
- 복잡한 ESLint plugin 의존이 적은 프로젝트
- 포맷팅·린팅 속도와 설정 단순성이 중요한 프로젝트
- pre-commit, CI에서 빠른 피드백이 필요한 프로젝트

다음 조건이면 ESLint + Prettier 유지도 가능하다.

- 프레임워크나 조직 규칙이 특정 ESLint plugin에 강하게 의존
- 커스텀 lint rule이 많음
- Biome이 아직 지원하지 않는 언어/파일/규칙이 핵심
- 마이그레이션 비용이 품질 이점보다 큼

## 선택 이유

### 1. 포맷터와 린터를 한 도구로 운영한다
Prettier와 ESLint를 같이 쓰면 포맷 규칙 충돌을 별도 설정으로 관리해야 한다. Biome은 포맷터와 린터를 같은 도구 체인에서 제공해 설정 표면을 줄인다.

### 2. 빠른 실행 속도가 개발 루프를 짧게 만든다
Biome은 빠른 포맷터를 목표로 하며 JavaScript, TypeScript, JSX, TSX, JSON, HTML, CSS, GraphQL 포맷을 지원한다. 저장 시 포맷, pre-commit, CI에서 빠른 피드백을 기대할 수 있다.

### 3. Prettier 호환성이 높다
Biome 공식 문서는 Prettier와 높은 호환성을 강조한다. 기존 Prettier 기반 팀이 완전히 다른 스타일로 갈아타는 부담이 작다.

### 4. ESLint 마이그레이션 명령을 제공한다
`biome migrate eslint`, `biome migrate prettier` 같은 마이그레이션 경로가 있다. 기존 설정을 무조건 수동으로 다시 작성하지 않아도 된다.

### 5. 단일 설정 파일이 에이전트 작업에 유리하다
AI 에이전트가 프로젝트 품질 규칙을 따라야 할 때 설정 진입점이 적을수록 실수가 줄어든다. `biome.json` 중심이면 포맷·린트 규칙 확인이 쉽다.

## 포기한 것
- ESLint plugin 생태계 전체를 그대로 쓰기 어렵다.
- 조직이 가진 custom rule은 별도 검토가 필요하다.
- 프레임워크별 특수 lint 규칙은 ESLint가 더 풍부할 수 있다.
- 기존 프로젝트에서는 마이그레이션 중 rule 차이로 diff가 커질 수 있다.

## 에이전트 적용 기준
- 새 프로젝트면 Biome을 우선 제안한다.
- 기존 프로젝트면 ESLint/Prettier 설정과 plugin 의존을 먼저 확인한다.
- Biome으로 옮길 때는 포맷 diff와 lint rule 차이를 별도 PR로 분리하는 것이 좋다.
- `biome check`를 기본 품질 게이트로 두고, 프레임워크 특수 규칙이 필요하면 ESLint 병행을 검토한다.

## 관련 문서
- [[코드 품질]]
- [[가독성]]
- [[예측 가능성]]
- [[커밋 컨벤션]]

## 출처
- Biome: https://biomejs.dev/ (확인: 2026-06-22)
- Biome Linter: https://biomejs.dev/linter/ (확인: 2026-06-22)
- Biome migration guide: https://biomejs.dev/guides/migrate-eslint-prettier/ (확인: 2026-06-22)
