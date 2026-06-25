---
type: concept
platform: app
status: draft
tags: [frontend, react-native, expo, hermes, js-engine]
source: https://hermesengine.dev/, https://reactnative.dev/docs/hermes
created: 2026-06-25
updated: 2026-06-25
---

# Hermes

> React Native용으로 만들어진 JavaScript 엔진. RN/Expo의 기본 엔진. 상위: [[index]] · 관련: [[React Native]] · [[Metro]] · [[New Architecture]]

## TL;DR
모바일에서 **앱 시작 속도·메모리·앱 크기**를 최적화하려고 Meta가 만든 경량 JS 엔진. JSC(JavaScriptCore) 대신 기본 채택. 핵심은 **빌드 시점 바이트코드 사전 컴파일(AOT)**.

## 왜 Hermes
모바일은 데스크톱 브라우저와 제약이 다르다 — 느린 시작, 적은 메모리, 큰 번들이 곧 비용.

| 지표 | 효과 |
|---|---|
| **TTI (Time To Interactive)** | 바이트코드 사전 컴파일로 시작 시 JS 파싱·컴파일 생략 → 시작 빠름 |
| **메모리 사용** | JIT 없는 설계로 메모리 footprint 작음 |
| **앱 크기(APK/IPA)** | 엔진이 가벼워 바이너리 크기 절감 |

## 동작 핵심
- **AOT 바이트코드** — 일반 JS 엔진은 런타임에 JS를 파싱·컴파일하지만, Hermes는 **빌드 때 Hermes 바이트코드(.hbc)로 미리 변환**. 기기에서는 바이트코드를 바로 실행 → 시작 지연 감소.
- **No JIT** — JIT(Just-In-Time) 컴파일 미사용. 장기 실행 핫 루프의 최고 성능은 JSC보다 낮을 수 있으나, 모바일 앱 특성상 시작·메모리 이득이 더 큼. [추론]
- **[[Metro]] 연동** — 프로덕션 번들을 Metro가 만들고 Hermes 컴파일러(`hermesc`)가 바이트코드로 변환.

## RN/Expo에서
- RN 0.70+, Expo SDK 기본 엔진이 Hermes. [검증 필요] (정확한 버전은 릴리스 노트 확인)
- **Hermes 디버깅** — Chrome DevTools 프로토콜 지원. 소스맵으로 바이트코드 스택트레이스를 원본 위치로 복원.
- 일부 최신 JS 기능 지원이 V8/JSC보다 늦을 수 있음 → 사용 전 지원 여부 확인. [검증 필요]

## 출처
- Hermes 공식: https://hermesengine.dev/
- RN 문서: https://reactnative.dev/docs/hermes
