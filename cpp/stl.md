# STL

## 1. vector

### size()는 unsigned임에 주의
vector.size() 의 자료형은 unsigned인 size_type이다.

벡터의 size가 0일 때 vector.size()-1을 고려해보자.

size_type은 int와 같이 4바이트이기 때문에 int 관점에서는 올바르게 -1로 보이지만,
long long 관점에서는 8바이트이므로 0....01.....1로 보여서 2^32-1 처럼 보이게 된다.

실제로 아래와 같은 코드는 2^32-1번 루프를 돈다

```cpp
vector<int> vec;
for(long long i=0;i<vec.size()-1;i++){
    printf("%lld\n",i);
}
```

적절한 형변환을 통해 이를 방지하도록 주의해야 한다.  
e.g.)

```cpp
vector<int> vec;
for(long long i=0;i<(long long)vec.size()-1;i++){
    printf("%lld\n",i);
}
```

