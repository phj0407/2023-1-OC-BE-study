# Week1

# Sec0. 프로젝트 환경설정

### 1.

- start.spring.io, gradle project, java11
- dependencies : spring web, thymeleaf

### 2. 뷰 환경설정

- main-resources - static - index.html 생성 : welcome page 기능 제공
- main/project / package(controller) 생성
    - 웹 브라우저 : [localhost:8080/hello](http://localhost:8080/hello) 전달→ 스프링 컨테이너에서 해당하는 컨트롤러 탐색 & 매칭 → 없으면 resources 폴더에서 탐색 & 매칭
    
    ```java
    @Controller
    public class HelloController{
    		@GwtMapping("hello")
    		public String hello(Model model){            // model(data:hello)
    				model.addAttribute("data","hello"); 
    				return "hello"  
    				// resources 파일의 html 이름을 찾아 반환 ->뷰 리졸버가 화면을 찾아서 처리한다
    				// viewName 매핑 : resources/templates/이름.html
    ```
    
    - resources/templates/hello.html 생성

### 3. 빌드 & 실행

- cmd → 폴더로 이동 → ./gradlew build : build 폴더 생성
- libs → java -jar {생성된 jar 파일} : 실행
- ./gradlew clean : 클린 빌드, 기존의 build 폴더 삭제

# Sec1. 웹 개발

### 1. 정적 컨텐츠 : Static Contents

- resources/static/hellostatic.html →localhost8080/hellostatic.html
- 웹에서 호출  →스프링 컨테이너에서 관련 컨트롤러 탐색→ 없으면resources폴더의 html 파일을 그대로 호출 & 화면 구성

### 2. MVC와 템플릿 엔진

- 사용자 인터페이스로부터 비즈니스 로직을 분리하기 위한 목적. Controller가 Model을 통해 데이터를 가져오고, 그 데이터를 바탕으로 View가 시각적 표현을 사용자에게 전달한다.
    - ex) 유저가 웹페이지 접속 →컨트롤러가 요청받은 웹페이지를 서비스하기 위해 모델 호출 → 모델이 데이터 소스를 제어한 후 결과를 Return → 컨트롤러는 Return 받은 결과를 View 에 반영 → 데이터가 반영된 View가 유저에게 보여짐.
- Model : 데이터를 가진 객체.
    - 모델의 상태에 변화가 있으면 Controller와 View에 이를 통보함으로써 정보를 업데이트한다.
    - 사용자가 편집하길 원하는 모든 데이터를 가지고 있어야 함
    - View나 Controller에 대해 어떠한 정보도 알지 못해야 한다.
    - 변경이 일어나면 변경 통지에 대한 처리방법을 구현해야함
- View : 사용자가 볼 결과물을 생성하기 위해 모델로부터 정보를 전달받음.
    - 모델의 정보를 따로 저장해서는 안됨
    - Model, Controller의 정보는 알 수 없음
    - 변경 통지 처리방법 구현
- Controller : 사용자가 접근한 URL 에 따라 사용자의 요청 사항을 파악한 후, 요청에 맞는 데이터 Model을 의뢰하고, 데이터를 VIew에 반영해서 사용자에게 알려준다.
    - 모델에 명령을 보냄으로써 View의 상태를 변경할 수 있다
    - Model/View에 대해서 알고, 변경을 모니터링해야한다.
- 장/단점
    - 비즈니스 로직과 UI로직을 분리→독립적 유지보수 가능
    - Model과 View가 다른 컴포넌트에 종속되지 않음 →앱의 확장성, 유연성 보장
    - But Controller에 다수의 Model과 View가 복잡하게 연결되는 상황이 생길 수 있음

```java
@GetMapping("hello-mvc")
public string helloMvc(@RequestParam("name") String name, Model model){
		model.addAttribute("name", name);
		return "hello-template";
}
// - localhost:8080/hello-template?name={ parameter name의 value}
// - resources/templates/hello-template.html 생성
// 전달받은 name 값이 모델에 담김 & html 파일의 key 자리에 그 value 가 들어감
// model(name:spring) (key : value)
```

- Model 1 방식 : JSP가 사용자의 요청을 전부 처리, Controller 영역에 View 영역을 같이 구현한다.
- Model 2 방식 : 컨트롤러(Servlet) 가 요청을 받고 View로 보여줄 것인지 Model 로 보여줄 것인지 판단 후 전송.

### 3. API ([API](https://www.notion.so/API-c485db308ccc40609dfe4fe8aa1bcf8c))

