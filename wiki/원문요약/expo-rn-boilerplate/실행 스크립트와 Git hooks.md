---
tags: [app, expo, scripts, git-hooks]
---

# 실행 스크립트 · Git hooks

pnpm 스크립트와 husky 품질 게이트. 상위: [[index]]

## 스크립트

| 스크립트 | 실제 명령 | 용도 |
|---|---|---|
| `pnpm start` | `expo start` | 개발 서버 (Metro JS 번들) |
| `pnpm ios` | `expo run:ios` | 로컬 네이티브 빌드 → iOS |
| `pnpm android` | `expo run:android` | 로컬 네이티브 빌드 → Android |
| `pnpm lint` | `biome check .` | 린트 |
| `pnpm format` | `biome format --write .` | 포맷 |
| `pnpm typecheck` | `tsc --noEmit` | 타입 검사 |

- `pnpm start`는 JS 번들만 갱신. 네이티브 모듈·`babel`/`metro`/`app.json` 변경 후엔 `pnpm ios`/`pnpm android` 재빌드.
- 스토어 배포는 EAS Build.

## Git hooks (husky)

`prepare: "husky"`로 clone·install 시 자동 활성화. **node 22+ 필요** (`nano-staged@1.x`).

| 훅 | 실행 | 동작 |
|---|---|---|
| `pre-commit` | `pnpm exec nano-staged` | 스테이징 파일만 `biome check --write` 자동수정·재-stage, 못 고치는 에러는 커밋 차단 |
| `pre-push` | `pnpm typecheck` | 푸시 전 `tsc --noEmit` |

```json
// package.json
{ "nano-staged": {
  "*.{js,jsx,ts,tsx,json,jsonc,css}": "biome check --write --no-errors-on-unmatched"
}}
```

- ⚠️ 훅이 막아도 `--no-verify`로 우회하지 않음. 에러 고치고 다시 커밋·푸시.
- 대안: nano-staged 없이 `biome check --staged --write`만 — 단 자동수정분 재-stage는 수동 `git add`.

## 관련
[[Expo 프로젝트 부트스트랩]] · [[앱 기술 스택]] · [[커밋 컨벤션]]
