# Claude Code 자동 세팅 요청

이 세팅을 시작하기 전에, 아래 질문을 먼저 해줘.
답변을 받은 뒤 해당하는 단계만 실행할 것.

## 사전 질문

1. **Python 프로젝트를 할 예정인가요?** (Yes/No)
   - Yes → 5단계(Python 도구 설치) 실행
   - No → 5단계 스킵

2. **Claude 구독(Pro/Max) 중인가요, API 키를 사용하나요?**
   - 구독 중 → 인증 관련 안내 스킵
   - API 키 → API 키 입력 안내 포함

---

질문 답변을 받았으면 아래 작업을 순서대로 실행해줘.
OS를 먼저 확인하고, Windows면 PowerShell 명령어를, Mac/Linux면 bash 명령어를 사용해.

> **기존 Claude Code 세팅이 있어도 OK.** 기존 설정 파일은 `.bak`으로 백업 후 새 설정으로 교체함.

## 1. 기존 설정 백업

`~/.claude/` 디렉토리가 이미 존재하면, 아래 파일들을 `.bak`으로 백업:
- `~/.claude/CLAUDE.md` → `~/.claude/CLAUDE.md.bak`
- `~/.claude/settings.json` → `~/.claude/settings.json.bak`

존재하지 않는 파일은 스킵.

## 2. 환경 확인 및 CLI 설치

Node.js 18+ 확인:
```bash
node --version
```

없거나 18 미만이면 https://nodejs.org 에서 LTS 설치를 안내해줘.
설치 완료 후 다음 진행.

Claude Code CLI 확인:
```bash
claude --version
```

없으면:
```bash
npm install -g @anthropic-ai/claude-code
```

## 3. 플러그인 설치

```bash
claude install-skill jun0-ds/sonmat
```

설치 결과 확인. 이미 설치되어 있으면 스킵.

| 플러그인 | 역할 |
|---------|------|
| **sonmat** | 자율 루프 — 기획→실행→평가→판단 반복, 도메인별 규율 자동 주입 |

## 4. 글로벌 설정 생성

### 디렉토리 생성

**Windows (PowerShell):**
```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude"
```

**Mac/Linux:**
```bash
mkdir -p ~/.claude
```

### CLAUDE.md 생성

`~/.claude/CLAUDE.md` 파일 생성:

```markdown
# CLAUDE.md

## Python Project Rules

- Python 실행: `uv run python` (python 직접 호출 금지)
- 패키지 설치: `uv pip install` (pip 직접 호출 금지)
- 린트: `ruff check`
- 포맷: `ruff format`
```

> Python을 안 쓴다고 답했으면 이 파일은 빈 템플릿으로 생성:
> ```markdown
> # CLAUDE.md
>
> ## Project Rules
>
> <!-- 프로젝트에 맞는 지침을 여기에 추가하세요 -->
> ```

### settings.json 생성

`~/.claude/settings.json` 파일 생성:

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

## 5. Python 개발 도구 설치 (사전 질문에서 Yes인 경우만)

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

## 6. 최종 확인

아래 항목 체크해서 결과 알려줘 (해당 항목만):

- [ ] Node.js 18+ 설치됨
- [ ] Claude Code CLI 설치됨
- [ ] 플러그인 설치됨 (sonmat)
- [ ] ~/.claude/CLAUDE.md 생성됨
- [ ] ~/.claude/settings.json 생성됨
- [ ] (Python 선택 시) uv 설치됨
- [ ] (Python 선택 시) ruff 설치됨

전부 완료되면 "세팅 완료! 이제 코딩을 시작하세요." 라고 알려줘.
