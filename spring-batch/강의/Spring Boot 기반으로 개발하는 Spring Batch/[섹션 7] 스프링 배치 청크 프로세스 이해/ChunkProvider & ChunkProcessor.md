### ChunkProvider & ChunkProcessor

- ChunkProvider

  > ItemReader를 이용하여 chunk size만큼 아이템을 읽어 Chunk를 제공하는 도메인 객체
  >

  ![image](https://github.com/ulimy/study/assets/18046394/8b9838f7-0a9e-4b96-a86c-aeb2daaaea4f)

    - 외부로부터 ChunkProvider가 호출될 때 마다 항상 새로운 `Chunk<I>` 객체가 생성된다.
    - 반복문 종료 시점
        - chunk size 만큼 item을 읽은 경우

          ⇒ ChunkProcessor로 이동

        - itemReader가 읽은 item이 null인 경우

          ⇒ 해당 Step의 반복문까지 종료

    - 기본 구현체
        - SimpleChunkProvider
        - FaultTolerantChunkProvider

          ⇒ 오류 발생 시 skip, 재시도 등을 할 수 있는 기능이 포함되어있다.


- ChunkProcessor

  > ItemProcessor를 이용하여 item을 변형, 가공, 필터링 하고 ItemWriter를 이용하여 데이터를 저장하는 도메인 객체
  >

  ![image](https://github.com/ulimy/study/assets/18046394/9640ee1d-e20c-407a-b152-ff2f4335efb3)

    - 외부로부터 ChunkProcessor가 호출될 때 마다 항상 새로운 `Chunk<O>` 객체가 생성된다.
    - ItemProcessor가 없는 경우 ItemReader가 읽은 item 그대로 `Chunk<O>`에 저장된다.
    - ItemProcessor가 완료되면 `Chunk<O>`의 List<Item>을 꺼내 ItemWirter에 저장된다.
    - 기본 구현체
        - SimpleChunkProcessor
        - FaultTolerantChunkProcessor
