# Input / Output

## cin, cout

```cpp
ios::sync_with_stdio(false);
cin.tie(NULL);
cout.tie(NULL);
```
가 있어야 싱글스레드 상황에서 입출력이 빠름

또한 endl은 출력 버퍼에 flush하는 효과가 있기 때문에 웬만해서는 \n 을 쓰는게 (수 배 이상!)낫다.