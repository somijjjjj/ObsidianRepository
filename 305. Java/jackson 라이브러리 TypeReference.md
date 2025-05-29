
`TypeReference<T>`는 Jackson이 **제네릭 타입**(예: `List<Something>`, `Map<Key,Value>` 등)을 런타임에 정확히 알아낼 수 있도록 도와주는 추상 클래스입니다.

자바는 컴파일 시점에 제네릭 타입 정보가 소거(type erasure)되어, 단순히 `List.class` 나 `Map.class`만으로는 내부에 어떤 타입이 들어가는지 알 수 없어요. 하지만 JSON을 파싱할 때는 “이 JSON 배열을 `List<String>`으로 만들 것인지, `List<MyDto>`로 만들 것인지”가 중요하죠.

`TypeReference`를 익명 서브클래스로 생성하면, 그 서브클래스의 **제네릭 매개변수** 정보를 Jackson이 리플렉션으로 읽어들이게 됩니다. 예를 들어:

```java
ObjectMapper mapper = new ObjectMapper();

String json = "[{\"id\":1,\"name\":\"Alice\"},{\"id\":2,\"name\":\"Bob\"}]";

// TypeReference를 써서 List<User> 타입 정보를 전달
List<User> users = mapper.readValue(
    json,
    new TypeReference<List<User>>() {}
);
```

위 코드에서 `new TypeReference<List<User>>() {}` 부분이 중요해요:

1. `TypeReference<List<User>>`라는 클래스가 내부적으로 “List”라는 타입 정보를 갖고 있고,
    
2. Jackson은 이 정보를 보고 각 원소를 `User` 객체로 매핑해 줍니다.
    

> **정리**
> 
> - **용도**: JSON → 제네릭 컬렉션(또는 복합 타입) 매핑
>     
> - **패키지**: `com.fasterxml.jackson.core.type.TypeReference`
>     
> - **사용법**: `mapper.readValue(json, new TypeReference<원하는타입>() {})`
>     

이렇게 하면 `List<Map<String,Object>>`, `Map<String, List<MyDto>>` 같은 복잡한 타입도 안전하게 파싱할 수 있습니다.