---
name: release
description: "배포를 결정했을 때 GitHub Release를 발행한다 — develop 대상은 pre-release(대내), main 대상은 release(대외). 직전 같은 종류 릴리즈부터 대상 태그까지의 diff로 버전 간 비교 노트를 쓰고, main 모드는 릴리즈 브랜치를 태그로 먼저 전진시킨다. 사이클과 무관하게 부른다. 사용자가 '/cycle:release develop', '/cycle:release main v1.2.0'이라고 하면 쓴다."
argument-hint: "<develop|main> [태그]"
---

# release

사이클을 닫는 것과 배포는 다른 일이다. 이 스킬은 **배포를 결정한 시점**에 따로 부른다.

> **공통 규칙**
> - GitHub 접근은 `gh … --json`뿐이다.
> - 노트 파일을 레포에 남기지 않는다. GitHub Release 본문이 정본이다.
> - 발행 · 브랜치 전진 전에 사용자에게 한 번 묻는다.

---

## 0. 전제

- 인자 첫째는 `develop` 또는 `main`(`CLAUDE.md` `## cycle` 절에 다른 이름이 있으면 그것). 둘째는 태그. 없으면 최신 태그.
- 태그가 존재하고, 통합 브랜치가 그 태그를 포함한다(`git branch --contains {태그}`).
- `gh auth status` 통과. 톤 · 제품명은 `## cycle` 절. 없으면 레포 이름과 "-습니다" 체.

## 1. 기준 릴리즈

`gh release list --json tagName,isPrerelease,publishedAt --limit 50`.

| 모드 | 기준 |
|---|---|
| `develop` (pre-release) | 가장 최근 릴리즈. pre든 정식이든 |
| `main` (release) | 가장 최근 **정식** 릴리즈 |

기준이 없으면 첫 릴리즈다. 첫 태그부터.

## 2. 재료

- `git log {기준}..{태그} --oneline`, `git diff {기준}..{태그} --stat`. **diff가 진실이다.**
- 구간에 든 사이클 문서(`docs/cycles/v*/*.md`)의 범위 절 · 결과 절은 diff를 사람 말로 옮길 때 참고만. 사이클 밖 커밋(작은 수정)도 diff에 있으므로 빠뜨리지 않는다.
- 기준 릴리즈의 노트를 읽어 같은 것을 두 번 공지하지 않는다.

## 3. 노트

**버전 간 비교**다. 제품 사용자가 읽는다.

```
{제품명} {태그}
{기준} → {태그} · {날짜}

## 새로 생긴 것
## 달라진 것
## 고쳐진 것
## 없어진 것
```

- 해당 없는 절은 뺀다.
- 내부 식별자(파일 · 함수 · 이슈 번호) 대신 동작과 효익으로. 이슈 번호는 줄 끝에 `#n`으로만.
- 톤은 `## cycle` 절. 없으면 "-습니다" 체.
- diff에서 확인되지 않은 것은 쓰지 않는다. 사이클 문서가 주장해도 diff에 없으면 뺀다.
- pre-release면 첫 줄에 "검수용"이라고 밝힌다.

노트 전문을 보여주고 확인받는다.

## 4. main 모드 — 릴리즈 브랜치 전진

확인받은 뒤:

- `ff-only`(기본): `git switch main && git merge --ff-only {태그} && git push`.
- `## cycle` 절이 `PR merge commit`이면: `develop → main` PR을 만들고(`gh pr create`) 사용자가 병합한다. Squash · Rebase는 태그 SHA를 main 밖에 남기므로 쓰지 않는다. 병합 뒤 `git branch -a --contains {태그}`에 main이 나오는지 확인.

## 5. 발행

```
gh release create {태그} --title "{태그}" --notes-file {임시 노트} --prerelease      # develop
gh release create {태그} --title "{태그}" --notes-file {임시 노트} --latest          # main
```

제목은 태그만이다. 제품명은 노트 첫 줄에 있다. 임시 노트는 스크래치 경로에 쓰고 발행 뒤 지운다. 같은 태그에 pre-release가 이미 있고 main 모드면 `gh release edit {태그} --prerelease=false --latest --notes-file …`로 승격한다.

## 6. 보고

릴리즈 URL 한 줄. main 모드면 브랜치가 어디까지 왔는지 한 줄.

## 하지 않는 것

- 사이클 문서 · 이슈 · 마일스톤을 건드리지 않는다.
- 노트 파일을 커밋하지 않는다. CHANGELOG를 만들지 않는다.
- 확인 없이 발행 · 전진하지 않는다. 체리픽하지 않는다.
