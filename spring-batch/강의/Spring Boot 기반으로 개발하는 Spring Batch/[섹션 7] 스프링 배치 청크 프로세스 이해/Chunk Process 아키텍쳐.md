### Chunk Process 아키텍쳐

![image](https://github.com/ulimy/study/assets/18046394/cc1708a6-a19c-4e42-90ca-79e6105b5768)

1. Job 실행
2. TaskletStep 실행
3. RepeatTemplate
    - 반복 실행할 수 있도록 하는 반복 제어 역할을 한다.
4. ChunkedOrientedTasklet
    - 청크 작업 전체를 처리한다.
5. SimpleChunkProvider
    - itemReader를 통해 item을 읽는다.
    - RepeatTemplate을 통해 chunk size만큼 반복적으로 Item을 읽어 `Chunk<I>`에 add 한다.
    - itemReader가 반복을 시작하는 시점에 `트랜잭션이 시작`된다.
    - 읽은 데이터가 null이라면 더이상 작업할 것이 없으므로 Step을 종료한다.
6. SimpleChunkProcessor
    - itemProcessor, itemWriter를 통해 item을 가공하고 저장한다.

      ⇒ 이때 예외가 발생하는 경우 트랜잭션이 롤백된다.

    - itemProcessor
        - `Chunk<I>`의 Iterator를 통해 chunk size만큼 반복적으로 Item을 가공하여 `Chunk<O>`에 add 한다
        - 반복을 마쳤다면 `Chunk<I>`에서 List<Item> items를 꺼내 itemWriter에게 전달한다.
    - itemWriter
        - 전달받은 List<Item> items를 저장한다.
        - 이때, `트랜잭션이 커밋`된다.
