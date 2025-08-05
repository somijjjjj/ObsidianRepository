

## 1. 문제 요약

- **현상**
    
    - 브라우저에서 Ctrl+휠(줌) 또는 창 크기 조정 시 `append`/`prepend` 콜백이 의도치 않게 계속 호출된다.
        
    - 모듈 파괴 시 `destroy()` 호출로 인해 아래와 같은 예외가 발생한다.
        

## 2. 발생 로그 (Chrome DevTools)

```
infinitegrid.pk.gd.min.js:9 Uncaught TypeError: Cannot read properties of null (reading 'removeChild')
    at t.destroy (infinitegrid.pk.gd.min.js:9:15480)
    at e.destroy (infinitegrid.pk.gd.min.js:9:36326)
    at eval (eval at fn_getCategoryResultItems (<anonymous>:1:1), <anonymous>:1:22)
    …  
```

## 3. 원인 분석

1. **이벤트 핸들러 미해제**  
    `.on('append', …)` 으로 바인딩한 콜백이 모듈 언마운트나 재초기화 시 `off()` 없이 남아 있어, 줌/리사이즈 시에도 계속 실행됨.
    
2. **`destroy()` 이중 호출**  
    이미 한 번 `destroy()`로 DOM 컨테이너가 제거된 상태에서 다시 `destroy()`를 호출 → 내부 참조가 `null`이 되어 `removeChild` 실패.
    
3. **잘못된 호출 타이밍**  
    `append` 이벤트 루프 안이거나, 컨테이너 엘리먼트가 제거된 뒤 `destroy()`를 호출했기 때문에 내부 상태가 꼬임.
    

## 4. 해결 방법

1. **모듈 패턴에 `init()` / `destroy()` 분리**
    
    - `init()` 내부에서 기존 인스턴스가 있으면 `destroy()` 호출
        
    - `destroy()` 안에서 `ig = null` 처리로 이중 호출 방지
        
2. **`destroy()` 호출 가드 추가**
    
    ```js
    destroy() {
      if (!ig) return;   // 이미 destroy 됐거나 인스턴스 미생성 시 무시
      ig.destroy();      // 이벤트 바인딩 해제 및 내부 DOM 정리
      ig = null;
    }
    ```
    
3. **정확한 호출 시점 지정**
    
    - 페이지 언로드(`beforeunload`)나 탭 전환 등 “그리드를 더 이상 안 쓸 때”만 `destroy()` 실행
        
    - 절대 `append`/`prepend` 핸들러 내부에서 `destroy()` 호출 금지
        
4. **이벤트만 해제하고 싶다면 `off()` 사용**
    
    ```js
    ig.off('append', appendHandler);
    // 또는 모든 이벤트 해제
    ig.off();
    ```
    

## 5. 코드 예제

```js
const MyGridModule = (function(){
  let ig;

  function appendHandler(e) {
    // 기존 append 로직...
  }

  function prependHandler(e) {
    // 기존 prepend 로직...
  }

  return {
    init(selector) {
      // 이전 인스턴스가 있으면 파괴
      if (ig) this.destroy();

      ig = new eg.InfiniteGrid(selector, {
        count: igPerPage,
        defaultGroupKey: 0,
        isOverflowScroll: true,
      });
      ig.on('append', appendHandler)
        .on('prepend', prependHandler);
    },

    destroy() {
      if (!ig) return;   // 이미 파괴된 경우 무시
      ig.destroy();      // 이벤트 + DOM 내부 참조 모두 정리
      ig = null;
    }
  };
})();

// 페이지 언로드 시 그리드 정리
window.addEventListener('beforeunload', () => {
  MyGridModule.destroy();
});

// 모듈 사용 예
MyGridModule.init('#div_detailDataList');
```

---

이 구조를 적용하면

- **이벤트가 잔존하지 않고**
    
- **`destroy()` 호출 시 안전하게 내부 리소스를 정리**  
    할 수 있어 더 이상 `removeChild` 에러가 발생하지 않습니다.