
GitHub 원격 저장소를 완전히 초기화하고 `main` 브랜치 기준으로 강제로 다시 푸시한 작업 로그 정리

---


### 1. 기존 Git 정보 제거

```bash
$ rm -rf .git
```

### 2. Git 저장소 재초기화

```bash
$ git init
```

### 3. 변경 사항 추가 및 최초 커밋

```bash
$ git add .
$ git commit -m "initial commit"
```

### 4. 사용자 정보 설정 (처음에 에러 발생 시)

```bash
$ git config --global user.email ""
$ git config --global user.name ""
```

### 5. 브랜치명 변경 (기본 `master` → `main`)

```bash
$ git branch -m master main
```

### 6. 원격 저장소 연결

```bash
$ git remote add origin https://github.com/{}/{}
```

### 7. 강제 푸시로 원격 저장소 덮어쓰기

```bash
$ git push -u --force origin main
```

### 8. 필요 시 기존 `master` 브랜치 삭제

```bash
$ git push origin --delete master
```

※ 이미 `master` 브랜치가 없는 경우 아래와 같은 에러가 발생할 수 있으나 무시해도 무방하다.

```
error: unable to delete 'master': remote ref does not exist
```

---


- `main` 브랜치 기준으로 깔끔하게 리셋 완료
![[Pasted image 20250526141546.png]]