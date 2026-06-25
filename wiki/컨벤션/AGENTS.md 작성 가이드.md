---
platform: common
scope: agents-md-authoring
status: verified
tags: [common, convention, agent, wiki]
created: 2026-06-19
updated: 2026-06-25
---

# AGENTS.md 작성 가이드

신규 프로젝트에서 AI가 위키를 실제로 읽고 적용하는지 검증하는 구조를 포함한 AGENTS.md 작성 기준.
상위: [[index]]

---

## 핵심 원칙

AGENTS.md에는 네 가지가 반드시 있어야 한다:
1. **파일명 고정** — 파일명은 반드시 대문자 `AGENTS.md`로 쓴다. `agent.md`, `agents.md`, `Agent.md`처럼 소문자로 쓰지 않는다.
2. **명령형 읽기** — "참조한다" 아닌 "직접 열어서 읽는다"
3. **검증 장치** — AI가 읽었는지 추적할 수 있는 규칙
4. **경로 간접화** — 개인 절대경로가 아니라 환경변수와 symlink로 공통 위키 위치를 찾는다

---

## 적용 방식

> **기존 `AGENTS.md`가 있는 경우** — 아래 `Wiki Baseline` 섹션만 기존 파일에 삽입한다. 기존 내용은 건드리지 않는다.
>
> **`AGENTS.md`가 없는 경우** — 아래 전체 템플릿으로 새로 작성한다.
>
> **파일명 주의** — 에이전트가 소문자 파일명을 놓치는 경우가 있으므로 프로젝트 root 파일명은 반드시 `AGENTS.md`로 만든다.

### 팀원 로컬 설정

공통 위키는 각자 원하는 위치에 clone한다. 대신 AGENTS.md에서는 아래 두 경로 중 하나로 찾는다.

```sh
export FRONTEND_LLM_WIKI_ROOT="$HOME/path/to/llm-wiki"
ln -sfn "$FRONTEND_LLM_WIKI_ROOT" "$HOME/.frontend-llm-wiki"
```

환경변수는 유연하고, symlink는 에이전트가 환경변수를 받지 못할 때 fallback 역할을 한다.

### 삽입 전용 블록 (기존 파일에 추가할 내용)

```md
## Wiki Baseline

### 읽기 순서 (작업 시작 전 필수)

> 아래 파일을 반드시 직접 열어서 읽는다. 경로만 인지하는 것으로 갈음하지 않는다.

1. 공통 위키 루트를 다음 순서로 찾는다:
   - `FRONTEND_LLM_WIKI_ROOT`
   - 없으면 `$HOME/.frontend-llm-wiki`
   - 둘 다 없거나 `wiki/index.md`가 없으면 위키 경로 설정이 필요하다고 말하고 중단한다.

2. 찾은 위키 루트의 `wiki/index.md`를 직접 열어 읽는다:
   `$WIKI_ROOT/wiki/index.md`

3. 이 프로젝트의 platform 값에 맞는 문서만 추가로 읽는다 (platform은 이 파일 상단 또는 Project Identity 섹션 참고).

| 위키 태그 | `web` | `app` |
|---|---|---|
| `[공통]` | ✅ | ✅ |
| `[웹]` | ✅ | ❌ |
| `[앱]` | ❌ | ✅ |
| `[웹·앱]` | ✅ | ✅ |

4. 작업 주제와 연결된 문서를 index에서 찾아 추가로 읽는다.

### 응답 규칙 (검증용)

작업 결과물 하단에 참고한 위키 문서를 반드시 명시한다:

​```
<!-- wiki-ref: 컨벤션/커밋 컨벤션.md, 원칙/가독성.md -->
​```

명시하지 않은 경우, 위키를 읽지 않은 것으로 간주한다.
```

---

## 전체 템플릿

