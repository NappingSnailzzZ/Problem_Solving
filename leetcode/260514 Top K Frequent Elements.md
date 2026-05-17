# [LeetCode] 347. Top K Frequent Elements

> LeetCode | Medium(정답률 66.4%) | 해시맵, 정렬 | C++

## 문제 요약
- 정수 배열 nums에서 가장 빈도가 높은 k개의 원소를 반환
- 반환 순서는 상관없음
- 정답은 유일하게 결정됨이 보장됨

## 풀이 접근
1. unordered_map으로 각 원소의 빈도를 카운팅
2. 맵의 내용을 vector<pair>로 옮긴 뒤, 빈도 기준 내림차순 정렬
3. 앞에서 k개를 꺼내서 반환
4. 시간복잡도 분석: O(N + U log U) (U = 고유 원소 수)

## 내 코드
```cpp
bool cmp(pair<int, int> a, pair<int, int> b) {
    return a.second > b.second;
}

class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> mp;
        for(auto& it : nums)
            mp[it]++;
        vector<pair<int, int>> cnt(mp.begin(), mp.end());
        sort(cnt.begin(), cnt.end(), cmp);
        vector<int> answer;
        for(int i=0; i<k; i++)
            answer.push_back(cnt[i].first);
        return answer;
    }
};
```

## 리뷰
- cmp 함수에서 pair를 값으로 받고 있음(`pair<int, int> a`). `const pair<int, int>&`로 받으면 불필요한 복사를 피할 수 있음.
- 대안 풀이 — 최소 힙: priority_queue에 크기 k를 유지하며 빈도 기준으로 관리. 빈도가 힙의 top보다 크면 교체. O(U log K)