# [LeetCode] 74. Search a 2D Matrix

> LeetCode | Medium(정답률 53.9%) | 이진 탐색 | C++

## 문제 요약
- m x n 정수 행렬에서 target 존재 여부를 판별
- 각 행은 오름차순 정렬, 다음 행의 첫 원소는 이전 행의 마지막 원소보다 큼
- O(log(m * n)) 시간복잡도 필수

## 풀이 접근
1. 행렬 전체가 사실상 하나의 정렬된 1차원 배열이므로, 이분 탐색 두 번으로 해결
2. 탐색 전에 target이 전체 범위 밖인지 return으로 빠르게 걸러냄
3. 각 행의 첫 번째 원소를 기준으로 이진 탐색을 시행해 target이 속하는 행을 찾음
4. 찾은 행 내에서 이진 탐색으로 target 찾기
5. 시간복잡도 분석: O(log M + log N) = O(log(M*N))

## 내 코드
```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int row = matrix.size();
        int col = matrix[0].size();
        if(target < matrix[0][0] || target > matrix[row-1][col-1])
            return false;
        
        int l = 0;
        int r = row-1;
        int i = 0;

        while(l <= r) {
            i = (l+r)/2;
            if(matrix[i][0]<=target && (i+1 >= row || matrix[i+1][0] > target))
                break;
            else if(matrix[i][0] > target)
                r = i - 1;
            else
                l = i + 1;
        }
        
        l = 0;
        r = col - 1;
        int j = 0;
        while(l <= r) {
            j = (l+r)/2;
            if(matrix[i][j] == target)
                return true;
            else if(matrix[i][j] > target)
                r = j - 1;
            else
                l = j + 1;
        }

        return false;
    }
};
```

## 리뷰
- 행 찾기 조건 `matrix[i][0] <= target && matrix[i+1][0] > target`이 upper_bound 로직과 동일. `upper_bound`로 행을 찾은 뒤 한 칸 앞으로 가면 코드가 더 간결해질 수 있음
- 2차원 인덱스를 1차원으로 펴서 (`mid / col`, `mid % col`) 이진 탐색 한 번으로 처리하는 방법도 있음.