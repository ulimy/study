### Repeat

- Repeat

  > 특정 조건이 충족/미충족 될 때 까지 Job/Step을 반복한다.
  >
    - 기본 구현체로 RepeatTemplate을 제공한다.
    - RepeatTemplate의 구조

      ![image](https://github.com/ulimy/study/assets/18046394/dfc08029-377d-4db2-9862-3a1b67e8dd3b)

        - ChunkProvider가 가지는 RepeatTemplate
            - ItemReader를 통해 반복적으로 데이터를 읽을 수 있도록 한다.
        - Step이 가지는 RepeatTemplate
            - Tasklet을 반복적으로 실행될 수 있도록 한다.


- Repeat 종료 여부를 결정하는 항목 - 1) RepeatStatus
    - Enum형태이다.
    - COTINUABLE : 반복 중
    - FINISHED : 반복 종료


- Repeat 종료 여부를 결정하는 항목 - 2) CompletionPolicy

  ![image](https://github.com/ulimy/study/assets/18046394/c4025de6-502f-43f1-beb8-cf945bac981d)

    - 인터페이스이다.

      ⇒ 구현체를 통해 커스터마이징 가능!

    - 실행횟수, 완료 시기, 오류 발생 시 수행 할 작업 등을 결정한다.
    - 정상 종료를 알리는데 사용된다.
    - 대표적인 구현체
        - `TimeoutTerminationPolicy` : 소요시간이 설정된 시간보다 크다면 반복 종료
        - `SimpleCompletionPolicy` : 현재 반복 횟수가 Chunk 개수보다 크다면 반복 종료
        - `CountingCompletionPolicy` : 일정한 개수를 집계하여 집계 결과가 제한조건보다 크다면 반복 종료


- Repeat 종료 여부를 결정하는 항목 - 3)ExceptionHandler

  ![image](https://github.com/ulimy/study/assets/18046394/7227c9fe-d79c-4eff-a77b-a32f1ab3f4bc)

    - 인터페이스이다

      ⇒ 구현체 구현을 통해 커스터마이징 가능!

    - RepeatCallback 안에서 예외가 발생하면 RepeatTemplate이 ExceptionHandler를 참조하여 예외 처리를 결정한다.
    - 예외가 발생했을 때
        - 예외를 무시하면 반복을 이어나간다.
        - 예외를 던진다면 반복이 종료된다.
    - 비정상 종료를 알리는데 사용된다.
    - 대표적인 구현체
        - `LogOrRethrowExceptionHandler` : 로그로 기록할지 예외를 다시 던질지 결정
        - `RethrowOnThresholdExceptionHandler` : 지정된 유형의 예외가 설정된 횟수 이상으로 발생된다면 예외 발생
        - `SimpleLimitExcpetionHandler` : 예외가 설정된 횟수를 초과하여 발생된다면 예외 발생


- Repeat의 구조

  ![image](https://github.com/ulimy/study/assets/18046394/b1e95c14-c5f6-492b-8a7d-eb049346c59a)


- 반복이 종료되는 3가지 흐름

  ![image](https://github.com/ulimy/study/assets/18046394/0d2445ee-7b98-4738-8e2f-c244df7768d1)

    1. 예외가 발생하는 경우
        - ExceptionHandler를 통해 설정된 예외 정책에 의해 반복 여부가 결정된다.
    2. 반복 종료 정책에 도래한 경우
        - CompletionPolicy를 통해 설정된 반복 종료 정책에 의해 반복 여부가 결정된다.
    3. RepeatStatus가 FINISHED인 경우
