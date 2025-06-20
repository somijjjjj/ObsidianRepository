

이 에러는 Eclipse에서 Subversive SVN 플러그인 (`org.eclipse.team.svn.ui` 버전 4.8.0)이 내부적으로 사용하는 클래스 `org.eclipse.compare.internal.TabFolderLayout`를 찾지 못해서 발생하는 `NoClassDefFoundError`이다.


---

### 문제 상황

- Eclipse 2025-03 버전 새 설치 후
- `Windows -> Preferences -> Version Control -> SVN` 선택 시 오류 발생

```
org/eclipse/compare/internal/tabfolderlayout error
```

https://i.sstatic.net/AdjrHH8J.png
https://stackoverflow.com/questions/79351203/svn-eclipse-plugin-subversive-not-working-when-i-have-upgraded-to-eclipse-2024

---

### 해결 방법

1. 아래 업데이트 저장소 URL을 Eclipse에 추가 (링크가 아닌 텍스트 복사)
    

```
https://download.eclipse.org/technology/subversive//updates/release/latest
```

2. 이전에 설치된 Subversive 플러그인을 삭제 후, 위 저장소에서 최신 버전의 플러그인을 다시 설치
    
3. Eclipse 재시작 후, 새로 설치한 플러그인의 연결 커넥터(Connectors) 설치 제안이 뜨면 반드시 설치
    
4. 커넥터 설치 완료 후 SVN 저장소에 정상 연결 가능
    

---

### 요점

- Subversive 플러그인과 커넥터가 Eclipse 2025-03 버전에 맞게 최신 버전이어야 정상 작동
    
- 직접 최신 업데이트 저장소를 추가하고 재설치하면 문제 해결됨
    

---



---

### 원인 분석

- `org.eclipse.compare.internal.TabFolderLayout` 클래스는 Eclipse 내부 패키지(비공개/internal API)이기 때문에, Eclipse 버전 간에 해당 클래스가 없거나 위치가 바뀌었을 수 있습니다.
    
- 즉, 설치한 Subversive SVN 플러그인 버전이 현재 사용하는 Eclipse 버전과 호환되지 않아 필요한 내부 클래스를 찾지 못하는 상황입니다.
    
- 보통 이런 문제는 플러그인 버전과 Eclipse 버전이 맞지 않을 때 발생합니다.
    

---

### 해결 방법

1. **Eclipse와 Subversive 플러그인 버전 호환성 확인**
    
    - Eclipse 2025-03(또는 최신 버전)과 호환되는 Subversive 최신 버전인지 확인
        
    - 구버전 Subversive 플러그인을 설치했다면 최신 버전으로 업데이트 권장
        
2. **Subversive 플러그인 재설치**
    
    - 기존 플러그인 삭제 후, 공식 업데이트 사이트에서 최신 버전 재설치
        
    - 업데이트 사이트 URL 예:
        
        ```
        https://download.eclipse.org/technology/subversive/updates/release/latest
        ```
        
3. **Eclipse 최신 버전 사용 권장**
    
    - 가능하면 Eclipse IDE도 최신 버전으로 업데이트
        
4. **Eclipse 설치 상태 점검**
    
    - Eclipse 설치가 제대로 되지 않아 일부 필수 번들이 누락됐을 가능성도 있음
        
    - `Help > About Eclipse IDE > Installation Details`에서 설치된 플러그인 확인
        

---

요약:  
**Subversive SVN 플러그인이 현재 Eclipse 내부 API와 맞지 않는 버전을 사용 중이며, 이로 인해 내부 클래스가 없다는 오류가 발생한 것입니다. 플러그인과 Eclipse 버전을 맞추고 최신 상태로 유지해야 해결됩니다.**