---
tags: [app, expo, setup]
---

# Expo 프로젝트 부트스트랩

create-expo-app → SDK 핀 → src 이동 → FSD → NativeWind → Biome → husky. **최초 1회**. 상위: [[index]]

> 기준: Expo SDK 54 boilerplate. 패키지 매니저 **pnpm 전용**. (확인: 2026-06)

## 0. 사전 요구
- Node.js **22+** (`nano-staged@1.x`가 node `^22 || >=24` 요구)
- pnpm (필수), Xcode(iOS), Android Studio(Android)
- `.npmrc`에 `node-linker=hoisted` (네이티브 빌드·autolinking 오류 회피)

## 1. create-expo-app + 버전 핀
```sh
pnpm create expo-app@latest . --template expo-template-default@54.0.35 --yes
```
- ⚠️ **템플릿 버전 핀 필수.** `@latest`는 항상 최신 SDK(56+) 생성 → `expo`/`react`/`react-native`만 다운그레이드하면 나머지 `expo-*`가 신 SDK에 남아 **빌드 깨짐**.
- 기존 `docs/`·`agent.md` 보존 필요 시 → 임시 디렉터리 생성 후 `rsync`로 병합 (`.git`·`docs`·`agent.md`·`README.md`·`.gitignore` exclude).

핀 버전:
```sh
pnpm add expo@54.0.35 react@19.1.0 react-native@0.81.5
pnpm add -D @types/react@~19.1.10
```

## 2. src/로 이동
SDK 54 default 템플릿은 루트에 `app/`·`components/`·`hooks/`·`constants/` 생성. 전부 `src/`로.
```sh
mkdir -p src
mv app src/app
mv components hooks constants src/ 2>/dev/null || true
```
- ⚠️ `src/app`과 루트 `app/` 동시 존재 시 **`src/app`만** 사용됨 → 루트 `app/` 잔존 확인.
- `tsconfig.json` paths: `"@/*": ["./src/*"]`, `"@/assets/*": ["./assets/*"]` (없으면 `Unable to resolve module @/assets/...`).

## 3. FSD 폴더
```sh
mkdir -p src/features src/entities src/shared/ui src/shared/lib src/shared/config styles
```
- 데모 컴포넌트 재배치: `components/*`→`shared/ui`, `hooks/*`→`shared/lib`, `constants/theme.ts`→`shared/config`. 데모 전용(hello-wave 등) 삭제.
- 각 세그먼트 `index.ts` 배럴. → [[FSD 폴더구조]]

## 4. NativeWind
상세 → [[NativeWind]]
```sh
pnpm add nativewind@^4.2.1 class-variance-authority clsx tailwind-merge
pnpm add -D tailwindcss@^3.4.17
```
- ⚠️ SDK 54 = Reanimated v4 → **NativeWind 4.2.1+** 필수.
- ⚠️ reanimated·safe-area-context는 `pnpm add` 최신 ❌ → `pnpm expo install`로 SDK 호환 버전.
- babel: `jsxImportSource: "nativewind"` + `nativewind/babel` + `react-native-reanimated/plugin`. **`react-native-worklets/plugin` 추가 금지**(v4 중복 오류).
- metro: `withNativeWind(config, { input: "./styles/global.css" })`. `_layout.tsx` 최상단 `import "../../styles/global.css"`. 타입: 루트 `nativewind-env.d.ts`.

## 5. Biome
```sh
pnpm add -D @biomejs/biome && pnpm exec biome init
```
- `lineWidth:100`, double quote, semicolons always, trailingCommas all.
- ⚠️ `*.css`에서 `noUnknownAtRules: off` (Biome 2.x가 `@tailwind` 잡음).
- ESLint 제거(`pnpm remove eslint eslint-config-expo`).

## 6. husky + nano-staged
→ 상세 [[실행 스크립트와 Git hooks]]
```sh
pnpm add -D husky nano-staged && pnpm exec husky init
```
- `pre-commit`: `pnpm exec nano-staged`, `pre-push`: `pnpm typecheck`.
- nano-staged 글롭: `"*.{js,jsx,ts,tsx,json,jsonc,css}": "biome check --write --no-errors-on-unmatched"`.

## 7. 검증
```sh
pnpm lint && pnpm typecheck && pnpm start
```

## 관련
[[앱 기술 스택]] · [[NativeWind]] · [[실행 스크립트와 Git hooks]] · [[FSD 폴더구조]]
