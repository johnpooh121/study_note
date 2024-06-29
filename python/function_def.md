# Defining Function

def 다음줄을 문자열 리터럴로 쓰는 것을 독스트링이라 부름

 - 함수마다 로컬 심볼테이블이 존재
 - 함수 내에서 변수 참조시 로컬 심볼테이블을 보고, 그 뒤로 그 함수를 감싸는 함수(호출하는 함수가 아님)가 있으면 그 감싸는 함수의 심볼테이블을 보며, 마지막에는 global 심볼테이블을 본다.
 - parameter는 *call by object*를 따름
   - call by object
     - immutable한 객체(숫자, 스트링, 튜플 등)의 경우, 
        ```python
        def func(b):
            b+=2
        a=1
        func(a)
        print(a)
        ```
       - a는 이름표, b는 1을 가리키다가 3을 가리키게 됨, a는 그대로 1
     - mutable한 객체(리스트 등)의 경우
        - mutable하므로 넘겨받은 동일한 객체에 연산을 수행함 
- 함수는 기본적으로 None 을 리턴함

## Parameters

### Default value

default 값은 함수 선언시 계산됨(mutable 객체의 경우 호출될 때의 값이 반영됨)

```python
i = 5

def f(arg=i):
    print(arg)

i = 6
f() --> 1
```

### keyword argument


 - positional arg 다음에 나옴
 - 마지막 parameter가 **name 꼴이면 dictionary로 입력을 받을 수 있음
  
### arbitrary argument list

- 임의의 길이의 튜플을 입력받을 수 있음
- 위의 keyword arg와의 순서는
  - `def cheeseshop(kind, *arguments, **keywords):` 꼴
  - 