```md
# AGENTS.md

모든 AI 에이전트(Cursor, Claude Code, Codex 등)가 이 프로젝트에서 따를 공통 지침.

## Project Identity

​```yaml
platform: web   # web | app
​```

| 값 | 스택 예시 |
|---|---|
| `web` | Next.js, React (DOM), Tailwind CSS |
| `app` | React Native, Expo, NativeWind |

### 프로젝트 요약

- **이름**: {프로젝트명}
- **스택**: {스택 목록}

---

## Wiki Baseline

### 읽기 순서 (작업 시작 전 필수)

> 아래 파일을 반드시 직접 열어서 읽는다. 경로만 인지하는 것으로 갈음하지 않는다.

1. 공통 위키 루트를 다음 순서로 찾는다:
   - `FRONTEND_LLM_WIKI_ROOT`
   - 없으면 `$HOME/.frontend-llm-wiki`
   - 둘 다 없거나 `wiki/index.md`가 없으면 위키 경로 설정이 필요하다고 말하고 중단한다.

2. 찾은 위키 루트의 `wiki/index.md`를 직접 열어 읽는다:
   `$WIKI_ROOT/wiki/index.md`

3. 위 `platform` 값에 맞는 문서만 추가로 읽는다:

| 위키 태그 | `web` | `app` |
|---|---|---|
| `[공통]` | ✅ | ✅ |
| `[웹]` | ✅ | ❌ |
| `[앱]` | ❌ | ✅ |
| `[웹·앱]` | ✅ | ✅ |

4. 작업 주제와 연결된 문서를 index에서 찾아 추가로 읽는다.

### 참조 예시 (`platform: web`)

​```
✅ FSD 폴더구조, 코드 품질, 네이밍 컨벤션  [공통]
✅ Next.js, Tailwind CSS, shadcn-ui, 번들링              [웹]
❌ React Native, NativeWind, Expo SDK 54 버전 고정   [앱]
​```

### 응답 규칙 (검증용)

작업 결과물 하단에 참고한 위키 문서를 반드시 명시한다:

​```
<!-- wiki-ref: 컨벤션/커밋 컨벤션.md, 원칙/가독성.md -->
​```

명시하지 않은 경우, 위키를 읽지 않은 것으로 간주한다.
```

---

## 검증 방법

AGENTS.md를 작성한 뒤 아래 방법으로 AI가 실제로 위키를 읽었는지 확인한다.

### 방법 1. wiki-ref 확인 (수동)

응답 하단에 `<!-- wiki-ref: ... -->` 가 있는지 확인.
- 있음 → 위키를 읽고 경로를 추적했음
- 없음 → 위키 미참조 (AGENTS.md 규칙 미적용)

### 방법 2. 카나리아 규칙 (자동)

위키 문서에 **학습 데이터에 없는 독특한 규칙**을 심어둔다.
AI가 해당 규칙을 따르면 위키를 읽은 것으로 판단.

현재 심어진 카나리아:
- `컨벤션/커밋 컨벤션.md` — subject 반드시 한국어, 명사형 어미
- `컨벤션/MR PR 작성 가이드.md` — 브랜치 확인 없이 본문 작성 금지

새 카나리아 추가 방법:
1. 위키 문서에 비표준적이지만 합리적인 규칙 추가
2. 해당 규칙을 따르는지 테스트 프롬프트로 확인

### 방법 3. 툴콜 로그 확인 (Cursor 전용)

Cursor 채팅창에서 AI가 실제로 파일을 열었는지 확인.
`Read` 툴콜에 위키 경로가 찍히면 읽은 것.

---

## 흔한 실수

| 실수 | 결과 | 수정 |
|---|---|---|
| "참조한다"로 작성 | AI가 경로만 인지, 파일 미열람 | "직접 열어서 읽는다"로 변경 |
| wiki-ref 규칙 없음 | 읽었는지 추적 불가 | 응답 규칙 섹션 추가 |
| platform 미선언 | 앱/웹 필터 미적용 | `platform: web` or `app` 명시 |
| 절대경로 하드코딩만 | 다른 머신에서 동작 안 함 | `FRONTEND_LLM_WIKI_ROOT` + `$HOME/.frontend-llm-wiki` 사용 |

## 관련
[[index]] · [[커밋 컨벤션]] · [[MR PR 작성 가이드]]
