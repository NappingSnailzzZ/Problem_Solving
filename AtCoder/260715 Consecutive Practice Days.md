# [AtCoder] Consecutive Practice Days

> AtCoder | 400pts | 투 포인터, 누적합 | C++

## 문제 요약
- N일간의 연습 강도 배열이 주어짐
- 연속 구간 (l, r)이 "성취 기간"이 되려면: 길이 ≥ K, 구간 합 ≥ M
- 조건을 만족하는 (l, r) 쌍의 개수를 구함

## 풀이 접근
1. 연습 강도가 양수이므로, 고정된 l에서 r을 늘리면 합이 단조증가 → 투 포인터 적용 가능
2. 핵심 관찰: 어떤 (l, r)에서 두 조건을 모두 만족하면, r을 더 늘린 (l, r+1), (l, r+2), ..., (l, n-1)도 모두 만족 → `answer += n - r`로 한 번에 카운트
3. 조건을 만족하면 l을 오른쪽으로 이동(구간 축소), 불만족이면 r을 오른쪽으로 이동(구간 확장)
4. 초기 윈도우를 길이 K로 설정해서 최소 길이 조건을 자연스럽게 충족
5. 시간복잡도: O(N) — l과 r 각각 최대 N번 이동

## 내 코드
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(void) {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    int n, k;
    long long m, sum=0, answer=0;
    cin >> n >> k >> m;
    vector<int> practice(n);
    for(int i=0; i<n; i++)
        cin >> practice[i];
    
    int l=0, r=k-1;
    for(int i=l; i<=r; i++)
        sum += practice[i];
    
    while(r<n) {
        if(r-l+1>=k && sum>=m) {
            answer += n-r;
            sum -= practice[l];
            l++;
        }
        else {
            r++;
            if(r<n)
                sum += practice[r];
        }
    }
    cout << answer;
    return 0;
}
```

## 리뷰
- 양수 배열에서의 구간 합 조건 → 투 포인터는 정석 패턴. "조건 만족 시 나머지 확장도 전부 만족"이라는 성질을 이용해 `n - r`로 일괄 카운트하는 게 핵심
- 초기 윈도우를 `[0, k-1]`로 잡아서 길이 조건(`r-l+1 >= k`)을 자연스럽게 보장한 점이 좋음. 다만 l이 증가해서 길이가 K 미만이 될 수 있는데, 그 경우 조건 불만족으로 r이 확장되므로 문제없이 동작
- 비슷한 "조건 만족 구간 개수 세기"에서는 항상 단조성(한쪽을 늘리면 조건이 강화/약화)을 확인하고, 단조성이 있으면 투 포인터를 떠올릴 것