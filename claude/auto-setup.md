# Claude Code 자동 세팅 요청

아래 작업을 순서대로 실행해줘. 각 단계 완료 후 결과를 알려줘.
OS를 먼저 확인하고, Windows면 PowerShell 명령어를, Mac/Linux면 bash 명령어를 사용해.

> **기존 Claude Code 세팅이 있어도 OK.** 기존 설정 파일은 `.bak`으로 백업 후 새 설정으로 교체함.

## 1. 기존 설정 백업

`~/.claude/` 디렉토리가 이미 존재하면, 아래 파일들을 `.bak`으로 백업:
- `~/.claude/CLAUDE.md` → `~/.claude/CLAUDE.md.bak`
- `~/.claude/settings.json` → `~/.claude/settings.json.bak`

존재하지 않는 파일은 스킵.

## 2. 환경 확인

```bash
node --version
npm --version
```

Node.js 18 미만이면 https://nodejs.org 에서 설치하라고 안내해줘.

## 3. Claude Code 설치 확인

```bash
claude --version
```

설치 안 되어 있으면:
```bash
npm install -g @anthropic-ai/claude-code
```

## 4. VS Code 확장 프로그램 설치

```bash
code --install-extension AnthropicAI.claude-code
```

이미 설치되어 있으면 스킵.

## 5. 플러그인 설치 (3개)

```bash
claude install-skill anthropics/superpowers
claude install-skill anthropics/gsd
claude install-skill anthropics/andrej-karpathy-skills
```

각각 설치 결과 확인.

## 6. 글로벌 설정 디렉토리 생성

**Windows (PowerShell):**
```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude"
```

**Mac/Linux:**
```bash
mkdir -p ~/.claude
```

## 7. 글로벌 CLAUDE.md 생성

`~/.claude/CLAUDE.md` 파일을 아래 내용으로 생성 (1단계에서 이미 백업했으므로 덮어쓰기):

```markdown
# CLAUDE.md

## Python Project Rules

- Python 실행: `uv run python` (python 직접 호출 금지)
- 패키지 설치: `uv pip install` (pip 직접 호출 금지)
- 린트: `ruff check`
- 포맷: `ruff format`
```

> 코딩 원칙(단순성, TDD, 디버깅 등)은 플러그인(superpowers, gsd, andrej-karpathy-skills)이 자동 적용하므로 CLAUDE.md에 중복 작성하지 않음.

## 8. Python 개발 도구 설치

### uv 설치

먼저 `uv --version`으로 설치 여부 확인. 없으면:

**Windows (PowerShell):**
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

**Mac/Linux:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### ruff 설치

uv 설치 후:
```bash
uv tool install ruff
```

설치 확인:
```bash
uv --version
ruff --version
```

## 9. 명령어 허용 설정

`~/.claude/settings.json` 파일을 아래 내용으로 생성:

```json
{
  "permissions": {
    "allow": [
      "Bash(git *)",
      "Bash(npm *)",
      "Bash(npx *)",
      "Bash(node *)",
      "Bash(python *)",
      "Bash(uv *)",
      "Bash(ruff *)"
    ]
  }
}
```

## 10. 최종 확인

아래 항목 체크해서 결과 알려줘:

- [ ] Node.js 18+ 설치됨
- [ ] Claude Code CLI 설치됨
- [ ] VS Code 확장 설치됨
- [ ] 플러그인 3개 설치됨 (superpowers, gsd, andrej-karpathy-skills)
- [ ] uv 설치됨
- [ ] ruff 설치됨
- [ ] ~/.claude/CLAUDE.md 생성됨
- [ ] ~/.claude/settings.json 생성됨

전부 완료되면 "세팅 완료! 터미널에서 `claude` 입력하면 시작!" 이라고 알려줘.
