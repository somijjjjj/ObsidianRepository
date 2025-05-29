현재 `git pull`을 시도했지만, `yarn.lock` 파일에 로컬 변경 사항이 있어서 덮어쓰기가 불가능한 상태입니다.

### **강제로 `pull` 받는 방법**

#### **(1) 로컬 변경 사항을 버리고 강제 `pull`**

로컬 변경 사항을 유지할 필요가 없다면 아래 명령어를 실행하세요.

```bash
git reset --hard HEAD
git pull origin feature/signup-improvement
```

💡 **주의:** `git reset --hard HEAD`를 실행하면 **변경 사항이 모두 삭제**되므로, 필요하면 먼저 백업하세요.

---

#### **(2) 변경 사항을 임시로 저장 (`stash`) 후 `pull`**

로컬 변경 사항을 유지하고 싶다면 **`stash`를 사용**하면 됩니다.

```bash
git stash
git pull origin feature/signup-improvement
git stash pop  # 원래 상태로 되돌리기 (선택 사항)
```

이렇게 하면 `yarn.lock` 파일의 변경 사항을 임시 저장한 후 `pull`을 수행하고, 이후 필요하면 변경 사항을 다시 적용할 수 있습니다.

---

#### **(3) 특정 파일만 변경 사항 버리기**

`yarn.lock` 파일만 원래 상태로 되돌리고 싶다면:

```bash
git checkout -- yarn.lock
git pull origin feature/signup-improvement
```

이 방법은 `yarn.lock` 파일만 초기 상태로 복원하고, 다른 변경 사항은 유지합니다.

**필요에 따라 위 방법 중 하나를 선택해서 진행하세요!** 🚀