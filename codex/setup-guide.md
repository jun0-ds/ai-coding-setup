# Codex CLI 세팅 가이드 (VS Code)

> OpenAI의 AI 코딩 에이전트.
> ChatGPT 구독에 포함 (Plus $20/월, Pro $200/월) — 현재 무료 체험 가능

---

## 1단계: VS Code 확장 프로그램 설치

VS Code 확장 탭(Ctrl+Shift+X)에서 `Codex` 검색 후 설치.

또는 터미널에서:
```bash
code --install-extension openai.chatgpt
```

> ⚠️ **Windows 사용자:** Codex CLI는 macOS/Linux 네이티브 지원. Windows는 WSL 환경을 권장.
> VS Code 확장은 Windows에서도 사용 가능.

---

## 2단계: 인증 (둘 중 하나 선택)

### 방법 A: ChatGPT 계정 로그인 (무료 체험 포함)

Codex를 처음 실행하면 "Sign in with ChatGPT" 옵션이 나옴.
ChatGPT Free 계정으로도 현재 무료 체험 가능 (한시적).

### 방법 B: API 키 (종량제)

1. https://platform.openai.com 에서 계정 생성
2. 크레딧 충전
3. API 키 발급 후 `OPENAI_API_KEY` 환경 변수 설정

---

## 3단계: 나머지는 Codex에게 맡기기

VS Code에서 Codex를 열고,
아래 파일의 내용을 **첫 대화에 그대로 붙여넣기**:

> **[auto-setup.md](auto-setup.md)**

Codex가 질문하면서 알아서 세팅함:
- Node.js / CLI 설치 확인 및 안내
- 플러그인 설치 (sonmat — 검증규율 + domain hints)
- 글로벌 instructions.md 생성
- Python 도구 설치 (uv, ruff) — 필요한 경우만

---

## 참고

- 무료 체험: ChatGPT Free/Go 계정으로 사용 가능 (한시적)
- 유료: ChatGPT Plus $20/월, Pro $200/월 (Codex 포함)
- API 종량제: codex-mini ~$1.50/MTok, GPT-5 ~$10/MTok
- 기본 모델: GPT-5-Codex
- 모델 변경: `/model` 입력
- 공식 문서: https://developers.openai.com/codex
- Windows: WSL 환경 권장 (CLI), VS Code 확장은 네이티브 지원
