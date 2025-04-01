## [Algorithm] 오늘 푼 문제

### [산 모양 타일링](https://school.programmers.co.kr/learn/courses/30/lessons/258705) 

타일을 배치하는 문제의 경우 각 단계의 경우의 수가 이전 단계의 선택에 영향을 받는다. 그래서 DP로 해결할 수 있는데 `dp` 배열을 다음과 같이 정의한다.

```java
dp[i]: i번째 삼각형까지 채우는 경우의 수
```

첫번째는 무조건 1가지이고, 두번째부터 나뉘는데 짝수번째 삼각형은 위에 삼각형 여부에 따라 달라진다.

```java
dp[1] = 1;
//2번째 삼각형 위에 삼각형 존재 여부
if (tops[0] == 0) {
    dp[2] = 2;
} else {
    dp[2] = 3;
}
```

이를 점화식으로 구현하면 `dp[i] = dp[i - 1] + dp[i - 2]`가 되는데 다음의 두가지 경우의 수를 합해야 한다.

- `dp[i - 1]`: `i - 1`번째를 삼각형으로 채우고, `i`번째를 삼각형으로 채울 때
- `dp[i - 2]`: `i - 1`, `i`번째를 마름모로 채울 때

이때 짝수번째일 경우는 위에 삼각형 여부도 확인해야 하는데, 위에 삼각형이 있으면 위의 삼각형과 `i`번째 삼각형을 나뉘냐 아니면 마름모로 채우냐에 따라 나뉘므로`dp[i - 1]`은 2배가 된다.

```java
for (int i = 3; i <= total; i++) {
    if (i % 2 == 0) {  //짝수번째일 경우
        if (tops[i / 2 - 1] == 1) {  //위에 삼각형이 있으면 i번째에 삼각형으로 채우는 경우의 수의 2배
            dp[i] = (dp[i - 1] * 2 + dp[i - 2]) % MOD;
        } else {
            dp[i] = (dp[i - 1] + dp[i - 2]) % MOD;
        }
    } else {
        dp[i] = (dp[i - 1] + dp[i - 2]) % MOD;
    }
}
```


### [비밀 코드 해독](https://school.programmers.co.kr/learn/courses/30/lessons/388352)

문제의 조건을 만족하는 수열을 구하기 위해서 우선 백트래킹으로 가능한 조합을 구해야 한다. 입력한 정수에서 바로 백트래킹을 수행하면 정렬된 결과가 나오지 않기 때문에 너무 복잡해진다.

```java
//1 ~ n에서 오름차순으로 5개의 숫자를 선택하는 모든 경우의 수 생성
private void selectNumber(int[] temp, int depth, int start) {
    if (depth == 5) {  //5개의 숫자를 뽑으면 리턴턴
        candidates.add(temp.clone());  //temp는 참조값이기 때문에 clone()으로 복사
        return;
    }

    for (int i = start; i <= n; i++) {
        temp[depth] = i;
        selectNumber(temp, depth + 1, i + 1);
    }
}
```

그리고 뽑은 경우의 수에서 조건에 만족하는 개수를 카운트해야 하는데 각 조합에서 모든 입력된 정수와 비교하고 개수가 맞는지 확인해야 한다. 입력된 정수의 최대 갯수가 10개이고 수열의 길이도 5개이기 때문에 단순 반복문으로도 구할 수 있다.

```java
//현재 뽑힌 배열의 조건 만족 여부 판단
private boolean isValid(int[] candidate) {
    for (int i = 0; i < m; i++) {  //모든 입력된 정수와 비교
        int count = 0;

        for (int j = 0; j < 5; j++) {
            for (int k = 0; k < 5; k++) {
                if (candidate[k] == q[i][k]) {
                    count++;
                }
            }
        }

        if (count != ans[i]) {
            return false;
        }
    }

    return true;
}
```


***

[#algorithm](https://github.com/wda067/TIL/search?q=%23algorithm&type=code)