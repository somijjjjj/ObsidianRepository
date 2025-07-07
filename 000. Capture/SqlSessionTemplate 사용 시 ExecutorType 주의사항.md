

에러 메시지:

```
org.springframework.dao.TransientDataAccessResourceException: Cannot change the ExecutorType when there is an existing transaction
```

는 **MyBatis + Spring** 환경에서 다음과 같은 상황일 때 발생합니다:

---

### ✅ **원인 요약**

기존 트랜잭션이 **열려있는 상태**에서 `ExecutorType`이 다른 `SqlSession`을 새로 열려고 할 때 발생합니다.

예를 들어,

- 현재 트랜잭션은 `ExecutorType.SIMPLE`로 열려 있는데,
    
- 중간에 `ExecutorType.BATCH`로 `SqlSessionTemplate`을 열면 충돌이 발생합니다.
    

즉, **트랜잭션 범위 내에서 서로 다른 ExecutorType을 사용하려고 할 때** 이 예외가 발생합니다.

---

### 💡 주요 원인 상황 예시

1. 트랜잭션(@Transactional) 안에서 `batch SqlSession`을 수동으로 열려고 한 경우
    
2. `SqlSessionTemplate`을 직접 주입받아 사용하는데, 기본 `ExecutorType`과 다르게 명시한 경우
    

---

### 🛠 해결 방법

#### ✅ **방법 1: 같은 `ExecutorType` 사용 유지**

- 트랜잭션 안과 밖 모두 동일한 `ExecutorType` 사용 (`SIMPLE` 또는 `BATCH` 중 하나로 일관)
    

#### ✅ **방법 2: 트랜잭션 분리**

- `@Transactional(propagation = Propagation.REQUIRES_NEW)` 를 사용해 트랜잭션을 분리
    

```java
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void executeBatchInsert() {
    // 여기서 BATCH SqlSession 사용
}
```

#### ✅ **방법 3: `BatchSqlSessionTemplate`을 트랜잭션 외부에서 사용**

- 트랜잭션이 시작되기 전에 실행하거나,
    
- 별도 스레드에서 실행하여 트랜잭션 분리
    

---

### 📌 추가 팁

`SqlSessionTemplate`은 보통 다음과 같이 설정되어 있습니다:

```java
@Bean
public SqlSessionTemplate batchSqlSessionTemplate(SqlSessionFactory sqlSessionFactory) {
    return new SqlSessionTemplate(sqlSessionFactory, ExecutorType.BATCH);
}
```

이렇게 명시적으로 `ExecutorType`이 다르게 설정된 템플릿을 사용하는 경우, 트랜잭션 처리에 주의해야 합니다.

---

필요하다면 현재 사용 중인 코드 일부를 보여주시면 더 정확히 진단해드릴 수 있어요.