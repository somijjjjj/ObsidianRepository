---
tags:
  - publish/draft
---


```javascript
<script src="font/NotoSansKR/NotoSansKR-base64.js"></script>
```
[[Base64Encoder 파일을 base64로 인코딩하는 사이트]]


```javascript
var highlightWordList = new Array(); //텍스트 처리 내용 하이라이트 단어 배열
```

```javascript
// 원문 데이터 탭 > 텍스트 처리 내용 저장 버튼 클릭
$("#i_save").on("click", function(e) {
	if(e.detail > 1) { 
		e.preventDefault();
		return;
	}
	
	$(this).css("pointer-events", "none");
	
	var target = $("#div_morphologicTextArea div.area_content div.content")[0];
	if (!target) {
		return;
	}
	
	window.jsPDF = window.jspdf.jsPDF;
	var pdf = new jsPDF("p", "mm", "a4");
	pdf.addFileToVFS("NotoSansKR.ttf", fontEncoder);
	pdf.addFileToVFS("NotoSansKR-Bold.ttf", fontBoldEncoder);
	pdf.addFont('NotoSansKR.ttf', 'NotoSansKR', 'normal');
	pdf.addFont('NotoSansKR-Bold.ttf', 'NotoSansKR', 'bold');
	pdf.setFont("NotoSansKR");
	pdf.setFontSize(9);
	
	// 텍스트 추가
	var pageHeight = pdf.internal.pageSize.height; // 페이지 높이
	var pageWidth = pdf.internal.pageSize.width - 20; // 페이지 너비
	var marginTop = 10;
	var yPosition = marginTop;
	var lineHeight = 5; // 줄 간격
	var xPosition = 10;
	
	var dataId = $("#div_orgDataBody .tr_selected").attr("id");
	var content = orgMorphologicText;
	
	//모든 하이라이트 단어의 위치 찾기
	var matches = [];
	var processedContent = [];
	var lastIndex = 0;
	
	$.each(highlightWordList, function(index, word){
		if (word.trim() === "") return; // 빈 문자열 방지
		var startIdx = content.indexOf(word);
		while (startIdx !== -1) {
			matches.push({ index: startIdx, text: word });
			startIdx = content.indexOf(word, startIdx + word.length);
		}
	});

	matches.sort((a, b) => a.index - b.index); //위치순 정렬

	//하이라이트와 일반 텍스트 나누기
	$.each(matches, function(index, match){
		if (match.index > lastIndex) {
			processedContent.push({ text: content.substring(lastIndex, match.index), highlight: false });
		}

		processedContent.push({ text: match.text, highlight: true });
		lastIndex = match.index + match.text.length;
	});
	
	
	if (lastIndex < content.length) { // 마지막 일반 텍스트 추가
		processedContent.push({ text: content.substring(lastIndex), highlight: false });
	}

	// 텍스트를 한 줄씩 추가
	$.each(processedContent, function(index, item){
		var words = item.text.trim().split(" ");
		
		$.each(words, function(i, word) {
			var word = word.replaceAll("\n", " ");
			
			if ((yPosition + lineHeight) > pageHeight - 10) { 
				pdf.addPage(); 
				yPosition = marginTop;
				xPosition = 10;
			}

			if (item.highlight) {
				pdf.setFont("NotoSansKR", "bold");
				pdf.setTextColor(255, 0, 0);
			} else {
				pdf.setFont("NotoSansKR", "normal");
				pdf.setTextColor(0, 0, 0);
			}
			
			//추가될 텍스트 너비
			var addTextWidth = pdf.getTextWidth(word) + 1;
			
			//페이지 너비보다 추가될 텍스트 너비가 큰 경우 다음줄
			if ((xPosition+addTextWidth)-15 > pageWidth) {
				xPosition = 10;
				yPosition += lineHeight;
			}
			
			pdf.text(word, xPosition, yPosition);
			xPosition += addTextWidth;
			
			//페이지 너비 초과 시 다음줄
			if (xPosition > pageWidth) {
				xPosition = 10;
				yPosition += lineHeight;
			}
			
		});
	});
	
	pdf.save(taNm+"_"+dataId+".pdf");
	$(this).css("pointer-events", "auto");
});
```

---

> 참고링크
> - [[React.js] jsPDF를 이용한 웹 화면 PDF 내보내기 중 이슈: 페이지 오버플로우 이미지 잘림 문제](https://postforty.tistory.com/474)