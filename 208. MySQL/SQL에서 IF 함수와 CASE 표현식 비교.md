  


  

SQL에서 `IF()` 함수와 `CASE` 표현식은 모두 조건에 따라 값을 반환하지만, **표준성, 가독성, 이식성**에서 중요한 차이가 있습니다.

  

---

  

## 1. 가독성 (Readable Code)

  

### IF 함수

- MySQL 등 일부 DBMS에서만 지원

- 단순한 조건일 때는 간단하지만, 조건이 여러 단계가 되면 중첩으로 인해 복잡해집니다.

  

```sql

IF(score >= 90, 'A',
   IF(score >= 80, 'B',
      IF(score >= 70, 'C', 'F')))

```

  

### CASE 표현식

- 조건과 결과를 명확하게 나열 가능

- 가독성과 유지보수성이 뛰어남

  

```sql

CASE
  WHEN score >= 90 THEN 'A'
  WHEN score >= 80 THEN 'B'
  WHEN score >= 70 THEN 'C'
  ELSE 'F'
END

```

  

---

  

## 2. 표준화 (SQL 표준 여부)

  

- **CASE** : ANSI SQL 표준, 거의 모든 DBMS에서 동일하게 동작
- **IF** : ANSI 표준이 아님. MySQL, MariaDB 같은 일부 DBMS에서만 제공

  

---

  

## 3. 확장성 (복잡한 조건 처리)

  

- CASE: 여러 조건을 순차적으로 처리하기 유리
- IF: 중첩이 많아질수록 코드가 난잡해짐

  

### SIMPLE CASE 예제

```sql

CASE 컬럼
  WHEN 'A' THEN 100
  WHEN 'B' THEN 200
  ELSE 0
END

```

  

### SEARCHED CASE 예제

```sql

CASE
  WHEN age >= 65 THEN '노인'
  WHEN age >= 18 THEN '성인'
  ELSE '미성년자'
END

```

  

---

  

## 4. 성능 차이

  

- 간단한 조건일 때 성능 차이는 거의 없음

- 복잡한 로직에서는 CASE가 최적화에 유리

  

---

  

## 5. 사용 가이드

  

- 단순한 2분기:

```sql

IF(age >= 18, '성인', '미성년자')

```

  

- 복잡하거나 3개 이상의 조건:

```sql

CASE
  WHEN age >= 65 THEN '노인'
  WHEN age >= 18 THEN '성인'
  ELSE '미성년자'
END

```

  

---

  

## 비교 표

| 구분             | CASE 표현식                        | IF 함수                              |
| -------------- | ------------------------------- | ---------------------------------- |
| 표준 여부          | ANSI SQL 표준                     | MySQL 등 일부 DBMS 전용                 |
| 가독성            | 명확하고 직관적                        | 중첩되면 복잡해짐                          |
| 이식성            | 높음                              | 낮음                                 |
| 복잡한 조건 처리      | 유리                              | 중첩 IF로 처리 → 가독성 저하                 |
| 권장 사용처      | 협업, 복잡한 조건, 표준 필요 시 | 간단한 이진 조건 (단순 분기)        |


