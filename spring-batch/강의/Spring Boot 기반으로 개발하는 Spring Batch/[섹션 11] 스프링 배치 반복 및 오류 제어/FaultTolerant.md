### FaultTolerant

- FaultTolerant

  > 오류가 발생하는 경우 장애를 처리하는 기능을 제공한다.
  >
    - skip
        - 오류가 발생해도 아무런 처리를 하지 않고 넘어간다.
        - ItemReader / ItemProcessor / ItemWriter 모두 적용 가능하다.
    - retry
        - 오류가 발생하는 경우 해당 작업을 재시도한다.
        - ItemProcessor / ItemWriter 에만 적용 가능하다.


- 구조

  ![image](https://github.com/ulimy/study/assets/18046394/872c981d-2c79-4cb5-8bfe-d1453fb20a7e)

    - SimpleXXX 클래스를 상속하여 기능을 확장한 형태로 구현되어있다.


- FaultTolerantStepBuilder

  ![image](https://github.com/ulimy/study/assets/18046394/7fee844e-74e2-43d0-8592-5a0aba82367e)


- 실행 흐름

  ![image](https://github.com/ulimy/study/assets/18046394/9d78f324-6d5c-4a1e-ac0f-7312091fa87f)

    1. ItemReader에서 예외 발생
        - skip Limit 만큼 예외를 건너뛴다.
    2. ItemProcessor / ItemWriter 과정에서 예외 발생
        - retry limit 만큼 재시도한다.
        - skip limit 만큼 예외를 건너뛴다.
