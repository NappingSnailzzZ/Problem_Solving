# [AtCoder] Splitting Delivery Packages

> AtCoder | 400pts | 이분 탐색, 그리디 | C++

## 문제 요약
- N개의 짐을 순서 유지한 채 K개의 연속 구간으로 분할
- 각 트럭의 적재량 = 해당 구간의 무게 합
- K대 트럭 중 최소 적재량을 최대화

## 풀이 접근
1. "최솟값의 최댓값" → 이분 탐색의 전형적 시그널
2. mid를 "최소 적재량의 후보"로 잡고, 각 트럭에 최소 mid 이상을 실을 수 있는지 그리디로 검증
3. 검증 함수: 앞에서부터 누적합을 쌓다가 mid 이상이 되면 트럭 하나를 채운 것으로 카운트하고 누적합 리셋
4. 만들 수 있는 트럭 수(cnt) ≥ K이면 mid를 더 올릴 수 있음 → `l = mid + 1`
5. cnt < K이면 mid가 너무 큼 → `r = mid - 1`
6. 시간복잡도: O(N log S), S = 전체 무게 합 (최대 약 10¹⁴)

## 내 코드
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(void) {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    int n, k, cnt;
    long long sum=0, l, r, mid, answer=0;
    cin >> n >> k;
    vector<int> a(n);
    for(int i=0; i<n; i++) {
        cin >> a[i];
        sum += a[i];
    }
    l = 1;
    r = sum;
    while(l<=r) {
        mid = (l+r) / 2;
        cnt = 0;
        sum = 0;
        for(int i=0; i<n; i++) {
            sum += a[i];
            if(sum>=mid) {
                cnt++;
                sum = 0;
            }
        }
        if(cnt>=k) {
            answer = mid;
            l = mid+1;
        }
        else
            r = mid-1;
    }
    cout << answer;
    return 0;
}
```

## 리뷰
- "최솟값 최대화" / "최댓값 최소화" 패턴이 보이면 이분 탐색 + 그리디 검증을 바로 떠올릴 것. 이 문제는 그 정석적인 형태
- 또한 `r = sum`을 설정한 뒤 while 내부에서 `sum = 0`으로 리셋하는데, r에는 이미 원래 총합이 저장되어 있으므로 동작에는 문제없음. 다만 역할이 다른 값에 같은 변수를 쓰면 가독성이 떨어지므로 `totalSum` 등 별도 변수를 두는 것을 추천
- 검증 로직에서 마지막 트럭의 남은 짐은 카운트되지 않을 수 있지만, 그 짐이 mid 미만이라도 K개 트럭이 이미 채워졌으면 나머지를 마지막 트럭에 합치면 되므로 정확함