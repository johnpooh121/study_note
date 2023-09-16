# Ownership

## Ch4.1 What is Ownership?

### Data 구조는 heap과 LIFO stack으로 나누어짐

### Ownership Rules

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

### borrowing
    creating a reference

    &s : s의 reference

### mutable reference

    let mut s = String::from ...
    &mut s

### rule of reference
- 한 번에 mutable reference 한개나 immutable ref 여러개가 존재할 수 있음
- 어떤 variable을 수정할 수 있는 변수가 2개 이상 존재하는 range가 있으면 안됨
```rust
fn main() {
    println!("Hello, world!");
    let mut ms = String::from("hi");
    let r1 = &mut ms;
    println!("{r1}");
    ms = String::from("gg");
    println!("{r1}");
}
```
- 컴파일 오류 발생
- 마지막 줄의 println 없으면 r1이 더 일찍 drop되어서 컴파일이 가능해짐

## Ch 4.3 Slice Type

- ownership이 없음
- String, Array 등 연속된 collection을 refernece 하는 용도
- validity가 원본 collection에 의존 -> 실수를 줄이는 데 도움 (ex: 원본을 날리고 slice를 사용하면 오류 발생)
- 문법
- ```&s[0..2]``` => s[0] s[1] (마지막 인덱스는 제외)
- `&s[..2]` 나 `&s[1..]` , `&s[..]` 으로 사용 가능
- `let s = "literal"` 로 받으면 s는 slice (&str)
- &String에 적용 가능한 함수는 &str에도 사용 가능하고, string을 &str로 바꾸는건 쉬우므로 함수의 파라미터 타입은 slice로 하는 것이 좋다 (advantage of deref coercion)
- int array의 slice type : `&[i32]`