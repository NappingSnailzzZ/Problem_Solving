# [LeetCode] 155. Min Stack

> LeetCode | Medium(정답률 58.1%) | 스택, 자료구조 설계 | C++

## 문제 요약
- push, pop, top, getMin 모든 연산이 O(1)인 스택을 설계
- getMin은 현재 스택에서 최솟값을 반환

## 풀이 접근
1. 메인 스택과 별도로 최솟값 추적용 보조 스택(min)을 운용
2. push 시 현재 값이 min 스택의 top 이하이면 min에도 push
3. pop 시 빠지는 값이 min의 top과 같으면 min에서도 pop
4. getMin은 min 스택의 top을 그대로 반환하므로 O(1)
5. 시간복잡도 분석: 모든 연산 O(1), 공간복잡도 최악 O(N)

## 내 코드
```cpp
class MinStack {
public:
    stack<int> st;
    stack<int> min;
    MinStack() {
        
    }
    
    void push(int val) {
        st.push(val);
        if(min.empty() || min.top() >= val)
            min.push(val);
    }
    
    void pop() {
        if(st.top() == min.top())
            min.pop();
        st.pop();
    }
    
    int top() {
        if(!st.empty())
            return st.top();
        else
            return 0;
    }
    
    int getMin() {
        if(!min.empty())
            return min.top();
        else
            return 0;
    }
};
```

## 리뷰
- 변수명 `min`이 `std::min`과 충돌할 수 있으므로 `minSt`나 `minStack` 등으로 바꾸는 게 안전
- 보조 스택 없이 메인 스택에 `{val, 현재까지의 min}` 페어를 저장하는 방법도 있음. 구현이 더 단순해지고 pop 시 조건 분기가 필요 없어짐