- 객체를 Json 데이터 구조 포맷으로 클라이언트에게 데이터 전송. 화면은 클라이언트가 알아서 그림.
- @ResponseBody → 문자인 경우 그대로 반환, 객체의 경우 json방식 (key:value) 으로 데이터 반환
    - viewResolver 대신 HttpMessageConverter가 동작
- 방식1 : SpringhttpMessageConverter 동작

```java
@GetMapping("hello-string")
@ResponseBody // html 의 body 부에 data를 직접 넣어주겠다는 의미
public String helloString(@RequestParam("name") String name){
	return "helo"+name;
}
```

- 방식2 : 객체를 반환 →MappingJackson2HttpMessageConverter 동작
    - json 방식으로 반환 → key : value
    
    ```java
    @GetMappig("hello-api")
    @ResponseBody // html 의 body 에 json방식으로 객체를 넣어줌
    public Hello helloApi(@RequestParam("name") String name){
    	Hello hello = new Hello();
    	Hello.setName(name);
    	return hello;
    }
    
    static class Hello{
    	private String name;
    	public String getName(){return this.name;}
    	public void setName(String name){
    		this.name = name;
    	}
    ```
    

---

# API(Application Programming Interface)

: **소프트웨어 프로그램(애플리케이션) 내부에 존재하는 기능 및 규칙의 집합**

: **프로그램들이 서로 상호작용하는 것을 도와주는 매개체**

