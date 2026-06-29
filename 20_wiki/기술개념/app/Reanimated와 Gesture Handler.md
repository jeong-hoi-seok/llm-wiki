---
type: concept
platform: app
status: verified
tags: [frontend, react-native, expo, animation, gesture, reanimated]
source:
  - 10_raw/expo/pages/tutorial/gestures.mdx
  - https://docs.swmansion.com/react-native-reanimated/
  - https://docs.swmansion.com/react-native-gesture-handler/
created: 2026-06-25
updated: 2026-06-25
---

# Reanimated와 Gesture Handler

> RN 애니메이션·제스처의 사실상 표준 콤보. 웹의 motion 라이브러리(Framer Motion / motion.dev) 역할을 RN에서 한다. 상위: [[index]] · 관련: [[React Native]] · [[NativeWind]]

## TL;DR
- **Reanimated** — 선언적 애니메이션 라이브러리. 핵심은 애니메이션을 **JS 스레드가 아니라 UI(네이티브) 스레드의 worklet**에서 실행 → JS가 바빠도 60fps 유지.
- **Gesture Handler** — 플랫폼 네이티브 터치 시스템으로 pan·tap·pinch·rotation 등 제스처 인식. RN 기본 `PanResponder`보다 부드럽고 강력.
- 둘을 **짝으로** 써서 제스처에 반응하는 애니메이션(드래그·스와이프·핀치 줌)을 만든다. (raw: `tutorial/gestures.mdx`)

## 웹 motion과의 관계
| | 웹 (Framer Motion / motion.dev) | Reanimated |
|---|---|---|
| 모델 | 선언적 애니메이션 | 선언적 애니메이션 (유사) |
| 실행 위치 | 브라우저 메인/컴포지터 스레드 | **UI 스레드 worklet** (JS 스레드 분리) |
| 목적 | 부드러운 웹 인터랙션 | JS 스레드 정체와 무관한 네이티브급 60fps |

→ "motion 애니메이션 같은 것"이 맞다. 차이는 **실행 스레드를 분리**해 RN의 JS 스레드 병목을 피하는 점.

## Reanimated 핵심 개념
- **Worklet** — UI 스레드에서 실행되는 JS 함수. 애니메이션 로직을 메인 JS 스레드에서 떼어냄.
- **Shared Value** (`useSharedValue`) — UI 스레드와 공유되는 애니메이션 값. 변경 시 리렌더 없이 네이티브에서 반영.
- **`useAnimatedStyle`** — shared value를 스타일에 매핑. 값 변화가 곧 애니메이션.
- **`withTiming` / `withSpring`** — 시간·스프링 기반 보간 함수.

## Gesture Handler 핵심
- **`GestureHandlerRootView`** — 앱(또는 화면) 루트를 감싸야 동작. (raw 튜토리얼 1단계)
- **`Gesture.Pan()` / `Gesture.Tap()` 등** — 제스처 정의 후 `GestureDetector`로 뷰에 부착.
- 네이티브 터치 핸들링 사용 → 스크롤·중첩 제스처 충돌 처리가 RN 기본보다 안정적.

## 전형 패턴 (raw 튜토리얼)
- **더블 탭으로 스케일** — tap 제스처 → shared value 변경 → `withSpring`으로 크기 애니메이션.
- **팬으로 이동** — pan 제스처의 translation을 shared value에 반영 → 요소를 화면에서 끌어 이동.

## 알아둘 점
- Expo에 사전 통합 — `expo install`로 버전 호환 설치. [검증 필요] (버전 호환은 [[Expo]] SDK 기준 확인)
- Reanimated는 Babel plugin 설정 필요(`babel.config.js`). [검증 필요]
- 무거운 JS 연산이 있어도 애니메이션은 UI 스레드라 안 끊김 — 이것이 RN 기본 `Animated`(JS 드리븐) 대비 가장 큰 이점. [추론]

## 출처
- Expo 제스처 튜토리얼: `10_raw/expo/pages/tutorial/gestures.mdx`
- Reanimated 공식: https://docs.swmansion.com/react-native-reanimated/
- Gesture Handler 공식: https://docs.swmansion.com/react-native-gesture-handler/
