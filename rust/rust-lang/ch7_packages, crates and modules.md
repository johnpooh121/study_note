# Ch7. Packages, Crates, and Modules

## Ch7.1 Packages and Crates

- ### crate
smallest amount of code that rust compiler(rustc) considers at a time

module을 포함할 수 있음

- ### crate의 type
  - binary crates
    - run할 수 있는 파일 target으로 컴파일할 수 있음
  - library crates
    - main function이 없으며 executable target으로 컴파일이 불가능
    - 대부분의 crates가 lib crates
- ### crate의 root
  - source file that rustc starts from to make a root module of crate
  - binary -> main.rs, library -> lib.rs
- ### package
  - bundle of crates that provides functionality
  - main.rs와 lib.rs 둘다 있으면 crate가 2개인 것