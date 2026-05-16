# [LeetCode] 198. House Robber

> LeetCode | Medium(정답률 53.2%) | DP | C++

## 문제 요약
- 일렬로 늘어선 집에서 인접한 두 집을 연속으로 털 수 없음
- 각 집에 있는 금액이 주어질 때, 털 수 있는 최대 금액을 구하기

## 풀이 접근
1. `answer[i]` = i번째 집까지 고려했을 때의 최대 금액
2. 점화식: `answer[i] = max(answer[i-1], answer[i-2] + nums[i])`
3. 크기 1, 2인 경우 별도 early return 처리
4. 시간복잡도 O(N), 공간복잡도 O(N)

## 내 코드
```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size() == 1)
            return nums[0];
        if(nums.size() == 2)
            return max(nums[0], nums[1]);

        vector<int> answer = {nums[0]};
        answer.push_back(max(nums[0], nums[1]));
        for(int i=2; i<nums.size(); i++) {
            answer.push_back(max(answer[i-1], answer[i-2]+nums[i]));
        }
        return answer[nums.size()-1];
    }
};
```

## 리뷰
- 배열 대신 변수 두 개(`prev1`, `prev2`)만 유지하면 공간복잡도를 O(1)로 줄일 수 있음
- 처음부터 `answer` 배열을 nums 크기로 잡고 `answer[0]`, `answer[1]`을 세팅하면 early return 없이 통합 처리 가능