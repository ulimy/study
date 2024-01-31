### ItemReaderAdapter

- ItemReaderAdapter

  > 배치 Job 안에 이미 있는 DAO나 다른 서비스를 ItemReader에서 사용하기 위한 위임 역할을 한다.
  >

  ![image](https://github.com/ulimy/study/assets/18046394/4b33c6b3-8cdc-4119-bad2-ada9d436d76e)

    - `setTargetObject()` : 위임 대상이 될 클래스 설정
    - `setTargetMethod()` : 위임 대상이 될 메서드 설정

  ⇒ Java의 Reflection을 사용하여 구현되는 것!
