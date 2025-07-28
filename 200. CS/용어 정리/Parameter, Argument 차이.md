

## 1. Parameter (매개변수)

함수를 **정의할 때** 사용되는 변수.

- **형식 매개변수 (Formal Parameter)**
    
    - 함수 선언부에 정의되는 변수
        
    - 예: `def add(a, b):` 에서 `a`, `b`
        
- **실제 매개변수 (Actual Parameter)**
    
    - 함수 호출 시 넘겨지는 값과 연결되는 변수
        

---

## 2. Argument (인자/인수)

함수를 **호출할 때 전달하는 실제 값**.

- 예:
    
    ```python
    add(3, 5)
    ```
    
    여기서 `3`, `5`가 argument.
    

---

## 차이 요약

- **Parameter**: 함수 정의 시 선언되는 변수 이름
    
- **Argument**: 함수 호출 시 실제로 전달되는 값
    

---

### 예시 (Python)

```python
def greet(name):  # name = parameter
    print("Hello,", name)

greet("Eunyoung")  # "Eunyoung" = argument
```

---

### 태그

`#CS기초` `#프로그래밍기초` `#함수` `#용어정리`
