응 정확해! 순서를 딱 정리하면 아래처럼 하면 돼 👇

---

## ✅ 단계별 요약

1. **기존 저장소 클론**
    

```bash
git clone https://github.com/yourname/old-repo.git
cd old-repo
```

2. **기존 remote 확인 및 삭제 (선택사항)**
    

```bash
git remote -v
git remote remove origin
```

3. **Organization 레포지토리를 remote로 등록**
    

```bash
git remote add origin https://github.com/your-org/new-repo.git
```

4. **코드 푸시**
    

```bash
git push -u origin main
```


> `main` 대신 `master`일 수도 있으니 `git branch`로 브랜치 이름 확인 먼저 해줘.

5. ✅ 끝!
    

---

### 💡 팁

- 새 레포지토리에 기본 파일들(Readme, .gitignore 등)이 있다면 `--force` 붙여서 푸시해야 돼:
    

```bash
git push -u origin main --force
```

- GitHub Organization에서 **새 레포지토리 생성 후 빈 상태로 두는 것** (README 없이) 이 제일 깔끔해.
    

---

필요하면 `.git` 디렉토리 제외하고 소스코드만 복사해서 새로운 Git 시작하는 방법도 알려줄 수 있어.  
그런데 기존 커밋 히스토리까지 옮길 거면 지금 방식이 제일 좋아.

추가적으로 옮길 때 `branch`, `tag`, `release`, `issue` 등 옮겨야 할 것도 있으면 말해줘!