---
type: concept
platform: app
status: draft
tags: [frontend, react-native, expo, bundler, metro]
source: https://metrobundler.dev/, https://docs.expo.dev/
created: 2026-06-25
updated: 2026-06-25
---

# Metro

> React Native의 기본 JavaScript 번들러. 웹의 [[번들링]](webpack·Vite·esbuild)에 대응하는 RN 쪽 도구. 상위: [[index]] · 관련: [[React Native]] · [[Expo]] · [[Hermes]]

## TL;DR
RN 앱의 JS·자산을 하나의 번들로 묶어 기기에 전달하는 번들러. 웹 번들러와 역할은 같지만 **모바일·네이티브 환경에 특화**됐다. Expo는 Metro를 그대로 쓰되 설정을 확장한다.

## 웹 번들러와의 차이
| | 웹 번들러 (Vite·webpack) | Metro |
|---|---|---|
| 대상 | 브라우저 (HTML/CSS/JS) | 네이티브 앱 (JS 번들 + 자산) |
| 진입점 해석 | URL·ESM | Haste / 파일 시스템 기반 모듈 해석 |
| 플랫폼 분기 | 없음 | `.ios.js` / `.android.js` / `.native.js` 확장자 자동 선택 |
| 자산 | import/url-loader 등 | 이미지 등 자산 해상도별(`@2x`,`@3x`) 자동 처리 |
| 결과 | 정적 파일 | 단일 JS 번들 (개발 시 dev server가 HMR 제공) |

## 핵심 동작
1. **Resolution** — 진입점부터 의존성 그래프 구성. 플랫폼별 확장자(`.ios`/`.android`) 우선 해석.
2. **Transformation** — 각 모듈을 Babel로 변환(JSX·TS·최신 문법 → 호환 JS). [[트리 쉐이킹]]은 웹 번들러만큼 강하지 않음 [검증 필요].
3. **Serialization** — 변환된 모듈을 하나의 번들로 직렬화. 프로덕션은 minify + (선택) [[Hermes]] 바이트코드 변환.

## 개발 경험
- **Fast Refresh** — 컴포넌트 상태 유지하며 변경 반영. 웹 [[HMR]] 대응.
- Dev server가 기기·시뮬레이터에 번들 서빙. `metro.config.js`로 transformer·resolver·자산 확장자 커스터마이즈.
- Expo는 `@expo/metro-config`로 기본 설정 제공 (CSS·NativeWind·자산 처리 추가).

## 알아둘 점
- 대형 앱에서 **콜드 스타트 번들 빌드**가 느릴 수 있음 → 캐시(`.metro-cache`)·`--reset-cache` 이슈 잦음. [추론]
- 번들 크기가 곧 시작 비용 → 코드 분할은 웹만큼 자유롭지 않으나 RAM bundle·인라인 require로 일부 지연 로딩 가능. [검증 필요]

## 출처
- Metro 공식: https://metrobundler.dev/
- Expo Metro 설정: https://docs.expo.dev/ (`10_raw/expo/pages/faq.mdx` 등에서 언급)
