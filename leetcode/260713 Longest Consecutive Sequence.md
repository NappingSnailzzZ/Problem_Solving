# [LeetCode 128] Longest Consecutive Sequence

> LeetCode | Medium(Acceptance 47.2%) | 해시셋 | C++

## 문제 요약
- 정렬되지 않은 정수 배열에서 가장 긴 연속 원소 수열의 길이를 반환
- O(N) 시간복잡도 필수

## 풀이 접근
1. 전체 원소를 `unordered_set`에 삽입 → O(1) 탐색 확보
2. 핵심 아이디어: `it-1`이 셋에 없는 원소만 시퀀스의 시작점으로 판단
3. 시작점에서부터 `it+1, it+2, ...` 을 셋에서 연속으로 찾으며 길이 측정
4. 시작점이 아닌 원소는 while문을 돌지 않으므로, 전체적으로 각 원소가 최대 한 번만 카운트됨
5. 시간복잡도: O(N), 공간복잡도: O(N)

## 내 코드
```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        int answer = 0;
        unordered_set<int> num(nums.begin(), nums.end());
        for(int it : num) {
            if(num.find(it-1) == num.end()) {
                int length = 1;
                while(num.find(it+length) != num.end()) {
                    length++;
                }
                answer = max(answer, length);
            }
        }
        return answer;
    }
};
```

## 리뷰
- "시작점만 탐색한다"는 아이디어가 이 풀이의 핵심. 이중 루프처럼 보이지만 각 원소가 while 내부에서 최대 한 번만 방문되므로 O(N)이 보장됨
- `unordered_set` 생성자에 이터레이터를 바로 넘겨서 중복 제거와 삽입을 한 줄로 처리한 점이 깔끔
- 정렬 기반 풀이(O(N log N))도 가능하지만, 문제가 O(N)을 명시적으로 요구함