# Claude Code 세팅 가이드 (VS Code)

> Anthropic의 AI 코딩 어시스턴트.
> API 종량제 (쓴 만큼만 과금) 또는 Max 구독 ($100/$200/$220/월)

---

## 1단계: Node.js 설치

Claude Code는 Node.js 18 이상이 필요함.

1. https://nodejs.org 에서 LTS 버전 다운로드 & 설치
2. 터미널에서 확인:
```bash
node --version   # v18 이상이면 OK
```

---

## 2단계: Claude Code 설치

터미널에서:

```bash
npm install -g @anthropic-ai/claude-code
```

설치 확인:
```bash
claude --version
```

---

## 3단계: 인증 (둘 중 하나 선택)

### 방법 A: API 키 (종량제, 구독 없이 사용)

1. https://console.anthropic.com 에서 계정 생성
2. 크레딧 충전 (최소 $5)
3. API 키 발급
4. 터미널에서 `claude` 실행 → API 키 입력

### 방법 B: Max 구독

Claude Pro($20/월) 이상 구독 시 자동 연동.
`claude` 실행하면 브라우저 로그인으로 인증.

---

## 4단계: VS Code 확장 프로그램 설치

VS Code 확장 탭(Ctrl+Shift+X)에서 검색 후 설치:

| # | 이름 | 검색어 | 역할 |
|---|------|--------|------|
| 1 | **Claude Code** | `AnthropicAI.claude-code` | VS Code 내 Claude Code 통합 (채팅, diff 뷰, 파일 인식) |

---

## 5단계: 플러그인 설치 (3개)

Claude Code의 핵심 워크플로우 플러그인. 터미널에서:

```bash
claude install-skill anthropics/superpowers
claude install-skill anthropics/gsd
claude install-skill anthropics/andrej-karpathy-skills
```

| # | 이름 | 역할 |
|---|------|------|
| 1 | **superpowers** | 브레인스토밍, TDD, 체계적 디버깅, 코드리뷰, 검증 워크플로우 |
| 2 | **gsd (Get Shit Done)** | 프로젝트 관리 — 스펙 → 플랜 → 실행 → 검증 파이프라인 |
| 3 | **andrej-karpathy-skills** | 코딩 원칙 — 단순성, 정밀 수정, 가정 명시, 검증 기준 수립 |

---

## 6단계: 명령어 허용 설정 (allow)

Claude Code가 셸 명령어를 실행할 때마다 허용 여부를 물어봄.
자주 쓰는 명령어는 미리 허용해두면 편함.

### 방법 1: settings.json에 허용 목록 추가 (권장)

`~/.claude/settings.json` 에서 설정:

```json
{
  "permissions": {
    "allow": [
      "Bash(git *)",
      "Bash(npm *)",
      "Bash(npx *)",
      "Bash(node *)"
    ]
  }
}
```

### 방법 2: 실행 시 전부 허용 (주의)

```bash
claude --dangerously-skip-permissions
```

> 모든 명령을 묻지 않고 실행함. 신뢰할 수 있는 프로젝트에서만 사용.

---

## 7단계: Python 개발 도구 설치 (선택)

Python 프로젝트를 할 경우 아래 도구 설치 권장.

### uv 설치 (빠른 Python 패키지/프로젝트 매니저, pip/venv 대체)

**Windows (PowerShell):**
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

**Mac/Linux:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### ruff 설치 (빠른 Python 린터+포매터, flake8/black 대체)

uv가 설치되었으면:
```bash
uv tool install ruff
```

### 설치 확인
```bash
uv --version
ruff --version
```

> Python 실행은 `python script.py` 대신 `uv run python script.py` 사용 권장.
> 패키지 설치도 `pip install` 대신 `uv pip install` 사용.

---

## 8단계: CLAUDE.md 설정 (선택)

AI에게 프로젝트 컨텍스트를 알려주는 파일.

### 글로벌 설정 (모든 프로젝트 공통)

`~/.claude/CLAUDE.md` 에 아래 내용 저장:

```markdown
# CLAUDE.md

## Python Project Rules

- Python 실행: `uv run python` (python 직접 호출 금지)
- 패키지 설치: `uv pip install` (pip 직접 호출 금지)
- 린트: `ruff check`
- 포맷: `ruff format`
```

> 코딩 원칙(단순성, TDD, 디버깅 등)은 5단계에서 설치한 플러그인이 자동으로 적용하므로 CLAUDE.md에 중복 작성할 필요 없음.

### 프로젝트별 설정

각 프로젝트 루트에 `CLAUDE.md`를 따로 만들어서 프로젝트 고유 지침 추가:

```markdown
# CLAUDE.md

## Project Guidelines

- 이 프로젝트는 TypeScript + React 사용
- 테스트: `npm run test`
- 빌드: `npm run build`
```

---

## 9단계: 첫 채팅 테스트

VS Code에서 `Ctrl+Shift+P` → "Claude Code: Open" 또는 터미널에서:

```bash
claude
```

프롬프트가 뜨면 아무거나 입력:
```
> 이 프로젝트 구조 설명해줘
```

응답이 오면 세팅 완료!

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
  - `/gsd:help` — GSD 워크플로우 도움말
