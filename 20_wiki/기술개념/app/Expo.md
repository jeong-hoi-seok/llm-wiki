---
type: concept
scope: expo-app-framework
platform: app
status: verified
tags: [frontend, app, react-native, expo]
source:
  - 10_raw/expo/pages/core-concepts.mdx
  - 10_raw/expo/pages/workflow/overview.mdx
  - 10_raw/expo/pages/develop/development-builds/introduction.mdx
  - 10_raw/expo/pages/workflow/continuous-native-generation.mdx
  - 10_raw/expo/pages/config-plugins/introduction.mdx
  - 10_raw/expo/pages/develop/tools.mdx
  - 10_raw/expo/pages/build/introduction.mdx
  - 10_raw/expo/pages/eas-update/introduction.mdx
  - 10_raw/expo/pages/router/introduction.mdx
created: 2026-06-24
updated: 2026-06-24
---

# Expo

## 한 줄 요약

[[React Native]] 앱을 개발·네이티브 생성·빌드·업데이트하기 위한 오픈소스 도구와 hosted service 묶음. Expo app은 별도 런타임이 아니라 **Expo 도구를 사용하는 React Native 앱**이다.

## 핵심 모델

- `expo` npm package는 거의 모든 React Native 프로젝트에 부분적으로 도입할 수 있다.
- Expo SDK는 Android·iOS·web에서 동작하는 React Native 모듈 묶음이다.
- Expo CLI, Expo Router, Expo SDK, CNG, EAS는 각각 독립적으로 쓸 수 있다.
- EAS는 Build·Submit·Update·Workflow 같은 hosted service다. Expo 오픈소스 도구를 쓴다고 EAS를 반드시 써야 하는 것은 아니다.

즉 Expo를 "Expo Go에서만 돌아가는 앱"으로 이해하면 범위가 좁다. 실무 기준에서는 React Native 앱 위에 Expo SDK·CLI·Router·CNG·EAS를 얼마나 쓸지 정하는 문제에 가깝다.

## 주요 구성 요소

| 구성 | 역할 | 판단 기준 |
|---|---|---|
| Expo SDK | 카메라·알림·업데이트 등 검증된 React Native 모듈 묶음 | 네이티브 기능을 직접 붙이기 전에 우선 검토 |
| Expo CLI | `start`, `prebuild`, `run`, `install`, `lint` 같은 개발 명령 | 로컬 개발과 네이티브 생성의 기본 도구 |
| Expo Router | `app/` 디렉터리 기반 파일 라우팅 | 새 Expo 앱이면 기본 후보 |
| Expo Go | 고정된 네이티브 런타임을 가진 학습·프로토타입 앱 | 교육·빠른 확인용. 프로덕션 기준으로 삼지 않음 |
| Development Build | `expo-dev-client`가 포함된 앱 전용 debug build | 실제 앱 개발의 기본 환경 |
| Prebuild / CNG | `app.json`·`package.json` 기준으로 `android/`, `ios/` 생성 | 네이티브 프로젝트를 직접 유지하지 않으려면 사용 |
| Config Plugin | prebuild 중 네이티브 설정을 코드로 적용 | `Info.plist`, `AndroidManifest.xml` 직접 수정 대신 사용 |
| EAS Build | Android·iOS 설치 파일을 cloud/local에서 빌드 | 스토어 배포, 내부 배포, 팀 공유 빌드 |
| EAS Update | JS·스타일·이미지 OTA 업데이트 | 네이티브 코드가 바뀌지 않는 수정에 사용 |
| Expo Modules API | Swift·Kotlin으로 네이티브 모듈·뷰 작성 | 기존 라이브러리로 부족할 때 사용 |

## Expo Go와 Development Build

Expo Go는 빠르게 시작하기 위한 playground다. 이미 포함된 네이티브 라이브러리만 쓸 수 있고, 앱 이름·아이콘·권한·네이티브 라이브러리 추가처럼 설치된 네이티브 앱 자체가 바뀌는 작업은 검증하기 어렵다.

Development Build는 프로젝트 전용 debug build다. `expo-dev-client`를 포함하고, 앱이 실제로 사용할 네이티브 라이브러리와 설정을 담는다. 스토어 출시를 전제로 하는 앱은 Expo Go가 아니라 Development Build를 기준으로 개발해야 한다.

