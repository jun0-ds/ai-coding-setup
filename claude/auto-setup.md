# Claude Code 자동 세팅 요청

이 세팅을 시작하기 전에, 아래 질문을 먼저 해줘.
답변을 받은 뒤 해당하는 단계만 실행할 것.

## 사전 질문

1. **Python 프로젝트를 할 예정인가요?** (Yes/No)
   - Yes → 6단계(Python 도구 설치) 실행
   - No → 6단계 스킵

2. **Claude 구독(Pro/Max) 중인가요, API 키를 사용하나요?**
   - 구독 중 → 인증 관련 안내 스킵
   - API 키 → API 키 입력 안내 포함

3. **이미 Claude Code를 쓰고 있고 sonmat 플러그인만 추가하고 싶은 건가요?**
   - Yes → 3단계(플러그인 설치)부터 시작, 기존 설정은 건드리지 않고 병합
   - No → 처음부터 전체 세팅

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
| **sonmat** | 검증규율(Break/Cross/Ground) + domain hints — 코드 품질 자동 검증 체계 |

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

## Thinking Discipline

### Attitude: I can be wrong
When a result comes back, when you feel certain — that is exactly the moment doubt is needed.

### Thinking Directions
1. **Strip to essentials**: Which of my assumptions are truly certain? Am I building from first principles, not analogy?
2. **See differently**: What would a different tool or perspective reveal? Every tool has invisible scope boundaries.
3. **Predict before acting**: What outcome will this produce? Does it match what the user expects?

---

Global instructions for all projects.

## 0. 호칭

- 사용자 → 클로드: "막내"
- 클로드 → 사용자: "선배"
- 존대/반말/반존대를 상황에 맞게 자연스럽게 섞어 쓸 것. 하나로 고정하지 않기.

## 1. Python Project Rules

- When refactoring or developing new Python projects, use the **Astral** toolchain:
  - **uv** for project/package management and script execution.
  - **ruff** for linting and formatting.
  - **ty** for type checking.

- **Always use `uv`**: When running Python scripts, installing dependencies, or managing the environment, you must use `uv`.
  - Example: `uv run python script.py` instead of `python script.py`
  - Example: `uv pip install package` instead of `pip install package`

## 2. Repository Setup

- sonmat hook이 git repo 진입 시 `CLAUDE.md`를 자동 생성한다 (없을 때만).
- 프로젝트 규칙은 처음부터 채우지 않는다. 대화 중 발견된 규칙을 사용자 확인 후 추가한다.
- 템플릿:

> # CLAUDE.md
>
> > 글로벌 지침(`~/.claude/CLAUDE.md`) 상속: 사고규율, 검증규율, 호칭, Python/uv 규칙 등.
>
> ## Project Rules
>
> <!-- 대화 중 발견된 프로젝트 규칙이 여기에 추가됩니다. -->
```

> Python을 안 쓴다고 답했으면 "## 1. Python Project Rules" 섹션을 제거하고 생성할 것.

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
  },
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "~/.claude/plugins/marketplaces/sonmat/hooks/run-hook.cmd session-start",
            "async": false
          }
        ]
      }
    ]
  },
  "enabledPlugins": {
    "sonmat@sonmat": true
  },
  "extraKnownMarketplaces": {
    "sonmat": {
      "source": {
        "source": "github",
        "repo": "jun0-ds/sonmat"
      }
    }
  }
}
```

> **Windows 사용자:** hooks.SessionStart의 command 경로를 확인할 것.
> `~/.claude/plugins/marketplaces/sonmat/hooks/run-hook.cmd`가 실제 존재하는지 확인하고,
> 없으면 `run-hook.sh`로 교체.

## (선택) sonmat만 추가하는 경우 — 사전 질문 3번에서 Yes일 때

기존 `~/.claude/settings.json`이 있으면 **덮어쓰지 말고** 아래 키만 병합:
- `hooks.SessionStart` — 위의 sonmat hook 추가
- `enabledPlugins` — `"sonmat@sonmat": true` 추가
- `extraKnownMarketplaces` — sonmat 마켓플레이스 추가

기존 permissions, 다른 hooks 등은 그대로 유지.

## 6. Python 개발 도구 설치 (사전 질문에서 Yes인 경우만)

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

## 7. 최종 확인

아래 항목 체크해서 결과 알려줘 (해당 항목만):

- [ ] Node.js 18+ 설치됨
- [ ] Claude Code CLI 설치됨
- [ ] 플러그인 설치됨 (sonmat)
- [ ] ~/.claude/CLAUDE.md 생성됨
- [ ] ~/.claude/settings.json 생성됨 (hooks, enabledPlugins, extraKnownMarketplaces 포함)
- [ ] (Python 선택 시) uv 설치됨
- [ ] (Python 선택 시) ruff 설치됨

전부 완료되면 "세팅 완료! 이제 시작하세요." 라고 알려줘.
