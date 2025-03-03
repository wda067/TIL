## [Algorithm] 투 포인터 vs. 이진 탐색

[세 용액](https://www.acmicpc.net/problem/2473) 문제에서 배열에서 3개의 원소의 합이 0에 가장 가까울 때를 찾아야 하는데 한 가지 값을 고정한 다음 두 값을 투 포인터로 찾거나, 두 가지 값을 고정한 다음 한 가지 값을 이진 탐색으로 찾을 수 있었다.

특정 값을 찾는 경우 이진 탐색의 시간 복잡도가 더 효율적이지만 이 문제에서는 1개 또는 2개의 값을 고정해야 되기 때문에 $O(n^2)$ vs $O(n^2logn)$으로 투 포인터가 더 효율적이었다.

### 투 포인터
- 시간 복잡도: $O(n)$
  - 포인터가 한 번씩만 이동

```java
private static boolean twoPointer(int i) {
    int s = i + 1;
    int e = N - 1;
    while (s < e) {
        long sum = features[i] + features[s] + features[e];

        if (Math.abs(sum) < min) {
            min = Math.abs(sum);
            result[0] = features[i];
            result[1] = features[s];
            result[2] = features[e];
        }

        //포인터를 한 칸씩 이동
        if (sum < 0) {
            s++;
        } else if (sum > 0) {
            e--;
        } else {  //합이 0이면 출력 후 종료
            System.out.println(result[0] + " " + result[1] + " " + result[2]);
            return true;
        }
    }
    return false;
}
```

### 이진 탐색
- 시간 복잡도: $O(logn)$
  - 탐색 구간을 절반씩 줄여나감

```java
private static void binarySearch(int i, int j) {
    long sum = features[i] + features[j];
    int s = j + 1;
    int e = N - 1;
    while (s <= e) {
        int m = (s + e) / 2;
        long total = sum + features[m];

        if (Math.abs(total) < min) {
            min = Math.abs(total);
            result[0] = features[i];
            result[1] = features[j];
            result[2] = features[m];
        }

        //탐색 범위를 절반
        if (total <= 0) {
            s = m + 1;
        } else {
            e = m - 1;
        }
    }
}
```

***
[#algorithm](https://github.com/wda067/TIL/search?q=%23algorithm&type=code)