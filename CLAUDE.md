# CLAUDE.md

이 파일은 저장소에서 작업할 때 Claude Code(claude.ai/code)에 제공하는 지침이다.

## 프로젝트 개요

`blog.lul.kr`에 호스팅되는 개인 기술 블로그. GitHub Pages에서 Jekyll 4.3.2와 Minima 2.5 테마로 구동되며, 콘텐츠는 한국어(Markdown)로 작성된다.

## 명령어

```bash
# 의존성 설치
bundle install

# 라이브 리로드로 로컬 서브
bundle exec jekyll serve --livereload

# 프로덕션 빌드
bundle exec jekyll build
```

## 요구사항

### 콘텐츠

어시스턴트는 반드시 블로그 포스트 본문을 한국어로 작성해야 한다.

어시스턴트는 반드시 블로그 포스트 파일을 `_posts/` 디렉토리에 `YYYY-MM-DD-slug.md` 형식의 파일명으로 생성해야 한다.

어시스턴트는 반드시 모든 신규 포스트의 front matter에 최소한 `layout: post`와 `title` 필드를 포함해야 한다:

```yaml
---
layout: post
title: "포스트 제목"
---
```

기술적 의견이나 예측을 작성하는 경우, 어시스턴트는 반드시 해당 내용이 저자의 견해임을 명시해야 한다.

포스트 내용을 추가하거나 수정하는 경우, 어시스턴트는 주장을 검증 가능하게 유지하는 것이 좋다 — 출처를 인용하거나 `fact-checker` 에이전트를 활용한다.

### 에이전트

포스트 품질 보증에 활용되는 에이전트 목록이다. 상세 동작 규칙은 각 에이전트 파일을 참고한다.

| 에이전트 | 역할 |
|----------|------|
| `editor` | 오케스트레이터. 에이전트 실행 결정·순서 조율·컨텍스트 전달 |
| `security-reviewer` | 개인정보·보안 정보 노출 점검 |
| `fact-checker` | 기술적 사실 주장 검증 |
| `reviewer` | 논리적 흐름·근거 자료 평가 |

### 발행 워크플로우

어시스턴트는 `master`에 직접 push해서는 안 된다.

포스트를 발행할 준비가 된 경우, 어시스턴트는 반드시 먼저 `security-reviewer`를 실행한 뒤 feature 브랜치에서 `master`로 pull request를 열어야 한다.

`security-reviewer`가 🔴 차단 항목을 반환하는 경우, 어시스턴트는 해당 문제가 해결될 때까지 pull request를 열어서는 안 된다.

논리적으로 구별되는 여러 변경사항이 있는 경우, 어시스턴트는 반드시 `/commit` 스킬을 사용해 원자적 커밋으로 분리해야 한다.

### 파일 시스템

어시스턴트는 `_site/` 디렉토리의 파일을 수정해서는 안 된다. 이 디렉토리는 Jekyll이 자동으로 생성하며 gitignore 처리된다.

어시스턴트는 다이어그램 소스를 GraphML 파일로 `assets/` 디렉토리에 PNG 내보내기 파일과 함께 보관하는 것이 좋다.

## 참고 자료

1. [GitHub Pages 문서 - GitHub 문서][1]
2. [Jekyll • Simple, blog-aware, static sites | Transform your plain text into static websites and blogs][2]
3. [Bundler: The best way to manage a Ruby application's gems][3]
4. [Alistair Mavin EARS: Easy Approach to Requirements Syntax | Official Guide][4]
5. [RFC 2119 - Key words for use in RFCs to Indicate Requirement Levels][5]

[1]: https://docs.github.com/pages
[2]: https://jekyllrb.com
[3]: https://bundler.io
[4]: https://alistairmavin.com/ears/
[5]: https://datatracker.ietf.org/doc/html/rfc2119
