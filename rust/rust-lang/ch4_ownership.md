# Ownership

## Ch4.1 What is Ownership?

### Data 구조는 heap과 LIFO stack으로 나누어짐

### | Ownership Rules

- 각 value는 유일한 owner가 존재
- owner가 scope를 나가면 value는 ***drop***됨

### Variable Scope
    value가 valid한 program의 range


### variable move

```rust
let s1 = String::from("hello");
    let s2 = s1;
```
    단순한 ***stack only data*** 는 복사하지만 그 외의 경우에는 ownership이 이동됨
- copy trait이 존재

### String

    string literal의 ptr, len, capacity로 이루어짐

### function과 ownership

단순히 parameter에 pass하면 ownership이 함수로 이동함

return value할때도 ownership이 밖으로 이동함

## Ch 4.2 References and Borrowing

