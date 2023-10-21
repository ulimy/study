### Job

- Job
    - 하나의 배치 작업 자체를 의미하는 최상위 인터페이스
    - 1개 이상의 Step을 포함하는 컨테이너로 구현된다.


- Job의 기본 구현체
    - SimpleJob
        - 순차적으로 Step을 실행
    - FlowJob
        - 특정한 조건과 흐름에 따라 Step을 구성
        - Flow 객체를 실행시켜서 작업을 진행한다.


- JobLauncher
    - Job을 실행시키는 주체


- Job의 실행 흐름
   ![image](https://github.com/ulimy/study/assets/18046394/17c8be62-f410-460e-a32e-5f6698eae8c9)


- JobInstance
    - Job이 실행될 때 생성되는 논리적 실행 단위
    - 고유하게 식별 가능한 작업 실행이다!
    - EX
        - 1월 1일에 실행한 Job → 0101JobInstance
        - 1월 2일에 실행한 Job → 0102JobInstance
        - ⇒ 같은 Job을 통해 실행된 고유한 실행 단위!
    - Job : JobInstance = 1 : N
    - 이전과 동일한 Job + JobParameter로 실행하는 경우 이미 존재하는 JobInstance를 반환한다.
        - 내부적으로 JobName + JobKey ( JobParameter의 해시값 ) 를 가지고 JobInstance를 구분한다.


- BATCH_JOB_INSTANCE
    - JobInstance의 정보를 저장하는 테이블
    - 같은 JobName + JobKey ( JobParameter의 해시값 )가 동일한 데이터는 저장될 수 없다.


- JobParameter
    - Job을 실행할 때 함께 포함되어 사용되는 도메인 객체
    - 하나의 Job에 존재할 수 있는 여러 JobInstance를 구분하기 위해 사용된다.
    - 어플리케이션 실행시 주입, 코드로 주입 등등의 방법이 있다.
    - JobParameter : JobInstance = 1 : 1


- BATCH_JOB_EXCUTION_PARAM
    - JobParameter의 정보를 저장하는 테이블
    - JOB_EXCUTION : BATCH_JOB_EXCUTION_PARAM = 1 : N


- JobParameters
    - JobParameter를 LinkedList 형태로 가지고 있는 객체
    - LinkedHashMap<String, JobParameter>
        - String ⇒ key이다.

    ```java
    final var jobParameters = new JobParametersBuilder()
                .addString("name", "user1")
                .toJobParameters();
    
    jobLauncher.run(job, jobParameters);
    ```


- JobExecution
    - JobInstance에 대한 한번의 시도를 의미하는 객체
    - Job 실행 중에 발생한 정보들을 저장하고 있다.
        - 시작시간, 종료시간, 상태, 종료상태 등
    - JobExecution의 실행 상태가 COMPLETED가 될 때 까지 JobInstance는 여러번의 시도를 가질 수 있다.

      ⇒ 하나의 JobInstance에도 JobExecution이 여러개 있을 수 있음을 의미!


- 배치 실행 흐름도
  ![image](https://github.com/ulimy/study/assets/18046394/afdab338-614d-4205-a9f7-e304ace3ba6e)
  - Job : 일별 정산
  - JobInstance A
      - id 1번
      - 한번 실행 → JobExecution 1
  - JobInstance B
      - id 2번
      - 한번 실행 → JobExecution 1 / FAILED
      - 재시도 → JobExecution 2 / COMPLETED


