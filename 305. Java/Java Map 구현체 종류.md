

좋은 질문입니다. `categoryNameInfo.entrySet()`을 사용할 때 **put한 순서대로 key가 나오느냐**는 `categoryNameInfo`의 **구체적인 Map 구현체가 무엇이냐에 따라 달라집니다.**

---

### 🔍 각각의 Map 구현체 정리

| Map 타입                  | 순서 보장 여부        | 설명                                              |
| ----------------------- | --------------- | ----------------------------------------------- |
| `HashMap`               | ❌ 순서 보장 안됨      | 내부적으로 해시 버킷 기반이므로 put 순서와 무관하게 key 순서가 뒤섞일 수 있음 |
| `LinkedHashMap`         | ✅ 순서 보장됨        | put한 순서대로 key/value가 유지됨                        |
| `TreeMap`               | ❌ 알파벳/숫자 정렬 순서  | 키의 자연 순서(Comparable)나 Comparator 기준으로 정렬        |
| `ConcurrentHashMap`     | ❌ 순서 보장 안됨      | 병렬 처리 중심으로 설계되어 순서 없음                           |
| `Map.of(...)` (Java 9+) | ❌ 순서 보장 없음 (불변) | immutable map으로, 순서에 의존하지 말라는 명시 있음             |

---

### ✅ 결론

- 만약 다음과 같이 선언했다면:
    
    ```java
    Map<String, String> categoryNameInfo = new LinkedHashMap<>();
    categoryNameInfo.put("A", "Alpha");
    categoryNameInfo.put("B", "Beta");
    categoryNameInfo.put("C", "Charlie");
    ```
    
    → `categoryNameInfo.entrySet()`을 순회하면 A → B → C 순서로 나옵니다.
    
- 하지만 `HashMap`이면 순서는 **예측 불가능**합니다.
    

---

### ➕ 팁

순서를 **보장해야 하는 경우**라면 **반드시 `LinkedHashMap`을 사용**하세요.

---

사용 중인 코드에서 `categoryNameInfo`가 어떤 타입으로 선언되어 있나요? 확인해드릴 수 있어요.