 **트랜잭션 롤백과 비동기 처리의 충돌 문제**로 발생 사례

---

### 🧨 문제 요약

- 트랜잭션 2에서 예외가 발생하면 `@Transactional`에 의해 전체 트랜잭션이 롤백됩니다.
    
- `catch` 블럭에서 상태값을 `E`로 설정해도, `finally`에서 `updateStatus('E')`를 호출해도 **롤백되기 때문에 DB에 반영되지 않음**.
    
- 그래서 최종적으로 상태값이 여전히 `I`(진행중) 상태로 남아버리는 거예요.
    

---

### ✅ 해결 전략

#### ✅ 방법 1. **예외를 잡고 롤백되지 않도록 처리**

`@Transactional` 메서드 안에서 예외를 던지지 않도록 설계하여 롤백이 발생하지 않게 함

```java
@Transactional
public void generateAiDictionary(...) {
    try {
        // LLM 요청 + 응답 처리
        // ...
    } catch (Exception e) {
        log.error("AI 생성 중 오류", e);
        updateStatus(dictionarySeq, "E"); // 명시적으로 오류 상태 업데이트
        return; // 예외 던지지 않음
    }
}
```

- **장점**: 예외를 잡고 롤백을 방지해 `E` 상태가 DB에 남음
    
- **단점**: 오류 발생 상황을 상위에서 감지하려면 로깅 또는 상태값 조회에 의존해야 함
    

---

#### ✅ 방법 2. **별도 트랜잭션으로 상태값만 업데이트** (`REQUIRES_NEW`)

`@Transactional(propagation = Propagation.REQUIRES_NEW)`을 이용해 상태값 업데이트만 별도의 트랜잭션으로 실행 → 롤백 영향 안 받음

```java
@Service
public class DictionaryStatusService {
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void updateStatus(String dictionarySeq, String status) {
        dictionaryRepository.updateStatus(dictionarySeq, status);
    }
}
```

그리고 `finally` 블럭 안에서 이 서비스를 호출:

```java
@Autowired
private DictionaryStatusService statusService;

...

finally {
    try {
        statusService.updateStatus(dictionarySeq, "E");
    } catch (Exception ex) {
        log.error("상태 업데이트 실패", ex);
    }
}
```

- **장점**: 메인 트랜잭션이 롤백돼도 상태값은 정상 반영됨
    
- **권장**: 이 방식이 가장 안정적이고 실무에서 자주 사용됨
    

---

#### ✅ 방법 3. 상태값 업데이트를 `@Async`가 아닌 별도 스케줄러로 분리 (비추천)

비동기 요청이 실패하면 상태를 직접 바꾸지 않고, **스케줄러나 워커가 실패 상태를 감지해서 변경**하는 방식도 있지만, 오버엔지니어링일 수 있어요.

---

### 🎯 결론

> **`REQUIRES_NEW`로 상태값 업데이트 분리하는 방식이 가장 실용적이며 안전합니다.**

이 방식은 서비스 내에 `statusService.updateStatus(...)`만 잘 분리하면 되니, 기존 구조에 큰 영향 없이 도입할 수 있어요.

필요하시면 이 구조를 코드 예제 포함해서 더 정리해드릴게요!