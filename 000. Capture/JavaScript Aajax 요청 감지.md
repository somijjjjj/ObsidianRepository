


```
// 모든 jQuery Ajax 요청 추적
$(document).ajaxSend(function (event, jqXHR, settings) {
  console.log("🚨 Ajax 요청 발생:", settings.url);
});
```