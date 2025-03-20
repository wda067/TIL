## [Algorithm] 오늘 푼 문제

### [이모티콘](https://www.acmicpc.net/problem/14226) 

처음에는 단순히 완전탐색으로 모든 경우에 대해 탐색하였는데 이러니까 메모리 초과가 발생하였다. 불필요한 탐색을 방지하기 위해 방문을 체크해야 됐는데 여기서는 일반적인 좌표로 체크를 하는게 아니라 다음과 같이 상태에 따라 체크를 해야 했다.

```java
boolean[][] visited = new boolean[S + 1][S + 1];  //화면 개수, 클립보드 개수
```
같은 이모티콘 개수라도 클립보드 값에 따라 상태가 달라지기 때문에 2차원 배열로 각 경우에 대해 체크해야 한다.

### [컨베이어 벨트 위의 로봇](https://www.acmicpc.net/problem/20055) 

배열의 값을 한 칸씩 이동시킬 때 기존에는 매번번 새로운 배열을 만들어 이전 인덱스의 값을 저장했는데
```java
int[] clone = new int[len + 1];
boolean[] temp = new boolean[N + 1];
for (int i = 2; i <= len; i++) {
    clone[i] = A[i - 1];  //벨트 회전
    if (i <= N) {
        temp[i] = robot[i - 1];  //로봇 회전
    }
}
clone[1] = A[len];  //2N -> 1
temp[N] = false;  //로봇이 내리는 위치
```

다음과 같이 역순으로 한 칸씩 이동시키면 새로운 배열을 생성할 필요없이 기존 배열에서 이동시킬 수 있었다.
```java
//벨트 회전
int last = A[len];
for (int i = len; i > 1; i--) {
    A[i] = A[i - 1];  //한 칸씩 회전
}
A[1] = last;  //마지막 값 -> 첫 번째 칸

//로봇 회전
for (int i = N; i > 1; i--) {
    robot[i] = robot[i - 1];  // 로봇 한 칸씩 이동
}
robot[1] = false;  // 첫 번째 칸은 항상 false
robot[N] = false;  // 내리는 위치에서 로봇 제거
```

***

[#algorithm](https://github.com/wda067/TIL/search?q=%23algorithm&type=code)