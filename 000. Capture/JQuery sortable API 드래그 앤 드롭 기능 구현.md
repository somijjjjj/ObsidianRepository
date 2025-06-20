

Sortable 주로 드래그 앤 드롭으로 요소의 순서를 바꾸는 기능을 구현


- `items`: 정렬 가능한 요소 선택자. (예: `items: "> li"`)
- `connectWith`: 다른 정렬 가능한 목록과 연결하여 요소 이동을 가능하게 합니다. (예: `connectWith: ".connectedSortable"`)
- `handle`: 드래그 시작을 위한 핸들 요소 지정. (예: `handle: ".my-handle"`)
- `placeholder`: 드래그 중 표시될 자리 표시자 요소 설정. (예: `placeholder: "ui-state-highlight"`)
- `forcePlaceholderSize`: 자리 표시자의 크기를 고정할지 여부.
- `tolerance`: 드래그 시 요소 간의 허용 오차. (예: `tolerance: "pointer"`)
- `delay`: 드래그 시작까지의 지연 시간 (밀리초).
- `disabled`: 정렬 기능 활성화/비활성화.
- `containment`: 정렬 가능한 영역 제한. (예: `containment: "#sortable-container"`)
- `cursor`: 드래그 시 커서 모양 설정.
- `scroll`: 스크롤 가능 여부 설정.
- `opacity`: 드래그 중 요소 투명도 설정.
- `revert`: 드롭 후 원래 위치로 되돌아갈지 여부.
- `start`, `stop`, `sort`, `change`, `update`, `receive`, `remove`, `over`, `out`, `activate`, `deactivate`: 이벤트 콜백 함수.
    - `sort` : 드래그 중 아이템 위치가 변할 때마다 발생하는 이벤트



```javascript
$( "#sortable" ).sortable({
  items: "> li",
  connectWith: ".connectedSortable",
  handle: ".my-handle",
  placeholder: "ui-state-highlight",
  forcePlaceholderSize: true,
  tolerance: "pointer",
  delay: 150,
  disabled: false,
  containment: "#sortable-container",
  cursor: "move",
  scroll: true,
  opacity: 0.7,
  revert: true,
  start: function(event, ui) {
    // 드래그 시작 시 실행될 로직
  },
  stop: function(event, ui) {
    // 드래그 종료 시 실행될 로직
  },
  sort: function(event, ui) {
    // 드래그 중 placeholder 위치 및 인덱스 확인
    console.log("Sorting, placeholder index:", ui.placeholder.index());
    // 필요 시 위치 조정 또는 추가 로직 실행 가능
  }
});
```