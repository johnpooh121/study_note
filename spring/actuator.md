# Actuator

## 1. Background

스프링 부트 액추에이터는 HTTP 엔드포인트나 JMX를 활용해 앱을 모니터링하고 관리할 수 있게 해줌

## 2. Gradle

```gradle
implementation("org.springframework.boot:spring-boot-starter-actuator")
```

## 3. 액추에이터 설정

기본 액추에이터 경로는 /actuator

다른 경로를 쓰고 싶으면 application.properties에서 다음 줄을 추가하면 됨
```properties
management.endpoints.web.base-path=/actuator
```

엔드포인트들에는 기본 엔드포인트와 Spring MVC, WebFlux에서 추가로 사용할 수 있는 엔드포인트들이 있음

### 엔드포인트의 활성화/비활성화

``` java
management.endpoint.env.enabled=true
```

엔드포인트 별로 디폴트 활성화 여부가 다름

엔드포인트를 사용하려면 노출 여부도 활성화로 해주어야함

```java
management.endpoints.web.exposure.include = *
```
위와 같이 하면 모든 엔드포인트를 HTTP에서 노출시킴  
HTTP 말고 JMX를 통한 노출도 있음

기본적으로 활성화 되어있는 엔드포인트로는 /info, /health, /beans, /conditions, /env 등이 존재

/info는 다른 엔드포인트들과 다르게 커스텀으로 application.properties 등에 추가한 info를 적용하기 위해서 아래와 같이 추가 설정이 필요하다

```java
management.info.env.enabled=true
```

info의 속성에는 사용자가 설정한 env 외에 os, java, build 등이 있다.

## 4. 액추에이터 커스터마이징 - 기존 액추에이터에 추가

인터페이스 InfoContributor를 구현하는 클래스를 만들면 됨

```java
@Component
public class CustomInfoContributor implements InfoContributor {
    @Override
    public void contribute(Info.Builder builder) {
        Map<String,Object> content = new HashMap<>();
        content.put("code-info","InfoContributor 구현체에서 정의한 정보");
        builder.withDetail("custom-info-contributor",content);
    }
}
```

이제 /info에 위 코드에서 put한 내용들이 표시됨

## 5. 액추에이터 커스터마이징 - 새로 엔드포인트 만들기

@Endpoint로 빈에 추가한 객체들은 @ReadOperation, @WriteOperation, @DeleteOperation으로 JMX나 HTTP에 노출시킬 수 있다.

```java
@Component
@Endpoint(id="note")
public class NoteEndpoint {
    private Map<String,Object> noteContent = new HashMap<>();

    @ReadOperation
    public Map<String,Object> getNote(){
        return noteContent;
    }

    @WriteOperation
    public Map<String,Object> writeNote(String key,Object value){
        noteContent.put(key,value);
        return noteContent;
    }

    @DeleteOperation
    public Map<String,Object> deleteNote(String key){
        noteContent.remove(key);
        return noteContent;
    }
}
```

이제 POST로 /actuator/note에 새로 note를 추가하고 get으로 읽거나 delete로 지울 수 있다.