| 상황 | Expo Go | Development Build |
|---|---|---|
| React Native 기본 학습 | 적합 | 과함 |
| 빠른 UI 프로토타입 | 가능 | 가능 |
| 네이티브 라이브러리 추가 | 제한적 | 적합 |
| 앱 아이콘·스플래시·권한 검증 | 제한적 | 적합 |
| remote push, universal link | 부적합 | 적합 |
| 스토어 배포 전 개발 | 부적합 | 기본 선택 |

## Prebuild와 CNG

`create-expo-app`으로 만든 기본 프로젝트에는 보통 `android/`, `ios/` 디렉터리가 없다. 필요할 때 `npx expo prebuild`가 app config와 package 정보를 바탕으로 네이티브 프로젝트를 생성한다. 이 흐름이 CNG(Continuous Native Generation)다.

CNG의 핵심은 네이티브 프로젝트 전체를 장기 유지하지 않고, 네이티브 변경의 정의만 유지하는 것이다.

- 네이티브 설정은 `app.json` 또는 `app.config.js`에 둔다.
- 기본 app config로 표현할 수 없는 변경은 config plugin으로 옮긴다.
- `android/`, `ios/`를 직접 수정하면 이후 `prebuild --clean`에서 덮일 수 있다.
- 직접 네이티브 프로젝트를 관리해야 하는 팀은 CNG를 쓰지 않거나, 일부만 도입할 수 있다.

## EAS 사용 기준

EAS는 Expo 오픈소스 도구와 별개인 hosted service다. 팀이 자체 CI, Fastlane, 자체 업데이트 서버를 쓸 수도 있다. 다만 앱 빌드·배포·업데이트를 직접 운영하는 비용이 커지면 EAS가 기본 후보가 된다.

| 서비스 | 사용 시점 | 사용하지 말아야 할 시점 |
|---|---|---|
| EAS Build | 스토어용 binary, 내부 테스트 배포, 팀 공통 빌드, credential 관리 | 네이티브 디버깅을 로컬 도구로 해야 할 때 |
| EAS Update | JS 로직, 문구, 스타일, 이미지 수정 배포 | 네이티브 코드, 권한, SDK 버전, native dependency 변경 |
| EAS Submit | 빌드 결과물을 App Store / Play Store에 제출 | 스토어 제출을 이미 다른 파이프라인이 처리할 때 |

EAS Update는 OTA 업데이트라서 빠르지만, 네이티브 binary와 호환되는 변경만 보낼 수 있다. 네이티브 코드가 바뀌면 새 build가 필요하고, runtime version 기준을 맞춰야 한다.

## 실무 적용 기준

- 새 React Native 앱이면 `create-expo-app` + Expo Router를 기본 후보로 둔다.
- Expo Go는 학습·초기 확인용으로만 둔다.
- 실제 앱 개발은 Development Build 기준으로 전환한다.
- `android/`, `ios/`를 직접 커밋하기 전에 CNG로 유지할 수 있는 변경인지 먼저 본다.
- 네이티브 설정 변경은 config plugin으로 표현할 수 있는지 확인한다.
- 라이브러리 설치는 `npx expo install`을 우선 사용해 SDK 호환 버전을 맞춘다.
- dependency·config 문제가 의심되면 `npx expo-doctor`로 먼저 점검한다.
- 배포 binary는 EAS Build 또는 팀의 native build 파이프라인 중 하나로 명확히 정한다.

## 주의

- Expo는 React Native를 대체하는 별도 앱 플랫폼이 아니다. React Native 앱에 도입하는 도구층이다.
- EAS는 필수가 아니다. 다만 build/update/submit 인프라를 직접 운영할지, EAS에 맡길지의 선택이다.
- Expo Go 호환성을 위해 SDK를 고정하는 결정과, 프로덕션 개발 환경을 Development Build로 잡는 결정은 별개다.
- SDK 버전 선택은 Expo Go, React Native New Architecture, 의존성 호환성에 영향을 받는다. 별도 결정은 [[Expo SDK 54 버전 고정]]에 정리한다.

## 관련 문서

[[React Native]] · [[NativeWind]] · [[Expo SDK 54 버전 고정]]

## 출처

- `10_raw/expo/pages/core-concepts.mdx`
- `10_raw/expo/pages/workflow/overview.mdx`
- `10_raw/expo/pages/develop/development-builds/introduction.mdx`
- `10_raw/expo/pages/workflow/continuous-native-generation.mdx`
- `10_raw/expo/pages/config-plugins/introduction.mdx`
- `10_raw/expo/pages/develop/tools.mdx`
- `10_raw/expo/pages/build/introduction.mdx`
- `10_raw/expo/pages/eas-update/introduction.mdx`
- `10_raw/expo/pages/router/introduction.mdx`
