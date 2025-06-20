

## 현상
- 동일한 부모 요소 내에서 아래에서 위로는 드래그가 잘 되는데,  
- 맨 위에 있는 아이템을 아래로 드래그할 때 placeholder 영역이 잡히지 않아 드래그가 잘 안됨

---

## 원인 추정
- jQuery UI Sortable의 위치 계산 및 placeholder 영역 감지 문제
- 기본 `tolerance` 옵션의 작동 방식에 의한 감지 실패
- placeholder 크기 미설정 또는 DOM 위치 문제
- CSS 스타일 차이 및 margin/padding 영향
- helper, appendTo, scroll 옵션과의 상호작용 문제

---

## 점검 및 해결 방안

### 1. `tolerance` 옵션 변경
- 기본값 `intersect` → 마우스 포인터 기준 감지 `pointer`로 변경
```js
$("#sortable").sortable({
  tolerance: "pointer"
});
````

### 2. `forcePlaceholderSize` 옵션 추가

- placeholder 크기를 드래그 중인 아이템 크기와 강제로 일치시켜 영역 확보
    

```js
$("#sortable").sortable({
  forcePlaceholderSize: true
});
```

### 3. `helper` 옵션 확인

- 기본값 `original` 유지 또는 필요시 `clone` 사용 여부 점검
    

```js
$("#sortable").sortable({
  helper: "original" // 또는 "clone"
});
```

### 4. CSS 스타일 통일

- 아이템 간 margin, padding, border 등 스타일이 동일한지 확인 및 조정
    
- placeholder 스타일 명시적으로 지정
    

```css
.ui-sortable-placeholder {
  visibility: visible !important;
  display: block !important;
  height: 40px; /* 아이템 높이와 동일하게 */
  background: #f0f0f0;
  border: 1px dashed #ccc;
}
```

### 5. `appendTo` 옵션 설정

- placeholder가 부모 컨테이너 밖으로 계산되는 경우 방지
    

```js
$("#sortable").sortable({
  appendTo: "parent" // 또는 "body"
});
```

### 6. `scroll` 옵션 점검

- 스크롤 위치에 따른 위치 계산 오류 방지용
    

```js
$("#sortable").sortable({
  scroll: false
});
```

### 7. 이벤트 로그로 상태 확인

- 드래그 시작, 위치 변경, 업데이트 시점에 인덱스와 위치 콘솔 출력하여 원인 파악
    

```js
$("#sortable").sortable({
  start: function(event, ui) {
    console.log("start index:", ui.item.index());
  },
  change: function(event, ui) {
    console.log("placeholder index:", ui.placeholder.index());
  },
  update: function(event, ui) {
    console.log("updated index:", ui.item.index());
  }
});
```




