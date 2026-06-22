---
type: source-summary
platform: web
status: summarized
tags: [frontend, testing, storybook, cdd]
source: "https://medium.com/29cm/%EB%8B%B9%EC%8B%A02-9%ED%95%98%EB%8D%98-storybook-a6b10a62e825"
created: 2026-06-22
updated: 2026-06-22
---

# 29CM Storybook 운영 방식 요약

## 한 줄 요약
29CM Storybook 글은 React 전환 과정에서 컴포넌트와 Story를 함께 관리하고, API 상태와 PR Preview를 통해 UI 리뷰 품질을 높인 사례다.

## 핵심 내용
- 29CM은 Angular 기반 프론트엔드에서 React로 전환하는 과정에서 컴포넌트 개발 방식 개선 목적으로 Storybook을 도입했다.
- 팀 단위 개발에서 이미 있는 컴포넌트를 찾기 어렵고, 사용법 확인 시간이 들며, UI 테스트 비용이 커지는 문제가 있었다.
- 컴포넌트와 Story를 1:1로 대응시키고 작은 컴포넌트부터 페이지까지 Story로 저장한다.
- 가벼운 Story는 VSCode snippet으로 빠르게 생성하게 했다.
- API를 호출하는 컴포넌트는 MSW로 다양한 API 응답을 만들고, 실제 데이터가 있는 상황과 없는 상황을 Storybook에서 확인한다.
- PR마다 GitHub Actions와 Vercel Preview로 Storybook URL을 만들고, PR 코멘트에서 리뷰어가 UI를 직접 확인하게 했다.
- Storybook은 CDD, 신규 입사자 온보딩, 디자이너와의 커뮤니케이션에 도움을 준다.

## 위키 반영
- [[컴포넌트와 Story를 함께 관리하기]]
- [[Storybook에서 API 상태 표현하기]]
- [[PR Preview 기반 UI 리뷰]]
- [[Component-Driven Development]]

## 출처
- 29CM, 당신2 9하던 Storybook: https://medium.com/29cm/%EB%8B%B9%EC%8B%A02-9%ED%95%98%EB%8D%98-storybook-a6b10a62e825
- 로컬 원문: `raw/testing-sources/29CM Storybook 원문.txt`
