### Step

- Step
    - 실제 배치 처리를 하는게 필요한 모든 정보를 가지고있는 도메인 객체
    - 작업을 구성하고, 실행하는 세부 사항을 task 기반으로 설정 및 명세한다.


- Step의 구현체
  ![image](https://github.com/ulimy/study/assets/18046394/7e18c33a-ff56-4bb4-a9ae-8c6db3a6f9b1)
  - TaskletStep
      - 가장 기본!
      - Tasklet 타입의 구현체를 제어한다.
  - PartitionStep
      - 멀티스레드 방식으로 Step을 여러개로 분리하여 실행한다.
  - JobStep
      - Step 내에서 Job을 실행하도록 한다.
  - FlowStep
      - Step내에서 Flow를 실행하도록 한다.

      ```java
      public Step flowStep() {
          return this.stepBuilderFactory.get("step")
                  .flow(myFlow())
                  .build();
      ```
    

- Step의 실행 흐름
  ![image](https://github.com/ulimy/study/assets/18046394/114c8977-cc6d-4fd3-bbca-c2f45f8ab88f)

  
- TaskletStep을 활용하는 두가지 방법
    1. 직접 Tasklet 구현체 생성하기

        ```java
        return this.stepBuilderFactory.get("step")
                    .tasklet(myTasklet())
                    .build();
        ```

    2. ChunkOrientedTasklet 이용하기

       > Spring Batch에서 제공하는 Tasklet 인터페이스의 구현체
       >
        - 내부적으로 ItemReader, ItemProcessor, ItemWriter를 가지고 있다.
        - 위의 세가지 객체를 이용하여 Chunk 프로세스 처리를 한다.

        ```java
        return this.stepBuilderFactory.get("step")
                    .<Member,Member>chunk(100)
                    .reader(reader())
                    .writer(writer())
                    .build();
        ```
       
    
- StepExecution
    - Step에 대한 한번의 시도를 의미하는 객체
    - Step 실행 중에 발생한 정보들을 저장하고 있다.
        - 시작시간, 종료시간, 상태, 종료상태 등
    - Job이 재시작 되더라도 이미 성공적으로 완료된 Step은 재실행 되지 않는다.
        - 재실행 시키고 싶다면 따로 설정 필요
    - 이전 단계의 Step이 실패하여 현재 Step이 실행되지 않았다면 StepExecution도 생성되지 않는다.

      ⇒ 실제로 Step이 시작되었을 때만 StepExecution을 생성한다.


- JobExecution과 StepExecution의 관계
  ![image](https://github.com/ulimy/study/assets/18046394/401cd931-9fa7-4201-b75b-d63787aab9ac)
    - JobExecution : 부모 / StepExecution : 자식
    - JobExecution : StepExecution = 1 : N
    - 모든 Step의 StepExecution이 정상적으로 완료되어야 JobExecution도 정상적으로 완료될 수 있다.
    - 하나의 Step의 StepExecution이라도 실패한다면, JobExecution은 실패한다.


- StepContribution
  ![image](https://github.com/ulimy/study/assets/18046394/9ab1ab2b-7b6e-4336-aac6-69c63467b1b9)
    - 청크 프로세스의 변경사항을 버퍼링하여 StepExecution 상태를 업데이트하는 도메인 객체
    - 청크 커밋 직전에 StepExecution의 `apply()` 메서드를 호출하여 상태를 업데이트 한다.
