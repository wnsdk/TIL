# Spring 기본 이론

## Spring의 탄생
스프링 탄생 전에는 EJB가 쓰였는데 불편함이 많았다. 
2002년 로드 존슨이 'J2EE Design and Development'라는 책을 통해 EJB의 문제점을 지적하며, EJB 없이도 충분히 고품질의 확장 가능한 애플리케이션을 개발할 수 있음을 보여주었다. 이는 스프링의 시작점이 되었다.

<br>

## Spring 프레임워크
- 핵심 기술
    - 스프링 DI 컨테이너
    - AOP
    - 이벤트 등
- 웹 기술
    - Spring MVC, Spring WebFlux
- 데이터 접근 기술
    - 트랜잭션
    - JDBC
    - ORM 지원
    - XML 지원
- 기술 통합
    - 캐시
    - 이메일
    - 원격접근
    - 스케줄링

    <br>

## Spring의 핵심
스프링은 자바 언어 기반 프레임워크로, 자바라는 객체지향 언어가 가진 강력한 특징을 살려내는 프레임워크이다. 즉 스프링은 <b>좋은 객체지향</b> 애플리케이션 개발을 도와주는 프레임워크이다. 

<br>

## 객체지향
### 객체지향
확장과 변경, 유지보수가 용이하다.

### 객체지향의 특징
- 추상화
- 캡슐화
- 상속
- 다형성 polymorphism
    - 역할(인터페이스)과 구현(구현 클래스)으로 세상을 구분
    - 장점
        - 클라이언트는 대상의 역할만 알면 된다.
        - 클라이언트는 구현 대상의 내부 구조를 몰라도 된다.
        - 클라이언트는 구현 대상이 변경되어도 영향을 받지 않는다.
    - 한계
        - 역할(인터페이스) 자체가 변하면 서버, 클라이언트 모두에 큰 변경이 발생한다. (따라서 인터페이스를 안정적으로 잘 설계하는 것이 중요하다.)
    - 스프링은 다형성이 가장 중요하다. 제어의 역전(IoC), 의존관계 주입(DI)은 다형성을 활용해서 역할과 구현을 편리하게 다룰 수 있도록 지원한다.

### 좋은 객체지향 설계의 5가지 원칙 (SOLID)
- SRP 단일 책임 원칙 (Single Responsibility Principle)
    - 한 클래스는 하나의 책임만 가져야한다.
    - 하나의 책임의 범주가 다소 애매한데, 중요한 기준은 <b>변경</b>이다.
- OCP 개방-폐쇄 원칙 (Open/Closed Principle)
    - 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.
    - *다형성 만으로는 지킬 수 없음.
- LSP 리스코프 치환 원칙 (Liskov Substitution Principle)
    - 하위 클래스는 인터페이스 규약을 다 지켜야한다.
    - 다형성을 지원하기 위한 원칙으로, 인터페이스를 구현한 구현체를 믿고 사용하기 위해 이 원칙이 필요하다.
- ISP 인터페이스 분리 원칙 (Interface Segregation Principle)
    - 범용 인터페이스 하나 보다는, 인터페이스 여러 개로 쪼개는 것이 낫다.
- DIP 의존관계 역전 원칙 (Dependency Inversion Principle)
    - 추상화(인터페이스)에 의존해야지, 구체화(구현 클래스)에 의존하면 안 된다.
    - *다형성 만으로는 지킬 수 없음.

<br>

## IoC와 DI

### 구현클래스가 아닌 인터페이스에 의존하게 하기
```java
public class OrderServiceImpl implements OrderService {
    //private final DiscountPolicy discountPolicy = new RateDiscountPolicy();
    private DiscountPolicy discountPolicy;
}
```

```java
@Configuration
public class AppConfig {
    @Bean
    public OrderService orderService() {
        return new OrderServiceImpl(new RateDiscountPolicy());
    }
}
```

### 제어의 역전 IoC
- Inversion of Control
- 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 외부에서 관리하는 것
- 프레임워크 vs 라이브러리
    - 프레임워크가 내가 작성한 코드를 제어하고, 대신 실행하면 그것은 프레임워크가 맞다. (JUnit)
    - 반면에 내가 작성한 코드가 직접 제어의 흐름을 담당한다면 그것은 프레임워크가 아니라 라이브러리다.

### 의존관계 주입 DI
- Dependency Injection
- 애플리케이션 실행 시점(런타임)에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결 되는 것
- 의존관계 주입을 사용하면 클라이언트 코드를 변경하지 않고, 클라이언트가 호출하는 대상의 타입 인스턴스를 변경할 수 있다.
- 객체 인스턴스를 생성하고, 그 참조값을 전달해서 연결된다.

### DI 컨테이너
- AppConfig 처럼 객체를 생성하고 관리하면서 의존관계를 연결해주는 것을 IoC 컨테이너 또는 DI 컨테이너라고 한다.
- 최근에는 주로 DI 컨테이너라 한다.

### 스프링 컨테이너
- ApplicationContext를 스프링 컨테이너라 한다.
- 스프링 컨테이너는 @Configuration 이 붙은 AppConfig 를 설정(구성) 정보로 사용한다. 여기서 @Bean 이라 적힌 메서드를 모두 호출해서 반환된 객체를 스프링 컨테이너에 등록한다. 이렇게 스프링 컨테이너에 등록된 객체를 스프링 빈이라 한다.
- 스프링 빈은 @Bean 이 붙은 메서드의 명을 스프링 빈의 이름으로 사용한다.
- 이전에는 개발자가 필요한 객체를 AppConfig 를 사용해서 직접 조회했지만, 이제부터는 스프링 컨테이너를 통해서 필요한 스프링 빈(객체)를 찾아야 한다. 스프링 빈은 applicationContext.getBean() 메서드를 사용해서 찾을 수 있다.
- 기존에는 개발자가 직접 자바코드로 모든 것을 했다면 이제부터는 스프링 컨테이너에 객체를 스프링 빈으로 등록하고, 스프링 컨테이너에서 스프링 빈을 찾아서 사용하도록 변경되었다.

<br>

