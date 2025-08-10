


### useState


- **상태 값**을 선언하고, 변경할 수 있는 함수를 제공합니다.    
- 예: 버튼 클릭 시 값 변경, 입력창 값 저장 등

```tsx
import { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState(0); // count: 현재 값, setCount: 변경 함수
  return (
    <button onClick={() => setCount(count + 1)}>
      {count}
    </button>
  );
};

```

### useEffect

- 컴포넌트 **렌더링 이후** 또는 **특정 값이 변경될 때** 실행되는 코드(사이드 이펙트)를 작성.  
- API 호출, 이벤트 등록, 타이머 설정 등에 사용

```tsx
import { useEffect } from 'react';

useEffect(() => {
  console.log("컴포넌트가 처음 렌더링됨");
}, []); // 빈 배열이면 첫 렌더링 때만 실행

```
