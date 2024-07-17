# inline

예제 코드

```cpp
inline void propagate(int now,int s,int e){
    if(lazy[now]==0)return;
    tree[now]+=lazy[now];
    if(s<e){
        lazy[now<<1]+=lazy[now];
        lazy[now<<1|1]+=lazy[now];
    }
    lazy[now]=0;
}
void upd(int now,int s,int e,int l,int r,int d){
    if(l<=s&&e<=r){
        lazy[now]+=d;
        propagate(now,s,e);
        return;
    }
    propagate(now,s,e);
    if(e<l||r<s)return;
    upd(now<<1,s,(s+e)>>1,l,r,d);
    upd(now<<1|1,(s+e>>1)+1,e,l,r,d);
    tree[now]=max(tree[now<<1],tree[now<<1|1]);
}
```

inline을 쓰면 어셈블리어 상에서 인라인 함수의 호출부가 함수 로직으로 대체됨

성능이 꽤 잘 개선된다
