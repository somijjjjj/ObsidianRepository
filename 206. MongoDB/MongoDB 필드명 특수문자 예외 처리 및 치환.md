

## 1. 문제 상황

MongoDB에 데이터를 적재할 때, 도큐먼트의 필드명(key)에 다음과 같은 문자가 포함되면 저장 오류가 발생합니다.

- 점(`.`)
- 달러 기호(`$`)
- null 문자(`\0`)
- 백슬래시(`\`)
- 큰따옴표(`"`)

예외 메시지 예시:

```java
org.springframework.data.mapping.MappingException: Map key 1. OR contains dots but no replacement was configured! Make sure map keys don't contain dots in the first place or configure an appropriate replacement!
```

또는

```json
"errmsg": "Document can't have $ prefixed field names: $CAGDATE"
```

MongoDB 내부적으로 `$`로 시작하는 필드명은 예약어이고, `.`은 도큐먼트 경로 탐색에 사용되는 특수 문자이기 때문입니다.

---

## 2. 발생 원인

- 데이터 소스(엑셀, CSV 등)에서 컬럼명이 그대로 key로 매핑되는 경우
- 규칙 컬럼명이 점(`.`), 달러(`$`) 등의 특수 문자를 포함
- 데이터 후처리나 매핑 과정에서 치환 작업 미흡
    

---

## 3. 해결 방법

### 1) 필드명(key)에서 예외 문자 치환 처리

MongoDB에 적재 전에 key명에 포함된 특수문자를 안전한 문자(예: 언더스코어 `_`)로 치환합니다.

예를 들어, Java 코드에서는 아래와 같이 처리했습니다.

```java
String filedName = StringUtils.nvl(headerList.get(i));
filedName = filedName.replaceAll("[\\\\$.\\\"]", "_"); // MongoDB 필드명 예외 문자 치환
```

- 정규식 `[\\\\$.\\\"]` 는 백슬래시(`\`), 달러(`$`), 점(`.`), 큰따옴표(`"`)를 찾아서 `_`로 대체합니다.
    
- 필요에 따라 null 문자(`\0`)도 추가해서 치환할 수 있습니다.
    

---

### 2) 컬렉션별 예외 처리 범위

- **수정 불필요:**  
    정답지, 형태소, 분석결과 등은 이미 정형화된 필드명을 사용하므로 치환 불필요
    
- **수정 필요:**  
    데이터 컬렉션과 후처리 결과 컬렉션은 규칙 컬럼명이 key로 매핑되어 예외 문자가 포함될 수 있으므로 치환 처리 필요
    

---

### 3) 처리 시점

- 후처리 규칙 파일 생성 시점에 문자를 치환하도록 구현하여 데이터 적재 이전에 필드명을 정제함으로써 오류 방지
    

---

## 4. 실무 적용 예시

- `postfixMngServiceImpl.insertPostfixResult()` 메서드 내에서 필드명 치환 로직 적용
    
- 배치나 API에서 데이터를 MongoDB에 적재할 때 반드시 key값을 검증 및 치환 후 삽입
    
- 예외 발생 시 즉시 원인을 파악할 수 있도록 로깅 추가 권장
    

---

## 5. 참고 사항

|문자|MongoDB 도큐먼트 필드명 제한 이유|
|---|---|
|`$`|예약어, 연산자 접두어로 사용|
|`.`|도큐먼트 내 하위 필드 접근 구분자 (dot notation)|
|`\0`|널 문자, 문자열 종료 신호|
|`\`|이스케이프 문자|
|`"`|문자열 구분용 큰따옴표|

---

## 6. 마무리

MongoDB에 데이터를 적재할 때는 반드시 필드명(key)으로 사용할 문자열에 대해 특수문자 검증 및 치환 작업을 선행해야 합니다. 그렇지 않으면 아래와 같은 오류가 발생하고 데이터 적재에 실패할 수 있습니다.

- `Map key ... contains dots but no replacement was configured`
    
- `Document can't have $ prefixed field names`
    

2023년 12월 28일에 버그를 수정하며 위 방식으로 필드명 치환 로직을 반영했고, 후처리 규칙 생성 시점에서 처리하도록 개선하여 안정적인 데이터 적재가 가능해졌습니다.

필드명 예외 처리에 대해 더 궁금한 점 있으면 언제든 문의 주세요!

---

필요하면 이 내용을 기반으로 사내 위키나 개발 블로그에 올려서 팀원들과 공유하기 좋을 것입니다.  
추가로 코드 예시나 테스트 케이스도 정리해 드릴 수 있으니 말씀해 주세요!

---

필요한 부분 있으면 편하게 말씀해 주세요.