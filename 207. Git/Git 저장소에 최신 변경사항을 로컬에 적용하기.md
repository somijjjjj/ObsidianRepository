
```
# 1. 원격 최신 커밋 가져오기
git fetch origin

# 2. 현재 브랜치에서 main의 최신 변경사항 반영
git merge origin/main
# 또는
git rebase origin/main

```


```
git stash       # 변경사항 임시 저장
git rebase origin/main
git stash pop   # 다시 되돌림

```