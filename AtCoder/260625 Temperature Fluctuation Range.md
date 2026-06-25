# [AtCoder AWC0001] E - Temperature Fluctuation Range

> AtCoder Weekday Contest 0001 Beta | 466pts | 슬라이딩 윈도우, 모노톤 덱 | C++

## 문제 요약
- N일간의 기온 데이터에서 연속 K일을 선택
- 해당 구간의 (최대 기온 - 최소 기온)을 최대화
- 1 ≤ K ≤ N ≤ 2×10⁵, -10⁹ ≤ Hᵢ ≤ 10⁹

## 풀이 접근
1. 길이 K의 슬라이딩 윈도우에서 최대/최소를 빠르게 구해야 함
2. 모노톤 덱 두 개를 사용: maxDq(내림차순 유지), minDq(오름차순 유지)
3. 각 덱의 front가 현재 윈도우의 최대/최소를 가리킴
4. 윈도우를 벗어난 인덱스는 front에서 제거
5. 매 스텝마다 `v[maxDq.front()] - v[minDq.front()]`의 최대를 갱신
6. 시간복잡도: O(N) — 각 원소가 덱에 최대 한 번 들어가고 한 번 나옴

## 내 코드
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
#include <deque>
using namespace std;
int main(void) {
    int n, k, answer=0;
    cin >> n >> k;
    vector<int> v(n);
    for(int i=0; i<n; i++)
        cin >> v[i];
    deque<int> maxDq, minDq;
    for(int i=0; i<n; i++) {
        while(!maxDq.empty() && v[maxDq.back()]<=v[i])
            maxDq.pop_back();
        maxDq.push_back(i);
        while(!minDq.empty() && v[minDq.back()]>=v[i])
            minDq.pop_back();
        minDq.push_back(i);
        if(maxDq.front()<=i-k)
            maxDq.pop_front();
        if(minDq.front()<=i-k)
            minDq.pop_front();
        answer = max(answer, v[maxDq.front()]-v[minDq.front()]);
    }
    cout << answer;
    return 0;
}
```

## 리뷰
- 고정 길이 윈도우에서 최대-최소가 모노톤 덱 활용
- answer 초기값이 0인데, 기온이 음수여도 (최대 - 최소)는 항상 ≥ 0이므로 문제없음
- i < k-1일 때도 answer를 갱신하고 있지만 윈도우가 최종 답에 영향은 없음. `if(i >= k-1)` 조건을 넣는다면 의도가 더 명확해짐