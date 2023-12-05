### Chunk

- Chunk

  > 여러개의 아이템을 묶은 하나의 덩어리
  >
    - 한번에 하나씩 아이템을 입력받아 Chunk 단위의 덩어리로 만든다.
    - Chunk 단위로 트랜잭션을 처리한다.
    - 대용량 데이터를 Chunk 단위로 쪼개고, 더이상 처리할 데이터가 없을 때까지 반복 실행!

- `Chunk<I>` & `Chunk<O>`

  ![image](https://github.com/ulimy/study/assets/18046394/498cc0ed-f664-4925-9532-f70b03a6b8be)

    - `Chunk<I>` `input`
        - ItemReader로 읽은 하나의 아이템을 Chunk에서 지정한 개수만큼 반복해서 저장한 타입

      ⇒ 한번에 처리해야하는 Item 모음

    - `Chunk<O>` `output`
        - ItemReader로부터 전달받은 `Chunk<I>`를 ItemProcessor에서 적절하게 가공한뒤 ItemWriter에게 전달하는 타입

      ⇒ 한번에 처리 완료되어 저장될 Item 결과 모음


- item의 처리 흐름

  ![image](https://github.com/ulimy/study/assets/18046394/a1c1f474-bbc7-4a71-a89f-edb289276a68)

    - itemReader
        - chunk size만큼의 item이 될 때 까지 반복적으로 item을 읽는다.
    - itemProcessor
        - item을 하나씩 가공한다.
    - itemWriter
        - chunk size만큼의 item을 일괄 저장한다.

  ⇒ itemReader, itemProcessor : 개별 item 처리 / itemWriter : 일괄 처리


- Chunk 객체의 필드
    - List items
        - chunk 개수만큼의 item
    - List<SkipWrapper> skips
        - 오류 발생되어 skip한 item
        - 각 item의 exception

        ```java
        public class SkipWrapper<T> {
            private final Throwable exception;
            private final T item;
        }
        ```

    - List<Exception> errors
        - skip된 item의 오류 모음
    - ChunkIterator
        - Chunk에 저장된 item을 추출하기 위한 inner class
