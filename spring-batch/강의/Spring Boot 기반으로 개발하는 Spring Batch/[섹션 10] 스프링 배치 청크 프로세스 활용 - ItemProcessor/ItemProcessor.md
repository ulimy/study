### ItemProcessor

- CompositeItemProcessor
    - ItemProcessor들을 연결(chaining)하여 위임하면, 각 ItemProcessor를 실행시킨다.
    - 이전 ItemProcessor의 반환값은 다음 ItemProcessor 값으로 연결된다.


- CompositeItemProcessor 생성 빌더

  ![image](https://github.com/ulimy/study/assets/18046394/f7ccbea0-49c8-4080-bf42-5f44b6f1a57a)


- CompositeItemProcessor의 처리 과정

  ![image](https://github.com/ulimy/study/assets/18046394/808fd93f-9da9-4885-a13c-ccf5c8982d5b)


- ClassifierCompositeItemProcessor
    - Classifier로 라우팅 패턴을 구현하여 ItemProcessor중 하나를 호출한다.

      ⇒ 분류자를 통해 실행될 ItemProcessor를 선정하는 것!


- ClassifierCompositeItemProcessor 생성 빌더

  ![image](https://github.com/ulimy/study/assets/18046394/185d7e6b-759e-43ca-96ca-7bde03eb7700)


- ClassifierCompositeItemProcessor의 처리 과정

  ![image](https://github.com/ulimy/study/assets/18046394/a952bee5-e8a2-47b9-a097-b1c8a2b191e6)

    - `classifier.classify(item)` : itemProcessor 선정을 위한 메서드
