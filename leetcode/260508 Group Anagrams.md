# [LeetCode] 49 Group Anagrams

> LeetCode | Medium(Acceptance 72.6%) | 해시맵, 정렬 | C++

## 문제 요약
- 문자열 배열에서 애너그램끼리 그룹으로 묶어 반환
- 문자열 길이 ≤ 100, 배열 길이 ≤ 10,000

## 풀이 접근
1. 각 문자열을 정렬하면 애너그램끼리 동일한 키가 됨 ("eat", "tea", "ate" → "aet")
2. 정렬된 문자열을 키로 사용하여 같은 그룹인지 판별
3. `for_answer` 배열에 정렬된 키를, `answer` 배열에 원본 문자열을 병렬로 관리
4. 새 문자열이 올 때마다 기존 그룹을 순회하며 키가 일치하는지 탐색
5. 시간복잡도 분석: O(N² * K) (N: 문자열 수, K: 문자열 최대 길이)

## 내 코드
```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<string> sorted_strs = strs;
        for(int i=0; i<strs.size(); i++) {
            sort(sorted_strs[i].begin(), sorted_strs[i].end());
        }
        vector<vector<string>> for_answer;
        vector<vector<string>> answer;
        bool isNew = true;
        for(int i=0; i<strs.size(); i++) {
            for(int j=0; j<answer.size(); j++) {
                if(sorted_strs[i] == for_answer[j][0]) {
                    answer[j].push_back(strs[i]);
                    isNew = false;
                    break;
                }

            }
            if(isNew) {
                vector<string> temp;
                temp.push_back(strs[i]);
                answer.push_back(temp);
                temp[0] = sorted_strs[i];
                for_answer.push_back(temp);
            }
            isNew = true;
        }
        return answer;
    }
};
```

## 리뷰
- 현재 그룹 탐색을 이중 for문으로 하고 있어 O(N²). `unordered_map`을 쓰면 키 조회가 O(1)이 되어 전체 O(N * KlogK)로 개선 가능
- 다음에 비슷한 문제를 만나면: "같은 그룹끼리 묶어라" 패턴에서는 그룹의 키를 정의한 뒤 해시맵으로 분류하는 것이 가장 깔끔하고 효율적