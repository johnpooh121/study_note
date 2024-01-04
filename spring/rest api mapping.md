# REST API
로이 필딩의 박사논문에서 소개된 HTTP 웹의 장점을 최대한 활용하는 아키텍쳐

자원 (URI), 행위(HTTP 메소드), 표현으로 구성됨
## Properties of REST API

-  유니폼 인터페이스
-  Stateless
-  캐시 가능성
-  Layered System - 서버의 복잡도와 상관 없이 클라이언트는 서버와의 연결 포인트만 신경쓰면 됨
-  Client-Server 아키텍쳐 : 서로의 의존성 낮추는 구조 위함 (REST 서버는 API 제공, 클라이언트는 사용자 정보 관리)

## REST의 URI 설계 규칙

- 마지막에 '/' 쓰지 않기
- 언더바 대신 하이픈 사용
- URL에는 행위가 아닌 결과 포함, 행위는 HTTP 메서드로 나타냄
- URI는 소문자로, 확장자는 포함하지 않기(Accept 헤더 사용)

대부분 URL은 인터넷에서 서버의 주소, URI는 그 서버 내의 리소스까지의 경로를 의미

## HTTP 메소드별 역할
| Method | 역할                     |
| ------ | ------------------------ |
| Post   | 리소스를 생성, Create    |
| Get    | 해당 리소스를 조회, Read |
| Put    | 리소스 수정, Update      |
| Delete | 리소스 삭제, Delete      |


# Ways to implement REST API in Spring Boot

## @RequestMapping

`@RequestMapping(value = "/hello", method = RequestMethod.GET)`

HTTP의 모든 메소드에 적용 가능  
Controller 단에서 컨트롤러 별 공통 URL을 설정할 때 사용됨  
스프링 4.3부터는 @Request 내에 method 설정 대신 GetMapping, PostMapping 등으로 대체됨

## @PathVariable 로 변수 받기

```java
@GetMapping(value="/name/{var}")
public String getvar(@PathVariable String var){
    return "Hello, "+var;
}
```

## @RequestParam으로 변수 받기

> input : uri?name=value1&email=value2
```java
@GetMapping(value="/request")
public String getparam(
        @RequestParam String name,
        @RequestParam String email
){
    return "Hello, "+name+"<br>email : "+email;
}
```
> output body : Hello, value1 \nemail : value2

위와 동일한 입력에 대해 아래와 같이 맵 형식으로 받을 수도 있다
> input : uri?name=value1&email=value2
```java
@RequestMapping(value="/map")
public String getmap(@RequestParam Map<String,String> param){
    StringBuilder sb = new StringBuilder();
    param.entrySet().forEach(pa -> sb.append(pa.getKey()+" : "+pa.getValue()+"<br>"));
    return sb.toString();
}
```

## DTO(Data Transfer Object)를 이용한 GET 구현

```java
@GetMapping(value="/request3")
public String getRequestParam3(CustomDto dto){
    return dto.toString();
}
```
쿼리스트링의 키와 값들이 Dto의 필드에 매칭됨

## @RequestBody로 POST 등에서 Body 입력 받기

Post패킷의 Body를 해석해서 MemberDTO를 만듦

```java
@PostMapping(value="/post")
public String postBody(@RequestBody MemberDto mem){
    return mem.toString();
}
```
아래처럼 Map 형태로도 Body를 파싱 가능
```java
@PostMapping("/member")
public String postMember(@RequestBody Map<String,Object> postData){
    StringBuilder sb=new StringBuilder();
    postData.entrySet().forEach(ent->{
        sb.append(ent.getKey()+" : "+ent.getValue()+"\n");
    });
    return sb.toString();
}
```

## Body의 리턴
아래 예시처럼 Dto를 리턴하면 리턴 패킷의 HTTP 헤더에서 일반 문자열을 나타내는 `text/plain` 대신 `application/json` 로 content-type이 바뀜

```java
@PutMapping("/dto2")
public CustomDto putDto2(@RequestBody CustomDto dto){
    return dto;
}
```

## ResponseEntity를 활용해서 리턴
RequestEntity와 ResponseEntity는 HttpEntity를 상속한 클래스  
아래 예시처럼 쉽게 헤더와 body를 설정할 수 있다.
```java
@PutMapping("/dto3")
public ResponseEntity<CustomDto> putDto3(@RequestBody CustomDto dto){
    return ResponseEntity.status(200)
            .body(dto);
}
```

## References
https://meetup.nhncloud.com/posts/92
