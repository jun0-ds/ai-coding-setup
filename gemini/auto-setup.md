# Gemini 자동 세팅 요청

이 세팅을 시작하기 전에, 아래 질문을 먼저 해줘.
답변을 받은 뒤 해당하는 단계만 실행할 것.

## 사전 질문

1. **Python 프로젝트를 할 예정인가요?** (Yes/No)
   - Yes → 4단계(Python 도구 설치) 실행
   - No → 4단계 스킵

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

## 2. 글로벌 설정 디렉토리 및 파일 생성

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

## 코딩 원칙

### 생각 먼저, 코드는 나중에
- 구현 전에 요구사항과 설계를 먼저 정리할 것
- "뭘 만들지"가 명확해진 다음에 코드를 쓸 것
- 복잡한 작업은 단계별로 나눠서 계획을 세울 것

### 단순하게
- 최소한의 코드로 목표를 달성할 것
- 지금 필요 없는 기능을 미리 만들지 말 것
- 비슷한 코드 3줄이 섣부른 추상화보다 나음
- 불필요한 주석, docstring, 타입 어노테이션 추가하지 말 것

### 정확하게 고치기
- 요청한 범위만 수정할 것. 주변 코드 리팩토링 금지
- 코드 변경 전 반드시 기존 코드를 먼저 읽을 것
- 동작하는 코드를 "개선"한다고 건드리지 말 것
- 에러 처리나 방어 코드를 과도하게 넣지 말 것

### 테스트로 검증
- 기능 구현 전에 테스트부터 작성할 것 (TDD)
- "됐다"고 말하기 전에 실제로 테스트를 돌려서 확인할 것
- 테스트 실패 시 원인을 분석하고, 같은 시도를 반복하지 말 것

### 체계적으로 디버깅
- 버그를 만나면 바로 고치려 하지 말고, 원인을 먼저 파악할 것
- 가설 → 검증 → 수정 순서를 따를 것
- 안 되면 brute force로 밀어붙이지 말 것

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

## 3. IDE 연동

```
/ide enable
```

## 4. Python 개발 도구 설치 (사전 질문에서 Yes인 경우만)

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

## 5. 최종 확인

아래 항목 체크해서 결과 알려줘 (해당 항목만):

- [ ] ~/.gemini/GEMINI.md 생성됨
- [ ] ~/.gemini/policies/dev.toml 생성됨
- [ ] ~/.gemini/settings.json 생성됨
- [ ] /ide enable 실행됨
- [ ] (Python 선택 시) uv 설치됨
- [ ] (Python 선택 시) ruff 설치됨

전부 완료되면 "세팅 완료! 이제 코딩을 시작하세요." 라고 알려줘.
