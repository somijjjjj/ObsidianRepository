
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign



### 객체 병합
```JavaScript
const o1 = { a: 1 };
const o2 = { b: 2 };
const o3 = { c: 3 };

const obj = Object.assign(o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
console.log(o1); // { a: 1, b: 2, c: 3 }, 목표 객체 자체가 변경됨.
```

