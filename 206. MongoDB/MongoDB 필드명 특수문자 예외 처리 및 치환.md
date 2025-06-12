

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

- 사용자가 직접 입력한 명칭이 그대로 파일에 저장됩니다.
- 이 명칭에 점(`.`), 달러 기호(`$`) 같은 특수문자가 포함될 수 있습니다.
- 그런데 MongoDB는 필드명(키)에 이런 특수문자가 들어가면 저장할 수 없습니다.
- 그래서 특수문자가 포함된 필드명을 MongoDB에 저장하려 하면 오류가 발생합니다.

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



- `Map key ... contains dots but no replacement was configured`
    
- `Document can't have $ prefixed field names`
    

2023년 12월 28일에 버그를 수정하며 위 방식으로 필드명 치환 로직을 반영했고, 후처리 규칙 생성 시점에서 처리하도록 개선하여 안정적인 데이터 적재가 가능해졌습니다.

필드명 예외 처리에 대해 더 궁금한 점 있으면 언제든 문의 주세요!
