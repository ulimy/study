### Simple Job

- SimpleJob
    - Step을 실행시키는 Job의 구현체
    - 여러단계의 Step으로 구성할 수 있으며 순차적으로 실행시킨다.
    - 모든 Step이 성공적으로 완료되어야 Job이 성공적으로 완료된다

      ⇒ 맨 마지막에 실행한 Step의 BatchStatus == Job의 최종 BatchStatus


- SimpleJob 생성 빌더
  ![image](https://github.com/ulimy/study/assets/18046394/77acaa91-98a6-4bdb-aa43-792adc600ebc)
    - `start(Step)`
        - 처음 실행할 Step 설정

    - `next(Step)`
        - 다음에 실행할 Step을 순차적으로 연결
        - 여러번 설정 가능

    - `validator(JobParameterValidator)`
        - Job 실행에 필요한 파라미터 검증
        - 기본적으로 `DefaultJobParametersValidator` 구현체를 지원한다.
          ![image](https://github.com/ulimy/study/assets/18046394/642f13a0-8799-4b41-9717-fa46ef73a4f7)
          ```java
          public class DefaultJobParametersValidator implements JobParametersValidator, InitializingBean {
              private Collection<String> requiredKeys;
              private Collection<String> optionalKeys;

              public DefaultJobParametersValidator(String[] requiredKeys, String[] optionalKeys) {
                  this.setRequiredKeys(requiredKeys);
                  this.setOptionalKeys(optionalKeys);
              }
          }
          ```
            - requiredKeys : 필수 key
            - optionalKeys : 필수 X인 key
        - 검증을 커스터마이징 하고싶다면 인터페이스를 직접 구현하면 된다.

            ```java
            public class CustomJobParametersValidator implements JobParametersValidator {
    
                @Override
                public void validate(JobParameters jobParameters) throws JobParametersInvalidException {
                        // validate 로직 작성
                  throw new JobParametersInvalidException("에러발생!");
                }
    
            }
            ```

    - `preventRestart(boolean)`
        - Job의 재시작 여부 설정
            - true : 재시작 가능 ( 기본값 )
            - false : 재시작 불가
        - 만약 false로 설정 했는데 재시작하려한다면? `JobRestartException` 발생

    - `incrementer(JobParametersIncrementer)`
        - JobParameters에서 필요한 값을 증가시켜 다음에 사용될 JobParameters 반환
        - 기존의 JobParameters의 변경 없이 Job을 여러번 시작하고싶을 때 이용할 수 있다.
        - 기본적으로 RunIdIncrementer 구현체를 지원한다.
            - 1부터 시작하는 id를 1씩 증가시키며 반환한다.
        - 변경을 커스터마이징 하고싶다면 인터페이스를 직접 구현하면 된다.

            ```java
            public class CustomJobParametersIncrementer implements JobParametersIncrementer {
      
                @Override
                public JobParameters getNext(JobParameters jobParameters) {
                    // 필요한 로직 구현
              
                    return new JobParametersBuilder()
                        .addString("key", "value")
                        .toJobParameters();
                }
      
            }
            ```
          


- SimpleJob의 흐름
  ![image](https://github.com/ulimy/study/assets/18046394/c9d50090-3a62-4884-b0f1-c6975e961c16)

  
- 클래스의 상속 관계
  ![image](https://github.com/ulimy/study/assets/18046394/822dde38-32f2-4ba8-ab36-b872a6db993c)

