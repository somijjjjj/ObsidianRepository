
문서의 레이아웃을 변경하지 않고 요소를 표시하거나 숨깁니다.



```CSS
/* Keyword values */
visibility: visible;
visibility: hidden;
visibility: collapse;

/* Global values */
visibility: inherit;
visibility: initial;
visibility: revert;
visibility: revert-layer;
visibility: unset;
```


[`visible`](https://developer.mozilla.org/en-US/docs/Web/CSS/visibility#visible)

요소 상자가 표시됩니다.

[`hidden`](https://developer.mozilla.org/en-US/docs/Web/CSS/visibility#hidden)

`visibility`요소 상자는 보이지 않지만(그리지 않음), 레이아웃에는 평소처럼 영향을 미칩니다. 요소의 하위 요소는 로 설정된 경우 표시됩니다 `visible`. 요소는 포커스를 받을 수 없습니다(예: [탭 인덱스를](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/tabindex) 탐색할 때 ).

[`collapse`](https://developer.mozilla.org/en-US/docs/Web/CSS/visibility#collapse)

키워드 `collapse`는 요소마다 다른 효과를 갖습니다.

- 행, 열, 열 그룹 및 행 그룹 의 경우 [`<table>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/table), 행 또는 열이 숨겨지고 해당 행 또는 열이 차지했던 공간이 제거됩니다( ``[`display`](https://developer.mozilla.org/en-US/docs/Web/CSS/display): none``표의 열/행에 적용된 것처럼). 그러나 다른 행과 열의 크기는 축소된 행 또는 열의 셀이 있는 것처럼 계속 계산됩니다. 이 값을 사용하면 표 전체의 너비와 높이를 다시 계산하지 않고도 표에서 행이나 열을 빠르게 제거할 수 있습니다.
- 접힌 플렉스 항목과 루비 주석은 숨겨지고, 이들이 차지했을 공간도 제거됩니다.
- 다른 요소의 경우에는 . `collapse`과 동일하게 처리됩니다 `hidden`.

## [접근성](https://developer.mozilla.org/en-US/docs/Web/CSS/visibility#accessibility)

요소에 `visibility`값을 사용하면 [접근성 트리](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Accessibility/What_is_accessibility#accessibility_apis) 에서 해당 요소가 제거됩니다 . 이렇게 되면 해당 요소와 그 하위 요소는 더 이상 화면 읽기 기술을 통해 표시되지 않습니다.`hidden`


https://developer.mozilla.org/en-US/docs/Web/CSS/visibility