# cycle-skill

Claude Code로 개인 프로젝트를 사이클 단위로 굴리는 스킬 묶음. 한국어 전용.

## 이게 뭔가

혼자 개발하는 사람이 "이슈 받기 → 범위 합의 → 배치 구현 → 닫기"를 매번 같은 모양으로 돌리게 한다. 백로그는 GitHub Issues, 사이클은 마일스톤, 문서는 레포의 `docs/cycles/`에 한 장, 설정 파일은 없다. 사이클 하나가 세션 하나다 — 고르기는 사이클 밖에서 하고, 스킬은 받은 이슈를 "한다"만 한다.

```
start #n … → "다음" × 배치 → close        [끊기면] resume   [배포 결정 시] release
```

## 설치

```
/plugin marketplace add SpiritFlag/cycle-skill
/plugin install cycle@spiritflag-cycle
```

`gh auth status`가 통과해야 한다.

## 사용

| 스킬 | 언제 | 하는 일 |
|---|---|---|
| `/cycle:init` | 프로젝트를 세울 때 | 라벨 · 통합 브랜치 · `docs/cycles/` 중 빠진 것을 채우고 검증 명령을 실측해 `CLAUDE.md`에 적는다 |
| `/cycle:scaffold` | 한 번 | README(표준 골격) · LICENSE · CONTRIBUTING · `.gitignore` · 이슈 템플릿 |
| `/cycle:start #n …` | 사이클을 열 때 | 이슈를 읽고 항목별로 범위를 합의한 뒤 사이클 문서 · 마일스톤 · 브랜치를 만들고 B-1에 착수한다 |
| "다음" | 배치마다 | 배치 하나를 구현 · 검증 · 커밋한다. 스킬이 아니라 프롬프트다 |
| `/cycle:resume` | 세션이 끊겼을 때 | 범위 절 · 미완 배치 · git log만 보고 다음 배치부터 잇는다 |
| `/cycle:close` | 전 배치 뒤 | 전체 검증 · 결과 절 · `docs/SPEC.md` 흡수 · `docs/cycles/RISKS.md` · README 대조 · 머지 · 태그 · 이슈 정리 |
| `/cycle:release` | 배포를 결정했을 때 | `develop`은 pre-release, `main`은 release. 버전 간 비교 노트 |

한 바퀴는 이렇게 친다. 한 세션이다.

```
/cycle:start #52 #89
다음
다음
…
/cycle:close
/cycle:release develop
```

## 구조

```
.claude-plugin/     플러그인 · 마켓플레이스 매니페스트
skills/
  init/             SKILL.md, claude-section.template.md
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
