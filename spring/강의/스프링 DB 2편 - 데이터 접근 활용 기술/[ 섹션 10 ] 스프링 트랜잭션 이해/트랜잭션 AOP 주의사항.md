# 트랜잭션 AOP 주의사항 

### 프록시 내부 호출


<img width="1180" alt="image" src="https://github.com/user-attachments/assets/bcaec65f-7736-4dbe-a7bd-cc66e1fe9e9a">


- 트랜잭션은 AOP를 통해 구현되어있기 때문에, 언제나 프록시 객체를 통해 호출된다.
- 하지만 대상 객체의 내부 메서드를 통해 호출한다면,
  **프록시 없이 직접 메서드가 실행되므로 트랜잭션이 적용되지 않음에 주의해야한다!!**
    - 위 사진에서 external() 에서 internal() 을 호출한 경우
- 어떻게 해결할 수 있을까?
    - (가장 단순) internal()을 별도의 클래스로 분리한다.

### public 메서드

- 스프링에서 @Transactional AOP 기능은 **public 메서드에서만 사용 가능**하다.
    - AOP는 원래 public, protected, default 에서 사용 가능하지만,
      @Transactional 만 public으로 제한해두었다.
    - 이는 프록시 내부 호출과는 무관하게, 스프링이 막아둔 것이다.
    - **그럼에도 선언했다면, 예외가 발생하는 것이 아니라 그냥 무시됨에 주의해야한다!!**
    - FYI ) 스프링 부트 3.0 부터는 protected, default까지 모두 사용가능해졌다.
- WHY?
    - 트랜잭션은 주로 비즈니스 로직의 시작점에 걸어야한다.
    - 해당 지점은 주로 클래스에서 public 하게 열려있는 메서드이다.
    - 따라서 의도하지 않은 곳까지 트랜잭션이 과도하게 적용되는 것을 막기 위함이다.

### 초기화 시점

- **초기화 코드 ( ex. @PostConstruct ) 에 @Transactional을 사용하면 적용되지 않는다.**
- 초기화 코드 동작 이후 트랜잭션 AOP가 실행되기 때문이다!
