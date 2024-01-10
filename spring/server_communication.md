# server communication

스프링에서 REST api를 호출하는 방법들

## 1. RestTemplate

공식 문서에서는 modern api를 지원하는 restclient를 추천하고 있지만 현업에서 지금까지 많이 쓰였다고 한다.

### Get 쿼리 보내기

```java
@Service
public class RestTemplateService {
    public String getNameWithPathVariable() {
        URI uri = UriComponentsBuilder
            .fromUriString("http://localhost:9090")
            .path("/api/v1/crud-api/{name}")
            .encode()
            .build()
            .expand("Flature") // 복수의 값을 넣어야할 경우 , 를 추가하여 구분
            .toUri();

        RestTemplate restTemplate = new RestTemplate();
        ResponseEntity<String> responseEntity = restTemplate.getForEntity(uri, String.class);

        return responseEntity.getBody();
    }
}
```

map 형태의 쿼리의 경우 .queryParam(key,value) 를 추가하면 된다.

### POST 쿼리 보내기

```java
public ResponseEntity<MemberDto> postWithParamAndBody() {
    URI uri = UriComponentsBuilder
        .fromUriString("http://localhost:9090")
        .path("/api/v1/crud-api")
        .queryParam("name", "Flature")
        .queryParam("email", "flature@wikibooks.co.kr")
        .queryParam("organization", "Wikibooks")
        .encode()
        .build()
        .toUri();

    MemberDto memberDto = new MemberDto();
    memberDto.setName("flature!!");
    memberDto.setEmail("flature@gmail.com");
    memberDto.setOrganization("Around Hub Studio");

    RestTemplate restTemplate = new RestTemplate();
    ResponseEntity<MemberDto> responseEntity = restTemplate.postForEntity(uri, memberDto,
        MemberDto.class);

    return responseEntity;
}
```

### RestTemplate 커스텀 설정

잘 안됨  
org.apache.httpcomponents:httpclient에 메소드가 최신버전에서 없어짐

## 2. WebClient 이용

특징

 - 논블로킹 I/O 지원
 - 리액티브 스트림의 back-pressure 지원
 - 적은 하드웨어 리소스로 동시성 지원
 - 함수형 API, 동기/비동기 상호작용 지원

```java
public String getNameWithPathVariable() {
    WebClient webClient = WebClient.create("http://localhost:9090");

    ResponseEntity<String> responseEntity = webClient.get()
        .uri(uriBuilder -> uriBuilder.path("/api/v1/crud-api/{name}")
            .build("Flature"))
        .retrieve().toEntity(String.class).block();

    ResponseEntity<String> responseEntity1 = webClient.get()
        .uri("/api/v1/crud-api/{name}", "Flature")
        .retrieve()
        .toEntity(String.class)
        .block();

    return responseEntity.getBody();
}
```


## TODO

WebFlux와 연관해서 webclient도 공부할 필요는 있어보임

httpclient 대신 새로 등장한 restclient 익히기



























