# [AtCoder] Staircase-Shaped Flower Bed

> AtCoder | 333pts | 그리디, 양방향 스위핑 | C++

## 문제 요약
- N개의 화단에 각각 Aᵢ송이의 꽃이 심어져 있음
- 인접한 화단의 꽃 수 차이가 K 이하여야 "아름다운" 상태
- 꽃은 추가만 가능(제거 불가), 추가하는 꽃의 총 수를 최소화
- 1 ≤ N ≤ 2×10⁵, 1 ≤ K, Aᵢ ≤ 10⁹

## 풀이 접근
1. 각 화단 i의 최종 값 Bᵢ는 자기 자신(Aᵢ) 이상이면서, 양쪽 이웃과의 차이가 K 이하여야 함
2. 왼쪽에서의 제약: i-1번 화단이 Bᵢ₋₁이면 Bᵢ ≥ Bᵢ₋₁ - K → 왼쪽에서 전파되는 하한을 `l[i] = max(l[i-1] - K, a[i])`로 계산
3. 오른쪽에서의 제약: 동일한 논리로 `r[i] = max(r[i+1] - K, a[i])`를 역방향으로 계산
4. 최종 Bᵢ = max(l[i], r[i]) → 양쪽 제약을 모두 만족하는 최솟값
5. 답 = Σ(Bᵢ - Aᵢ)
6. 시간복잡도: O(N)

## 내 코드
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(void) {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    int n, k;
    long long answer=0;
    cin >> n >> k;
    if(n==1) {
        cout << 0;
        return 0;
    }
    vector<int> a(n), b(n), l(n), r(n);
    for(int i=0; i<n; i++)
        cin >> a[i];
    l[0] = a[0];
    r[n-1] = a[n-1];
    for(int i=1; i<n; i++) {
        l[i] = max(l[i-1]-k, a[i]);
        r[n-1-i] = max(r[n-i]-k, a[n-1-i]);
    }
    for(int i=0; i<n; i++) {
        b[i] = max(l[i], r[i]);
        answer += abs(b[i]-a[i]);
    }
    cout << answer;
    return 0;
}
```

## 리뷰
- "양방향 스위핑으로 각 위치의 하한을 구한 뒤 max를 취한다"는 패턴이 핵심. 한쪽 방향만으로는 반대편의 제약을 반영할 수 없기 때문에 양방향이 필수
- `abs(b[i]-a[i])`에서 b[i] ≥ a[i]가 항상 보장되므로 abs 없이 `b[i]-a[i]`로도 충분하지만, 안전하게 넣은 것으로 보임
- l, r, b 배열이 int인데 `l[i-1]-k`에서 값이 음수가 될 수 있음. a[i]와 max를 취하므로 최종 결과는 양수지만, 중간 계산에서 언더플로우 걱정은 없는 범위(10⁹ - 10⁹ = 0이 최소)
- 비슷한 "양쪽 이웃 제약 + 최솟값 최적화" 문제에서는 좌→우, 우→좌 두 번 스위핑 후 합치는 패턴을 떠올릴 것