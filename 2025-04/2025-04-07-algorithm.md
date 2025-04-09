## [Algorithm] 오늘 푼 문제

### [징검다리 건너기](https://school.programmers.co.kr/learn/courses/30/lessons/64062) 

최대 몇 명까지 건널 수 있는지를 구해야 한다. 이는 곧 어떤 수 `m`명이 건널 수 있는지를 판별하는 것이고 이진 탐색으로 `m`을 구할 수 있다.

```java
int s = 1;
int e = 200_000_000;  //내구도 최댓값 -> 사람의 최댓값

while (s <= e) {
    int m = (s + e) / 2;
    if (canCross(m)) {  //m명은 건널 수 있음 -> 다음 사람 시도
        answer = m;
        s = m + 1;
    } else {  //m명은 못 건넘 -> 인원 감소
        e = m - 1;
    }
}

private boolean canCross(int m) {
    int skip = 0;
    for (int stone : stones) {
        if (stone - m < 0) {  //0 이하인 디딤돌일 때
            skip++;
            if (skip >= k) {  //그 개수가 k개 이상이면 false
                return false;
            }
        } else {
            skip = 0;
        }
    }
    return true;
}
```

### [호텔](https://www.acmicpc.net/problem/1106)

일반적인 **0/1 배낭 문제**에서는 물건을 한 번만 선택할 수 있기 때문에 반복문을 역순으로 처리하지만, 이 문제는 중복해서 각 도시의 비용을 선택할 수 있는 **완전 배낭 문제**이기 때문에 정순으로 진행해야 한다.

```java
for (int i = 0; i < N; i++) {
    int cost = city[i][0];
    int customer = city[i][1];
    
    for (int j = customer; j < dp.length; j++) {
        if (dp[j - customer] != Integer.MAX_VALUE) {
            dp[j] = Math.min(dp[j], dp[j - customer] + cost);
        }
    }
}
```

***

[#algorithm](https://github.com/wda067/TIL/search?q=%23algorithm&type=code)