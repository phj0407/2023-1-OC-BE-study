# 의존성

# **의존과 의존성**

### 의존

객체 간의 의존을 의미한다. 한 클래스(A)가 다른 클래스(B)의 메서드를 실행할 때 (B의 변경이 A에 영향을 끼칠 때) 의존한다고 표현한다 → A가 B에 의존한다. 

**의존 관계의 문제**

- 두 클래스의 결합성이 높다 → 한 클래스가 변경될 때마다 의존 관계에 있는 클래스도 변경해줘야한다. 유연성이 떨어지고, 유지보수 과정에서 문제 유발 가능.
- 객체들 간의 관계가 아닌 클래스 간의 관계가 맺어진다. (SOLID 원칙 중 DIP 원칙 위반)
- DI를 통해 이 문제를 해결한다.

### 의존성 주입 (: DI)

**의존 관계를 외부에서 결정, 주**입해주는 것을 말한다. 인터페이스를 사이에 둬서 클래스 레벨에서는 의존관계가 고정되지 않도록 하고 런타임 시에 관계를 동적으로 주입한다. 의존성 주입 방식에는 **생성자 주입**, 필드 주입, 수정자 주입 등이 있으며, Spring4 이후에는 생성자 주입을 권장한다.

- 생성자 주입 : 생성자의 호출 시점에 1회 호출되는 것이 보장된다. 주입받은 객체가 변하지 않거나, 반드시 객체의 주입이 필요한 경우 사용한다.
- 수정자 주입 : 객체의 Setter를 통해 의존 관계를 주입하는 것. 주입받는 객체가 변경될 가능성이 있는 경우 사용한다. 다만 불필요하게 변경의 가능성을 열어두기 때문에, 생성자 주입을 통해 불변성을 보장하는 것이 좋다.

```java
public class Store(){
		private Product product;
		public Store(Product paramProduct) { this.product = paramProduct; }
}

public class beanFactory{ 
// Pencil 이라는 객체를 만들고, 그 객체를 Store로 주입시켜주는 DI 컨테이너
		public void Store(){
				Product pencil = new Pencil(); // bean 생성
				Store store = new Store(pencil); // 의존성 주입
				}
		}
```

# **@Autowired 의존성 주입**

스프링 컨테이너에 등록한 빈에 의존성 주입을 도와주는 어노테이션. **생성자, 수정자, 필드**를 사용하는 세 가지 방법이 있다. 스프링 컨테이너 빈 모두 등록한 후, @Autowired 어노테이션이 부여된 메서드가 실행되며 필요한 인스턴스를 주입하는 방식으로 이뤄진다. 필드 주입은 제일 간단한 방법으로, 변수에 @Autowired를 붙여서 사용한다 (단점이 너무 많아 사용 x)

### 생성자 주입

Constructor 생성자를 통해 의존 관계를 주입하는 방법. 생성자가 하나일 경우 @Auotowired 를 생략할 수 있다.

- 객체가 생성될 때 딱 한 번 호출되는 것이 보장된다 : **의존관계가 변하지 않는 경우나 필수 의존관계**에 사용한다.
- 의존 관계에 있는 **객체들을 final로 선언**할 수 있다 : 생성자에서 객체를 무조건 설정해줘야하기 때문에 **누락이 발생하지 않는다.**

```java
@Component
public class OrderServiceImpl implements OrderService {
	private final MemberRepository memberRepository;
	private final DiscountPolicy discountPolicy;
	@Autowired
	public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
            this.memberRepository = memberRepository;
            this.discountPolicy = discountPolicy;
	}
/*
의존해야하는 객체를 우선 private final 로 선언 →생성자를 통해 객체 주입받도록 설정 
→ 어노테이션 추가
*/
```

