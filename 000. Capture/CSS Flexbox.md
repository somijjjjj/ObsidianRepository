
## 1. Flexbox 개요

- **Flex Container**
    
    - `display: flex;` 또는 `display: inline-flex;`를 선언한 부모
        
    - 내부의 자식요소들이 “유연한 박스”로 배치됨
        
- **축(axis)**
    
    - **주 축(main-axis)**: `flex-direction`에 따라 가로/세로
        
    - **교차 축(cross-axis)**: 주 축에 수직
        

---

## 2. 주요 Flex 아이템 속성

|속성|기본값|설명|
|---|---|---|
|`flex-grow`|`0`|남는 공간(positive free space)을 얼마나 차지할지 비율.|
|`flex-shrink`|`1`|공간이 부족할 때 줄어드는 비율.|
|`flex-basis`|`auto`|초기 크기(또는 콘텐츠 크기) 설정.|
|**축약형**|||
|`flex`|`0 1 auto`|위 세 개를 한 줄에 지정 (`grow shrink basis`)|

---

## 3. `flex-shrink` 상세

- **역할**: 컨테이너 폭이 작아져서 자식들이 모두 들어가지 않을 때, 어떤 비율로 줄일지 결정
    
- **값**:
    
    - `0`: 전혀 축소하지 않음
        
    - `>0`: 비율에 따라 축소
        
- **사용 예**:
    
    ```css
    /* 텍스트가 무조건 원본 크기로 유지되도록 */
    .iconArea > span {
      flex-shrink: 0;
    }
    ```
    

---

## 4. 축약형 `flex` 사용법

```css
/* flex-grow:1, flex-shrink:1, flex-basis: auto */
.item {
  flex: 1;            
}

/* flex-grow:1, flex-shrink:0, flex-basis: auto */
.item {
  flex: 1 0 auto;
}

/* flex-grow:0, flex-shrink:0, flex-basis: 100px */
.item {
  flex: 0 0 100px;
}
```

- **주의**:
    
    - `min-width:0;`을 함께 걸어야, 부모가 좁아질 때 자식의 `flex-basis: auto` 크기(콘텐츠 너비)가 제대로 줄어들 수 있음.
        

---

## 5. 실전 예제

```css
.link_name .iconArea {
  display: flex;
  align-items: center;  /* 수직 중앙 정렬 */
  gap: 5px;             /* 아이콘-텍스트 간격 */
}

/* 1) span들은 축소되지 않도록 */
.link_name .iconArea > span {
  flex-shrink: 0;
}

/* 2) 텍스트만 남는 공간 차지 & 말줄임 처리 */
.link_name .postfixNm {
  flex: 1 1 auto;
  min-width: 0;          
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* 3) 아이콘/뱃지는 고정 크기 */
.link_name .postfix_type,
.link_name .answerkeyBtn {
  flex: 0 0 auto;
}
```

---

## 6. 기록 포인트

- Flex 컨테이너와 아이템의 관계
    
- `flex-grow` / `flex-shrink` / `flex-basis` 기본값 및 의미
    
- 축약형 `flex` 쓰는 법
    
- `flex-shrink: 0` 으로 **축소 방지**
    
- `min-width: 0` 과 함께 써야 **overflow** 컨트롤 가능
    
- 텍스트 말줄임을 위한 `white-space` + `overflow` + `text-overflow`
    

이렇게 정리해 두시면, 다음에도 flex 동작 원리를 빠르게 떠올릴 수 있을 거예요. 😊