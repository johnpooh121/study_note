# Structs

# Ch 5.1

## definition

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
```

## initialization

### Basic

```rust
let user1 = User {
    active: true,
    username: String::from("someusername123"),
    email: String::from("someone@example.com"),
    sign_in_count: 1,
};
```

### Field Init Shorthand
생성함수의 parameter가 field 이름과 같을때 그냥 쓰면 됨
```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
}
```

### Update 이용
이전에 정의된 struct에서 일부 필드만 업데이트 하는 식
```rust
fn main() {
    // --snip--

    let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
}
```

이 때 user1은 본래 객체의 ownership 잃음

## Usage

```rust
user1.email = String::from("anotheremail@example.com");
```

## Tuple Struct
필드에 특별히 이름이 필요없을 때
```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```

필드 접근은 튜플 다루듯이 하면 됨

`sturct AlwaysEqual;` 처럼 필드가 없는 Unit-Like Struct도 사용 가능

## references in field

가능은 하나 lifetime 필요 -> ch10에서 다룸

# Ch 5.3 Methods

## First Parameter is always "self"

represent instance of struct being called

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}
```
impl 내에 method 추가

impl 블록이 여러개일 수 있음

self, &self, &mut self 모두 가능

메소드 이름이 필드 이름과 같아도 됨 (getter에 이용 가능)

## automatic referencing and dereferencing
알아서 &, *, &mut 붙여줌
```rust
p1.distance(&p2);
(&p1).distance(&p2);
```
