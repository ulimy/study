### ItemStream

- ItemStream
    - ItemReader와 ItemWriter 처리 과정 중
        - 상태를 저장한다. ( update )
        - 오류가 발생하면 해당 상태를 참조하여 실패한 곳에서 재시작하도록 지원한다.

      ex) ItemReader에서 100개의 데이터를 읽다가 50번째에서 실패한 경우 재시도시 50번째부터 읽을 수 있도록!

    - 리소스를 열고/닫는( open/close ) 등의 작업, 입출력 장치 초기화 등의 작업을 지원한다.
    - ItemReader, ItemWriter는 ItemStream을 구현해야한다.

      ⇒ 따라서 함께 구현하는 구현체가 많은 것!


- ItemStream의 구조

  ![image](https://github.com/ulimy/study/assets/18046394/cede667b-1f28-4ab3-abbf-3328805513a7)


- Step에서 ItemStream의 역할

  ![image](https://github.com/ulimy/study/assets/18046394/789ac0ae-0ecb-40f1-a190-c861e1707ff4)

    - ItemReader, ItemWriter 실행 `전`
        - `open()` : 리소스 열기
    - ItemReader, ItemWriter 실행 `후`
        - `update()` : 상태 저장
    - 모든 Step 종료
        - `close()` : 리소스 닫기
