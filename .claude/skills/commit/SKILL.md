---
name: commit
description: 변경사항을 성격에 따라 분류해 원자적 커밋으로 나눠 커밋한다. gitmoji + 한줄 요약 + 자세한 내용 형식 사용.
---

## Requirements

### 분석

The skill SHALL inspect all changes (staged and unstaged) using `git status` and `git diff HEAD` before classifying.

The skill SHALL classify each changed file into exactly one commit type from the table below.

WHEN files of the same type belong to different features or concerns, the skill SHALL split them into separate commits.

### 커밋 유형

| 유형 | gitmoji | 설명 |
|------|---------|------|
| 새 기능 | ✨ | 새로운 기능 추가 |
| 버그 수정 | 🐛 | 버그 수정 |
| 문서 | 📝 | 문서 추가·수정 |
| 스타일 | 💄 | UI/CSS, 포맷 변경 (로직 무관) |
| 리팩터 | ♻️ | 동작 변경 없는 코드 개선 |
| 성능 | ⚡️ | 성능 개선 |
| 테스트 | ✅ | 테스트 추가·수정 |
| 빌드·의존성 | 📦 | 패키지·빌드 설정 변경 |
| CI/CD | 👷 | CI/CD 설정 변경 |
| 설정 | 🔧 | 설정 파일 변경 |
| 삭제 | 🔥 | 코드·파일 삭제 |
| 이동·이름 변경 | 🚚 | 파일 이동·이름 변경 |
| 초기 커밋 | 🎉 | 프로젝트 시작 |
| 콘텐츠 | 🖊️ | 블로그 포스트 등 콘텐츠 추가·수정 |
| 기타 | 🔨 | 위 유형에 해당하지 않는 변경 |

The skill SHOULD consult [gitmoji][1] for emoji not listed above.

### 사용자 확인

The skill SHALL present the proposed commit plan to the user and wait for confirmation before executing any commit.

### 커밋 실행

The skill SHALL stage files by explicit path; the skill SHALL NOT use `git add -A` or `git add .`.

The skill SHALL NOT use the `--no-verify` flag.

The skill SHALL commit using the following message format:

```
{gitmoji} {한줄 요약}

{자세한 내용}
```

- **한줄 요약**: 명령형 현재 시제, 한국어, 50자 이내
- **자세한 내용**: 변경한 이유·맥락 위주. 무엇을 바꿨는지보다 왜 바꿨는지를 설명. 필요 없으면 생략 가능.

### 예외

IF a file appears to contain secrets (`.env`, credentials, tokens), the skill SHALL exclude it from all commits and notify the user.

IF a file is a large binary unrelated to content (e.g., not a diagram export under `assets/`), the skill SHOULD warn the user before including it.

## 참고

1. [gitmoji | An emoji guide for your commit messages][1]

[1]: https://gitmoji.dev
