---
tags:
  - status/done
  - publish/done
tistory: https://jn-silveryoung.tistory.com/59
---

Java에서 큰 숫자(1천, 1백만 이상)를 `K` (천) 또는 `M` (백만) 단위로 변환하는 방법을 소개합니다. 이 변환은 수치를 읽기 쉽게 표현하고, 사용자에게 더 직관적인 정보를 제공하기 위해 유용합니다. 이 글에서는 `K`와 `M` 단위를 포함한 숫자 포맷을 구현하는 방법을 설명하고, 소수점 뒤의 `.0`을 제거하는 방법도 다룹니다.

#### 1. 기본적인 K, M 단위 변환
다음은 `K`와 `M` 단위로 숫자를 포맷하는 `setFormatNumber` 메소드입니다. 이 메소드는 천 단위 이상의 숫자를 `K` 단위로, 백만 단위 이상의 숫자를 `M` 단위로 변환합니다.

```java
/** 
     * K, M 단위 포맷  
     * @param 
     * @return resultMap Map<String, Object>
     * @throws NullPointerException, Exception
     */
    public String setFormatNumber(int number) {
        if (number >= 1000000) {
            return String.format("%.1fM", Math.floor((number / 1000000.0) * 10) / 10);
        } else if (number >= 1000) {
            return String.format("%.1fK", Math.floor((number / 1000.0) * 10) / 10);
        } else {
            return Integer.toString(number);
        }
    }
```

이 메소드는 다음과 같이 동작합니다

- **1,000,000 이상**: 숫자는 `M` 단위로 변환됩니다. 예: `1,500,000` -> `1.5M`
- **1,000 이상**: 숫자는 `K` 단위로 변환됩니다. 예: `1,500` -> `1.5K`
- 그 외의 숫자는 그대로 반환됩니다. 예: `500` -> `500`

### 2. 소수점 `.0` 제거하기

위 코드는 `String.format`을 사용하여 소수점 첫째 자리까지 표시합니다. 그러나 `10.0M` 또는 `1.0K`와 같은 출력에서 `.0`을 제거하고 싶다면, `replaceAll`을 사용하여 `.0`을 없앨 수 있습니다. 

아래 코드는 이를 개선한 버전입니다.

```java
public static String formatCount(long count) {  
    String sign = count < 0 ? "-" : "";  
    long absCount = Math.abs(count);  
  
    // 소수점 첫째자리 유지하며 단위 표시, 나머지는 버림  
    if (absCount >= 1_000_000) {  
        double v = absCount / 1_000_000.0;  
        return sign + ((v % 1 == 0.0) ? (int) v + "M" : String.format("%.1fM", Math.floor(v * 10) / 10).replaceAll("\\.0", ""));  
    } else if (absCount >= 1_000) {  
        double v = absCount / 1_000.0;  
        return sign + ((v % 1 == 0.0) ? (int) v + "K" : String.format("%.1fK", Math.floor(v * 10) / 10).replaceAll("\\.0", ""));  
    } else {  
        return sign + absCount;  
    }  
}
```

이 코드에서 중요한 점은 `replaceAll("\\.0", "")`을 사용하여 `.0`을 제거한 것입니다. 

예를 들어,
- `10.0M`은 `10M`으로 변경됩니다.
- `3.9K`는 그대로 `3.9K`로 유지됩니다.

#### 3. 예시

몇 가지 예시를 통해 어떻게 출력되는지 확인해 보겠습니다.

- `1500` -> `1.5K`
- `3957` -> `3.9K`
- `1000000` -> `1M`
- `500` -> `500`
- `10001` -> `10K`

이와 같이 큰 숫자를 K 또는 M 단위로 변환하여 표시할 수 있습니다. 사용자 인터페이스에서 대규모 데이터나 숫자를 보다 직관적으로 표시할 수 있어 유용합니다.

### 연관 링크
- [[JavaScript 숫자를 K, M 단위로 변환하는 방법]]


```
#Java 
#숫자포맷
#K단위
#M단위
#Java기술
#소수점제거
#숫자변환
#프로그래밍
#코딩팁
#자바팁
#유용한코드
```