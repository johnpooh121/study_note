# Ch3. Common Programming Concepts

## 1. Variables and Mutability

### Variables

### Constants

### Shadowing

## 2. Types

### Scalas

### Compound Types

- Tuple
  
```rust 
  fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
  ```
  
- Arrays

`let A = [1,2,3]`  
`let A = [3;5]` => [3,3,3,3,3]

## 3. Functions

- ### Parameters
  
- ### Statements
  - `let x = 3;` -> no return value
- ### Expressions
  - `let x = 3; x+1`
  - `cargo --version`으로 설치 확인
  - `cargo new pr_name` 으로 현재 디렉토리에 pr_name 폴더 생성
  - 내부 안은 src 폴더 내의 소스파일과 Cargo.toml로 이루어짐
    - Cargo.toml은 package와 dependencies ( _crates_ 와 관련) 등을 명시
  - `cargo build` 로 빌드 -> ./target/debug에 execuateble 생성됨
    - `cargo build --release` 는 릴리즈용으로 실행파일을 ./target/release에 생성
  - `cargo run` 로 빌드와 실행 동시에 할 수 있음
  - `cargo check` 로 실행파일은 생성하지 않되 컴파일이 되는지 체크 가능 (큰 프로젝트에서 사용)


## 4. Comments
`// comment`

## 5. Control Flow

### if Expressions

    let number = if condition { 5 } else { 6 };

### loop

```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```
### while
### for
- `for element in array { ... }`
- `for number in (1..4).rev() { ... }` => ***3 2 1 순서***