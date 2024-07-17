# Glossary

## iterable

한 번에 하나씩 자기의 member를 리턴할 수 있는 객체

e.g. 모든 sequence 타입(list, str, tuple), 몇몇 non-sequence 타입(dict, file 객체), 

`__iter__()` 또는 sequence semantic의 `__getitem__()`을 구현한 사용자 정의 클래스

for문으로 순회가 가능하다

zip, map등 sequence가 필요한 곳에도 사용 가능

## sequence

효율적인 정수 인덱스 접근이 가능한 `iterable`

`__len__()`과 `__getitem__()`를 구현해야하며, 이 함수는 int를 인자로 받아야함

(dict의 경우에는 int만이 아닌 임의의 key를 받기 때문에 sequence가 아님)

