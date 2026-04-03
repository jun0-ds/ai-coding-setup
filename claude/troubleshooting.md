# Claude Code 트러블슈팅

실제 사용 중 겪은 문제와 해결법 모음.

---

## 플러그인 (sonmat) 관련

### Skills가 안 보일 때

**증상:** `/sonmat:guard` 등 슬래시 명령어가 자동완성에 나타나지 않음.

**진단 순서:**

1. 플러그인 설치 확인
   ```bash
   ls ~/.claude/plugins/marketplaces/sonmat/
   ```
   디렉토리가 없거나 비어있으면 미설치 상태.

2. `settings.json` 확인
   ```bash
   cat ~/.claude/settings.json
   ```
   아래 키가 모두 있는지 체크:
   - `enabledPlugins` 에 `"sonmat@sonmat": true`
   - `extraKnownMarketplaces` 에 sonmat 항목
   - `hooks.SessionStart` 에 sonmat hook

3. 훅 실행 테스트
   ```bash
   bash ~/.claude/plugins/marketplaces/sonmat/hooks/run-hook.sh session-start
   ```
   에러가 나면 sonmat 자체 문제 가능성.

**해결:** 재설치가 가장 확실하다 (터미널에서 직접 실행):
```bash
claude plugin marketplace add jun0-ds/sonmat
claude plugin install sonmat@sonmat
```
설치 후 **새 세션을 시작**해야 반영됨. 기존 세션에서는 보이지 않는다.

### 플러그인 재설치 절차

기존 설치가 꼬였을 때:

```bash
# 1. 기존 플러그인 제거
claude plugin uninstall sonmat
rm -rf ~/.claude/plugins/marketplaces/sonmat/

# 2. 재설치
claude plugin marketplace add jun0-ds/sonmat
claude plugin install sonmat@sonmat

# 3. 새 세션 시작 (기존 세션은 이전 상태가 캐시됨)
```

위 방법이 안 되면 수동 클론 후 등록:
```bash
rm -rf ~/.claude/plugins/marketplaces/sonmat/
git clone https://github.com/jun0-ds/sonmat.git ~/.claude/plugins/marketplaces/sonmat
claude plugin install sonmat@sonmat
```

> **주의:** `settings.json`의 `enabledPlugins`, `extraKnownMarketplaces`, `hooks` 항목은
> 재설치로 자동 복구되지 않을 수 있다. 빠져 있으면 [auto-setup.md](auto-setup.md)의
> settings.json 섹션을 참고해서 수동 추가.

---

## Windows 환경 이슈

### WSL vs Windows 네이티브 차이

Claude Code는 Windows에서 두 가지 방식으로 실행 가능:

| | WSL (Ubuntu) | Windows 네이티브 (PowerShell/Git Bash) |
|---|---|---|
| 경로 형식 | `/home/user/.claude/` | `C:\Users\user\.claude\` |
| 훅 스크립트 | `run-hook.sh` (bash) | `run-hook.cmd` (cmd) |
| 슬래시 명령어 | 정상 동작 | MSYS2 경로 변환 문제 가능 |
| 추천 여부 | **권장** | 가능하지만 주의 필요 |

**WSL 사용을 권장하는 이유:**
- 경로 호환성 문제 없음
- bash 훅이 네이티브로 동작
- Linux 도구체인과 호환

### MSYS2 경로 변환 문제 (Git Bash)

**증상:** Git Bash에서 `/plugin`, `/sonmat:guard` 같은 슬래시 명령어를 입력하면
MSYS2가 자동으로 Windows 경로로 변환한다.

```
# 입력한 것
/plugin

# MSYS2가 변환한 것
C:/Program Files/Git/plugin
```

이 때문에 Claude Code가 명령어를 인식하지 못한다.

**원인:** Git for Windows에 내장된 MSYS2 레이어가 `/`로 시작하는 모든 문자열을
Windows 절대경로로 변환하는 동작.

**해결 방법:**

1. **환경변수 설정** (세션 단위)
   ```bash
   export MSYS_NO_PATHCONV=1
   ```

2. **영구 설정** (`~/.bashrc` 또는 `~/.bash_profile`에 추가)
   ```bash
   # Git Bash MSYS2 경로 자동 변환 비활성화
   export MSYS_NO_PATHCONV=1
   ```

3. **WSL로 전환** (근본적 해결)
   ```bash
   wsl
   # WSL 안에서 claude 실행
   ```

> **참고:** `MSYS_NO_PATHCONV=1`은 Git Bash 전체에 영향을 준다.
> 일부 Windows 네이티브 도구가 `/` 경로를 받아야 하는 경우 부작용이 있을 수 있다.
> 문제가 생기면 세션 단위로만 설정하거나, WSL 사용을 권장.

### 어시스턴트가 `/plugin` 명령을 실행하지 못하는 경우

**증상:** Claude Code 어시스턴트에게 "플러그인 설치해줘"라고 요청했지만,
어시스턴트가 `/plugin` 이나 `claude install-skill` 등의 CLI 명령을 직접 실행하지 못함.

**원인:** 슬래시 명령어(`/plugin`, `/model` 등)는 사용자가 직접 입력해야 하는
인터랙티브 명령이다. 어시스턴트는 이를 대신 실행할 수 없다.

**해결:** 사용자가 직접 터미널에서 실행:
```bash
claude plugin marketplace add jun0-ds/sonmat
claude plugin install sonmat@sonmat
```

---

## 일반

### 세팅 변경 후 반영이 안 될 때

`settings.json`, `CLAUDE.md`, 훅 스크립트 등을 수정한 경우
**반드시 새 세션을 시작**해야 반영된다. 기존 세션은 시작 시점의 설정이 캐시되어 있다.

### 훅 경로 확인

현재 어떤 훅이 설정되어 있는지 빠르게 확인:
```bash
cat ~/.claude/settings.json | grep -A5 '"command"'
```
