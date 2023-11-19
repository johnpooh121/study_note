# Ways to implement REST API in Spring Boot

## @RequestMapping

`@RequestMapping(value = "/hello", method = RequestMethod.GET)`

HTTP의 모든 메소드에 적용 가능

## @PathVariable 로 변수 받기

```java
@GetMapping(value="/name/{var}")
public String getvar(@PathVariable String var){
    return "Hello, "+var;
}
```

## @RequestParam으로 맵 형식으로 변수 받기

```java
@GetMapping(value="/request")
public String getparam(
        @RequestParam String name,
        @RequestParam String email
){
    return "Hello, "+name+"<br>email : "+email;
}
```
```java
@RequestMapping(value="/map")
public String getmap(@RequestParam Map<String,String> param){
    StringBuilder sb = new StringBuilder();
    param.entrySet().forEach(pa -> sb.append(pa.getKey()+" : "+pa.getValue()+"<br>"));
    return sb.toString();
}
```

## DTO(Data Transfer Object)를 이용한 GET 구현

PostMapping이라서 Body를 해석해서 MemberDTO를 만듦

```java
@PostMapping(value="/post")
public String postBody(@RequestBody MemberDto mem){
    return mem.toString();
}
```

## Swagger - REST API Spec의 문서화