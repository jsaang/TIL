## Problem
- 마지막 커밋 이후 사소한 수정사항이 생겼다. 덮어씌우고 싶다.
- 커밋 메시지에 오타가 들어갔다. 거슬려 죽겠다.

## Solution
### 1. 커밋 메시지 수정하기
```
git commit --amend -m "new commit message"
```

### 2. 커밋 내용 수정하기
```
git add . // 변경 사항 수정 후 stage에 올리기
git commit --amend
git push --force // 이미 remote에 push한 경우
```