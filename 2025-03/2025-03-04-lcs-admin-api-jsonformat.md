## [Algorithm] LCS

LCS(Longest Common Subsequence)은 두 수열이 있을 때 공통되는 수열 중 가장 길이가 긴 수열을 뜻한다. 오늘 푼 [팰린드롬 만들기](https://www.acmicpc.net/problem/1695) 문제에서 사용되었는데 LCS는 DP로 구현할 수 있다. 

2중 for문으로 두 수열을 탐색하면서 수가 같을 때 해당 위치의 DP 값을 1 증가 시킨다. 그리고 값이 다를 때는 이전 LCS의 길이 중 최댓값을 가져온다.

```java
//LCS 구하기
for (int i = 1; i <= N; i++) {
    for (int j = 1; j <= N; j++) {

        if (arr[i - 1] == rev[j - 1]) {  //두 수가 같다면 LCS 길이 +1
            dp[i][j] = dp[i - 1][j - 1] + 1;
        } else {  //LCS 최대 길이는 유지된다.
            dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
        }
    }
}
```

[링크](https://velog.io/@emplam27/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-LCS-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-Longest-Common-Substring%EC%99%80-Longest-Common-Subsequence#%EC%B5%9C%EC%9E%A5-%EA%B3%B5%ED%86%B5-%EB%B6%80%EB%B6%84%EC%88%98%EC%97%B4longest-common-subsequence-%EA%B8%B8%EC%9D%B4-%EA%B5%AC%ED%95%98%EA%B8%B0)를 참고해서 이해하였다.

## [Kafka] Admin API

카프카의 내부 옵션을 확인하기 위해서 명령어를 사용할 수 있지만 매우 번거롭다. 그래서 카프카 클라이언트는 `AdminClient` 클래스로 내부 옴션을 설정하거나 조회할 수 있다.

가령 브로커의 정보, 토픽 리스트, 컨슈머 그룹 조회 같은 api를 사용할 수 있다.

```java
@Slf4j
@Component
@RequiredArgsConstructor
public class KafkaAdminClient {

    private final AdminClient adminClient;

    //Kafka 토픽 목록 조회
    public Set<String> listTopics() throws ExecutionException, InterruptedException {
        return adminClient.listTopics().names().get();
    }

    //새로운 토픽 생성
    public void createTopic(String topicName, int partitions, short replicationFactor) {
        NewTopic newTopic = new NewTopic(topicName, partitions, replicationFactor);
        adminClient.createTopics(Collections.singleton(newTopic));
        log.info("토픽 생성: {}", topicName);
    }

    //토픽의 파티션 개수 늘리기
    public void increasePartitions(String topicName, int newPartitionCount) {
        Map<String, NewPartitions> newPartitions = Collections.singletonMap(
                topicName, NewPartitions.increaseTo(newPartitionCount)
        );
        adminClient.createPartitions(newPartitions);
        log.info("토픽 파티션 증가: {} -> {}", topicName, newPartitionCount);
    }
}
```

## [Spring] @JsonFormat

쿠폰을 생성할 때 만료일이나 발급 시작 시간을 DTO로 받아 요청하는데 처음에는 `String`으로 받아 서비스에서 변환하는 작업을 했다. 그런데 애초에 요청에서 받을 때 바로 `LocalDateTime`으로 받을 수 있는 방법을 찾아보다가 `@JsonFormat`을 알게 되었다.

```java
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@AllArgsConstructor
public class CouponCreate {

    @NotBlank(message = "쿠폰 이름을 입력해 주세요.")
    private String couponName;

    @NotNull(message = "쿠폰 총 발급 수량을 입력해 주세요.")
    @Min(value = 1, message = "1개 이상 입력해 주세요.")
    private int totalQuantity;

    @NotNull(message = "쿠폰 만료일을 입력해 주세요.")
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd", timezone = "Asia/Seoul")
    private LocalDate expirationDate;

    @NotNull(message = "쿠폰 발급 시작 시간을 입력해 주세요.")
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
    private LocalDateTime issueStartTime;

    @NotNull(message = "쿠폰 발급 종료 시간을 입력해 주세요.")
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
    private LocalDateTime issueEndTime;
}
```

***

[#algorithm](https://github.com/wda067/TIL/search?q=%23algorithm&type=code)
[#kafka](https://github.com/wda067/TIL/search?q=%23kafka&type=code)
[#spring](https://github.com/wda067/TIL/search?q=%23spring&type=code)