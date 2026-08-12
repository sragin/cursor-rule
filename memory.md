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
| 작업 머신 | GRAM-16ZD90TR (역할: 노트북) |
| 브랜치 | master (origin/master 와 동일, `0 0`) |
| 마지막 커밋 | 57db9e3 feat: 머신 간 인계 구조 추가 |
| 작업 트리 | 내용 변경 없음. `git status` 가 `M CLAUDE.md` 를 표시하나 `git diff` 는 빈 출력이고 워킹 파일 해시와 HEAD 블롭 해시가 `dd04c7f5` 로 동일하다. `core.autocrlf=true` 로 워킹 6145 바이트(CRLF) / 블롭 6079 바이트(LF) 크기가 달라 stat 캐시만 dirty 한 상태다. |

**진행 중인 작업**
- 머신 간 인계 구조 최초 구축(`memory.md`, `/pickup`, `/handoff`, `.claude/rules/field-test.md`, `CLAUDE.local.md`, `.gitignore`).
- 이 저장소는 **뼈대(템플릿)** 다. 위 파일들을 모든 저장소에 반영해 사용한다.

**미해결 이슈**
- 사무실 PC 의 머신 이름이 미확인. `GRAM-16ZD90TR` = 노트북 은 확정됨(`CLAUDE.local.md` 의 역할 항목). 사무실 PC 이름은 해당 머신에서 `$env:COMPUTERNAME` 으로 확인해 "환경 차이" 표에 채운다.

**다음 머신에서 할 일**
1. 사무실 PC 에서 `$env:COMPUTERNAME` 을 확인해 "환경 차이" 표의 머신 이름을 채운다.

## 환경 차이

머신별로 다른 항목만 적는다. **실제 값(IP·포트·경로·시리얼명)은 여기 적지 않는다.**

| 항목 | 사무실 PC | 노트북 | 실제 값 위치 |
| --- | --- | --- | --- |
| 머신 이름 | (미확인) | GRAM-16ZD90TR | `CLAUDE.local.md` |
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
- 커밋 본문은 이유를 `memory.md` 에 남겼으면 쓰지 않고 **제목만** 커밋한다. 변경 파일 목록은 `git show --stat` 이 대신하므로 본문에 나열하지 않는다. 같은 내용을 git log 와 `memory.md` 두 곳에 두면 갈라졌을 때 어느 쪽이 맞는지 알 수 없다.
- Git 커밋 메시지 규칙은 `.cursor/rules/unified-workflow.mdc` 와 `CLAUDE.md` 10번에 **둘 다** 둔다. Cursor 는 `CLAUDE.md` 를 읽지 않아 도구마다 자기 파일이 필요하다. 한쪽을 고치면 다른 쪽도 같이 고친다.
- `/pickup` 은 `disable-model-invocation: true` 라 사용자가 직접 입력해야만 실행되고, 세션 시작 1회뿐이다. 세션이 날짜를 넘길 때의 재동기화가 비어 있어 `CLAUDE.md` 9번에 뒀다. 동기화 절차(미커밋 시 중단, `--ff-only`)는 `pickup/SKILL.md` 에만 두고 9번에서 반복하지 않는다. 경과 시간 기준은 세션 중 확인할 신호가 없어 넣지 않았다. 강제하려면 `SessionStart` 훅이 필요하다.
- 뼈대 `CLAUDE.md` 는 3단 구조다. **역할·1~9번 지침**(언어 무관) / **공통 기술 규칙**(저장소 무관하게 항상 적용, 예: matplotlib 한글 폰트) / **기술 요구사항**(저장소별, `/init` 이 채움). 특정 저장소 전용 내용을 앞 두 곳에 넣지 않는다.
- 사용자 역할은 **제어 엔지니어**다. UI·웹 프런트엔드 전제를 깔지 않는다. 단 matplotlib 시각화는 자주 쓴다.
- 주석 밀도는 **8번 최소 문서화 원칙이 우선**이다. "초보자용 상세 주석"과 충돌할 경우 최소 주석을 따른다.
- 이 저장소는 뼈대(템플릿)다. `.claude/rules/field-test.md` 의 **안전**·**실차 상태에서 금지하는 동작** TODO 와 `CLAUDE.local.md` 의 **실차 접속**·**로컬 실행 환경** 항목은 여기서 채우지 않고 **공란으로 둔다.** 각 저장소·각 머신에 반영할 때 그곳에서 채운다. 공란은 미완성이 아니라 의도된 상태이므로 미해결 이슈로 올리지 않는다.

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
