---
type: concept
platform: app
status: verified
tags: [frontend, react-native, expo]
source: https://reactnative.dev/, https://docs.expo.dev/
created: 2026-06-19
updated: 2026-06-23
---

# React Native

> [[React]]로 iOS·Android 네이티브 앱을 만드는 프레임워크. 상위: [[index]]

## TL;DR
같은 [[React]] 모델(컴포넌트·훅·상태)을 쓰되, `<div>` 대신 **네이티브 컴포넌트**(`<View>`·`<Text>`)를 렌더. JS가 실제 네이티브 UI를 구동.

## 웹 React와의 차이
| | 웹 React | React Native |
|---|---|---|
| 기본 요소 | `<div>`·`<span>` | `<View>`·`<Text>` |
| 스타일 | CSS / [[Tailwind CSS]] | StyleSheet / [[NativeWind]] |
| 라우팅 | [[Next.js]] 등 | Expo Router |
| 실행 | 브라우저 | 네이티브 + JS 엔진 |
- React 자체(컴포넌트·훅·재조정)는 동일 → 학습 전이 쉬움.

## Expo
- RN 개발·빌드·배포 툴체인. SDK가 카메라·알림 등 네이티브 모듈을 묶어 제공.
- **Expo Go**: 스토어의 샌드박스 앱 — 코드를 즉시 실행(네이티브 빌드 없이). 교육·빠른 시작용.
- **Development Build / EAS Build**: 커스텀 네이티브 모듈·프로덕션용.

## New Architecture
- RN은 New Architecture(Fabric·TurboModules)로 전환 중.
- Legacy Architecture는 RN 0.80 freeze → **0.82에서 제거**. 이 전환이 SDK 버전 선택에 직접 영향.

## ⚠️ SDK 버전 선택 — 왜 SDK 54인가
Expo SDK 54.0.35를 핀하는 이유(Expo Go·App Store·레거시 아키텍처)는 별도 정리:
→ **[[Expo SDK 54 버전 고정]]**

## 스타일 · 구조
- 스타일: [[NativeWind]] (`className`) + [[cn & cva]]
- 구조: 앱 규모가 커지면 [[FSD 폴더구조]] 같은 레이어·Public API 기준을 검토한다.

## 관련 문서
[[React]] · [[NativeWind]] · [[Expo SDK 54 버전 고정]] · [[FSD 폴더구조]]

## 출처
- React Native: https://reactnative.dev/ · Expo: https://docs.expo.dev/
