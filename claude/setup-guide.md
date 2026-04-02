# Claude Code 세팅 가이드 (VS Code)

> Anthropic의 AI 코딩 어시스턴트.
> API 종량제 (쓴 만큼만 과금) 또는 Max 구독 ($100/$200/월)

---

## 1단계: VS Code 확장 프로그램 설치

VS Code 확장 탭(Ctrl+Shift+X)에서 `Claude Code` 검색 후 설치.

또는 터미널에서:
```bash
code --install-extension AnthropicAI.claude-code
```

---

## 2단계: 인증 (둘 중 하나 선택)

### 방법 A: API 키 (종량제, 구독 없이 사용)

1. https://console.anthropic.com 에서 계정 생성
2. 크레딧 충전 (최소 $5)
3. API 키 발급

### 방법 B: Max 구독

Claude Pro($20/월) 이상 구독 중이면 별도 설정 불필요.

---

## 3단계: 나머지는 Claude에게 맡기기

VS Code에서 Claude Code를 열고 (Ctrl+Shift+P → "Claude Code: Open"),
아래 파일의 내용을 **첫 대화에 그대로 붙여넣기**:

> **[auto-setup.md](auto-setup.md)**

Claude가 질문하면서 알아서 세팅함:
- Node.js / CLI 설치 확인 및 안내
- 플러그인 설치 (sonmat — 검증규율 + domain hints)
- 글로벌 CLAUDE.md 생성 (사고규율, 호칭, Python 규칙 등)
- settings.json 생성 (hooks, 플러그인, 명령어 허용)
- Python 도구 설치 (uv, ruff) — 필요한 경우만

> 💡 **이미 Claude Code를 쓰고 있다면?**
> auto-setup.md를 붙여넣으면 "sonmat만 추가" 옵션을 선택할 수 있음.
> 기존 설정을 유지하면서 플러그인만 병합.

---

## 참고

- 요금: API 종량제 (Sonnet ~$3/MTok, Opus ~$15/MTok) 또는 Max 구독
- 기본 모델: Claude Sonnet 4.6
- 모델 변경: 채팅 중 `/model` 입력
- 공식 문서: https://docs.anthropic.com/en/docs/claude-code
- 주요 슬래시 명령어:
  - `/help` — 도움말
  - `/model` — 모델 변경
  - `/compact` — 컨텍스트 압축
  - `/sonmat:loop` — 자율 루프 실행 (기획→실행→평가→판단)
  - `/sonmat:guard` — 커밋 전 가드레일 검증
  - `/sonmat:plan` — 마일스톤/페이즈 관리
  - `/sonmat:inspect` — 깊은 검증 모드 (의존성, 부작용, 롤백 점검)
