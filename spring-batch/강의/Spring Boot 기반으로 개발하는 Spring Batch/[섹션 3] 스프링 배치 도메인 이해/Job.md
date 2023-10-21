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
