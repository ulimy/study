### StepBuilderFactory

- StepBuilderFactory
    - StepBuilder를 생성하는 팩토리 클래스
    - `StepBuilderFactory.get(”stepName”)`
        - stepName이라는 이름의 Step 생성


- StepBuilder

  > Step을 구성하는 설정 조건에 따라 아래 다섯개의 클래스를 생성하고, 실제 Step 생성을 위임한다.
  >
    - `TaskletStepBuilder`
        - TaskletStep 을 생성하는 기본 빌더 클래스
    - `SimpleStepBuilder`
        - TaskletStep 을 생성
        - 내부적으로 청크기반의 작업을 처리하는 ChunkOrientedTasklet  클래스를 생성
    - `PartitionStepBuilder`
        - PartitionStep 을 생성하며 멀티 스레드 방식으로 Job 실행
    - `JobStepBuilder`
        - JobStep 을 생성하여 Step 안에서 Job 실행
    - `FlowStepBuilder`
        - FlowStep 을 생성하여 Step 안에서 Flow 실행


- Step 생성 흐름

  ![image](https://github.com/ulimy/study/assets/18046394/08ac78bb-b3d1-4046-b59c-e98b9343fcf2)

    ```java
    StepBuilderFactory
        .get(”stepName”)
        .XXX(YYY) // 원하는 생성 메서드 호출
    ```

    - 어떤 메서드를 호출하냐에 따라 적절한 빌더가 반환된다.

        ```java
        public class StepBuilder extends StepBuilderHelper<StepBuilder> {
            public StepBuilder(String name) {
                super(name);
            }
        
            public TaskletStepBuilder **tasklet**(Tasklet tasklet) {
                return (new TaskletStepBuilder(this)).tasklet(tasklet);
            }
        
            public <I, O> SimpleStepBuilder<I, O> **chunk**(int chunkSize) {
                return (new SimpleStepBuilder(this)).chunk(chunkSize);
            }
        
            public <I, O> SimpleStepBuilder<I, O> **chunk**(CompletionPolicy completionPolicy) {
                return (new SimpleStepBuilder(this)).chunk(completionPolicy);
            }
        
            public PartitionStepBuilder **partitioner**(String stepName, Partitioner partitioner) {
                return (new PartitionStepBuilder(this)).partitioner(stepName, partitioner);
            }
        
            public PartitionStepBuilder **partitioner**(Step step) {
                return (new PartitionStepBuilder(this)).step(step);
            }
        
            public JobStepBuilder **job**(Job job) {
                return (new JobStepBuilder(this)).job(job);
            }
        
            public FlowStepBuilder **flow**(Flow flow) {
                return (new FlowStepBuilder(this)).flow(flow);
            }
        }
        ```
