# Codex 자동 세팅 요청

이 세팅을 시작하기 전에, 아래 질문을 먼저 해줘.
답변을 받은 뒤 해당하는 단계만 실행할 것.

## 사전 질문

1. **Python 프로젝트를 할 예정인가요?** (Yes/No)
   - Yes → 6단계(Python 도구 설치) 실행
   - No → 6단계 스킵

2. **ChatGPT 구독 중인가요, API 키를 사용하나요?**
   - 구독 중 또는 무료 체험 → ChatGPT 로그인 안내
   - API 키 → 환경 변수 설정 안내

---

질문 답변을 받았으면 아래 작업을 순서대로 실행해줘.
OS를 먼저 확인하고, Windows면 WSL/PowerShell 명령어를, Mac/Linux면 bash 명령어를 사용해.

> **Windows 사용자 참고:** Codex CLI는 macOS/Linux 네이티브 지원. Windows에서는 WSL 사용을 권장.

## 1. 기존 설정 백업

`~/.codex/` 디렉토리가 이미 존재하면, 아래 파일들을 `.bak`으로 백업:
- `~/.codex/instructions.md` → `~/.codex/instructions.md.bak`

존재하지 않는 파일은 스킵.

## 2. 환경 확인 및 CLI 설치

Node.js 22+ 확인:
```bash
node --version
```

없거나 22 미만이면 https://nodejs.org 에서 LTS 설치를 안내해줘.
설치 완료 후 다음 진행.

Codex CLI 확인:
```bash
codex --version
```

없으면:
```bash
npm install -g @openai/codex
```

## 3. 인증 설정

### ChatGPT 계정 사용 시

```bash
codex
```
실행 후 "Sign in with ChatGPT" 선택. 브라우저에서 로그인.

### API 키 사용 시

**Mac/Linux (bash/zsh):**
```bash
echo 'export OPENAI_API_KEY="여기에_API_키_입력"' >> ~/.bashrc
source ~/.bashrc
```

**Windows (PowerShell):**
```powershell
[Environment]::SetEnvironmentVariable("OPENAI_API_KEY", "여기에_API_키_입력", "User")
```

## 4. 플러그인 설치 (sonmat)

sonmat 플러그인 디렉토리를 설치한다:

```bash
git clone https://github.com/jun0-ds/sonmat-codex.git ~/.codex/plugins/sonmat
```

설치 결과 확인. 이미 설치되어 있으면 스킵.

| 플러그인 | 역할 |
|---------|------|
| **sonmat** | 검증규율(Break/Cross/Ground) + domain hints — 코드 품질 자동 검증 체계 |

## 5. 글로벌 설정 생성

### instructions.md 생성

`~/.codex/instructions.md` 파일 생성:

```markdown
# Codex Instructions

## 코딩 원칙

### 생각 먼저, 코드는 나중에
- 구현 전에 요구사항과 설계를 먼저 정리할 것
- "뭘 만들지"가 명확해진 다음에 코드를 쓸 것
- 복잡한 작업은 단계별로 나눠서 계획을 세울 것

### 단순하게
- 최소한의 코드로 목표를 달성할 것
- 지금 필요 없는 기능을 미리 만들지 말 것
- 비슷한 코드 3줄이 섣부른 추상화보다 나음

### 정확하게 고치기
- 요청한 범위만 수정할 것. 주변 코드 리팩토링 금지
- 코드 변경 전 반드시 기존 코드를 먼저 읽을 것

### 테스트로 검증
- "됐다"고 말하기 전에 실제로 테스트를 돌려서 확인할 것

## Python 설정

- Python 실행: `uv run python` (python 직접 호출 금지)
- 패키지 설치: `uv pip install` (pip 직접 호출 금지)
- 린트/포맷: `ruff check`, `ruff format`
```

> Python을 안 쓴다고 답했으면 "## Python 설정" 섹션을 제거하고 생성할 것.

## 6. Python 개발 도구 설치 (사전 질문에서 Yes인 경우만)

### uv 설치

먼저 `uv --version`으로 설치 여부 확인. 없으면:

**Windows (PowerShell) / WSL:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
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

- [ ] Node.js 22+ 설치됨
- [ ] Codex CLI 설치됨
- [ ] 인증 완료 (ChatGPT 로그인 또는 API 키 설정)
- [ ] sonmat 플러그인 설치됨
- [ ] ~/.codex/instructions.md 생성됨
- [ ] (Python 선택 시) uv 설치됨
- [ ] (Python 선택 시) ruff 설치됨

전부 완료되면 "세팅 완료! 이제 코딩을 시작하세요." 라고 알려줘.
