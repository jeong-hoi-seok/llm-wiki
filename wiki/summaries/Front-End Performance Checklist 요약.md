---
type: source-summary
platform: web
status: summarized
tags: [frontend, performance, checklist, web]
source: raw/front-end-performance-checklist/README.md
created: 2026-06-22
updated: 2026-06-22
---

# Front-End Performance Checklist 요약

## 한 줄 요약
Front-End Performance Checklist는 HTML, CSS, 폰트, 이미지, JavaScript, 서버 전달 계층에서 프론트엔드가 확인해야 할 성능 항목을 우선순위로 정리한 체크리스트다. 이 위키에서는 범용 프론트엔드 프로젝트에 재사용 가능한 항목만 추려 [[프론트엔드 성능 최적화]] 문서로 정리했다.

## 핵심 관점
원문의 핵심 규칙은 “성능을 염두에 두고 설계하고 구현한다”이다. 성능 최적화는 배포 직전 압축만 하는 일이 아니라, 리소스 구조·렌더링 차단·이미지 전달·폰트 로딩·JavaScript 실행 비용·캐싱 정책을 함께 관리하는 작업이다.

## 필요한 부분만 추린 기준
포함한 항목:
- HTML, CSS, JavaScript의 minification과 렌더링 차단 제어
- Critical CSS, unused CSS, CSS 복잡도 점검
- WOFF2, `preconnect`, `font-display`, 폰트 용량 제한
- 이미지 압축, 적절한 포맷, SVG, 크기 지정, lazy loading, responsive image
- JavaScript `async`/`defer`, dependency size, runtime profiling, Service Worker
- HTTPS, page weight, load time, TTFB, HTTP cache, Brotli/Gzip, CDN
- Lighthouse, PageSpeed Insights, WebPageTest, bundle analyzer 같은 측정 도구

제외하거나 축소한 항목:
- WordPress 전용 최적화
- Angular/Vue 등 프레임워크별 외부 링크 모음
- 번역, 후원, 기여 가이드
- 오래된 도구 링크 중심 목록
- HTTP/1 기준의 CSS concatenation 같은 환경 의존 항목

## 위키 반영
- [[프론트엔드 성능 최적화]] — 실무 체크리스트와 적용 기준으로 재구성
- [[번들링]] — 리소스 묶기·압축·최적화와 연결
- [[코드 스플리팅]] — 필요한 시점에 필요한 코드만 로드
- [[트리 쉐이킹]] — 미사용 코드 제거
- [[HMR]] — 개발환경 성능 개념과 연결

## 출처
- Front-End Performance Checklist README: `raw/front-end-performance-checklist/README.md`
- 원본 라이선스: `raw/front-end-performance-checklist/LICENSE`
