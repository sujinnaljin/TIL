# Isolation level

- transaction은 흔히 이론적으로 ACID 원칙을 보장해야함

  > Atomicity(원자성), Consistency(일관성), Isolation(독립성), Durability(영구성)

- 하지만, 실제로는 **ACID 원칙은 종종 지켜지지 않음**. 왜냐하면 **ACID 원칙을 strict 하게 지키려면 동시성이 매우 떨어지기 때문**

- 그렇기 때문에 DB 엔진은 ACID 원칙을 희생하여 동시성을 얻을 수 있는 방법을 제공하는데, 이것이 바로 **transaction의 isolation level**

- Isolation **원칙을 덜 지키는 level**을 사용할수록 **문제가 발생할 가능성**은 커지지만 동시에 더 **높은 동시성**을 얻을 수 있음.

- DB 엔진은 isolation level에 따라 서로 다른 locking 전략을 취함 **isolation level이 높아질수록 더 많이, 더 빡빡하게 lock을 거는 것**

- isolation level 은 아래와 같이 4가지로 나뉨

  1. **READ UNCOMMITTED**

     - 트랜잭션에서 처리 중인, **아직 커밋되지 않은 데이터**를 다른 트랜잭션이 읽는 것을 허용

     - Dirty Read, Non-Repeatable Read, Phantom Read 현상 발생

       > **Dirty Read** - 아직 커밋되지 않은 **수정 중인 데이터**를 다른 트랜잭션에서 읽을 수 있도록 허용할 때 발생 (해당 트랜잭션이 롤백하는 경우).
       >
       > **Non-Repeatable Read** - **한 트랜잭션 내에서 같은 쿼리**를 두번 수행할 때, 그 사이에 다른 트랜잭션이 값을 수정 또는 삭제함으로써 **두 쿼리가 상이하게 나타나는 비 일관성**이 발생하는 것
       >
       > **Phantom Read** - 한 트랜잭션 안에서 일정범위의 레코드를 두번 이상 읽을 때, 첫 번재 쿼리에서 **없던 유령 레코드**가 두번째 쿼리에서 **나타나는** 현상

  2. **READ COMMITTED**
     - Dirty Read 방지 : 트랜잭션이 **커밋되어 확정된 데이터만 읽는 것**을 허용
     - 대부분의 DBMS가 **기본모드**로 채택하고 있는 일관성모드
     - Non-Repeatable Read, Phantom Read 현상은 여전히 발생
  3. **REPEATABLE READ**
     - 선행 트랜잭션이 읽은 데이터는 트랜잭션이 종료될 때가지 후행 트랜잭션이 갱신하거나 상제한는 것은 불허함으로써 **같은 데이터를 두 번 쿼리했을 때 일관성 있는 결과**를 리턴
     - Phantom Read 현상은 여전히 발생
  4. **SERIALIZABLE**
     - 모든 작업을 하나의 트랜젝션에서 처리하는 것과 같은 **가장 높은 고립수준**을 제공
     - 신형 트랜잭션이 읽은 데이터를 후행 트랜잭션이 갱신하거나 삭제하지 못할 뿐만 아니라 중간에 새로운 레코드를 산입하는 것도 막아줌
     - 완벽하게 읽기 일관성 모드를 제공하지만 성능 측면에서는 **동시 처리성능**이 가장 **낮음**.
     - `SERIALIZABLE`은 데이터를 안전하게 보호할 수는 있지만 굉장히 쉽게 **`DEADLOCK`**에 걸릴 수 있음

- CRUD에 적절한 Isolation level은 아래와 같다

  - Read시에는 `REPEATABLE READ`
  - Create, Update, Delete 때는 `SERIALIZABLE`



# 출처

- [Lock으로 이해하는 Transaction의 Isolation Level](https://suhwan.dev/2019/06/09/transaction-isolation-level-and-lock/)
- [Transaction Isolation Level 정리](https://velog.io/@lsb156/Transaction-Isolation-Level)

- [트랜잭션의 격리 수준(isolation Level)이란?](https://nesoy.github.io/articles/2019-05/Database-Transaction-isolation)
- [트랜잭션 수준 읽기 일관성](http://wiki.gurubee.net/pages/viewpage.action?pageId=21200923)

