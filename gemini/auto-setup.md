# Gemini 자동 세팅 요청

이 세팅을 시작하기 전에, 아래 질문을 먼저 해줘.
답변을 받은 뒤 해당하는 단계만 실행할 것.

## 사전 질문

1. **Python 프로젝트를 할 예정인가요?** (Yes/No)
   - Yes → 5단계(Python 도구 설치) 실행
   - No → 5단계 스킵

---

질문 답변을 받았으면 아래 작업을 순서대로 실행해줘.
OS를 먼저 확인하고, Windows면 PowerShell 명령어를, Mac/Linux면 bash 명령어를 사용해.

> **기존 Gemini 세팅이 있어도 OK.** 기존 설정 파일은 `.bak`으로 백업 후 새 설정으로 교체함.

## 1. 기존 설정 백업

`~/.gemini/` 디렉토리가 이미 존재하면, 아래 파일들을 `.bak`으로 백업:
- `~/.gemini/GEMINI.md` → `~/.gemini/GEMINI.md.bak`
- `~/.gemini/settings.json` → `~/.gemini/settings.json.bak`
- `~/.gemini/policies/dev.toml` → `~/.gemini/policies/dev.toml.bak`

존재하지 않는 파일은 스킵.

## 2. 환경 확인 및 CLI 설치

Node.js 20+ 확인:
```bash
node --version
```

없거나 20 미만이면 https://nodejs.org 에서 LTS 설치를 안내해줘.
설치 완료 후 다음 진행.

Gemini CLI 확인:
```bash
gemini --version
```

없으면:
```bash
npm install -g @google/gemini-cli
```

## 3. 글로벌 설정 디렉토리 및 파일 생성

### 디렉토리 생성

**Windows (PowerShell):**
```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.gemini"
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.gemini\policies"
```

**Mac/Linux:**
```bash
mkdir -p ~/.gemini/policies
```

### GEMINI.md 생성

`~/.gemini/GEMINI.md` 파일 생성:

```markdown
# GEMINI.md

## Discipline

See `~/.gemini/discipline/core.md` for verification rules.
See `~/.gemini/discipline/hints.md` for domain-specific traps.

## Python 설정

- Python 실행: `uv run python` (python 직접 호출 금지)
- 패키지 설치: `uv pip install` (pip 직접 호출 금지)
- 린트/포맷: `ruff check`, `ruff format`
```

> Python을 안 쓴다고 답했으면 "## Python 설정" 섹션을 제거하고 생성할 것.

### settings.json 생성

`~/.gemini/settings.json` 파일:

```json
{
  "model": "gemini-2.5-flash",
  "theme": "default"
}
```

### 명령어 허용 정책 생성

`~/.gemini/policies/dev.toml` 파일 생성:

```toml
# 기본 개발 명령어 허용
[[rules]]
tool = "ShellTool"
command_prefix = "git"
decision = "allow"

[[rules]]
tool = "ShellTool"
command_prefix = "npm"
decision = "allow"

[[rules]]
tool = "ShellTool"
command_prefix = "npx"
decision = "allow"

[[rules]]
tool = "ShellTool"
command_prefix = "node"
decision = "allow"

[[rules]]
tool = "ShellTool"
command_prefix = "python"
decision = "allow"

[[rules]]
tool = "ShellTool"
command_prefix = "uv"
decision = "allow"

[[rules]]
tool = "ShellTool"
command_prefix = "ruff"
decision = "allow"

[[rules]]
tool = "ShellTool"
command_prefix = "ls"
decision = "allow"

[[rules]]
tool = "ShellTool"
command_prefix = "cat"
decision = "allow"

[[rules]]
tool = "ShellTool"
command_prefix = "mkdir"
decision = "allow"
```

## 4. 플러그인 설치 (sonmat)

sonmat의 discipline과 skills를 Gemini 환경에 복사한다.

```bash
# sonmat 클론 (임시)
git clone https://github.com/jun0-ds/sonmat.git /tmp/sonmat

# discipline + skills 복사
cp -r /tmp/sonmat/discipline ~/.gemini/
cp -r /tmp/sonmat/skills ~/.gemini/

# 정리
rm -rf /tmp/sonmat
```

복사 결과 확인:
```bash
ls ~/.gemini/discipline/  # core.md, hints.md 있어야 함
ls ~/.gemini/skills/       # guard, loop, plan 있어야 함
```

| 플러그인 | 역할 |
|---------|------|
| **sonmat** | 검증규율(Break/Cross/Ground) + domain hints — 코드 품질 자동 검증 체계 |

> 참고: sonmat은 Claude Code 플러그인이 기본이고, 다른 CLI에서는 discipline/skills 파일을 수동으로 복사해서 사용합니다. 자세한 내용은 [sonmat README](https://github.com/jun0-ds/sonmat#other-ai-clis)를 참고하세요.

## 5. IDE 연동

```
/ide enable
```

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

- [ ] Node.js 20+ 설치됨
- [ ] Gemini CLI 설치됨
- [ ] sonmat 확장 설치됨
- [ ] ~/.gemini/GEMINI.md 생성됨
- [ ] ~/.gemini/policies/dev.toml 생성됨
- [ ] ~/.gemini/settings.json 생성됨
- [ ] /ide enable 실행됨
- [ ] (Python 선택 시) uv 설치됨
- [ ] (Python 선택 시) ruff 설치됨

전부 완료되면 "세팅 완료! 이제 코딩을 시작하세요." 라고 알려줘.
