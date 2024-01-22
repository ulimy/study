### Jdbc&Jpa PagingItemReader

- JdbcPagingItemReader

  > Paging 기반의 Jdbc 구현체
  >
    - 새로운 페이지를 조회할 때마다 새로운 쿼리가 실행된다.
    - 페이지에 따라 결과 데이터의 순서가 보장될 수 있도록 order by 구문이 작성되도록 한다.
    - 멀티스레드 환경에서 스레드 안정성을 보장한다!


- PagingQueryProvider
    - 페이징 쿼리 실행을 위한 쿼리문을 ItemReader에 제공하는 클래스
    - 데이터베이스마다 페이징 전략이 다르기 때문에 각 데이터베이스 유형에 따른 PagingQueryProvider를 사용한다.
        - SqlPagingQueryProviderFactoryBean에서 DataSource를 보고 알맞은 PagingQueryProvider의 구현체를 주입해준다.
    - 필수 O  : select , from , sortKey
    - 필수 X  :  where , group by


- JdbcPagingItemReader 생성 빌더

  ![image](https://github.com/ulimy/study/assets/18046394/9200d244-d077-43bc-a3a1-3895f593cfbb)


- JdbcPagingItemReader 데이터 조회 흐름

  ![image](https://github.com/ulimy/study/assets/18046394/3422bab2-86a0-4cf6-a29b-2b7f2e7bd190)


- JpaPagingItemReader

  > Paging 기반의 JPA 구현체
  >
    - EntityManagerFactory 객체가 필요하다
    - JPQL을 이용한다.


- JpaPagingItemReader 생성 빌더

  ![image](https://github.com/ulimy/study/assets/18046394/fb8aad04-9032-49b9-8327-326e99e802de)


- JpaPagingItemReader 데이터 조회 흐름

  ![image](https://github.com/ulimy/study/assets/18046394/b0de2e78-a251-4371-86dc-3b6b299ae757)
