---
type: concept
platform: web
status: verified
tags: [frontend, bundling, dev]
source: raw/toss/frontend-fundamentals/bundling/deep-dive/dev/hmr.md
created: 2026-06-19
updated: 2026-06-19
---

# HMR (Hot Module Replacement)

## 한 줄 요약
**변경된 코드 조각(모듈)만 런타임에 교체**하는 개발 기능. 페이지 새로고침 없이 수정이 즉시 반영된다. [[번들링]] 개발환경 기능.

## vs Live Reload
| | 동작 | 상태 |
|---|---|---|
| **Live Reload** | 변경 감지 → 브라우저 전체 새로고침 | **초기화됨** |
| **HMR** | 변경 모듈만 교체 | **유지됨** |
- HMR은 폼 입력·라우트 등 앱 상태를 유지한 채 화면 갱신 → 개발 경험↑.

## 동작 원리
```
1. watch가 변경 파일 감지 → 변경 모듈만 재컴파일(메모리 번들)
2. 웹소켓으로 브라우저에 변경 알림
3. 브라우저가 변경 모듈 요청 → 런타임 교체 → 화면 업데이트
```

## 관련 문서
[[번들링]] · [[코드 스플리팅]]

## 출처
- 토스 번들링 Fundamentals (`raw/.../dev/hmr.md`)
