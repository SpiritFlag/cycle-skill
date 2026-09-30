# cycle-skill

Codex와 Claude Code로 개인 프로젝트를 사이클 단위로 굴리는 스킬 묶음. 한국어 전용.

## 이게 뭔가

혼자 개발하는 사람이 "이슈 받기 → 범위 합의 → 배치 구현 → 닫기"를 매번 같은 모양으로 돌리게 한다. 백로그는 GitHub Issues, 사이클은 마일스톤, 문서는 레포의 `docs/cycles/`에 한 장, 설정 파일은 없다. 사이클 하나가 세션 하나다 — 고르기는 사이클 밖에서 하고, 스킬은 받은 이슈를 "한다"만 한다.

```
start #n … → "다음" × 배치 → close        [끊기면] resume   [배포 결정 시] release
```

## 설치

Codex CLI — 로컬에서 설치:

```sh
codex plugin marketplace add .
codex plugin add cycle@spiritflag-cycle
```

Codex CLI — GitHub에서 설치:

```sh
codex plugin marketplace add SpiritFlag/cycle-skill
codex plugin add cycle@spiritflag-cycle
```

Codex 데스크톱은 이 저장소를 연 뒤 앱을 재시작하고 플러그인 목록에서 **SpiritFlag Cycle** 소스의 **cycle-skill**을 설치한다.

Claude Code:

```text
/plugin marketplace add SpiritFlag/cycle-skill
/plugin install cycle@spiritflag-cycle
```

`git` · `gh`가 설치되어 있고 `gh auth status`가 통과해야 한다.

## 사용

| Codex | Claude Code | 언제 |
|---|---|---|
| `$cycle:init` | `/cycle:init` | 프로젝트를 cycle 체계로 세울 때 |
| `$cycle:scaffold` | `/cycle:scaffold` | 기본 파일을 깔 때 |
| `$cycle:start #n …` | `/cycle:start #n …` | 받은 이슈로 사이클을 열 때 |
| "다음" | "다음" | 다음 배치를 구현 · 검증 · 커밋할 때 |
| `$cycle:resume` | `/cycle:resume` | 끊긴 사이클을 이을 때 |
| `$cycle:close` | `/cycle:close` | 전 배치가 끝났을 때 |
| `$cycle:release develop` | `/cycle:release develop` | 검수용 배포를 결정했을 때 |
| `$cycle:release main` | `/cycle:release main` | 정식 배포를 결정했을 때 |

Codex는 `$` 선택기에서 **cycle 플러그인 소속 스킬**을 선택한다. 프로젝트 규칙은 Codex의 `AGENTS.md`, Claude Code의 `CLAUDE.md` 안에 있는 `## cycle` 절에 둔다. 환경을 바꾸면 `init`으로 기존 절을 점검해 옮긴다.

한 바퀴는 이렇게 친다. 한 세션이다.

```
$cycle:start #52 #89
다음
다음
…
$cycle:close
$cycle:release develop
```

## 구조

```
.agents/plugins/   Codex 마켓플레이스
.codex-plugin/     Codex 플러그인 매니페스트
.claude-plugin/    Claude Code 플러그인 · 마켓플레이스 매니페스트
skills/
  runtime.md        실행 환경 · 프로젝트 규칙 파일 · 리소스 경로
  init/             SKILL.md, cycle-section.template.md
  scaffold/         SKILL.md, readme.standard.md, contributing.template.md, issue.template.md
  start/            SKILL.md, cycle.template.md, batch.md
  resume/           SKILL.md
  close/            SKILL.md
  release/          SKILL.md
docs/design.md      왜 이렇게 만들었나
```

## 더 보기

- `docs/design.md` — 계약 · 원칙 · 흐름 · 문서 정의 · 폐기한 것
- 각 `skills/*/SKILL.md` — 단계별 절차의 정본. 배치 루프는 `skills/start/batch.md`
- Releases — 버전별 변경
