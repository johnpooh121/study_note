# Ch1. Installation and Environment Setting

## 1. Installation (for windows)

1. rustup 설치
2. `rustc --version` 으로 rustc 체크

## 2. Update

`rustup update`

## 3. Starting a project

- ### cargo 없이 만들기
  - 그냥 project 폴더 내에 .rs 소스파일 생성 (ex : main.rs)
  - `rustc main.rs` 로 컴파일 -> main.pdb, main.exe 생성
  - `./main` 으로 실행
- ### cargo 이용하기
  - `cargo --version`으로 설치 확인
  - `cargo new pr_name` 으로 현재 디렉토리에 pr_name 폴더 생성
  - 내부 안은 src 폴더 내의 소스파일과 Cargo.toml로 이루어짐
    - Cargo.toml은 package와 dependencies ( _crates_ 와 관련) 등을 명시
  - `cargo build` 로 빌드 -> ./target/debug에 execuateble 생성됨
    - `cargo build --release` 는 릴리즈용으로 실행파일을 ./target/release에 생성
  - `cargo run` 로 빌드와 실행 동시에 할 수 있음
  - `cargo check` 로 실행파일은 생성하지 않되 컴파일이 되는지 체크 가능 (큰 프로젝트에서 사용)


## 4. dd

- [Ch1. Installation and Environment Setting](#ch1-installation-and-environment-setting)
  - [1. Installation (for windows)](#1-installation-for-windows)
  - [2. Update](#2-update)
  - [3. Starting a project](#3-starting-a-project)
  - [4. dd](#4-dd)
