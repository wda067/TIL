## [Algorithm] 오늘 푼 문제

### [단속카메라](https://school.programmers.co.kr/learn/courses/30/lessons/42884) 

문제 자체는 간단하였다. 차량의 진출입 지점이 주어지고 모든 카메라를 단속할 수 있는 최소의 카메라 수를 구해야 한다. 

우선 각 차량 정보를 정렬을 해야되는데 진입 지점으로 정렬한다면 진출 지점이 다른 차량보다 클 경우 다른 차량은 단속할 수 없다.
`[1, 100], [2, 3] 카메라를 100에 설치하면 [2, 3]은 단속 불가`

따라서 진출 지점을 기준으로 정렬하여 카메라 위치를 진출 지점으로 갱신해야 한다.

```java
public int solution(int[][] routes) {
    //진출 지점을 기준으로 정렬
    Arrays.sort(routes, Comparator.comparingInt(o -> o[1]));
    int count = 0;
    int min = Integer.MIN_VALUE;

    for (int[] route : routes) {
        if (min < route[0]) {  //기존 카메라로 단속 불가
            count++;  //카메라 추가
            min = route[1];  //카메라를 진출 지점에 위치
        }
    }
    return count;
}
```

### [고냥이](https://www.acmicpc.net/problem/16472)

주어진 알파벳의 수로 문자열에서 최대 길이의 부분 문자열을 구해야 한다. 문자열의 최대 길이가 100,000이기 때문에 완전 탐색을 할 수 없다.

따라서 투 포인터로 알파벳의 종류를 카운트하며 최대 개수를 갱신해나간다.

```java
while (e < len) {
    char end = str.charAt(e);
    map.put(end, map.getOrDefault(end, 0) + 1);
    if (map.get(end) == 1) {  //새로운 알파벳
        kind++;
    }

    while (kind > N) {  //종류의 최대 개수를 안넘을 때까지 윈도우 크기를 줄임
        char start = str.charAt(s);
        map.put(start, map.get(start) - 1);
        if (map.get(start) == 0) {  //제외된 알파벳
            kind--;
        }
        s++;
    }

    max = Math.max(max, e - s + 1);  //현재 윈도우의 크기로 갱신
    e++;
}
```

***

[#algorithm](https://github.com/wda067/TIL/search?q=%23algorithm&type=code)