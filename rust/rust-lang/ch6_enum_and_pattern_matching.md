# Ch 6.1 Enum

## definition

```rust
enum IpAddrKind {
    V4(String),
    V6(String),
}

let ipkind = IpAddrKind::V4(String::from("127.0.0.1"));
```

필드에 이름 붙이는 것 가능

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```
## Option
```rust
enum Option<T> {
    None,
    Some(T),
}
let some_num = Some(3);
```

# Ch6.2 match

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}
```

*matches are exhaustive*

## special pattern _

```rust
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    _ => reroll(),
}
```

# Ch 6.3 if let control flow

```rust
let config_max = Some(3u8);
if let Some(max) = config_max {
    println!("The maximum is configured to be {}", max);
}
```