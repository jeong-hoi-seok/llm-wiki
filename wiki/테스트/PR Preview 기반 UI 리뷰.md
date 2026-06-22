---
type: concept
platform: web
status: draft
tags: [frontend, testing, storybook, pr-review]
source:
  - "[[29CM Storybook 운영 방식 요약]]"
created: 2026-06-22
updated: 2026-06-22
---

# PR Preview 기반 UI 리뷰

## 한 줄 요약
PR Preview는 리뷰어가 로컬 실행 없이 변경된 UI를 Storybook이나 Preview URL에서 직접 확인하게 해 리뷰 품질을 높인다.

## 핵심 내용
스크린샷만으로는 hover, focus, loading, error 같은 상태를 충분히 보기 어렵다. PR마다 Storybook Preview를 제공하면 리뷰어가 실제 컴포넌트 상태를 확인하며 코드 리뷰를 할 수 있다.

29CM은 GitHub Actions와 Vercel Preview를 이용해 PR별 Storybook URL을 만들고, PR 코멘트에 링크를 남기는 방식을 소개한다. 리뷰어는 로컬 실행 없이 변경된 컴포넌트를 UI와 함께 확인할 수 있다.

## 실무 적용 기준
- PR마다 Storybook 또는 앱 Preview URL을 만든다.
- 주요 변경 컴포넌트 Story 링크를 PR 설명에 첨부한다.
- MSW로 API 상태를 재현한다.
- 시각적 회귀 테스트 리포트와 함께 제공하면 효과가 크다.
- 리뷰 대상이 UI라면 스크린샷만 올리지 말고 조작 가능한 Preview를 우선 제공한다.
- 리뷰어가 봐야 할 Story 이름을 PR 본문에 직접 적는다.

## 주의점
- Preview가 최신 커밋 기준인지 확인한다.
- 인증이나 외부 API 의존성을 Preview에서 제거한다.
- 민감 데이터가 fixture에 들어가지 않게 한다.

## 관련 문서
- [[컴포넌트와 Story를 함께 관리하기]]
- [[Playwright 스크린샷 테스트]]
- [[MR PR 작성 가이드]]

## 출처
- [[29CM Storybook 운영 방식 요약]]
