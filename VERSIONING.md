# Versioning

ai-assistant-setup follows [Semantic Versioning](https://semver.org/): `MAJOR.MINOR.PATCH`.

## What each number means

| | When to bump | Example |
|---|---|---|
| **MAJOR** | Setup flow structure changes that break existing guides | `1.0.0` → `2.0.0` |
| **MINOR** | New CLI support, significant guide updates, sonmat integration changes | `0.2.0` → `0.3.0` |
| **PATCH** | Typos, wording fixes, minor guide corrections | `0.3.0` → `0.3.1` |

## How to release

1. Commit all changes
2. Tag and push:
   ```bash
   git tag vX.Y.Z
   git push origin main --tags
   ```

## Version history

| Version | Date | Summary |
|---------|------|---------|
| `0.5.0` | 2026-04-03 | 호칭/말투 사용자 맞춤형 전환, sonmat 스킬명 업데이트 (autoloop/imp/scribe), 단계 순서·번호 수정, API키 보안 경고, Windows 훅 자동 분기 |
| `0.4.0` | 2026-04-02 | sonmat v0.4.0 대응: prompt-first 아키텍처 반영, inspect 스킬 추가, discipline 자동 참조 (하드코딩 제거), Codex/Gemini auto-update |
| `0.3.0` | 2026-04-01 | Sonmat unified directory install, embed discipline in instruction files, drop sonmat-codex/gemini refs |
| `0.2.0` | 2026-03-31 | Codex guide added, sonmat v0.2 integration, multi-CLI comparison table |
| `0.1.0` | 2026-03-29 | Initial release — Claude + Gemini setup guides |
