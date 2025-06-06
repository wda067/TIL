## 테스트 컨테이너

Redis와 Kafka를 사용하던 중 개발 환경과 구분하여 테스트 코드를 작성하고 싶었다. DB는 단순히 H2를 사용하면 됐지만 Redis와 같은 것들은 테스트 환경을 구현하기가 번거로운 작업이었다. 그러던 중 `Testcontainers`를 알게 되었고, 기존에 사용하던 Docker 컨테이너를 활용하여 테스트 시에만 필요한 컨테이너를 구성할 수 있었다. 테스트 완료 후에는 알아서 종료되어 편리했다.

### Redis Test Container

```java
@Configuration
@Profile("test")
public class RedisTestContainer {

    private static final FixedHostPortGenericContainer<?> REDIS_CONTAINER;
    private static final String REDIS_IMAGE = "redis:alpine";

    static {
        REDIS_CONTAINER = new FixedHostPortGenericContainer<>(DockerImageName.parse(REDIS_IMAGE).toString())
                .withFixedExposedPort(6380, 6379)  //호스트 6380 -> 컨테이너 6379
                .withCommand("redis-server", "--requirepass", "1234");
        REDIS_CONTAINER.start();
    }
}
```

### Kafka Test Container

```java
@Configuration
@Profile("test")
public class KafkaTestContainer {

    private static final KafkaContainer KAFKA_CONTAINER;

    static {
        KAFKA_CONTAINER = new KafkaContainer(DockerImageName.parse("confluentinc/cp-kafka:latest"))
                .withImagePullPolicy(PullPolicy.alwaysPull());
        KAFKA_CONTAINER.start();
        System.setProperty("KAFKA_BOOTSTRAP_SERVERS", KAFKA_CONTAINER.getBootstrapServers());
        System.setProperty("spring.kafka.bootstrap-servers", KAFKA_CONTAINER.getBootstrapServers());
    }

    public static String getBootstrapServers() {
        return KAFKA_CONTAINER.getBootstrapServers();
    }
}
```

***

[#docker](https://github.com/wda067/TIL/search?q=%23docker&type=code) [#test](https://github.com/wda067/TIL/search?q=%23test&type=code)