## 연산자 오버로딩

### pair 오버로딩

```cpp
pii operator-(pii p,pii q){
	return {p.fi-q.fi,p.se-q.se};
}

ll operator *(pii p,pii q){return (ll)p.fi*q.fi+(ll)p.se*q.se;}

template <typename T, typename D>
std::ostream& operator<<(std::ostream& os, std::pair<T, D> &p) {
	os << "{" << p.first << ", " << p.second<<"}";
    return os;
}
```