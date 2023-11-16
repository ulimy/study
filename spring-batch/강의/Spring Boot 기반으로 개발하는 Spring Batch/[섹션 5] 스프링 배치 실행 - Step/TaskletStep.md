### TaskletStep

- TaskletStep

  > Step의 구현체로써 Tasklet을 실행시키는 도메인 객체
  >
    - RepeatTemplate을 사용하여 Tasklet의 구분을 트랜잭션 경계 내에서 반복실행한다.


- Task 기반 TaskletStep

  ![image](https://github.com/ulimy/study/assets/18046394/0f5556c3-731c-4829-802d-6c6d154f51d2)

    - 단일 작업으로 실행한다.
    - 주로 Tasklet 구현체를 만들어 사용한다.


- Chunk 기반 TaskletStep

  ![image](https://github.com/ulimy/study/assets/18046394/e1387769-4dd8-4d63-af1e-3296dfbe00b9)

    - 하나의 큰 덩어리를 n개씩 ( chunk 단위로 ) 나눠 실행한다.
    - 대량 처리를 하는 경우 효과적이다.
    - ItemReader, ItemProcessor, ItemWriter를 사용한다.
    - 청크기반 전용 Tasklet인 ChunkOrientedTasklet 구현체가 제공된다.


- TaskletStep생성을 위한 StepBuilderFactory의 주요 메서드

  ![image](https://github.com/ulimy/study/assets/18046394/3edbc49d-885c-4f83-adc9-41f1bb358f1d)

    - `tasklet(Tasklet)`
        - TakletStepBuilder를 반환한다.

    - `startLimit(int)`
        - Step의 실행 횟수 지정
        - 해당 숫자를 초과하여 실행될 경우 예외가 발생한다.

    - `allowStartIfComplete(boolean)`
        - Job이 재시작 될 때 이미 완료된 Step은 자동으로 skip된다.
        - 이때, Step의 성공 실패 여부에 상관없이 항상 Step을 실행할 수 있도록 하는 설정값이다.
        - true : 항상 재실행!

    - `listener(StepExecutionListener)`
        - Step의 lifecycle 중에 특정 작업을 수행할 수 있도록 listener를 셋팅한다.


- Tasklet

  > Step 내에서 구성되고 실행되는 도메인 객체. 단일 태스크를 수행하기 위해 이용된다.
  >
    - 하나의 Step에는 하나의 Tasklet만 설정할 수 있다.
    - RepeatStatus

      > Tasklet의 반복 여부를 나타내는 상태값
      >
        - `FINISHED` : 종료
            - null을 반환해도 해당 값으로 인식된다.
        - `CONTINUABLE`  : 반복

    ```java
    RepeatStatus execute(StepContribution, ChunkContext);
    ```


- TaskletStep 실행 흐름

  ![image](https://github.com/ulimy/study/assets/18046394/c49eed8e-7e44-4d68-b101-33e7786c652e)

  1. 배치 실행 상태 값 변경
      1. 시작상태 :  BatchStatus.*STARTED*
      2. 종료상태 : ExitStatus.*EXECUTING*
  2. Step 실행 전에 실행되는 Listener 호출
  3. RepeatStatus.CONTINUABLE인 동안 ( == 반복이 종료되지 않는 동안 ) ****Tasklet 실행
  4. RepeatStatus.FINISHED 라면 Tasklet 종료
  5. Step 실행 후에 실행되는 Listener 호출
  6. Step, 배치 실행 상태값 변경
      1. BatchStatus.*COMPLETED*
      2. ExitStatus.COMPLETED
  7. 추가적인 ExitStatus 상태값 변경

     ⇒ 의도적으로 ExitStatus를 조정할 수 있다. 이를 위한 기능이 제공되어있는 것!
