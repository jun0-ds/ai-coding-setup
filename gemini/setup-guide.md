# Gemini CLI 세팅 가이드 (VS Code)

> 무료로 AI 코딩 어시스턴트 쓰기. Google 계정만 있으면 됨.
> 일일 1,000 요청 무료 (60 req/min)

---

## 1단계: VS Code 확장 프로그램 설치 (2개)

VS Code 확장 탭(Ctrl+Shift+X)에서 검색 후 설치:

| # | 이름 | 검색어 | 역할 |
|---|------|--------|------|
| 1 | **Gemini Code Assist** | `Google.geminicodeassist` | AI 코드 자동완성 + 채팅 + 에이전트 모드 |
| 2 | **Gemini CLI Companion** | `Google.gemini-cli-vscode-ide-companion` | CLI ↔ VS Code 연동 (diff 뷰, 파일 인식) |

또는 터미널에서:
```bash
code --install-extension Google.geminicodeassist
code --install-extension Google.gemini-cli-vscode-ide-companion
```

---

## 2단계: Google 계정 로그인

Gemini Code Assist 확장을 열면 Google 계정 로그인을 안내함.
(API 키 불필요, 구독 불필요)

---

## 3단계: 나머지는 Gemini에게 맡기기

VS Code에서 Gemini Code Assist 채팅을 열고,
아래 파일의 내용을 **첫 대화에 그대로 붙여넣기**:

> **[auto-setup.md](auto-setup.md)**

Gemini가 질문하면서 알아서 세팅함:
- Node.js / Gemini CLI 설치 확인 및 안내
- 글로벌 GEMINI.md 생성 (코딩 원칙 포함)
- Python 도구 설치 (uv, ruff) — 필요한 경우만
- 명령어 허용 정책 설정 (policies/dev.toml)
- settings.json 생성
- IDE 연동

---

## 참고

- 무료 한도: 1,000 req/day, 60 req/min
- 기본 모델: Gemini Flash (빠르고 가벼움)
- 모델 변경: `gemini --model gemini-2.5-pro`
- 공식 문서: https://geminicli.com