- 서버와 DB에 대한 출입구 역할 → 애플리케이션과 기기의 원활한 통신
- 보안 유지 목적, 허용된 사용자에게만 접근을 허용한다.
- 모든 접속을 표준화 →기계, os에 상관 없이 동일한 액세스를 얻을 수 있다.
- private / public / partner API 등이 존재. 공개 범주에 따라 구분
- **웹 개발**에서 보통 API는 **개발자가 앱을 통해 사용자의 웹 브라우저에서 상호작용할 수 있도록 하는 코드 기능**들( e.g. [methods](https://developer.mozilla.org/ko/docs/Glossary/Method), [properties](https://developer.mozilla.org/ko/docs/Glossary/Property), 이벤트, [URLs](https://developer.mozilla.org/ko/docs/Glossary/URL)), **사용자의 컴퓨터 상에 있는 다른 소프트웨어 및 하드웨어, 또는 서드파티 웹사이트나 서비스의 집합**을 의미한다.

# REST API

- URI 와 HTTP 프로토콜 기반 & 단일 인터페이스 → 단순함이 핵심, 구축과 확장이 간단함.
- 데이터 포맷으로 JSON사용 → 브라우저 간 호환성 좋음, payload 가볍게 할 수 있음
- 웹에 최적화된 API
- 클라이언트와 서버가 통신 가능 & data의 message가 두 지점 사이를 왕복한다.
- 보안 : SSL 지원, HTTPS 프로토콜 이용 가능(HTTP의 보안 버전)
- 서버와 느슨하게 연결된 형태 → 수정, 업데이트가 쉬운 편
    - **추상화 계층이  많을수록 양쪽의 상호작용에서 통제할 수 있는 부분이 줄어들기는 하지만 오히려 구현하기는 쉬우며, 나중에 그 방식을 전부 고치지 않고도 업데이트를 쉽게 할 수 있다.**
- 추가 설명 : [REST](https://www.notion.so/REST-0437efef7c99431da491c11130806cbb)

# SOAP API

- **그 자체로 프로토콜이며, 보안이나 메시지 전송 등에 있어서 REST보다 더 많은 표준들이 정해져있음 → 더 복잡 & 오버헤드가 많다.**
- But 종합적 기능 지원 가능 (보안, 트랜잭션, ACID(원자성, 일관성, 고립성, 지속성)) →웹보다는 기업용 애플리케이션 작업에 적합
- 보안 엄격
- 신뢰성 보장
    - REST : 표준화된 메시징 시스템 X, 통신 장애 시 재시도 only
    - SOAP : **성공/반복 실행 로직이 규정되어있음**
- ACID 준수 →**데이터의 변형 적음**
- DB와 상호작용에 대해 사전에 정확하게 정해둠→ **데이터의 무결성 보장**
- 서버와 긴밀하게 연결 → 업데이트, 수정 어려움

![Untitled](Week1%204e2fdf01219543fbbc42a03d88711dae/Untitled.png)

- 참고
    - [https://developer.mozilla.org/ko/docs/Glossary/API](https://developer.mozilla.org/ko/docs/Glossary/API)
    - [https://blog.wishket.com/soap-api-vs-rest-api-두-방식의-가장-큰-차이점은/](https://blog.wishket.com/soap-api-vs-rest-api-%EB%91%90-%EB%B0%A9%EC%8B%9D%EC%9D%98-%EA%B0%80%EC%9E%A5-%ED%81%B0-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%80/)

---

# REST

- REST (Representational State Transfer) : **자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고 받는** 모든 것.  소프트웨어 개발 아키텍처의 한 형식이자, 네트워크 상에서 클라이언트와 서버 사이의 통신 방식 중 하나다. 기본적으로 웹의 기존 기술과 HTTP 프로토콜을 그대로 활용하기 때문에 웹의 장점을 최대한 활용할 수 있는 아키텍처 스타일이다.
- REST의 구체적인 개념
    - **HTTP URI**(Uniform Resource Identifier)를 통해 **자원**(Resource)을 명시하고, **HTTP Method(POST, GET, PUT, DELETE)**를 통해 **해당 자원에 대한 CRUD Operation**을 적용하는 것을 의미한다.
    - **CRUD Operation** : Create(생성) / Read(조회) / Update(수정) / Delete(삭제) / HEAD(헤더 정보 조회) , 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능
    - **HTTP Method & 예시**
        - POST : 특정 URI를 요청하면 리소스를 생성한다 (Create)
        - GET : 특정 리소스를 조회하고 해당 도큐먼트에 대한 자세한 정보를 가져온다 (Read)
        - PUT : 특정 리소스를 수정한다 (Update)
        - DELETE : 특정 리소스를 삭제한다 (Delete)
        - 예시
        
        | 기능 | HTTP 메서드 | HTTP URL & Body |
        | --- | --- | --- |
        | 모든 회원 정보 조회 | HTTP GET | http://www.study.co.kr/users |
        | 특정 회원 정보 조회 | HTTP GET | http://www.study.co.kr/users/terry |
        | 회원 정보 검색 | HTTP GET | http://www.study.co.kr/users?query=xxx |
        | 회원 등록 | HTTP POST | http://www.study.co.kr/users
        {
        	"name"."terry"
        	:
        } |
        | 회원 삭제 | HTTP DELETE | http://www.study.co.kr/users/terry |
        | 회원 정보 변경 | HTTP PUT | http://www.study.co.kr/users
        {
        	"name":"terry"
        	”address”:”seoul”
        } |
- 구성 요소
    - **자원 (URI)** : 모든 자원은 서버 내에 존재 & 고유ID가 존재한다. 클라이언트는 URI를 이용해서 자원을 지정하고 해당 자원의 상태에 대한 조작을 서버에 요청한다.
    - 행위 (HTTP Method) : HTTP 프로토콜의 Method를 사용한다 (GET, POST, PUT, DELETE 등)
    - 표현 (HTTP Message PayLoad): 자원에 대한 행위의 내용
    - **클라이언트**가 **자원의 상태에 대한 조작을 요청**하면 **서버**가 이에 **적절한 응답**을 보낸다. 자원은 JSON, XML, TEXT, RSS 등 여러 형태로 나타내어질 수 있으며 **JSON/XML** 등을 통해 이뤄지는 것이 일반적이다.

# REST의 특징

- **서버-클라이언트 구조**
    - 자원이 있는 쪽이 Server, 자원을 요청하는 쪽이 Client가 된다.
    REST Server: API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다.
    Client: 사용자 인증이나 context(세션, 로그인 정보) 등을 직접 관리하고 책임진다.
    - 서로 간 의존성이 줄어든다.
- **Stateless (무상태)**
    - Client의 context를 Server에 저장하지 않는다 ; context = 세션, 쿠기 등
    - HTTP 프로토콜이 Stateless Protocol → REST 도 Stateless
    - Server는 각각의 요청을 완전히 별개의 것으로 인식하고 처리한다 →이전 요청이 다음 요청의 처리에 영향을 주지 않는다 (DB 수정에 의해 영향받는 것 제외)
        - Server의 처리 방식에 일관성을 부여하고 부담이 줄어들며, 서비스의 자유도가 높아진다.
- **Cacheable ( 캐시 처리 가능)**
    - 캐시 : 대량의 요청 효율적으로 처리하기 위해 요구된다. 응답시간이 빨라지고 REST server 트랜잭션이 발생하지 않기 때문에 전체 응답시간, 성능, 서버의 자원 효율성 향상.
        - REST server 트랜잭션 : DB의 상태를 변화시키기 위해 수행하는 작업의 단위
    - 웹 표준 HTTP 프로토콜을 그대로 사용 → 웹에서 사용하는 기존 인프라를 그대로 활용할 수 있다 → 캐싱 기능도 적용 가능!
- **Layering (계층화)**
    - Client는 REST API 서버만 호출한다  ; 서버의 각 유형은 클라이언트가 볼 수 없음
    - API 서버는 순수 비즈니스 로직만 수행하고, 여기에 다른 Layer를 추가할 수 있다 → 구조상 유연성 추구.
    - PROXY, Gateway 등 네트워크 기반의 중간 매체도 사용할 수 있다.
- **Uniform Interface (인터페이스 일관성)**
    - URI 로 지정한 Resource에 대한 조작을 통일되고 한정적인 인터페이스로 수행한다.
    - HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용 가능 →특정 언어,기술에 종속되지 않는다.
- Code on Demand (선택적)
    - 요청을 받으면 서버가 스크립트(실행 가능한 코드)를 전송, 클라이언트에서 실행(기능을 확장)한다.

# RESTful API

- REST 기반으로 서비스 API 를 구성한 것. ( RESTful API = REST API 를 제공하는 웹 서비스)
- **이해 및 사용이 쉬운 REST API 를 만들고, 일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는 것이 주 목적이다.**
    - 컨벤션 : 코드 스타일 (코딩 규칙)의 통일
- 특징
    - REST 기반 **시스템 분산**을 통해 **확장성, 재사용성**을 높여 **유지보수 및 운용이 편리**해진다.
    - HTTP 표준을 기반으로 구현하므로, **HTTP를 지원하는 프로그램 언어로 클라이언트, 서버를 구현할 수 있다.**
- 클라이언트, 서버, 리소스로 구성 & 요청이 HTTP를 통해 관리되는 클라이언트-서버 아키텍쳐

# REST API 설계 규칙

### Resource

- 리소스 정의 규칙
    - //{리소스 그룹명}/{리소스 ID}
    - 리소스 간 관계 표현
        - 서브 URL 사용 : GET /owner/{terry}/dogs → 리소스 간 관계가 많지 않고 단순한 경우
        - 관계 표현 리소스를 별도 정의 → 리소스 간 관계가 많고 복잡한 경우
            - /resource/identifier/relation/other-related-resource
            - Get /people/terry/ownsdog/puppy
    - Resource의 행위는 HTTP Method 로 표현
        - URI에 ( HTTP Method, 행위에 대한 동사 표현) 이 들어가면 안됨
        - 경로 부분 중 변하는 부분은 유일한 값으로 대체한다.
    - URI 는 정보의 자원을 표현해야한다.
        - URI : url 의 상위 개념. 웹에 존재하는 모든 자원에 고유한 URI 를 부여해 활용한다.
        - Resource : 명사, 소문자 사용
        - Resource의 도큐먼트 : 단수 명사
        - Resource의 컬렉션 & 스토어 이름 : 복수 명사

---

### 기타

- / : 계층 관계를 나타낸다. URI 마지막 문자로 포함하지 않는다.
- - : URI 가독성을 높이는 데 사용, _는 사용하지 않는다
- URI 경로에는 소문자가 적합
- 파일 확장자는 URI 에 포함하지 않는다.
- 리소스 간 연관 관계가 있는 경우

# 장단점

- HTTP 프로토콜의 인프라, 기능을 그대로 사용
    - HTTP 의 장점을 공유하고, 별도의 인프라를 구축할 필요가 없다.
    - HTTP 표준 프로토콜을 따르는 모든 플랫폼에서 사용 가능
- 범용성 보장
- 메시지가 의도하는 바를 쉽게 파악 할 수 있다
- 서버-클라이언트의 역할 분리

- 표준이 존재하지 않는다 : De-facto 표준
- 사용 가능 메소드가 4가지 뿐 : GET, POST, PUT, DELETE
- 구형 브라우저가 아직 제대로 지원하지 못하는 부분이 존재
- pushState를 지원하지 않는다 ; 페이지를 reload 하지 않고 url 만 변경할 경우 사용

# REST API - 예시

- URI (URL) 에 리소스 (user/강나영) 이 나타나있고, 같은 리소스에 대해 HTTP Method를 다르게 명시함으로써 리소스에 어떤 조작을 수행할 지 알려준다.

![Untitled](Week1%204e2fdf01219543fbbc42a03d88711dae/Untitled%201.png)

![Untitled](Week1%204e2fdf01219543fbbc42a03d88711dae/Untitled%202.png)

- 참고
    - [https://khj93.tistory.com/entry/네트워크-REST-API란-REST-RESTful이란](https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80)
    - [https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)
    - [https://velog.io/@hjkdw95/RESTful-API를-알아봅시다](https://velog.io/@hjkdw95/RESTful-API%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B4%85%EC%8B%9C%EB%8B%A4)