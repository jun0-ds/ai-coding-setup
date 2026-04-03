# Gemini 자동 세팅 요청

이 세팅을 시작하기 전에, 아래 질문을 먼저 해줘.
답변을 받은 뒤 해당하는 단계만 실행할 것.

## 사전 질문

1. **Python 프로젝트를 할 예정인가요?** (Yes/No)
   - Yes → 7단계(Python 도구 설치) 실행
   - No → 7단계 스킵

2. **AI에게 원하는 호칭이나 말투가 있나요?** (자유롭게 답변)
   - 예: "나를 '선배'라고 불러줘, 너는 '막내'야"
   - 예: "편하게 반말로 해줘"
   - 예: "존댓말로 해줘"
   - 딱히 없으면 → 호칭 섹션 생략, 기본 존댓말

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

## 3. sonmat 설치 (AI 사고·검증 규율 체계)

sonmat을 통째로 Gemini 환경에 설치한다. **흩어진 파일이 아니라 하나의 체계**로 유지할 것.

이미 설치되어 있으면 최신 버전으로 업데이트:
```bash
if [ -d ~/.gemini/sonmat/.git ]; then
  git -C ~/.gemini/sonmat pull origin main --ff-only
else
  git clone https://github.com/jun0-ds/sonmat.git ~/.gemini/sonmat
fi
```

설치 확인:
```bash
ls ~/.gemini/sonmat/discipline/  # core.md, hints.md
ls ~/.gemini/sonmat/skills/       # autoloop, guard, imp, inspect, scribe
ls ~/.gemini/sonmat/agents/       # sonmat-worker.md
```

| 플러그인 | 역할 |
|---------|------|
| **sonmat** | AI의 사고·검증 규율 체계 — 코드를 짜기 전에 "이게 맞나?" 한 번 더 생각하게 만드는 플러그인 |

## 4. 글로벌 설정 디렉토리 및 파일 생성

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

`~/.gemini/GEMINI.md` 파일을 생성한다.

**중요:** `~/.gemini/sonmat/discipline/core.md`와 `~/.gemini/sonmat/discipline/hints.md`의 내용을 읽어서 GEMINI.md 안에 **직접 붙여넣기** 할 것. 파일 참조("see core.md")가 아니라 내용 자체를 넣어야 한다 — 참조만 하면 안 읽을 수 있다.

GEMINI.md 구조:

```markdown
# GEMINI.md

## 호칭

(사전 질문 2번 답변을 바탕으로 작성. 예시:)
- 사용자 → Gemini: "(사용자가 정한 호칭)"
- Gemini → 사용자: "(사용자가 정한 호칭)"
- (사용자가 원하는 말투 스타일)

## Core Discipline (sonmat)

(여기에 discipline/core.md 내용 전체를 붙여넣기)

## Domain Hints

(여기에 discipline/hints.md 내용 전체를 붙여넣기)

## Skills

사용 가능한 스킬 (`~/.gemini/sonmat/skills/`):
- `autoloop/` — 자율 루프 + 에스컬레이션
- `guard/` — pre-commit 검증
- `imp/` — 역발상 검증 (해석·계획·결정에 반론 제기)
- `inspect/` — 깊은 검증 모드 (의존성, 부작용, 롤백 점검)
- `scribe/` — 배경 메타 채널 (브릿지 노트, 진행 기록)

## Python 설정

- Python 실행: `uv run python` (python 직접 호출 금지)
- 패키지 설치: `uv pip install` (pip 직접 호출 금지)
- 린트/포맷: `ruff check`, `ruff format`
```

> 사전 질문 2번에서 호칭/말투 요청이 **없었으면** "## 호칭" 섹션을 제거하고 생성할 것.
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

## 5. IDE 연동

```
/ide enable
```

## 6. (선택) sonmat만 추가하는 경우

이미 Gemini를 쓰고 있고 sonmat만 추가하고 싶다면:
1. 3단계(sonmat 설치)만 실행
2. 기존 GEMINI.md에 Core Discipline / Domain Hints / Skills 섹션을 **추가** (기존 내용 유지)
3. 나머지 설정은 건드리지 않음

## 7. Python 개발 도구 설치 (사전 질문에서 Yes인 경우만)

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

## 8. 최종 확인

아래 항목 체크해서 결과 알려줘 (해당 항목만):

- [ ] Node.js 20+ 설치됨
- [ ] Gemini CLI 설치됨
- [ ] sonmat 설치됨
- [ ] ~/.gemini/GEMINI.md 생성됨
- [ ] ~/.gemini/policies/dev.toml 생성됨
- [ ] ~/.gemini/settings.json 생성됨
- [ ] /ide enable 실행됨
- [ ] (Python 선택 시) uv 설치됨
- [ ] (Python 선택 시) ruff 설치됨

전부 완료되면 "세팅 완료! 이제 시작하세요." 라고 알려줘.
