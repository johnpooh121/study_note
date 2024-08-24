# Input / Output

## cin, cout

```cpp
ios::sync_with_stdio(false);
cin.tie(NULL);
cout.tie(NULL);
```
가 있어야 싱글스레드 상황에서 입출력이 빠름

또한 endl은 출력 버퍼에 flush하는 효과가 있기 때문에 웬만해서는 \n 을 쓰는게 (수 배 이상!)낫다.

[백준 입출력 실험](https://www.acmicpc.net/blog/view/56) 참고

```cpp
struct FIO{
	char buf[100005];
	int buf_size,buf_cursor,cnt;

	inline char get_next_c(){
		if(buf_size<=buf_cursor){
			buf_size=fread(buf,1,1e5,stdin);
			printf("buf : %d\n",buf_size);
			buf_cursor=0;
		}
		return buf[buf_cursor++];
	}

	inline int myread_int(){
		char c;
		for(c=get_next_c();c<=32;c=get_next_c());
		int ret=0;
		while(c>32){
			ret*=10;
			ret+=c-'0';
			c=get_next_c();
		}
		return ret;
	}
} fio;
```