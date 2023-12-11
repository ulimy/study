### ItemReader & ItemWriter & ItemProcessor

- ItemReader

  > 다양한 입력으로부터 데이터를 읽어 제공하는 인터페이스
  >

  ![image](https://github.com/ulimy/study/assets/18046394/f348faf0-ac1d-464b-bcb4-1fa0f92bd380)

    - T read()
        - 입력 데이터를 읽고 다음 데이터로 이동한다.
        - 한번에 하나의 데이터를 읽는다.
        - 더이상 읽을 데이터가 없는 경우 null을 반환한다.
    - 다수의 구현체들이 ItemReader와 ItemStream을 동시에 구현하고있다.


- ItemWriter

  > Chunk 단위로 데이터를 받아 일괄 출력 작업을 하는 인터페이스
  >

  ![image](https://github.com/ulimy/study/assets/18046394/fcddcdcb-40a8-48d5-b099-ae3c4b90c6be)

    - void write(List items)
        - 출력 데이터를 리스트로 받아 처리한다.
        - 출력이 완료되어 트랜잭션이 종료되면, 새로운 Chunk 단위 프로세스로 이동한다.
    - 다수의 구현체들이 ItemWriter와 ItemStream을 동시에 구현하고있다.
    - 다수의 구현체들이 ItemReader 구현체와 1:1 대응 관계이다.


- ItemProcessor

  > 데이터를 가공, 변경, 필터링하는 작업을 하는 인터페이스
  >
  >
  > ⇒ ItemReader, ItemWriter와 분리되어 비즈니스 로직을 구현할 수 있다.
  >

  ![image](https://github.com/ulimy/study/assets/18046394/580aa761-da61-439f-9527-6890f8995578)

    - O process(I item)
        - `<I>` : ItemReader에서 받을 데이터타입
        - `<O>` : ItemWriter에 보낼 데이터 타입
        - null을 반환하면 해당 item은 필터링되어 ItemWriter를 통해 저장되지 않음을 의미한다. (skip)
    - 중간에서 베이스 로직을 구현하는 용도이기 때문에 ItemStream은 구현하지 않는다.
    - 대부분 커스터마이징하여 사용하기 때문에 기본적으로 제공되는 구현체가 적다.


- ChunkOrientedTasklet 실행 시 필수요소 여부
    - 필수 O : ItemReader, ItemWriter
    - 필수 X : ItemProcessor
