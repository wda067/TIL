## [Algorithm] 오늘 푼 문제

### [이모티콘 할인행사](https://school.programmers.co.kr/learn/courses/30/lessons/150368) 

주어진 데이터의 범위가 크지 않으므로 완전 탐색으로 해결할 수 있다. 모든 이모티콘의 가능한 할인율 조합을 구한 뒤 구독자 수와 판매액을 계산한다. 그리고 우선 순위에 맞게 최댓값을 갱신하면 된다.

### [가장 긴 증가하는 부분 수열 3](https://www.acmicpc.net/problem/12738) 

전체 수열의 길이가 최대 100만이다. 따라서 시간 복잡도 $O(n^2)$인 2중 for문을 사용하는 DP로는 해결할 수 없다. 대신 시간 복잡도가 $O(logn)$인 이진 탐색을 이용해 LIS의 길이를 구하면 최종적으로 $O(nlogN)$으로 해결할 수 있다.

[LIS DP vs 이진 탐색 정리](https://velog.io/@wda067/Algorithm-LIS-feat.-DP-vs-%EC%9D%B4%EC%A7%84-%ED%83%90%EC%83%89)
***

[#algorithm](https://github.com/wda067/TIL/search?q=%23algorithm&type=code)