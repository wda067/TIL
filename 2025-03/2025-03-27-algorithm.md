## [Algorithm] 오늘 푼 문제

### [단어 수학](https://www.acmicpc.net/problem/1339) 

처음에는 각 단어의 알파벳에 큰 수부터 할당하면서 모든 알파벳에 수가 할당됐을 때 합을 구해서 최댓값을 구하도록 백트래킹으로 구현했다.

```java
private static void recur(int depth, Map<Character, Integer> map, boolean[] visited) {
    if (depth == alphabetCount) {  //모든 알파벳에 수를 할당한 경우
        if (map.size() == alphabetCount) {
            int total = 0;  //각 단어 수의 합
            for (String word : words) {
                int n = 1;
                for (int i = word.length() - 1; i >= 0; i--) {
                    total += map.get(word.charAt(i)) * n;
                    n *= 10;
                }
            }
            max = Math.max(max, total);
        }
        return;
    }
    for (int i = 0; i < alphabetCount; i++) {
        int num = 9 - i;  //큰 수부터 할당
        if (!visited[num]) {  //숫자는 한 번씩만 사용
            visited[num] = true;
            map.put(list.get(depth), num);
            recur(depth + 1, map, visited);
            visited[num] = false;
            map.remove(list.get(depth));
        }
    }
}
```

통과는 했지만 최악의 경우 $10!$만큼의 경우의 수에 대해 탐색해야 하므로 비효율적이었다. 또한 백트래킹은 탐색 도중 불가능한 경우를 빠르게 배제할 수 있을 때 효과적이다. 여기서는 결국 끝까지 모든 알파벳에 대해 탐색해야 한다.

이 문제는 알파벳마다 가중치를 구해서 큰 자릿수일 때 9부터 할당하여 최대합을 계산하면 빠르게 구할 수 있다.

```java
public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int N = Integer.parseInt(br.readLine());
        Map<Character, Integer> map = new HashMap<>();

        //알파벳별 가중치 계산
        for (int i = 0; i < N; i++) {
            String word = br.readLine();
            int len = word.length();
            int pow = (int) Math.pow(10, len - 1);

            for (int j = 0; j < len; j++) {
                char c = word.charAt(j);
                map.put(c, map.getOrDefault(c, 0) + pow);
                pow /= 10;
            }
        }

        List<Integer> weights = new ArrayList<>(map.values());
        weights.sort(Comparator.reverseOrder());  //가중치가 큰 순서로 정렬

        //가중치 순서대로 9부터 할당하여 최대합 계산
        int num = 9, result = 0;
        for (Integer w : weights) {
            result += w * num;
            num--;
        }

        System.out.println(result);
    }
}
```

### [파괴되지 않은 건물](https://school.programmers.co.kr/learn/courses/30/lessons/92344)

주어진 맵에서 단순히 주어진 범위의 값을 갱신해나가는, 완전탐색만 하면 답은 쉽게 나온다. 하지만 시간 복잡도가 $O(N*M*K)$가 되어 시간 초과가 발생한다. 

가령 `[1, 2, 4, 8, 9]`의 배열이 있고 `(0, 3)` 범위의 값을 2만큼 빼야 된다고 할 때, 반복문으로 일일이 빼준다면 $O(M)$의 시간 복잡도가 걸린다. 그래서 `[-2, 0, 0, 0, 2]`인 배열을 생성하고, 앞에서 부터 누적합을 한다면 `[-2, -2, -2, -2, 0]`가 되므로 기존 배열과 합해주면 $O(1)$의 시간 복잡도로 해결이 가능하다.

이 문제에서는 이걸 2차원 배열로 적용하면 된다. 

```java
changes[r1][c1] += degree;  //좌측 상단
changes[r1][c2 + 1] -= degree;  //우측 상단
changes[r2 + 1][c1] -= degree;  //좌측 하단
changes[r2 + 1][c2 + 1] += degree;  //우측 하단

//행 방향 누적 합
for (int i = 0; i <= N; i++) {
    for (int j = 1; j <= M; j++) {
        changes[i][j] += changes[i][j - 1];
    }
}

//열 방향 누적 합
for (int j = 0; j <= M; j++) {
    for (int i = 1; i <= N; i++) {
        changes[i][j] += changes[i - 1][j];
    }
}

//맵 업데이트 및 결과 계산
int answer = 0;
for (int i = 0; i < N; i++) {
    for (int j = 0; j < M; j++) {
        board[i][j] += changes[i][j];
        if (board[i][j] > 0) {
            answer++;
        }
    }
}
```

***

[#algorithm](https://github.com/wda067/TIL/search?q=%23algorithm&type=code)