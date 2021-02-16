## Problem

- 특정한 이유로 Commit history를 전부 삭제하고 싶다.
- 물론 새 Repo를 만드는 것도 방법이지만, 현재의 Repo 그대로 사용하고 싶다면?

## Solution

1. 새 브랜치(orphan)를 딴다.
   - `git checkout --orphan latest_branch`
   - [orphan 브랜치란?](https://velog.io/@yjok/Git-Orphan-Branch-%EC%83%9D%EC%84%B1)
2. 모든 파일들을 Staging한다.
   - `git add -A`
   - **참고**: `git add -A`는 작업 디렉토리 상 어디에 있든 항상 동일하게 모든 변경 내용을 Staging하고, `git add .`는 명령어를 실행한 디렉토리 하위에서 발생한 변경만을 포함한다. 즉, 프로젝트 최상위 디렉토리에서 `git add .`를 실행하면 `git add -A`과 동일한 셈이다.
3. Commit한다.
   - `git commit -am "initialize commit history"`
4. 기존의 master 브랜치를 삭제한다.
   - `git branch -D master`
5. 현재 브랜치(latest_branch) 이름을 master로 바꾼다.
   - `git branch -m master`
6. Remote repository에 강제로 덮어씌운다.
   - `git push -f origin master`

### 참고 자료
[https://stackoverflow.com/questions/13716658/how-to-delete-all-commit-history-in-github](https://stackoverflow.com/questions/13716658/how-to-delete-all-commit-history-in-github)