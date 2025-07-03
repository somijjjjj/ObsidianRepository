
### 개요 

N사 플레이스 플랫폼에 등록된 식당들의 메뉴 정보를 수집을 하고 있다. 
식당의 메뉴 정보는 기본적으로 등록하는 메뉴 외에 N주문 서비스를 이용해서 주문 유형 별 메뉴를 등록할 수 있게 되어 있다.
수집 유형 중 N주문을 사용하는 업체 메뉴정보가 수집이 안되는 이슈가 있어서 원인 분석을 하면서 내가 모르는 개념들을 정리해보려고 한다.

### 기존 방식

기존에는 HTML 초기 렌더링 시 `window.__APOLLO_STATE__` 객체에 포함된 메뉴 데이터를 파싱하여 사용했으나, 2025-07-03 기준 해당 객체에서 메뉴 데이터가 제거되어 있음 확인됨.
- 원인: 화면 로딩 후 메뉴 정보가 GraphQL API(`/graphql?opName=menu`)를 통해 동적 로딩되는 구조로 변경됨.
- 확인 방법: 네트워크 탭에서 GraphQL POST 요청을 통해 `menu` 쿼리를 호출하고 있는 점 확인, 요청 페이로드 내 variables에 `slotId`, `businessId` 등 포함됨.
- 해결 방향:
  - 수집 로직을 기존 `APOLLO_STATE` 파싱 방식에서 GraphQL API 호출 방식으로 변경 필요
  - `operationName: menu` 쿼리 구조 파악 및 Postman을 통한 테스트 완료
  - 추후 스크래핑 또는 크롤링 시 동적 데이터 대응 방안 고려 필요


좋은 질문이에요! `window.__APOLLO_STATE__`는 **Apollo Client**를 사용하는 웹 애플리케이션에서 **초기 데이터를 클라이언트에게 전달**하기 위해 HTML에 포함시키는 전형적인 구조예요. 아래에서 쉽게 설명해볼게요.

---

## ✅ `window.__APOLLO_STATE__`가 뭐냐면?

### 💡 **GraphQL을 사용하는 SPA(또는 SSR/SSG) 앱에서, 서버가 초기 데이터를 HTML에 "직접 박아주는" 방식이에요.**

---

### 📦 예시 구조

```html
<script>
  window.__APOLLO_STATE__ = {
    "ROOT_QUERY": {
      "__typename": "Query",
      "menu({\"input\":{\"slotId\":\"652707031\"}})": {
        "type": "id",
        "generated": true,
        "id": "Menu:652707031",
        "typename": "Menu"
      }
    },
    "Menu:652707031": {
      "id": "652707031",
      "name": "김치찌개 세트",
      "price": 8900,
      "__typename": "Menu"
    }
  }
</script>
```

---

### 🔧 왜 이렇게 넣는 걸까?

#### 1. **서버 사이드 렌더링(SSR)**

- 초기 HTML을 렌더링할 때 서버가 API를 먼저 호출해서 데이터를 받아온 후, `window.__APOLLO_STATE__`에 넣어줌
    
- 클라이언트가 이 데이터를 보고 Apollo Client에 hydration 함
    
- **장점**: 초기 로딩 속도가 빠르고 SEO 친화적
    

#### 2. **정적 사이트 생성(SSG)**

- Next.js 같은 프레임워크에서 build-time에 데이터를 넣어주는 용도로도 사용
    

#### 3. **클라이언트 측 캐시 초기화**

- `__APOLLO_STATE__`는 Apollo Client가 사용할 **초기 캐시 값**으로 작동해요  
    즉, 첫 페이지 로딩 시 API를 다시 호출하지 않아도 되도록 하기 위한 장치예요.
    

---

### 🔄 그런데 지금은 왜 사라졌을까?

- 네트워크 탭에서 GraphQL 요청을 보는 걸 보면,  
    👉 **초기 렌더링에서 제외**하고,  
    👉 **CSR(Client-Side Rendering)**으로 동적 로딩 방식으로 바뀐 것 같아요.
    

**즉, HTML 내에 데이터가 없고, JS가 실행된 후 GraphQL API를 호출해서 데이터를 가져오게 구조가 바뀐 거예요.**

---

### 📌 정리

|항목|설명|
|---|---|
|`window.__APOLLO_STATE__`|초기 GraphQL 쿼리 결과를 JSON으로 담아놓은 전역 객체|
|목적|Apollo Client 초기화 및 캐시 hydration|
|사라진 이유|CSR 구조로 변경되어 데이터가 JS로 동적 로딩되기 때문|

---

원하면, `__APOLLO_STATE__`를 읽어서 데이터를 추출하는 방법도 코드 예시로 보여줄 수 있어요.