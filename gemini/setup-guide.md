# Gemini CLI 세팅 가이드 (VS Code)

> 무료로 AI 코딩 어시스턴트 쓰기. Google 계정만 있으면 됨.
> 일일 1,000 요청 무료 (60 req/min)

---

## 1단계: Node.js 설치

Gemini CLI는 Node.js 20 이상이 필요함.

1. https://nodejs.org 에서 LTS 버전 다운로드 & 설치
2. 터미널에서 확인:
```bash
node --version   # v20 이상이면 OK
```

---

## 2단계: Gemini CLI 설치

터미널에서:

```bash
npm install -g @google/gemini-cli
```

설치 확인:
```bash
gemini --version
```

---

## 3단계: Google 계정 로그인

```bash
gemini
```

처음 실행하면 브라우저가 열림 → Google 계정으로 로그인 → 끝.
(API 키 불필요, 구독 불필요)

---

## 4단계: VS Code 확장 프로그램 설치 (2개)

VS Code 확장 탭(Ctrl+Shift+X)에서 검색 후 설치:

| # | 이름 | 검색어 | 역할 |
|---|------|--------|------|
| 1 | **Gemini Code Assist** | `Google.geminicodeassist` | AI 코드 자동완성 + 채팅 + 에이전트 모드 |
| 2 | **Gemini CLI Companion** | `Google.gemini-cli-vscode-ide-companion` | CLI ↔ VS Code 연동 (diff 뷰, 파일 인식) |

### Companion 연동 활성화

VS Code 터미널에서 `gemini` 실행 후, Gemini 프롬프트 안에서:
```
/ide enable
```

---

## 5단계: 명령어 허용 설정 (allow)

Gemini가 셸 명령어를 실행할 때마다 허용 여부를 물어봄.
자주 쓰는 명령어는 미리 허용해두면 편함.

### 방법 1: 정책 파일로 일괄 허용 (권장)

글로벌 설정에 넣으면 모든 프로젝트에서 적용됨.

**Windows (PowerShell):**
```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.gemini\policies"
```

**Mac/Linux:**
```bash
mkdir -p ~/.gemini/policies
```

`~/.gemini/policies/dev.toml` 파일 생성:

```toml
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
command_prefix = "node"
decision = "allow"

[[rules]]
tool = "ShellTool"
command_prefix = "npx"
decision = "allow"
```

### 방법 2: 실행 시 인라인 허용

```bash
gemini --allowed-tools "ShellTool(git status),ShellTool(npm run build)"
```

### 방법 3: YOLO 모드 (전부 허용, 주의)

```bash
gemini --yolo
```

> YOLO 모드는 모든 명령을 묻지 않고 실행함. 신뢰할 수 있는 프로젝트에서만 사용.

---

## 6단계: Python 개발 도구 설치 (선택)

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

## 7단계: GEMINI.md 설정 (선택)

AI에게 프로젝트 컨텍스트를 알려주는 파일.

### 글로벌 설정 (모든 프로젝트 공통)

`~/.gemini/GEMINI.md` 에 아래 내용 저장:

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

### 프로젝트별 설정

각 프로젝트 루트에 `GEMINI.md`를 따로 만들어서 프로젝트 고유 지침 추가:

```markdown
# GEMINI.md

## Project Guidelines

- 이 프로젝트는 TypeScript + React 사용
- 테스트: `npm run test`
- 빌드: `npm run build`
```

---

## 8단계: 첫 채팅 테스트

VS Code 터미널에서:

```bash
gemini
```

프롬프트가 뜨면 아무거나 입력:
```
> 이 프로젝트 구조 설명해줘
```

응답이 오면 세팅 완료!

---

## 참고

- 무료 한도: 1,000 req/day, 60 req/min
- 기본 모델: Gemini Flash (빠르고 가벼움)
- 모델 변경: `gemini --model gemini-2.5-pro`
- 공식 문서: https://geminicli.com
