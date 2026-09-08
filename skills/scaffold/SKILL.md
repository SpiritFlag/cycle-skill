---
name: scaffold
description: "레포의 기본 파일을 표준 양식으로 깐다 — README(표준 골격), LICENSE, CONTRIBUTING, .gitignore, GitHub 이슈 템플릿. 있는 파일은 건드리지 않되 README만은 표준과 대조해 고칠 곳을 보여주고 확인받는다. 사용자가 '/cycle:scaffold'라고 부르거나 'README 표준대로 정리해줘', '기본 파일 깔아줘'라고 하면 쓴다."
---

# scaffold

기본 파일을 한 번 깐다. **README만 살아 있는 문서**라 표준과 대조하고, 나머지는 있으면 건드리지 않는다.

> **공통 규칙**
> - 산출물에서 사용자는 "사용자"다.
> - 있는 파일을 덮어쓰지 않는다. README는 예외로, 대조 결과를 보여주고 확인받은 뒤 고친다.

---

## 0. 실측

`ls` · `find . -maxdepth 2`로 프로젝트 종류(언어 · 엔진 · 빌드 도구)와 이미 있는 파일을 본다. 제품명 · 톤은 `CLAUDE.md`의 `## cycle` 절에 있으면 그것.

## 1. README

정본은 `${CLAUDE_SKILL_DIR}/readme.standard.md`다. 읽고 그대로 따른다.

- **없으면** 골격대로 쓴다. 각 절은 실측으로 채운다. 모르는 것은 `{…}`로 남기지 말고 사용자에게 묻는다.
- **있으면** 표준의 기계 대조 · 사람 대조를 돌린다. 걸린 것을 `절 · 무엇이 · 어디로 가야 하나` 표로 보여준다. 사용자가 확인하면 골격에 맞춰 다시 쓴다. 밀려나는 내용은 버리지 않고 어디로 옮길지(`docs/design.md` · CONTRIBUTING · Release)를 함께 적는다.

## 2. LICENSE

없으면 사용자에게 고르게 한다(MIT · Apache-2.0 · 비공개). 고른 것을 `gh api /licenses/{key}`로 받아 저작권자 · 연도를 채운다. `plugin.json` · `package.json` 등에 license 필드가 있으면 같은 값으로 맞춘다.

## 3. CONTRIBUTING

없으면 `${CLAUDE_SKILL_DIR}/contributing.template.md`로 만든다. 이 파일이 "이 레포는 cycle 체계를 따른다"를 말하는 자리다. 이슈를 어떻게 쓰는지, 브랜치가 어떻게 흐르는지 한 화면.

## 4. .gitignore

없으면 프로젝트 종류에 맞는 것을 `gh api /gitignore/templates/{name}`으로 받아 만든다. 있으면 건드리지 않는다.

## 5. 이슈 템플릿

`.github/ISSUE_TEMPLATE/backlog.md`가 없으면 `${CLAUDE_SKILL_DIR}/issue.template.md`로 만든다. 웹에서 만든 이슈도 같은 골격이 되게 하는 장치다. `propose`가 이 골격을 전제로 읽는다.

## 6. 보고

만든 것 · 고친 것 · 건드리지 않은 것을 각각 한 줄. README를 고쳤으면 밀려난 내용이 어디로 갔는지.

## 하지 않는 것

- CHANGELOG를 만들지 않는다. GitHub Release가 대신한다.
- README에 골격 밖 절을 만들지 않는다.
- 코드 · 설정 · 워크플로를 건드리지 않는다.
