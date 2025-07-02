



- **SQL Fragment**란 `공통으로 사용하는 SQL 구문`(ex. SELECT 절, JOIN 절, WHERE 일부 등)을 `<sql>` 태그로 정의하고 `<include>` 태그로 재사용하는 방식이다.
    
- 중복 코드를 줄이고, 유지보수성을 높이기 위한 기능이다.
    

---

### ✅ 기본 문법

#### 1. SQL 조각 정의

```xml
<sql id="fragmentId">
  SQL 문장 일부
</sql>
```

#### 2. 조각 포함 (`include`)

```xml
<include refid="fragmentId" />
```

#### ✅ 예제

```xml
<!-- 공통 SELECT 컬럼 정의 -->
<sql id="basePostColumns">
  SELECT A.POST_SEQ, A.POST_TITLE, A.REG_DATE, A.REG_ID, U.NICKNAME
</sql>

<!-- 공통 FROM 절 정의 -->
<sql id="basePostFrom">
  FROM tb_post A JOIN tb_user U ON A.REG_ID = U.USER_ID
</sql>

<!-- 재사용 -->
<select id="selectPostList" resultType="PostDTO">
  <include refid="basePostColumns" />
  <include refid="basePostFrom" />
  WHERE A.POST_STATUS = 'N'
</select>
```

---

### ✅ 주요 특징 및 팁

|항목|설명|
|---|---|
|`id`|`<sql id="...">`로 고유 식별자 정의|
|`refid`|`<include refid="..."/>`로 조각 참조|
|중첩 가능|여러 `<sql>` 조각을 하나의 쿼리에 중첩 사용 가능|
|가독성 향상|`SELECT`, `FROM`, `WHERE`, `JOIN` 절 분리로 구조화 가능|
|유지보수|컬럼 변경 시 조각만 수정하면 전체 적용됨|

---

### ✅ 실전 사용 팁

- 💡 **SELECT 절**이나 **JOIN 절**처럼 자주 바뀌는 부분만 쪼개서 관리
    
- 💡 필드명이 긴 경우 SQL Fragment를 사용하면 매퍼 XML이 깔끔해짐
    
- 💡 복잡한 조건절은 필요 시 `<where>`와 함께 `<sql>`로 관리 가능
    
- 💡 `ORDER BY`, `LIMIT`은 동적으로 바뀌는 경우 `<choose>`로 처리
    

---

### ✅ 한계 및 주의사항

|항목|설명|
|---|---|
|매개변수 불가|`<sql>` 안에서는 파라미터 바인딩은 불가능 (동적 조건은 `<if>` 등에서 처리)|
|문법 오류 주의|SQL 조각에 세미콜론 `;`이나 누락된 공백 등 주의|
|네임스페이스|`refid`는 매퍼 네임스페이스 기준으로 풀네임 사용 가능 (`namespace.fragmentId`)|

