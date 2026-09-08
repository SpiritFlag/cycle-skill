# cycle-skill

Claude Code로 개인 프로젝트를 사이클 단위로 굴리는 스킬 묶음. 한국어 전용.

## 이게 뭔가

혼자 개발하는 사람이 "무엇을 할지 합의 → 계획 → 구현 → 닫기"를 매번 같은 모양으로 돌리게 한다. 백로그는 GitHub Issues, 사이클은 마일스톤, 문서는 레포의 `docs/cycles/`에 남고, 설정 파일은 없다. 단계마다 새 세션을 열고 스킬 하나를 부른다.

```
propose → plan → do → close        [배포 결정 시] release
```

## 설치

```
/plugin marketplace add SpiritFlag/cycle-skill
/plugin install cycle@spiritflag-cycle
```

`gh auth status`가 통과해야 한다.

## 사용

| 스킬 | 언제 | 하는 일 | 권장 모델 |
|---|---|---|---|
| `/cycle:init` | 프로젝트를 세울 때 | 라벨 · 통합 브랜치 · `docs/cycles/` 중 빠진 것을 채우고 검증 명령을 실측해 `CLAUDE.md`에 적는다 | Opus |
| `/cycle:scaffold` | 한 번 | README(표준 골격) · LICENSE · CONTRIBUTING · `.gitignore` · 이슈 템플릿 | Opus |
| `/cycle:propose` | 다음 사이클을 정할 때 | 열린 이슈에서 고르고 묶어 항목별로 합의한 뒤 `scope.md` · 마일스톤 · 브랜치를 만든다 | Opus |
| `/cycle:plan` | scope 뒤 | 사용자용 `plan.md`와 구현용 `do.md` 지시서를 쓰고 검수를 받아 확정한다 | Fable |
| `/cycle:do` | plan 확정 뒤, 프롬프트마다 | 배치 하나를 구현 · 검증하고 회차로 기록한다 | Sonnet |
| `/cycle:close` | 전 배치 검증 뒤 | 보고서 `report.md` · README 대조 · 머지 · 태그 · 이슈 정리 | Opus |
| `/cycle:release` | 배포를 결정했을 때 | `develop`은 pre-release, `main`은 release. 버전 간 비교 노트 | Opus |

모델은 권장일 뿐이다. 스킬이 모델을 바꾸지 않는다. 세션을 열 때 사용자가 고른다.

한 바퀴는 이렇게 친다. 각 줄이 새 세션이다.

```
/cycle:propose
/cycle:plan
/cycle:do          … 다음 … 다시해 … (배치가 끝날 때까지)
/cycle:close
/cycle:release develop
```

## 구조

```
.claude-plugin/     플러그인 · 마켓플레이스 매니페스트
skills/
  init/             SKILL.md, claude-section.template.md
  scaffold/         SKILL.md, readme.standard.md, contributing.template.md, issue.template.md
  propose/          SKILL.md, scope.template.md
  plan/             SKILL.md, plan.template.md, do.template.md
  do/               SKILL.md
  close/            SKILL.md, report.template.md
  release/          SKILL.md
docs/design.md      왜 이렇게 만들었나
```

## 더 보기

- `docs/design.md` — 원칙 · 흐름 · 문서 정의 · 폐기한 것
- 각 `skills/*/SKILL.md` — 단계별 절차의 정본
- Releases — 버전별 변경
