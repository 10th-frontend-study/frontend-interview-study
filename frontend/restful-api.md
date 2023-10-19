## REST 의 정의

> **REST(Representational State Transfer)**

- 자원을 **이름으로 구분**하여 해당 자원의 상태(정보)를 주고받는 모든 것을 의미
- 즉, 자원의 표현에 의한 상태 전달
    - **자원의 표현**
        - 자원 : 해당 소프트웨어가 관리하는 모든 것
        - 자원의 표현 : 그 자원을 표현하기 위한 이름
        - -> Ex) DB의 학생 정보가 자원일 때, 'students'를 자원의 표현으로 정함
    - **상태(정보) 전달**
        - 데이터가 요청되어지는 시점에 자원의 상태(정보)를 전달
        - JSON 혹은 XML를 통해 데이터를 주고 받는 것이 일반적
- 기본적으로 웹의 기존 기술과 HTTP 프로토콜을 그대로 활용하기 때문에 웹의 장점을 최대한 활용할 수 있는 아키텍쳐 스타일
- 네트워크 상에서 **Client 와 Server 사이의 통신 방식** 중 하나

### REST 구성 요소

#### 1. 자원(Resource): URI
* 모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재한다.
* 자원을 구별하는 ID는 ‘/groups/:group_id’와 같은 HTTP URI 다.
* Client는 URI를 이용해서 자원을 지정하고 해당 자원의 상태(정보)에 대한 조작을 Server에 요청한다.
#### 2. 행위(Verb): HTTP Method
* HTTP 프로토콜의 Method를 사용한다.
* HTTP 프로토콜은 `GET`, `POST`, `PUT`, `DELETE` 와 같은 메서드를 제공한다.
#### 3. 표현(Representation of Resource)
Client가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답(Representation)을 보낸다.
REST에서 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation으로 나타내어 질 수 있다.
JSON 혹은 XML를 통해 데이터를 주고 받는 것이 일반적이다.

### REST 의 장단점
#### 장점
  - HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구축할 필요가 없다.
  - HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해준다.
  - HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
  - REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
  - 서버와 클라이언트 역할을 명확하게 분리
#### 단점
  - 표준이 존재하지 않음
  - 사용할 수 있는 메소드가 4가지(HTTP Method) 뿐
  - 구형 브라우저에서 아직 제대로 지원해주지 못하는 부분이 존재
      - PUT, DELETE를 사용하지 못하는 점

<br/><br/>

## REST API
##### API란
* 데이터와 기능의 집합을 제공하여 컴퓨터 프로그램간 상호작용을 촉진하며, 서로 정보를 교환가능 하도록 하는 것
##### REST API의 정의
- REST 기반으로 서비스 API를 구현한 것
- OpenAPI(누구나 사용할 수 있는 공개된 API) 는 대부분 REST API를 제공

### REST API의 설계 기본 규칙

REST API 설계 시 가장 중요한 항목은 다음의 2가지이다.

**첫 번째,** URI는 정보의 자원을 표현해야 한다.  
**두 번째,** 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다.

#### 1) URI는 정보의 자원을 표현해야 한다. (리소스명은 동사보다는 명사를 사용)

```
    GET /members/delete/1
```

위와 같은 방식은 REST를 제대로 적용하지 않은 URI이다. URI는 자원을 표현하는데 중점을 두어야 하는데, delete와 같은 행위에 대한 표현이 들어가서는 안된다.


#### 2) 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE 등)로 표현

위의 잘못 된 URI를 HTTP Method를 통해 수정해 보면

```
    DELETE /members/1
```

이 된다.

회원정보를 가져올 때는 GET, 회원 추가 시의 행위를 표현하고자 할 때는 POST METHOD를 사용하여 표현한다.


* ***회원정보를 가져오는 URI**

```
    GET /members/show/1     (x)
    GET /members/1          (o)
```

* ***회원을 추가할 때**

```
    GET /members/insert/2 (x)  - GET 메서드는 리소스 생성에 맞지 않습니다.
    POST /members/2       (o)
```

<br/>

**[참고]HTTP METHOD의 알맞은 역할**  
POST, GET, PUT, DELETE 이 4가지의 Method를 가지고 CRUD를 할 수 있습니다.

|METHOD|역할|
|---|---|
|POST|POST를 통해 해당 URI를 요청하면 리소스를 생성합니다.|
|GET|GET를 통해 해당 리소스를 조회합니다. 리소스를 조회하고 해당 도큐먼트에 대한 자세한 정보를 가져온다.|
|PUT|PUT를 통해 해당 리소스를 수정합니다.|
|DELETE|DELETE를 통해 리소스를 삭제합니다.|

다음과 같은 식으로 URI는 자원을 표현하는 데에 집중하고 행위에 대한 정의는 HTTP METHOD를 통해 하는 것이 REST한 API를 설계하는 중심 규칙입니다.


### REST API 설계 규칙

1. 슬래시 구분자(/)는 계층 관계를 나타내는데 사용한다.
    
    ```javascript
    http://restapi.example.com/houses/apartments
    ```
    
2. URI 마지막 문자로 슬래시를 포함하지 않는다
    - URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 하며 URI가 다르다는 것은 리소스가 다르다는 것이고, 역으로 리소스가 다르며 URI도 달라져야 한다.
        
        ```javascript
        http://restapi.example.com/houses/apartments/ (x)
        ```
        
3. 하이픈(-) 은 URI 가독성을 높이는데 사용
    - 불가피하게 긴 URI 경로를 사용하게 된다면 하이픈을 사용해 가독성을 높인다.
4. 밑줄(_)은 사용하지 않는다.
    - 밑줄은 보기 어렵거나 밑줄 때문에 문자가 가려지기도 하므로 가독성을 위해 밑줄은 사용 x
5. URI 경로에는 소문자가 적합
    - URI 경로에 대문자 사용은 피하도록 한다
    - RFC 3986(URI 문법 형식)은 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정하기 때문
6. 파일 확장자는 URI에 포함하지 않는다.

#### REST API 설계 예시  
![](https://velog.velcdn.com/images%2Fseokkitdo%2Fpost%2Fee310168-ede2-491d-a01a-c69e4327fc74%2FRESTAPI.PNG)

<br/><br/>

## RESTful이란

- RESTful은 일반적으로 REST라는 아키텍쳐를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어이다.
    - REST API를 제공하는 웹 서비스를 RESTful하다고 할 수 있다.
- RESTful은 REST를 REST 답게 쓰기 위한 방법으로, 누군가가 공식적으로 발표한 것이 아니다.
    - 즉, REST 원리를 잘 따르는 시스템은 RESTful 용어로 지칭된다.

### RESTful의 목적

- 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것

### RESTful하지 못한 경우

- CRUD 기능을 모두 POST 로만 처리하는 API
- route에 resource, id 외의 정보가 들어가는 경우