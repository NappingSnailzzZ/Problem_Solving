# [AtCoder] Sum of Intervals

> AtCoder | 누적합, 해시맵 | C++

## 문제 요약
- N개의 정수 배열(음수 포함)에서 연속 부분 배열의 합이 정확히 K인 (l, r) 쌍의 개수를 구함
- 1 ≤ N ≤ 2×10⁵, -10⁹ ≤ K, Aᵢ ≤ 10⁹

## 풀이 접근
1. 누적합 S[i] = A[1] + ... + A[i]로 정의하면, 구간 합 A[l] + ... + A[r] = S[r] - S[l-1]
2. S[r] - S[l-1] = K → S[l-1] = S[r] - K
3. 따라서 현재까지의 누적합 s에 대해, 이전에 등장한 `s - K` 값의 횟수가 곧 현재 위치에서 끝나는 유효 구간의 수
4. `unordered_map`에 각 누적합의 등장 횟수를 기록하며 한 번의 순회로 해결
5. 초기값 `mp[0] = 1`: 누적합 자체가 K인 경우(l=1부터 시작하는 구간)를 처리
6. 시간복잡도: O(N), 공간복잡도: O(N)

## 내 코드
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(void) {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    int n;
    long long k, s=0, temp, answer=0;
    unordered_map<long long, long long> mp;
    cin >> n >> k;
    mp[0] = 1;
    for(int i=0; i<n; i++) {
        cin >> temp;
        s += temp;
        if(mp.count(s-k))
            answer += mp[s-k];
        mp[s]++;
    }
    cout << answer;
    return 0;
}
```

## 리뷰
- 누적합 + 해시맵 조합은 "구간 합 = 특정 값" 문제의 정석
- 음수가 있어서 투 포인터는 사용 불가 → 누적합 변환이 필수. 양수만 있으면 투 포인터, 음수 포함이면 누적합+해시맵
- `mp[s-k]`를 조회한 뒤에 `mp[s]++`하는 순서가 중요. 순서가 바뀌면 K=0일 때 자기 자신을 카운트하는 오류 발생
- `unordered_map`의 최악 시간복잡도는 O(N)이므로 해시 충돌이 심하면 TLE 가능성이 있음. 걱정되면 `map`(O(N log N))이나 커스텀 해시를 사용하는 방법도 있음