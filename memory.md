<!--
기록 규칙 (반드시 지킬 것)

- 추측 금지. git 명령과 코드로 확인된 사실만 적는다.
  확인되지 않은 것은 "미해결 이슈"에 가설임을 명시해 적는다.
- "현재 상태" 블록은 매 세션 덮어쓴다. 과거 값을 남기지 않는다.
- "이력"은 추가만 한다(append only). 기존 항목을 수정·삭제하지 않는다.
- 실차 로그·덤프·레코딩 원본은 커밋하지 않는다. 결론만 여기에 남긴다.
- 실제 환경 값(IP·포트·경로·시리얼명)은 여기 적지 않는다. `CLAUDE.local.md` / `.env` 에 둔다.
-->

# memory.md

머신 간 인계의 유일한 채널. 세션 시작 시 `/pickup`, 세션 종료 시 `/handoff`.

## 현재 상태

| 항목 | 값 |
| --- | --- |
| 작업 머신 | GRAM-16ZD90TR |
| 브랜치 | master |
| 마지막 커밋 | 45b16ce feat: CLAUDE 룰, 스킬 추가 |
| 작업 트리 | 미커밋 변경 있음 — `M CLAUDE.md`, `?? .claude/`, `?? .gitignore`, `?? memory.md` |

**진행 중인 작업**
- 머신 간 인계 구조 최초 구축(`memory.md`, `/pickup`, `/handoff`, `.claude/rules/field-test.md`, `CLAUDE.local.md`, `.gitignore`).

**미해결 이슈**
- 두 머신의 이름과 역할(사무실 PC / 노트북) 매핑이 미확정. 현재 머신 이름 `GRAM-16ZD90TR` 만 확인됨. 나머지 머신 이름은 해당 머신에서 `$env:COMPUTERNAME` 으로 확인해 "환경 차이" 표에 채운다.
- `.claude/rules/field-test.md` 의 **안전** 및 **실차 상태에서 금지하는 동작** 섹션이 TODO 상태. 사용자가 채워야 한다.
- `CLAUDE.md` 에 미커밋 변경(`M CLAUDE.md`)이 남아 있다. 커밋 여부 미결정.
- 로컬 폴더 이름이 아직 `ai_rule` 이다. `ai-rule` 로 변경 예정. 세션 실행 중에는 Windows 가 작업 디렉터리를 잠가 변경 불가(실제 시도해 `being used by another process` 확인). 세션 종료 후 변경해야 한다.

**다음 머신에서 할 일**
1. 해당 머신에서 `CLAUDE.local.md` 를 생성하고 머신 이름·역할·저장소 경로·실차 접속 가능 여부·로컬 실행 환경을 채운다(gitignore 대상이라 동기화되지 않음).
2. `.claude/rules/field-test.md` 의 **안전** 섹션 TODO 를 실제 절차로 채운다.
3. `.claude/rules/field-test.md` 의 **실차 상태에서 금지하는 동작** 섹션 TODO 를 채운다.
4. `CLAUDE.md` 미커밋 변경의 처리(커밋 또는 되돌리기)를 결정한다.
5. 이 머신의 remote URL 을 새 이름으로 맞춘다. remote 설정은 `.git/config` 에 있어 머신 간 동기화되지 않는다:
   `git remote set-url origin git@github.com:sragin/ai-rule.git`

## 환경 차이

머신별로 다른 항목만 적는다. **실제 값(IP·포트·경로·시리얼명)은 여기 적지 않는다.**

| 항목 | 사무실 PC | 노트북 | 실제 값 위치 |
| --- | --- | --- | --- |
| 머신 이름 | (미확인) | (미확인, 현재 머신 후보: GRAM-16ZD90TR) | `CLAUDE.local.md` |
| 실차 네트워크 접속 | 불가 | 가능(실차와 동일 로컬 네트워크) | — |
| 실차 IP·포트·시리얼명 | 해당 없음 | 있음 | `CLAUDE.local.md`, `.env` |
| 저장소 경로 | 다름 | 다름 | `CLAUDE.local.md` |
| 가상환경 활성화 방법 | 다를 수 있음 | 다를 수 있음 | `CLAUDE.local.md` |
| 로컬 실행 포트 | 다를 수 있음 | 다를 수 있음 | `CLAUDE.local.md` |

## 결정 사항

되돌리지 않기로 한 것.

- 머신 간 인계는 저장소 안의 `memory.md` 한 곳으로만 한다. Claude Code 세션 기록과 auto memory 는 머신 로컬이므로 동기화 대상으로 삼지 않는다.
- `~/.claude` 를 파일 동기화 도구로 물리지 않는다. 세션 파일 경로가 작업 디렉터리 절대경로에서 파생되어 머신마다 다르고, 포맷이 내부 규격이며, 실행 중 동시 쓰기 충돌이 난다.
- auto memory 디렉터리를 git 으로 공유하지 않는다. 두 머신이 각각 `MEMORY.md` 를 갱신해 머지 충돌이 상시 발생한다.
- Remote Control 로 머신 간 인계를 시도하지 않는다. 실행이 원격 머신에 남아 "실차와 같은 망에 있어야 한다"는 요구를 만족하지 못한다.
- 슬래시 명령은 `.claude/commands/*.md` 가 아니라 `.claude/skills/<name>/SKILL.md` 로 정의한다(공식 문서가 commands 를 legacy 로 표기).
- `CLAUDE.md` 는 지시문만 보고 작성하지 않는다. 코드베이스를 읽어야 정확하므로 `/init` 으로 생성한다.
- 머신 이동 전 반드시 커밋+푸시한다. stash 는 머신을 넘어가지 못한다.

## 이력

append only. 아래에 추가만 한다.

### 2026-08-12 / GRAM-16ZD90TR / master

**한 일**
- 머신 간 인계 구조 최초 생성: `memory.md`, `.claude/skills/pickup/SKILL.md`, `.claude/skills/handoff/SKILL.md`, `.claude/rules/field-test.md`, `CLAUDE.local.md`(gitignore), `.gitignore`.

**확인한 사실**
- 생성 전 저장소에 `memory.md`, `.claude/`, `.gitignore`, `CLAUDE.local.md` 가 모두 없었다. 덮어쓴 파일 없음.
- 저장소 루트 구성: `.cursor/`, `CLAUDE.md`, `handoff-spec.md`, `keit-rnd-planner.skill`, `koceti-rulebook.skill`, `national-rd-innovation-act-qa.skill`, `user-rules.txt`.
- 현재 머신 이름은 `GRAM-16ZD90TR`, 브랜치 `master`, 마지막 커밋 `45b16ce`.

**막힌 것**
- 없음.
