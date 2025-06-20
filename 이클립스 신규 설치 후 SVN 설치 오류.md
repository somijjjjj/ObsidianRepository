

요약 및 정리:

---

### 문제 상황

- Eclipse 2025-03 버전 새 설치 후
- `Windows -> Preferences -> Version Control -> SVN` 선택 시 오류 발생



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

필요하면 설치 과정이나 세부 방법도 알려줄게요!