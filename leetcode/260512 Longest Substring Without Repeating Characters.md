# [LeetCode] 3. Longest Substring Without Repeating Characters

> LeetCode | Medium(정답률 39.0%) | 슬라이딩 윈도우, 해시맵 | C++

## 문제 요약
- 문자열 s에서 중복 문자가 없는 가장 긴 부분 문자열의 길이를 구하기
- 영문, 숫자, 기호, 공백 모두 포함 가능

## 풀이 접근
1. 슬라이딩 윈도우: start를 왼쪽 경계로, i를 오른쪽으로 확장하며 탐색
2. unordered_map으로 현재 윈도우 내 각 문자의 등장 횟수를 관리
3. 특정 문자의 카운트가 2가 되면 중복 발생 → 해당 문자의 카운트가 1이 될 때까지 start를 오른쪽으로 이동하며 윈도우 축소
4. 마지막 문자까지 중복 없이 도달하는 경우를 별도로 처리
5. 시간복잡도 분석: O(N)(각 문자가 start, i에 의해 최대 2번씩 방문)

## 내 코드
```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int answer = 0;
        int start = 0;
        int end = 0;
        unordered_map<char, int> mp;
        for(int i=0; i<s.length(); i++) {
            mp[s[i]]++;
            if(mp[s[i]] == 2) {
                answer = max(answer, i-start);
                while(mp[s[i]] == 2) {
                    mp[s[start]]--;
                    start++;
                }
            }
            else if(i == s.length()-1) {
                answer = max(answer, i-start+1);
            }
        }
        return answer;
    }
};
```

## 리뷰
- 중복 감지를 빈도 카운트 기반(`mp[s[i]] == 2`)으로 처리하여, while문에서 정확히 중복이 해소되는 시점까지 축소할 수 있음
- 윈도우 축소 시 start를 한 칸씩 이동시켜 중복 문자가 윈도우 중간에 있어도 오류가 없음
- `end` 변수를 선언했지만 i와 역할이 겹쳐 사용하지 않음
- map에 해당 문자의 마지막 등장 인덱스를 저장하면 중복 발생 시 `start = max(start, mp[s[i]] + 1)`로 한 번에 점프 가능. while문 없이 순수 O(N)이 되고, 윈도우 내 문자 카운트 감소 처리도 불필요해짐