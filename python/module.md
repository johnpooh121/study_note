# Module

## Definition

파이썬 definition과 stmt들을 담고 있는 파일

## 특징들

- 모듈 이름은 `__name__`으로 접근 가능
- 모듈의 private 네임스페이스 내 변수는 모듈명.변수명으로 접근
- from fibo import fib
  - fibo 모듈에서 fib만 import, 함수는 fib(50)으로 호출 가능
  - fibo 모듈은 네임스페이스에 로드 안됨
- from fibo import *
  - 모듈 내의 모든 name을 import
- import fibo as fb
  - fb로 모듈명을 대신
- from fibo import fib as fibonacci
  - fib함수를 fibonacci로 대신

## 스크립트로 실행

`python 모듈명 args`을 실행시 모듈이 스크립트처럼 실행되며 이때 `__name__`이 `__main__`으로 설정됨

## 모듈 검색 순서

import spam 시

먼저 `sys.builtin_module_names` 에서 빌트인 모듈에서 찾아봄

그 후에는 `sys.path`를 찾아봄
- sys.path는 폴더 이름들의 배열
  - cwd나 input 스크립트의 위치
  - 