
## 🔐 HTML 특수문자 처리

웹 개발을 하다 보면 태그에 특정 문자열을 넣었을 때, 의도한 값이 제대로 출력되지 않는 경우가 있습니다. 대표적으로 `<`, `>` 기호가 포함된 문자열이 HTML 태그로 인식되어 사라지거나 다른 형태로 렌더링되는 문제가 있습니다.

### 😕 문제 상황

예를 들어, 아래와 같은 코드를 생각해봅시다.

```html
<div> <script> </div>
```

이렇게 작성하면 브라우저는 `<script>`를 HTML 태그로 인식해 렌더링 과정에서 무시하거나 위험 요소로 판단합니다. 결과적으로 사용자가 보게 되는 값은 빈 문자열이거나, `<script>` 태그로 인해 보안 이슈까지 발생할 수 있습니다.

### 🤔 왜 이런 문제가 생길까?


HTML 문법에서 `<`, `>`는 태그의 시작과 끝을 의미하는 특수 문자입니다. 이 값이 `input`의 `value` 속성에 그대로 들어가면, 브라우저는 이를 마크업 구조의 일부로 해석해 실제 값으로 인식하지 않습니다. 이를 방지하려면 **이스케이프 문자(escape character)** 로 치환해줘야 합니다.

### ✅ 해결 방법: 특수문자 이스케이프 처리

HTML에서 주요 특수문자는 다음과 같이 이스케이프 시켜야 안전하게 사용할 수 있습니다:

|문자|이스케이프 코드|
|---|---|
|`<`|`&lt;`|
|`>`|`&gt;`|
|`&`|`&amp;`|
|`"`|`&quot;`|
|`'`|`&#39;`|

**예시 수정:**

```html
<div> &lt;script&gt;</div>
```

이렇게 바꾸면 브라우저는 `<script>`를 단순 문자열로 인식하고 화면에 그대로 출력합니다.

### 🛠️ 자바스크립트로 이스케이프 처리하기

서버나 프론트에서 사용자 입력값을 처리할 때, 자바스크립트를 활용해서 특수문자를 이스케이프할 수도 있습니다.

```javascript
function escapeHtml(str) {
  return str
    .replace(/&/g, "&amp;")
    .replace(/</g, "&lt;")
    .replace(/>/g, "&gt;")
    .replace(/"/g, "&quot;")
    .replace(/'/g, "&#39;");
}

const unsafe = "<script>alert('XSS')</script>";
const safe = escapeHtml(unsafe);

document.querySelector("input").value = safe;
```

### 🔐 보안 관점에서의 중요성

이러한 처리는 단순한 화면 표시 문제뿐 아니라, **[[XSS(Cross Site Scripting)]]** 같은 보안 위협을 방지하는 데도 중요합니다. 사용자 입력값이 HTML 구조에 무방비로 삽입되면, 악성 스크립트가 실행될 수 있기 때문입니다.


---

- http://kor.pe.kr/util/4/charmap2.htm#google_vignette
- https://taemssssss.tistory.com/23#google_vignette
- https://inpa.tistory.com/entry/HTML-%F0%9F%93%9A-%EF%BC%86nbsp-%EF%BC%86amp-%EF%BC%86lt-%EF%BC%86gt-%EF%BC%86quot-%EC%9D%98%EB%AF%B8