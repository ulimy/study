### Jdbc&Jpa CursorItemReader

- JdbcCursorItemReader

  > Cursor 기반의 Jdbc 구현체
  >
    - 스레드 안정성을 보장하지 않기 때문에 멀티 스레드 환경에서 사용할 경우 동시성 문제가 발생할 수 있다.


- JdbcCursorItemReader 생성 빌더

  ![image](https://github.com/ulimy/study/assets/18046394/30333052-60e9-4a1f-8d01-e18497cd32fc)

    - fetchSize와 chunkSize와 동일하게 지정하는 것이 좋다.

      ⇒ chunkSize 단위로 트랜잭션이 동작하기 때문!


- JdbcCursorItemReader의 실행 흐름

  ![image](https://github.com/ulimy/study/assets/18046394/09882176-1d95-4122-9b0d-9abcd10358e3)

    1. open()
        - ItemStream을 통해 커서를 오픈한다.
        - 이때 데이터를 가져올 준비가 되는 것!
        - 대부분은 JdbcCursorItemReader에 ItemStream이 구현되어있기 때문에 JdbcCursorItemReader가 ItemStream을 호출한다.
    2. read()
        - 하나의 커서를 읽도록 RowMapper에게 지시한다.
        - RowMapper는 데이터베이스를 읽어 ResultSet에 담는다.
    3. close()
        - ItemStream을 통해 커서를 닫는다.


- JpaCursorItemReader

  > Cursor 기반의 Jpa 구현체
  >
    - 스프링 배치 4.3부터 지원함 주의!


- JpaCursorItemReader 생성 빌더

  ![image](https://github.com/ulimy/study/assets/18046394/ab7234d1-08c3-4f6d-8b1a-52d058f1c122)


- JpaCursorItemReader의 실행 흐름

  ![image](https://github.com/ulimy/study/assets/18046394/93a11808-7e3a-4e45-a156-ae9ec8e6b63e)

    1. open()
        - ItemStream을 통해 커서를 오픈한다.
        - 이때 EntityManager를 생성한다.
        - 데이터베이스 접근 뿐만 아니라 데이터를 가져와 ResultStream에 담는 작업까지 이루어진다.
    2. read()
        - ResultStream에서 next()로 다음 데이터를 뽑아온다.
        - 여기서 데이터베이스 접근하는 것이 아님!! open()에서 이미 가져왔다.
    3. close()
        - ItemStream을 통해 EntityManager를 종료한다.
