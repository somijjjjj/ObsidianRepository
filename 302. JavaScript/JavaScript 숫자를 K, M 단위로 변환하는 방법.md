---
tags:
  - status/done
  - publish/done
tistory: https://jn-silveryoung.tistory.com/60
---

큰 숫자를 더 직관적으로 표시하려면, 1,000 단위로 "K", 1,000,000 단위로 "M"을 붙여 표시하는 방법이 유용합니다. 
예를 들어, `1500`을 `1.5K`, `1200000`을 `1.2M`처럼 변환할 수 있습니다.

이 글에서는 JavaScript로 숫자를 `K`와 `M` 단위로 변환하는 방법을 소개하고, 소수점 뒤에 `.0`을 제거하는 방법도 함께 다뤄 보겠습니다.

#### 1. 기본적인 K, M 단위 변환

먼저, 숫자가 `1,000` 이상이면 "K"를 붙이고, `1,000,000` 이상이면 "M"을 붙이는 기본적인 변환 방법을 구현해 보겠습니다.

```JavaScript
function formatNumber(number) {
    // 1,000,000 이상이면 "M" 단위로 변환
    if (number >= 1000000) {
        let formattedNumber = number / 1000000;
        // 소수점 첫째 자리까지 표시하되, ".0"을 제거
        return formattedNumber % 1 === 0 ? `${formattedNumber.toFixed(0)}M` : `${formattedNumber.toFixed(1)}M`;
    } 
    // 1,000 이상이면 "K" 단위로 변환
    else if (number >= 1000) {
        let formattedNumber = number / 1000;
        // 소수점 첫째 자리까지 표시하되, ".0"을 제거
        return formattedNumber % 1 === 0 ? `${formattedNumber.toFixed(0)}K` : `${formattedNumber.toFixed(1)}K`;
    } 
    // 1,000 미만은 그대로 표시
    else {
        return number.toString();
    }
}

// 예제 사용
let number1 = 500;        // 500
let number2 = 1500;       // 1.5K
let number3 = 1200000;    // 1.2M
let number4 = 1000000;    // 1M

console.log(formatNumber(number1));  // 500
console.log(formatNumber(number2));  // 1.5K
console.log(formatNumber(number3));  // 1.2M
console.log(formatNumber(number4));  // 1M

```



#### 2. 코드 설명

- **1,000,000 이상**: 숫자를 1,000,000으로 나누어 "M" 단위로 변환합니다.
    
- **1,000 이상**: 숫자를 1,000으로 나누어 "K" 단위로 변환합니다.
    
- **소수점 처리**: 숫자의 소수점 첫째 자리까지 표시하되, 소수점이 `.0`일 경우에는 이를 제거하고 정수로 표시합니다.
    
    - 예를 들어, `1000000`은 `1M`으로 표시되고, `1500`은 `1.5K`로 표시됩니다.      
        
    - 소수점이 0인 경우, 예를 들어 `1000`은 `1K`로 출력됩니다.
        

#### 3. 예시 출력

- **500** -> `500`    
- **1500** -> `1.5K`    
- **1200000** -> `1.2M`    
- **1000000** -> `1M`
    

#### 4. 왜 이 방법이 유용한가?

큰 숫자를 다룰 때, `K`나 `M` 단위로 변환하면 숫자가 훨씬 직관적으로 보입니다. 예를 들어, `1500`을 `1.5K`로 변환하면, 큰 수를 처리할 때 더 쉽게 이해할 수 있습니다. 이 방식은 UI에서 숫자를 간결하게 표시하는 데 유용합니다.

```
#JavaScript

#숫자포맷

#K단위

#M단위

#프로그래밍

#코딩팁

#자바스크립트팁

#자바스크립트기술
```


### 연관 정보

[[Java 숫자를 K, M 단위로 변환하는 방법]]