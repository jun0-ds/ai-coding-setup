# AI Coding Setup

AI 코딩 어시스턴트 세팅 가이드 모음. VS Code 기준.

## Claude Code (Anthropic)

유료 (API 종량제 또는 Max 구독)

| 파일 | 용도 |
|------|------|
| [claude/setup-guide.md](claude/setup-guide.md) | 사람이 따라할 단계별 세팅 가이드 |
| [claude/auto-setup.md](claude/auto-setup.md) | Claude에 던져서 자동 세팅하는 프롬프트 |

## Gemini CLI (Google)

무료 (Google 계정만 있으면 됨, 일일 1,000 요청)

| 파일 | 용도 |
|------|------|
| [gemini/setup-guide.md](gemini/setup-guide.md) | 사람이 따라할 단계별 세팅 가이드 |
| [gemini/auto-setup.md](gemini/auto-setup.md) | Gemini에 던져서 자동 세팅하는 프롬프트 |

## 구조

```
ai-coding-setup/
├── README.md
├── claude/
│   ├── setup-guide.md    # 수동 세팅 가이드
│   └── auto-setup.md     # 자동 세팅 프롬프트
└── gemini/
    ├── setup-guide.md    # 수동 세팅 가이드
    └── auto-setup.md     # 자동 세팅 프롬프트
```

## 사용법

### 직접 세팅
`setup-guide.md`를 열고 단계별로 따라하기.

### 자동 세팅
`auto-setup.md` 내용을 해당 AI CLI 첫 대화에 붙여넣기.
AI가 알아서 환경 세팅을 진행함.
