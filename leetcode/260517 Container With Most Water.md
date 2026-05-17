# [LeetCode] 11. Container With Most Water

> LeetCode | Medium(정답률 60.0%) | 투 포인터, 그리디 | C++

## 문제 요약
- 높이 배열이 주어질 때, 두 선분과 x축으로 만들어지는 컨테이너 중 최대 넓이를 구하기
- 넓이 = (두 선분 사이 거리) × min(두 선분 높이)

## 풀이 접근
1. 양 끝에서 시작하는 투 포인터로 탐색
2. 매 스텝마다 현재 넓이를 계산하여 최댓값 갱신
3. 두 포인터 중 높이가 더 낮은 쪽을 안쪽으로 이동해야 함
4. 시간복잡도 분석: O(N), 공간복잡도 O(1)

## 내 코드
```cpp
class Solution {
public:
    int maxArea(vector& height) {
        int answer = 0;
        int l = 0;
        int r = height.size()-1;
        while(lheight[r])
                r--;
            else
                l++;
        }
        return answer;
    }
};
```

## 리뷰
- height[l]과 height[r] 중 낮은 쪽을 버려야 최적이 반드시 탐색됨
- `height[l] == height[r]`인 경우 `l++`과 `r--`중 어느 것을 해도 손실 없음