

**setInterval()** 함수는 JavaScript에서 특정 코드를 일정한 시간 간격으로 반복 실행하고 싶을 때 사용하는 내장 함수입니다[1](https://velog.io/@hyerin0930/JavaScript-setTimeout-setInterval)[3](https://developer.mozilla.org/ko/docs/Web/API/Window/setInterval)[6](https://hong42.tistory.com/140).

## 기본 사용법

- 첫 번째 인자: 반복해서 실행할 함수(또는 코드)
    
- 두 번째 인자: 반복 간격(밀리초, ms 단위)
    
- 반환값: interval ID(숫자), 추후 clearInterval로 중단할 때 사용
    

```javascript
// 2초마다 현재 시간을 콘솔에 출력 const intervalId = setInterval(() => {   console.log(new Date()); }, 2000);
```


이렇게 하면 2초마다 한 번씩 함수가 실행됩니다[1](https://velog.io/@hyerin0930/JavaScript-setTimeout-setInterval)[3](https://developer.mozilla.org/ko/docs/Web/API/Window/setInterval)[6](https://hong42.tistory.com/140).

## 반복 중단: clearInterval

setInterval로 시작한 반복 실행을 멈추고 싶을 때는 반환된 interval ID를 clearInterval에 전달하면 됩니다.

```javascript
clearInterval(intervalId);
```

이렇게 하면 해당 interval이 더 이상 실행되지 않습니다[1](https://velog.io/@hyerin0930/JavaScript-setTimeout-setInterval)[3](https://developer.mozilla.org/ko/docs/Web/API/Window/setInterval)[6](https://hong42.tistory.com/140).

## 함수 인자 전달

setInterval에 넘기는 함수가 인자를 받을 경우, 세 번째 인자부터 함수에 전달할 값을 지정할 수 있습니다.

```javascript
function greet(name) {
  console.log(`Hello, ${name}!`);
}

setInterval(greet, 1000, "Alice"); // 1초마다 "Hello, Alice!" 출력

```

또는 익명 함수(화살표 함수)를 사용해 직접 인자를 넘길 수도 있습니다[5](https://kyounghwan01.github.io/blog/JS/JSbasic/intervalFunction/)[6](https://hong42.tistory.com/140).


## 주의사항 및 특징

- setInterval은 지정한 간격마다 함수를 계속 호출합니다. 콜백 함수의 실행 시간이 간격보다 길면, 함수 실행이 겹칠 수 있습니다[3](https://developer.mozilla.org/ko/docs/Web/API/Window/setInterval).
    
- 반복 실행을 멈추지 않으면, 메모리 누수 등이 발생할 수 있으므로 SPA 등에서는 반드시 clearInterval로 정리하는 습관이 필요합니다[1](https://velog.io/@hyerin0930/JavaScript-setTimeout-setInterval).
    
- 브라우저에 따라 너무 짧은 간격(4ms 미만 등)은 자동으로 4ms로 보정될 수 있습니다[3](https://developer.mozilla.org/ko/docs/Web/API/Window/setInterval).
    
- setInterval은 함수 실행이 끝나기를 기다리지 않고, 지정된 시간마다 콜백을 호출하므로, 콜백 내에서 긴 작업이 있다면 주의해야 합니다[3](https://developer.mozilla.org/ko/docs/Web/API/Window/setInterval).
    

## 대표 예시

javascript

`let count = 0; const intervalId = setInterval(() => {   count++;  console.log(count);  if (count === 5) {    clearInterval(intervalId); // 5회 반복 후 중단  } }, 1000);`

이 코드는 1초마다 count를 증가시키고, 5가 되면 반복을 멈춥니다[1](https://velog.io/@hyerin0930/JavaScript-setTimeout-setInterval)[6](https://hong42.tistory.com/140).

**정리**:  
setInterval 함수는 일정한 시간 간격으로 코드를 반복 실행할 때 사용하며, 반환된 interval ID로 clearInterval을 호출해 반복을 중지할 수 있습니다. 반복 실행이 필요 없는 경우 setTimeout을 사용하세요[1](https://velog.io/@hyerin0930/JavaScript-setTimeout-setInterval)[3](https://developer.mozilla.org/ko/docs/Web/API/Window/setInterval)[6](https://hong42.tistory.com/140).

1. [https://velog.io/@hyerin0930/JavaScript-setTimeout-setInterval](https://velog.io/@hyerin0930/JavaScript-setTimeout-setInterval)
2. [https://www.daleseo.com/js-timer/](https://www.daleseo.com/js-timer/)
3. [https://developer.mozilla.org/ko/docs/Web/API/Window/setInterval](https://developer.mozilla.org/ko/docs/Web/API/Window/setInterval)
4. [https://ko.javascript.info/settimeout-setinterval](https://ko.javascript.info/settimeout-setinterval)
5. [https://kyounghwan01.github.io/blog/JS/JSbasic/intervalFunction/](https://kyounghwan01.github.io/blog/JS/JSbasic/intervalFunction/)
6. [https://hong42.tistory.com/140](https://hong42.tistory.com/140)
7. [https://hianna.tistory.com/695](https://hianna.tistory.com/695)
8. [https://devuna.tistory.com/48](https://devuna.tistory.com/48)
9. [https://velog.io/@dianestar/JavaScript-React-React%EC%97%90%EC%84%9C-setInterval%EC%9D%98-%ED%99%9C%EC%9A%A9](https://velog.io/@dianestar/JavaScript-React-React%EC%97%90%EC%84%9C-setInterval%EC%9D%98-%ED%99%9C%EC%9A%A9)
10. [https://batcave.tistory.com/53](https://batcave.tistory.com/53)