## [Spring Batch] JPA 성능 문제와 JDBC

스프링 배치의 Reader와 Writer를 JPA로 구성할 경우 JDBC 대비 처리 속도가 엄청나게 저하된다. Reader의 경우 큰 영향을 미치진 않지만, Writer의 경우 엄청난 영향을 끼친다.

엔티티의 `id` 생성 전략은 보통 `IDENTITY`로 설정하게 되는데, 이 설정은 `save()` 수행시 DB 테이블을 조회하여 가장 마지막 값보다 1을 증가시킨 값을 저장하게 된다. 이때 배치 처리 청크 단위 벌크 `insert` 수행이 무너지게 된다.

JDBC 기반으로 작성하게 되면 청크로 설정한 값이 모이게 되면 벌크 쿼리로 단 1번의 `insert`가 수행되지만 JPA의 `IDENTITY` 전략때문에 bulk 쿼리 대신 각각의 수만큼 `insert`가 수행된다.

***

[#spring](https://github.com/wda067/TIL/search?q=%23spring&type=code)
[#batch](https://github.com/wda067/TIL/search?q=%23batch&type=code)