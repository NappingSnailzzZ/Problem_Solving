# [AtCoder] Parking Lot Assignment

> AtCoder | 그리디, 우선순위 큐 | C++

## 문제 요약
- N개의 주차 공간(1~N)과 M대의 차량
- i번째 차량은 [Lᵢ, Rᵢ] 범위의 주차 공간 중 하나에 배정 가능
- 모든 차량에 겹치지 않게 주차 공간을 배정할 수 있는지 판별

## 풀이 접근
1. 구간 스케줄링의 변형 — 각 차량의 주차 가능 구간을 "작업", 주차 공간을 "자원"으로 봄
2. 차량을 L 기준 오름차순 정렬
3. 주차 공간 1번부터 순회하면서, 해당 공간부터 주차 가능한 차량들을 min-heap에 R값 기준으로 삽입
4. 매 공간마다 heap에서 R이 가장 작은 차량(마감이 가장 급한 차량)을 배정 → 그리디
5. heap top의 R이 현재 위치 i보다 작으면 배정 불가능한 차량 존재 → "No"
6. 루프 종료 후 pq에 차량이 남아 있으면 배정받지 못한 차량 존재 → "No"
7. 시간복잡도: O(M log M) (정렬 + 힙 연산)

## 내 코드
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(void) {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    int n, m, j=0;
    cin >> n >> m;
    vector<pair<int, int>> car(m);
    for(int i=0; i<m; i++) {
        cin >> car[i].first >> car[i].second;
    }
    sort(car.begin(), car.end());
    priority_queue<int, vector<int>, greater<int>> pq;
    for(int i=1; i<=n; i++) {
        while(j<m && car[j].first==i) {
            pq.push(car[j].second);
            j++;
        }
        if(!pq.empty()) {
            if(pq.top()<i) {
                cout << "No";
                return 0;
            }
            else
                pq.pop();
        }
    }
    if(!pq.empty()) {
        cout << "No";
        return 0;
    }
    cout << "Yes";
    return 0;
}
```

## 리뷰
- "마감이 가장 급한 것부터 처리"하는 EDF(Earliest Deadline First) 그리디의 전형적인 적용
- 루프 중 `pq.top() < i` 체크는 "이미 지나간 구간의 차량"을 잡아내고, 루프 후 `!pq.empty()` 체크는 "공간이 부족해서 배정받지 못한 차량"을 잡아냄. 두 가지 실패 조건을 모두 커버해야 정답
- 비슷한 구간 매칭/스케줄링 문제에서는 "한쪽 끝 정렬 + 다른 쪽 끝 기준 힙"이 거의 정석 패턴