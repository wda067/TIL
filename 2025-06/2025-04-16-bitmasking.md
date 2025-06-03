## [Algorithm] 오늘 푼 문제

### [알파벳](https://www.acmicpc.net/problem/1987) 

백트래킹으로 이동한 좌표의 알파벳을 사용 여부에 따라 포함시키며 진행한다. 이때 이동경로의 알파벳의 포함 여부를 판단할 때 다음과 같이 `ArrayList`를 사용할 수 있다.

```java
private static void dfs(int r, int c, List<Character> list) {
    max = Math.max(max, list.size());

    for (int d = 0; d < 4; d++) {
        int nr = r + dr[d];
        int nc = c + dc[d];
        if (nr < 0 || nr >= R || nc < 0 || nc >= C) {
            continue;
        }

        char cur = board[nr][nc];
        if (!list.contains(cur)) {  //사용하지 않은 알파벳일 경우
            list.add(cur);
            dfs(nr, nc, list);
            list.remove(list.size() - 1);
        }
    }
}
```

하지만 `contains()`는 $O(n)$이므로 반복적인 탐색에 비효율적이다. 알파벳은 26개로 제한되어 있으므로 `boolean` 배열이나 비트마스킹을 사용하면 사용 여부 확인, 추가, 제거가 $O(1)$으로 가능하다.

현재 사용한 알파벳을 저장한 `int used`가 있을 때

```java
used |= (1 << (ch - 'A'));
```

`'C'`를 추가한다면 `1 << 2 - 00000100`으로 3번째 비트가 1이 된다.

사용 여부를 판단할 때는 다음과 같이 판단할 수 있다.

```java
(used & nextBit)
= 00001001 & 00001000
= 00001000  //0이 아님 -> 이미 사욯함
```

여기선 필요없지만 알파벳을 제거할 때는 다음과 같이 사용한다. 

```java
//~으로 비트를 반전시켜 해당 비트를 0으로 만든 후 AND 연산
used &= ~(1 << (ch - 'A'));
```

```java
private static void dfs(int r, int c, int used, int count) {
    max = Math.max(max, count);

    for (int d = 0; d < 4; d++) {
        int nr = r + dr[d];
        int nc = c + dc[d];
        if (nr < 0 || nr >= R || nc < 0 || nc >= C) {
            continue;
        }

        int nextBit = 1 << (board[nr][nc] - 'A');
        if ((used & nextBit) == 0) {  //0 & 1 = 0
            dfs(nr, nc, used | nextBit, count + 1);
        }
    }
}
```

***

[#algorithm](https://github.com/wda067/TIL/search?q=%23algorithm&type=code)