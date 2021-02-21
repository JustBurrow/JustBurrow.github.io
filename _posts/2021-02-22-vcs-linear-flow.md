---
title: VCS Linear Flow
layout: post
---

- 도구를 어떤 방법으로 사용할 것이 좋은가는 전제 조건에 따라 다르다.
- 버전 관리도구의 주요 목적
  - 수정 사항의 공유
  - 수정 사항의 추적
- 애초에 Git은 리눅스 커널 개발에 사용하기 위한 것이다.
  - Git Flow 역시 리눅스 커널 개발에 좋았기 때문에 나오고, 퍼진 것이다.
    - 오픈소스와 배타적 소프트웨어 판매 회사 모두 Git Flow를 채택한 이유 정리하기.
  - Git Flow가 나온 배경을 이해해야 더 나은 사용 방법을 정할 수 있다.
- Linear Flow
  - 프로덕션 환경에서 유지보수 하는 버전이 1개인 소프트웨어의 버전관리에 적합한 vcs 사용방식.
    - 자사 서비스 등.
    - A/B 테스트, 롤링 업데이트는 제외.
  - 참고 : [A tidy, linear Git history](https://www.bitsnbites.eu/a-tidy-linear-git-history/)
  - `git merge` 대신에 `git rebase`를 사용해서 분기, 교차가 없는 커밋 히스토리를 만드는 git 사용방식.
  - 장점
    - 이력 조사가 쉽다.
    - 각 커밋의 변경사항이 최소화된다.
  - 단점
    - 컨플릭트 발생 횟수가 늘어날 수도 있다.
      - 특히 부실한 사양, 개발자의 이해 부족, 설계 오류인 경우.
      - PoC 성격의 변경이 잦은 경우.
    - 작업 브랜치를 여러 개발자가 공유하면 비용이 늘어난다.
      - 공유 브랜치의 리베이스 발생시.
    - 작업 브랜치에서 공통 코드에 변경이 많을 경우 비용이 늘어난다.
  - 단점 완화
    - `git full` 대신 `git fetch` 기반으로 작업한다.
    - 수시로 `git fetch` 실행.
    - 공통 코드의 변경은 별도의 작업으로 분리한다.
    - 저장소 분리 등으로 소규모 팀에서 관리할 수 있는 크기로 유지한다.
- 머지 커밋의 기능 : 히스토리를 보는 사람은 머지 커밋에서 어떤 정보를 찾을 수 있는가?
  - Git Flow
    - 언제 어떤 변경이 생겼는가.
    - 변경이 어느 브랜치에서 어느 브랜치로 전달 되었는가.
  - Linear Flow
    - 각 릴리즈 사이의 변경.
    - 관련 커밋 묶음.
    - 릴리즈 가능한 시점.

## 참고

- [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)
- [A tidy, linear Git history](https://www.bitsnbites.eu/a-tidy-linear-git-history/)
- [Git - History](https://en.wikipedia.org/wiki/Git#History)
  - [Git - 역사](https://ko.wikipedia.org/wiki/%EA%B9%83_(%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4)#%EC%97%AD%EC%82%AC)