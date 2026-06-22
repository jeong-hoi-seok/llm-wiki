---
type: source-summary
status: summarized
tags: [frontend, react-native, expo]
source: raw/expo-rn-boilerplate/
created: 2026-06-19
updated: 2026-06-19
---

# RN + Expo 보일러플레이트 요약

## 원문 정보
- 제목: react-native-expo-boilerplate `docs/` + `agent.md`
- 원본 위치: `raw/expo-rn-boilerplate/` (`agent.md`, `init-setup.md`, `styling.md`, `naming-convention.md`, `commit-convention.md`, `git-merge-request-guide.md`, `공식 문서 참조.md`)
- 기준: Expo SDK 54 · RN 0.81 · NativeWind ^4.2.1
- 기준일: 2026-06-19

## 핵심 요약
- **스택**: Expo `54.0.35` · RN `0.81.5` · React `19.1.0` · TS strict · NativeWind · Zustand · Biome · **pnpm 전용**.
- **아키텍처**: FSD 폴더구조 간소화 — `src/{app,features,entities,shared}`. slice 외부는 `index.ts` public API만 import. → [[FSD 폴더구조]]
- **스타일**: NativeWind `className` 1순위, variant UI는 CVA + `cn`. → [[NativeWind]]
- **라우트**: 루트 `app/`이 아닌 **`src/app/`** (Expo SDK 54+ 자동 인식).
- **품질 게이트**: husky `pre-commit`(nano-staged + biome) / `pre-push`(typecheck). node 22+.

## 실무에서 가져갈 점
- create-expo-app은 **템플릿 버전 핀 필수**(`expo-template-default@54.0.35`) — 아니면 SDK 불일치로 빌드 깨짐.
- reanimated·safe-area-context는 `pnpm expo install`로 SDK 호환 버전 (최신 `pnpm add` 금지).
- NativeWind 4.2.1+ 필수(Reanimated v4), babel에 `react-native-worklets/plugin` 추가 금지(중복 오류).

## 세부 정리본
- [[앱 기술 스택]] — 스택·핀버전·작업 원칙
- [[Expo 프로젝트 부트스트랩]] — 부트스트랩 7단계 + 함정
- [[실행 스크립트와 Git hooks]] — pnpm 스크립트 · husky hooks
- [[공식 문서 참조]] — RN·Expo·NativeWind 공식 링크

## 관련 개념
- [[FSD 폴더구조]] · [[네이밍 컨벤션]] · [[NativeWind]] · [[커밋 컨벤션]] · [[MR PR 작성 가이드]]

## 검증 필요 사항
- 핀 버전·SDK 호환은 프로젝트 시점 기준. 최신 작업 시 [[공식 문서 참조]]로 재확인 `[검증 필요]`.
