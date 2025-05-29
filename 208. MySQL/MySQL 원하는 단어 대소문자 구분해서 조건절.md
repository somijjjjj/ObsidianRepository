`BINARY` 연산자는 MySQL에서 문자열 비교 시 대소문자를 구분하도록 강제하는 기능을 제공합니다. 이를 통해 대소문자를 정확히 일치시킬 수 있습니다.

### 원리

1. **문자열 비교**: 일반적으로 MySQL에서 문자열을 비교할 때 사용하는 `COLLATE` 설정에 따라 대소문자를 구분하지 않을 수 있습니다. 예를 들어, 기본 `utf8_general_ci` collation은 대소문자를 구분하지 않으므로 `RISK`와 `risk`는 동일하다고 판단합니다.

2. **`BINARY` 연산자 사용**: `BINARY` 연산자를 사용하면 문자열을 이진 형식으로 비교합니다. 이진 비교는 문자열의 각 문자의 바이트 값을 직접 비교하기 때문에, 대소문자 구분이 정확하게 이루어집니다. 즉, 'R'과 'r'을 다른 문자로 취급합니다.

   ```sql
   SELECT 'RISK' COLLATE utf8_general_ci; -- 'RISK'와 'risk'를 동일하게 취급
   SELECT BINARY 'RISK'; -- 'RISK'와 'risk'를 다른 문자로 취급
   ```

### 예시
```sql
SELECT COUNT(1)
FROM TB_RULE_DICTIONARY_KEYWORD_INFO
WHERE DICTIONARY_SEQ = '41594'
  AND BINARY KEYWORD = 'RISK'
  AND REGEX_YN = 'N';
```
이 쿼리에서는 `BINARY` 연산자를 사용하여 `KEYWORD` 컬럼의 값을 `'RISK'`와 대소문자를 구분하여 비교합니다. 따라서 `KEYWORD`가 `'RISK'`와 정확히 일치해야만 결과에 포함됩니다.

이 원리를 통해 대소문자를 구분하여 문자열을 비교할 수 있습니다.