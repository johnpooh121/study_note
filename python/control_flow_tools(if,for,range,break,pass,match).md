# Control Flow

## 1. if문

```python
if x < 0:
    print('Negative changed to zero')
elif x == 0:
    print('Zero')
elif x == 1:
    print('Single')
else:
    print('More')
```

## 2. for statements

맘대로 정지조건을 줄 수 있는 C와는 다르게 주어진 iterable에 대해 순서대로 순회한다.

### 2.1. range()

```python
list(range(5, 10))
[5, 6, 7, 8, 9]

list(range(0, 10, 3))
[0, 3, 6, 9]

list(range(-10, -100, -30))
[-10, -40, -70]
```

## 3. Looping techniques

### 3.1. dict 순회

`items()` 사용

```python
knights = {'gallahad': 'the pure', 'robin': 'the brave'}
for k, v in knights.items():
    print(k, v)

gallahad the pure
robin the brave
```

### 3.2. sequence 순회

`enumerate()` 사용

```python
for i, v in enumerate(['tic', 'tac', 'toe']):
    print(i, v)
```

### 3.3. 여러개의 sequence를 동시에 순회

`zip()` 사용

```python
questions = ['name', 'quest', 'favorite color']
answers = ['lancelot', 'the holy grail', 'blue']
for q, a in zip(questions, answers):
    print('What is your {0}?  It is {1}.'.format(q, a))
```

### 3.4. 역순

`reverse()` 사용

```python
for i in reversed(range(1, 10, 2)):
    print(i)
```

### 3.5. 정렬된 sequence 순회

`sorted()` 사용

```python
basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
for i in sorted(basket):
    print(i)
```

### 3.6. 중복 원소 제거 후 정렬된 sequence 순회

`set()`과 `sorted()` 사용

```python
basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
for f in sorted(set(basket)):
    print(f)
```

## 4. else clauses of loops

- for나 while에서 반복문이 break가 아니라 끝에 도달해서 (while의 경우 조건이 false) 끝날 때 else 아래의 코드가 실행됨

```python
for n in range(2, 10):
    for x in range(2, n):
        if n % x == 0:
            print(n, 'equals', x, '*', n//x)
            break
    else:
        # loop fell through without finding a factor
        print(n, 'is a prime number')
```

- break가 없을 때 else가 실행되는 것은 try문에서 exception이 없을 때 else가 실행되는 것과 유사

## 5. pass

아무것도 하지 않음

```python
class MyEmptyClass:
    pass
```

## 6. match

### 6.1. literal 비교

```python
def http_error(status):
    match status:
        case 400:
            return "Bad request"
        case 404:
            return "Not found"
        case 401 | 403 | 404:
            return "Not allowed"
        case _:
            return "Something's wrong with the internet"
```

### 6.2. class unpacking

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

def where_is(point):
    match point:
        case Point(x=0, y=0):
            print("Origin")
        case Point(x=0, y=y):
            print(f"Y={y}")
        case Point(x=x, y=0):
            print(f"X={x}")
        case Point():
            print("Somewhere else")
        case _:
            print("Not a point")
```

#### 6.2.1. `__match_args__` 를 지정해서 매칭 순서를 줄 수 있다.

```python
class Point:
    __match_args__ = ('x', 'y')
    def __init__(self, x, y):
        self.x = x
        self.y = y

match points:
    case []:
        print("No points")
    case [Point(0, 0)]:
        print("The origin")
    case [Point(x, y)]:
        print(f"Single point {x}, {y}")
    case [Point(0, y1), Point(0, y2)]:
        print(f"Two on the Y axis at {y1}, {y2}")
    case _:
        print("Something else")
```

### 6.3. if와 같이 사용 (guard)

```python
match point:
    case Point(x, y) if x == y:
        print(f"Y=X at {x}")
    case Point(x, y):
        print(f"Not on the diagonal")
```

### 6.4. sequence unpacking

- `[x,y,*rest]` 또는 `[x,y,*_]` 등으로 길이가 2 이상인 list를 unpacking 가능

- tuple에 대해서도 똑같이 할 수 있다.

### 6.5. dict unpacking

- `{"x_coord": x, "y_coord":y}` 처럼 unpacking 가능
  - 이 때 나머지 mapping들은 그냥 ignore됨
  - 나머지 mapping은 **rest로 binding 가능
  
### 6.6. subpattern

`as` 키워드로 사용 가능

```python
case (Point(x1, y1), Point(x2, y2) as p2): ...
```

### 6.7. named constant

```python
from enum import Enum
class Color(Enum):
    RED = 'red'
    GREEN = 'green'
    BLUE = 'blue'

color = Color(input("Enter your choice of 'red', 'blue' or 'green': "))

match color:
    case Color.RED:
        print("I see red!")
    case Color.GREEN:
        print("Grass is green")
    case Color.BLUE:
        print("I'm feeling the blues :(")
```