- [final : 여러 Context에서 한 번만 할당될 수 있는 entity.](https://advenoh.tistory.com/13)
    - final 변수
        - 원시 타입 : 한번 초기화된 변수는 변경할 수 없는 상수값
        - 객체 타입 : 그 변수에 다른 참조 값을 지정할 수 없다. 원시 타입과 동일하게 한번 쓰여진 변수는 재변경 불가. **단, 객체 자체가 immutable하다는 의미는 아니고,** 객체의 속성은 변경 가능.
        - 메서드 인자 : 메서드 안에서 변수값을 변경할 수 없다.
        - 멤버 변수: 상수값 / write-once 필드가 됨. 생성자 메서드가 끝나기 전 초기화된다.
    - final 메서드 : 상속받은 클래스에서 메서드의 오버라이드가 불가. 구현한 코드의 변경을 원하지 않을 때 사용한다.
    - final 클래스 : 상속 불가, 클래스 그대로 사용해야한다.

### 수정자 주입

스프링 빈을 등록한 후에 @Auotowired가 붙은 수정자를 모두 찾아서 의존 관계를 주입한다. **선택적이고 변화가능한 의존 관계**에 사용한다. 

```java
@Component
public class OrderServiceImpl implements OrderService {
	private final MemberRepository memberRepository;
	private final DiscountPolicy discountPolicy;
	@Autowired
	public void setMemberRepository (MemberRepository memberRepository) {
            this.memberRepository = memberRepository;
}
	@Autowired
public void setDiscountPolicy (DiscountPolicy discountPolicy) {
            this.discountPolicy = discountPolicy;
	}					

//의존해야하는 객체를 private final로 선언, Setter메서드 위에 @Auotowired를 적는다
```

# **DIP (의존 역전 원칙)**

객체 지향 5원칙 (SOLID) 중 하나로, 어떤 클래스를 참조해야하는 상황에서 **구체화 (클래스를 직접 참조) 가 아닌 추상화 (추상 클래스 / 인터페이스)에 의존**해야한다는 원칙이다.

하위 모듈의 구체적인 내용에 클라이언트가 의존하게 되면 하위 모듈에 변화가 있을 때마다 **클라이언트나 상위 모듈의 코드를 자주 수정**해야 한다. 때문에 클라이언트(사용자)가 상속 관계로 이루어진 모듈을 가져다 사용할 때, 하위 모듈을 ****************직접 인스턴스를 가져다 쓰지 말아야 한다.**************** 자신보다 변하기 쉬운 것에 의존하던 것을 추상화된 인터페이스나 상위 클래스를 두어 변하기 쉬운 것의 변화에 영향받지 않게 하는 것이 목적이다. 

# **스프링 빈과 스프링 컨테이너란?**

### 스프링 빈

스프링 컨테이너가 생성하고 관리하는 애플리케이션 **객체**. <bean> 엘리먼트로 정의된 객체를 사용하려면 직접 생성하지 말고 스프링 컨테이너로부터 빈을 얻어서 사용해야한다. 스프링은 컨테이너에 빈 인스턴스를 한 개만 저장한다(싱글톤 방식). 빈 이름은 항상 다르게 지정되어야한다.

### 스프링 컨테이너

객체를 생성, 생명주기를 관리하고, 의존관계를 주입하는 컨테이너. 개발자는 new 연산자, 인터페이스 호출, 팩토리 호출 방식으로 객체를 생성하고 소멸시킬 수 있는데, 스프링 컨테이너가 이 역할을 대신한다. 의존관계 등록 시에는 스프링 컨테이너에서 해당하는 빈을 찾고 그 빈과 의존성을 만든다. BeanFactory와 ApplicationContext가 있다.

- BeanFactory : 빈을 관리하는 역할 (등록/생성/조회/반환) getBean() 메서드를 통해 빈을 인스턴스화한다.
- ApplicationContext : BeanFactory의 빈을 관리하는 기능 외에도 부가 기능을 갖는다. 빈을 관리하는 건 BeanFactory와 동일하지만, Context 초기화 시점에 모든 싱글톤 빈을 미리 로드한 후, 애플리케이션을 가동하면 **빈을 지연 없이 받을 수 있다**.
- 생성자 인수로 객체를 전달받은 경우 : controller 클래스를 스프링 빈으로 설정 할 수 있다.
- 예시
    
    ```java
    @Controller
    public class MemberController {
    
        private final MemberService memberService;
    
        @Autowired // 이 MemberController 인스턴스는 MemberService와 의존관계를 가진다.
        public MemberController(MemberService memberService) {
            this.memberService = memberService;
    }
    
    @Service
    public class MemberService {
    
        private final MemberRepository memberRepository;
        ...
    }
    ```
    

### 빈 등록 방법

- 빈 등록 방법1. 컴포넌트 스캔
    
    ```java
    @Component
    public @interface Service {
    }
    ```
    
- 빈 등록 방법2. 빈을 직접 등록
    
    ```java
    @Configuration
    public class SpringConfig {
    
        @Bean
        public MemberService memberService(){
            return new MemberService(memberRepo());
        }
    }
    ```
    

### 빈 관련 메서드

- `getBeanDefinitionNames()` : 스프링에 등록된 모든 빈 이름을 조회
- `getBean()` : 특정 스프링빈을 조회하는데 사용.
    - `tmp.getBean(클래스 타입)`
    - `tmp.getBean(빈 이름, 클래스 타입)`
    - `tmp.getBean(빈 이름)`
- `getBeansOfType()` : 해당 타입에 해당하는 모든 빈을 조회

# **JDBC와 JPA**

### java application, JDBC, DB의 연결 구조

![Untitled](%E1%84%8B%E1%85%B4%E1%84%8C%E1%85%A9%E1%86%AB%E1%84%89%E1%85%A5%E1%86%BC%2081e85b623c18410397cb009d1ebbc63f/Untitled.png)

- java app에서 JDBC API를 임포트하고, JDBC Driver는 데이터베이스로의 접근, 클라이언트의 쿼리를 DB로 전달, 쿼리 결과를 클라이언트에 반환하는 프로토콜의 구현체다. 데이터베이스마다 차이가 있는 부분이므로, JDBC Driver는 데이터베이스마다 각각 존재한다.

### JDBC : Java Database Connectivity

DB에 접근할 수 있도록 자바에서 제공하는 API. Plain JDBC와 Spring JDBC가 있다. DriverManager를 사용하여 각 드라이버들을 로딩, 해제한다.

![Untitled](%E1%84%8B%E1%85%B4%E1%84%8C%E1%85%A9%E1%86%AB%E1%84%89%E1%85%A5%E1%86%BC%2081e85b623c18410397cb009d1ebbc63f/Untitled%201.png)

- **DB 접근을 위해서 쿼리문을 작성, 데이터베이스에서 데이터를 가져온 다음, 해당 데이터를 POJO 클래스에 넣는 작업**을 모두 개발자가 수행한다. ORM을 사용하면 ****이러한 작업을 **JPA가 내부에서 수행**하고 프로그래머는 그 결과로 생성된 POJO 클래스를 곧바로 이용할 수 있게 된다.
- SQL Mapper : SQL query문을 통해 직접 데이터베이스 데이터를 다루는 것.  자주 사용할 **쿼리문을 XML파일로 등록**해둔 후(쿼리문 맵핑), **DAO에서 XML파일을 호출**하면, 거기에 **등록된 쿼리문이 작동해서 실제 DB를 조작**하게 된다. Mybatis와 JdbcTemplate이 있다.
    - JDBCtemplate : Spring JDBC접근 방법 중 하나. 개발자가 datasource 설정, SQL 작성, 결과 처리 수행에만 집중할 수 있어 **Plain JDBC에 비해 코드가 훨씬 간결**해진다. 하지만 SQL query문을 작성해야하기 때문에 객체 지향 개발이 어렵다.
- ORM (Object-Relational Middleware) : SQL DB가 어떠한 Key와 Value를 가져야 하는지 Java로 맵핑한다 (**DB를 맵핑**) . 객체 간의 관계를 바탕으로 SQL을 자동으로 생성해준다 → SQL query문을 작성할 필요가 없으며(**자바 코드로 DB에 접근, 조작이 가능**) 객체 중심 개발이 가능하다.
- JDBC를 활용한 프로그램에서는 **쿼리문을 개발자가 직접 작성**하고, JDBC는 쿼리 수행을 전달해 줄 뿐이다. 이러한 번거로운 과정을 제거하고 DBMS - 프로그램 간 의존성을 걷어내기 위해 등장한 것이 JPA다.

### JPA : Java Persistent API

![Untitled](%E1%84%8B%E1%85%B4%E1%84%8C%E1%85%A9%E1%86%AB%E1%84%89%E1%85%A5%E1%86%BC%2081e85b623c18410397cb009d1ebbc63f/Untitled%202.png)

ORM 사용을 지원하는 API. EntityManager를 통해 엔티티를 영속성 컨텍스트에 저장하고 객체와 테이블을 매핑하여 객체 지향 개발이 가능하다. 

- Persistent란?
    
    (영속성) : 데이터를 생성한 프로그램의 실행이 종료되더라도 사라지지 않는 데이터의 특성이다.
    
    - persistent layer : 프로그램 아키텍쳐에서 데이터에 영속성을 부여해주는 계층. Persistence framework를 주로 이용한다.
    - persistent framework : Persistence framework는 JDBC 프로그래밍의 복잡함이나 번거로움없이 간단한 작업만으로 데이터베이스와 연동되는 시스템을 빠르게 개발할 수 있고 안정적인 구동을 보장한다. Persistence framework는 크게 SQL Mapper와 ORM으로 나뉜다.

### Spring Data JDBC

![Untitled](%E1%84%8B%E1%85%B4%E1%84%8C%E1%85%A9%E1%86%AB%E1%84%89%E1%85%A5%E1%86%BC%2081e85b623c18410397cb009d1ebbc63f/Untitled%203.png)

Spring 진영에서 DB를 쉽게 다루기 위해 시작한 프로젝트인 Spring Data중 하나로, 기본적인 드라이버 설정 기능부터 CRUD 기능을 제공한다. ORM이 제공하는 기본적인 기능을 제공할 뿐만 아니라, 사용자가 직접 SQL문을 질의하는 기능 역시 제공한다(@Query 어노테이션을 사용하면 된다).

### 결론

![Untitled](%E1%84%8B%E1%85%B4%E1%84%8C%E1%85%A9%E1%86%AB%E1%84%89%E1%85%A5%E1%86%BC%2081e85b623c18410397cb009d1ebbc63f/Untitled%204.png)

- 참고
    
    [https://code-lab1.tistory.com/122](https://code-lab1.tistory.com/122)
    
    [https://advenoh.tistory.com/13](https://advenoh.tistory.com/13)
    
    [https://velog.io/@im_lily/JDBC와-JPA](https://velog.io/@im_lily/JDBC%EC%99%80-JPA)
    
    [https://skyblue300a.tistory.com/7](https://skyblue300a.tistory.com/7)