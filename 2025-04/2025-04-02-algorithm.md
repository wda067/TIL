## [Algorithm] 오늘 푼 문제

### [탑](https://school.programmers.co.kr/learn/courses/30/lessons/2493) 

스택에 관한 문제는 잘 풀지 않아서 접근 방법이 생각이 나질 않았다. 이 문제의 N은 최대 500,000으로 이전 탑 모두를 브루트 포스로 탐색할 경우 $O(n^2)$으로 시간 초과가 발생한다.

스택은 이전 값 중 조건을 만족하는 값을 찾을 때 유용하다. 탑을 하나씩 보면서 이전 탑들을 스택에 저장해두고 확인하는 것이다. 탑을 탐색하면서 현재 탑의 높이보다 높은 탑들만 스택에 남겨둔다. 불필요한 값은 매번 제거하므로 $O(n)$으로 최적화할 수 있다.

```java
Stack<int[]> stack = new Stack<>();
for (int i = 0; i < N; i++) {
    int height = towels[i];

    while (!stack.isEmpty() && stack.peek()[1] < height) {  //현재 탑보다 작은 탑은 제거
        stack.pop();
    }

    if (stack.isEmpty()) {  //이전 탑 중 조건을 만족하는 탑이 없음
        sb.append("0").append(" ");
    } else {
        sb.append(stack.peek()[0]).append(" ");
    }

    stack.push(new int[]{i + 1, height});  //현재 탑의 (인덱스, 높이) 추가
}
```

***

[#algorithm](https://github.com/wda067/TIL/search?q=%23algorithm&type=code)