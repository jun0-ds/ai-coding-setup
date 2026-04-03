# AI Assistant Setup

> ⚠️ 썸네일 보고 놀라지 마세요. 군침이 싹 돕니다.

내 컴퓨터에 AI 어시스턴트 세팅하기. 3분이면 끝.

> 코딩뿐 아니라 **글쓰기, 데이터 분석, 문서 정리, 공부, 업무 자동화** 등 뭐든 시킬 수 있습니다.

### 이런 걸 할 수 있어요

- **코딩** — 코드 작성, 디버깅, 리팩토링
- **글쓰기** — 초안 작성, 퇴고, 번역
- **분석** — 데이터 정리, 시각화, 리포트
- **문서** — 요약, 포맷 변환, 템플릿 생성
- **학습** — 개념 설명, 예제 생성, 퀴즈
- **자동화** — 반복 작업 처리, 파일 정리, 스크립트 생성

---

## 어떤 AI를 고를까?

| | Claude Code (Anthropic) | Gemini CLI (Google) | Codex CLI (OpenAI) |
|---|---|---|---|
| **가격** | 유료 (API 종량제 / Max 구독) | **무료** (Google 계정만 있으면 됨) | 무료 체험 중 / 유료 (ChatGPT 구독) |
| **일일 한도** | 구독 플랜에 따라 다름 | 1,000 요청 | 구독 플랜에 따라 다름 |
| **Node.js** | 18+ | 20+ | 22+ |
| **강점** | 깊은 분석, 긴 맥락, 코딩 | 무료, 빠른 응답, 구글 연동 | 강력한 에이전트, GPT-5 기반 |
| **VS Code** | ✅ 확장 프로그램 | ✅ 확장 프로그램 | ✅ 확장 프로그램 |
| **Windows** | ✅ 네이티브 | ✅ 네이티브 | ⚠️ WSL 권장 (CLI) |
| **글로벌 지침 파일** | `CLAUDE.md` | `GEMINI.md` | `AGENTS.md` |
| **플러그인** | ✅ [sonmat](https://github.com/jun0-ds/sonmat) | ✅ [sonmat](https://github.com/jun0-ds/sonmat) (자동 설치) | ✅ [sonmat](https://github.com/jun0-ds/sonmat) (자동 설치) |

> 💰 **무료로 시작하고 싶다면** → Gemini 또는 Codex (무료 체험)
> 🔧 **깊은 코딩 + 검증 체계가 필요하다면** → Claude Code + sonmat

---

## Claude Code (Anthropic)

| 파일 | 용도 |
|------|------|
| [claude/setup-guide.md](claude/setup-guide.md) | 사람이 할 것: 확장 설치 + 인증 (3분) |
| [claude/auto-setup.md](claude/auto-setup.md) | AI에 붙여넣기 → 나머지 자동 세팅 |
| [claude/troubleshooting.md](claude/troubleshooting.md) | 문제 해결 (플러그인, Windows 이슈) |

## Gemini CLI (Google)

| 파일 | 용도 |
|------|------|
| [gemini/setup-guide.md](gemini/setup-guide.md) | 사람이 할 것: 확장 설치 + 로그인 (3분) |
| [gemini/auto-setup.md](gemini/auto-setup.md) | AI에 붙여넣기 → 나머지 자동 세팅 |

## Codex CLI (OpenAI)

| 파일 | 용도 |
|------|------|
| [codex/setup-guide.md](codex/setup-guide.md) | 사람이 할 것: 확장 설치 + 인증 (3분) |
| [codex/auto-setup.md](codex/auto-setup.md) | AI에 붙여넣기 → 나머지 자동 세팅 |

---

## 사용법

1. 원하는 AI의 `setup-guide.md`를 열고 단계별로 따라하기
2. AI가 동작하면 `auto-setup.md` 내용을 첫 대화에 붙여넣기
3. AI가 질문하면 답하기 → 세팅 끝
