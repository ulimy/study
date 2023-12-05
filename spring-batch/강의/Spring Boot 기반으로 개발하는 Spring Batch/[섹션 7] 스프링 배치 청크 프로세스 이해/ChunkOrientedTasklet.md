### ChunkOrientedTasklet

- ChunkOrientedTasklet

  > 스프링 배치에서 제공하는 Taslet 구현체
  >

  ![image](https://github.com/ulimy/study/assets/18046394/993d70fb-933f-4db4-8d03-8ed482eb7709)

    - Chunk 지향 프로세싱을 담당하는 도메인 객체이다.
    - ItemReader, ItemWriter, ItemProcessor를 사용하여 Chunk 기반의 데이터 입출력을 처리한다.
    - TaskletStep에 의해서 반복적으로 실행되며 ChunkOrientedTasklet이 실행될 때 마다 새로운 트랜잭션이 생성된다.
    - exception이 발생할 경우 해당 chunk는 롤백되며, 이전에 커밋한 chunk는 완료 상태이다.


- ChunkProvider와 ChunkProcessor

  > Chunk 처리를 위해 ChunkOrientedTasklet가 내부적으로 가지고있는 객체
  >
    - ChunkProvider
        - ItemReader를 핸들링 한다.
        - item을 chunk size만큼 읽어 `Chunk<I>`에 담아 반환한다.
    - ChunkProcessor
        - ItemProcessor, ItemWriter를 핸들링 한다.
        - ChunkProvider로부터 전달받은 `Chunk<I>`를 가공하여 `Chunk<O>`에 담아 저장한다.


- 실행 흐름

  ![image](https://github.com/ulimy/study/assets/18046394/d029c708-80e1-4323-aa5a-6b89fcf2632b)

    1. TaskletStep → `ChunkOrientedTasklet.execute()`
        - 실행 요청
    2. ChunkOrientedTasklet **→** `ChunkProvider.provde()`
        - item 획득을 위한 provide 요청
    3. ChunkProvider → `ItemReader.read()`
        - 하나의 Item 획득
        - chunk size만큼 반복하여 item을 모두 획득한다.
    4. ChunkOrientedTasklet → `ChunkProcessor.process()`
        - item 처리를 위한 process 요청
    5. ChunkProcessor → `ItemProcessor.process()`
        - 하나의 item 처리
        - chunk size만큼 반복하여 모든 item을 처리한다.
    6. ChunkProcessor → `ItemWriter.write()`
        - 모든 item을 저장한다.
