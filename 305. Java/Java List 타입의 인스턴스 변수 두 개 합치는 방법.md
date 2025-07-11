

자바에서 `List<String>` 타입의 인스턴스 변수 두 개를 하나로 합치는 대표적인 방법은 다음과 같습니다.

---

## ✅ 1. `addAll()` 사용

기존 리스트에 다른 리스트의 요소들을 모두 추가합니다.

```java
List<String> listA = new ArrayList<>();
listA.add("apple");
listA.add("banana");

List<String> listB = new ArrayList<>();
listB.add("cherry");
listB.add("date");

List<String> merged = new ArrayList<>(listA);
merged.addAll(listB);
System.out.println(merged);
// 출력: [apple, banana, cherry, date]
````

> `listA`를 변경하지 않고 `merged`라는 새로운 리스트를 만들어 사용

---

## ✅ 2. Java Stream 사용 (`flatMap`)

Java 8 이상에서 사용할 수 있는 함수형 방식입니다.

```java
List<String> merged = Stream.of(listA, listB)
    .flatMap(List::stream)
    .collect(Collectors.toList());
```

> 원본 리스트를 변경하지 않고 새 리스트를 생성할 때 유용

---

## ✅ 3. 중복 제거 (선택사항)

합친 리스트에서 중복된 값을 제거하고 싶다면 `Set`을 사용할 수 있습니다.

```java
Set<String> mergedSet = new HashSet<>();
mergedSet.addAll(listA);
mergedSet.addAll(listB);
```

---

## 📌 요약

|방법|특징|
|---|---|
|`addAll()`|가장 직관적, 원본 리스트 수정 가능성 있음|
|`Stream.flatMap()`|함수형 스타일, 불변성 유지 가능|
|`Set` 이용|중복 제거 가능|

---

## 🗂️ 참고

- `ArrayList`, `HashSet`은 `java.util` 패키지에 포함되어 있음
- `Stream`, `Collectors`는 `java.util.stream` 패키지에 포함
    

---

> 다음 공부할 내용
> - [[Java Collection Framework]]
