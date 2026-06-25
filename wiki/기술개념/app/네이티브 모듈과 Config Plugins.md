---
type: concept
platform: app
status: verified
tags: [frontend, react-native, expo, native-module, config-plugin]
source:
  - raw/expo/pages/modules/overview.mdx
  - raw/expo/pages/modules/module-api.mdx
  - raw/expo/pages/config-plugins/introduction.mdx
  - raw/expo/pages/workflow/continuous-native-generation.mdx
created: 2026-06-25
updated: 2026-06-25
---

# 네이티브 모듈과 Config Plugins

> JS만으로 안 되는 기능을 네이티브(Swift/Kotlin)로 연결하고, 네이티브 프로젝트 설정을 코드로 자동화하는 두 축. 상위: [[index]] · 관련: [[React Native]] · [[Expo]]

## TL;DR
- **네이티브 모듈** — JS ↔ 네이티브(iOS Swift/Obj-C, Android Kotlin/Java) 기능 연결. RN이 못 감싸는 OS API·SDK를 JS에서 호출 가능하게.
- **Config Plugin** — `app.json`/`app.config`에서 **네이티브 프로젝트 파일(android·ios)을 직접 안 건드리고** prebuild 때 자동 수정.

이 둘은 Expo의 **CNG(Continuous Native Generation)** 워크플로의 양 축이다.

## CNG와 prebuild 맥락
Expo CNG에서는 `android`/`ios` 디렉터리를 git에 두지 않고, `npx expo prebuild`로 app config에서 **매번 재생성**한다.
- 장점: 네이티브 폴더를 직접 관리 안 해도 됨 → 업그레이드·일관성 쉬움.
- 문제: 그럼 네이티브 설정(권한·플러그인·빌드 설정)은 어디서? → **Config Plugin**으로 코드화.

## Config Plugin 구조
app config의 `plugins` 배열에 등록하는 최상위 설정 지점. (raw: `config-plugins/introduction.mdx`)

| 용어 | 의미 |
|---|---|
| **Plugin** | `plugins`에 등록되는 최상위 함수. 관례상 `with<Name>` (예: `withMyPlugin`) |
| **Plugin function** | 플러그인을 이루는 함수들. 플랫폼별 수정 로직을 감쌈. 테스트·디버깅 위해 잘게 쪼갬 |
| **Mod / Mod plugin function** | `expo/config-plugins`가 제공하는 래퍼. 네이티브 파일을 **안전하게** 수정하는 방법 |

→ prebuild 시 plugin function들이 JS로 실행되어 네이티브 프로젝트를 수정.

## 네이티브 모듈 (Expo Modules API)
Expo Modules API로 Swift/Kotlin 모듈을 작성해 JS에 노출. (raw: `modules/overview.mdx`, `module-api.mdx`)
- **Autolinking** — 설치한 네이티브 모듈을 빌드에 자동 연결. 수동 링크 불필요. (raw: `modules/autolinking.mdx`)
- **Module API** — `Module` 정의에 함수·상수·뷰·이벤트를 선언적으로 등록.
- **Native View** — 커스텀 네이티브 UI를 RN 컴포넌트로 노출. (raw: `native-view-tutorial.mdx`)
- 기존 서드파티 라이브러리 래핑도 지원. (raw: `third-party-library.mdx`, `existing-library.mdx`)

## 언제 무엇을
- OS 기능·네이티브 SDK를 JS에서 호출해야 함 → **네이티브 모듈**.
- 네이티브 빌드 설정(권한, Info.plist, Gradle, 네이티브 의존성)을 자동화해야 함 → **Config Plugin**.
- 보통 네이티브 모듈 라이브러리가 자기 Config Plugin을 함께 제공해 설치만으로 설정까지 끝낸다. [추론]

## 출처
- Expo Modules: `raw/expo/pages/modules/` (overview·module-api·autolinking·native-view-tutorial 등)
- Config Plugins: `raw/expo/pages/config-plugins/` (introduction·mods·plugins)
- CNG: `raw/expo/pages/workflow/continuous-native-generation.mdx`
