# C++17 STL 코딩테스트 정리

---

## 1. 컨테이너 (Containers)

### vector — 가변 크기 배열

코딩테스트에서 가장 많이 쓰는 컨테이너. 크기를 미리 정하지 않아도 되고, 뒤에서 원소 추가/삭제가 O(1).

```cpp
#include <vector>

vector<int> v;             // 빈 벡터
vector<int> v(5, 0);       // 크기 5, 모두 0으로 초기화
vector<int> v = {1, 2, 3}; // 초기화 리스트

v.push_back(4);    // 뒤에 추가 → {1,2,3,4}
v.pop_back();      // 뒤에서 제거 → {1,2,3}
v.size();          // 원소 개수: 3
v.empty();         // 비었는지 확인
v[0];              // 인덱스 접근: 1
v.front();         // 첫 번째 원소
v.back();          // 마지막 원소
v.clear();         // 전부 삭제

// 2차원 벡터 (그래프, 행렬에 자주 사용)
vector<vector<int>> graph(n); // n개의 빈 벡터
graph[0].push_back(1);        // 0번 노드 → 1번 노드 연결
```

### array — 고정 크기 배열

C 스타일 배열 대신 쓸 수 있음. 크기가 컴파일 타임에 정해져야 함.

```cpp
#include <array>

array<int, 5> arr = {1, 2, 3, 4, 5};
arr.size();  // 5
arr[2];      // 3
```

### string — 문자열

문자열 처리 문제에서 필수.

```cpp
#include <string>

string s = "hello";
s += " world";           // 이어붙이기: "hello world"
s.substr(0, 5);          // "hello" (시작위치, 길이)
s.find("world");         // 6 (찾은 위치, 못 찾으면 string::npos)
s.length();              // 11
s[0] = 'H';              // 인덱스로 수정
s.erase(5, 1);           // 5번 위치에서 1글자 삭제

// 숫자 ↔ 문자열 변환
string num_str = to_string(42);   // "42"
int num = stoi("42");             // 42
long long big = stoll("123456789012345");
```

### pair — 두 값을 묶기

좌표, 간선(가중치+노드) 등을 표현할 때 자주 사용.

```cpp
#include <utility>

pair<int, int> p = {3, 5};
p.first;   // 3
p.second;  // 5

auto p2 = make_pair(1, 2);

// 비교: first 먼저, 같으면 second 비교
vector<pair<int,int>> edges = {{3,1}, {1,2}, {3,0}};
sort(edges.begin(), edges.end());
// → {1,2}, {3,0}, {3,1}
```

### tuple — 세 개 이상 묶기

pair로 부족할 때 사용.

```cpp
#include <tuple>

tuple<int, int, int> t = {1, 2, 3};
get<0>(t); // 1
get<1>(t); // 2

// C++17 구조화 바인딩
auto [a, b, c] = t;  // a=1, b=2, c=3
auto [x, y] = make_pair(10, 20);
```

### stack — 후입선출 (LIFO)

DFS, 괄호 검사, 히스토리 관리 등에 사용.

```cpp
#include <stack>

stack<int> st;
st.push(1);
st.push(2);
st.top();     // 맨 위 확인: 2 (제거 안 함)
st.pop();     // 맨 위 제거 (반환값 없음!)
st.empty();
st.size();
```

### queue — 선입선출 (FIFO)

BFS에서 거의 반드시 사용.

```cpp
#include <queue>

queue<int> q;
q.push(1);
q.push(2);
q.front();    // 앞 확인: 1
q.back();     // 뒤 확인: 2
q.pop();      // 앞에서 제거
```

### priority_queue — 우선순위 큐 (힙)

다익스트라 알고리즘, 가장 큰/작은 값 반복 추출 등에 필수.

```cpp
#include <queue>

// 기본: 최대 힙 (큰 것이 먼저)
priority_queue<int> maxPQ;
maxPQ.push(3); maxPQ.push(1); maxPQ.push(5);
maxPQ.top();  // 5

// 최소 힙 (작은 것이 먼저) — 코딩테스트에서 매우 자주 씀!
priority_queue<int, vector<int>, greater<int>> minPQ;
minPQ.push(3); minPQ.push(1); minPQ.push(5);
minPQ.top();  // 1

// pair와 함께 (다익스트라)
priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
pq.push({10, 1});
pq.push({3, 2});
auto [dist, node] = pq.top(); // dist=3, node=2
```

### deque — 양쪽에서 넣고 빼기

앞뒤 모두 O(1)으로 추가/삭제 가능. 슬라이딩 윈도우 등에 활용.

```cpp
#include <deque>

deque<int> dq;
dq.push_back(1);
dq.push_front(0);
dq.pop_back();
dq.pop_front();
dq.front();
dq.back();
```

### set — 정렬된 고유 원소 집합

중복 없이 정렬 상태 유지. 삽입/삭제/탐색 모두 O(log n).

```cpp
#include <set>

set<int> s;
s.insert(3);
s.insert(1);
s.insert(3);   // 중복 무시 → {1, 3}

s.count(3);    // 1 (있으면 1, 없으면 0)
s.erase(3);
s.find(1);     // 이터레이터 반환 (없으면 s.end())

// 범위 탐색
s.insert(5); s.insert(7); s.insert(10);
auto it = s.lower_bound(6);  // 6 이상인 첫 원소 → 7
auto it2 = s.upper_bound(7); // 7 초과인 첫 원소 → 10

*s.begin();    // 최솟값
*s.rbegin();   // 최댓값
```

### multiset — 중복 허용 정렬 집합

```cpp
#include <set>

multiset<int> ms;
ms.insert(3); ms.insert(3); ms.insert(1);
// ms = {1, 3, 3}

ms.count(3);           // 2
ms.erase(ms.find(3));  // 하나만 삭제! → {1, 3}
ms.erase(3);           // 3인 것 전부 삭제! → {1}
// ⚠️ erase(값)과 erase(이터레이터)의 차이를 꼭 기억!
```

### map — 키-값 쌍 (정렬됨)

키로 값을 빠르게 찾을 수 있고, 키 순서대로 정렬. O(log n).

```cpp
#include <map>

map<string, int> m;
m["apple"] = 3;
m["banana"] = 5;

m["apple"];       // 3
m.count("apple"); // 1

for (auto& [key, val] : m) {
    cout << key << ": " << val << "\n";
}

// 주의: 없는 키에 접근하면 자동으로 0이 들어감!
m["cherry"]; // m["cherry"] = 0이 생김
```

### unordered_set / unordered_map — 해시 기반 (비정렬)

정렬 필요 없으면 평균 O(1)로 더 빠름.

```cpp
#include <unordered_set>
#include <unordered_map>

unordered_set<int> us;
us.insert(5);
us.count(5); // 1

unordered_map<string, int> um;
um["key"] = 100;

// set/map과 사용법 거의 동일
// 단, lower_bound/upper_bound 같은 순서 기반 연산은 불가
```

### bitset — 비트 연산 집합

```cpp
#include <bitset>

bitset<8> b("10110010");
bitset<8> b2(42);         // 00101010

b[0];        // 0 (오른쪽부터!)
b.count();   // 1인 비트 수
b.set(0);    // 0번 비트를 1로
b.reset(1);  // 1번 비트를 0으로
b.flip();    // 전체 반전

bitset<8> result = b & b2;  // AND
result = b | b2;            // OR
result = b ^ b2;            // XOR
```

---

## 2. 알고리즘 (Algorithms)

`#include <algorithm>` 기본, 일부는 `#include <numeric>` 필요.

### sort — 정렬

O(n log n).

```cpp
vector<int> v = {5, 3, 1, 4, 2};

sort(v.begin(), v.end());              // 오름차순
sort(v.begin(), v.end(), greater<>()); // 내림차순

// 커스텀 정렬 (람다)
vector<pair<int,int>> arr = {{3,1}, {1,3}, {3,0}};
sort(arr.begin(), arr.end(), [](auto& a, auto& b) {
    if (a.first == b.first) return a.second > b.second;
    return a.first < b.first;
});
```

### stable_sort — 안정 정렬

같은 값의 상대적 순서를 유지.

```cpp
stable_sort(v.begin(), v.end());
```

### binary_search / lower_bound / upper_bound — 이진 탐색

**정렬된** 컨테이너에서만 사용!

```cpp
vector<int> v = {1, 3, 3, 5, 7, 9};

binary_search(v.begin(), v.end(), 3); // true

auto it = lower_bound(v.begin(), v.end(), 3);   // 3 이상 첫 위치 → 인덱스 1
auto it2 = upper_bound(v.begin(), v.end(), 3);  // 3 초과 첫 위치 → 인덱스 3

// 3의 개수 = upper_bound - lower_bound = 3 - 1 = 2
```

### next_permutation — 순열

완전 탐색 문제에서 유용.

```cpp
vector<int> v = {1, 2, 3};
sort(v.begin(), v.end()); // 반드시 정렬 먼저!

do {
    for (int x : v) cout << x << " ";
    cout << "\n";
} while (next_permutation(v.begin(), v.end()));
```

### reverse — 뒤집기

```cpp
vector<int> v = {1, 2, 3, 4, 5};
reverse(v.begin(), v.end()); // {5,4,3,2,1}

string s = "hello";
reverse(s.begin(), s.end()); // "olleh"
```

### unique — 연속 중복 제거

정렬 후 사용해야 전체 중복 제거. erase와 함께 쓰는 패턴을 외울 것.

```cpp
vector<int> v = {3, 1, 3, 2, 1, 2};
sort(v.begin(), v.end());
v.erase(unique(v.begin(), v.end()), v.end());
// {1, 2, 3}
```

### min / max / minmax

```cpp
min(3, 5);            // 3
max(3, 5);            // 5
min({3, 1, 4, 1, 5}); // 1
max({3, 1, 4, 1, 5}); // 5

auto [lo, hi] = minmax({3, 1, 4}); // lo=1, hi=4
```

### min_element / max_element

```cpp
vector<int> v = {3, 1, 4, 1, 5};
auto it = min_element(v.begin(), v.end());  // *it = 1
auto it2 = max_element(v.begin(), v.end()); // *it2 = 5
```

### count / count_if — 개수 세기

```cpp
vector<int> v = {1, 2, 3, 2, 1};
count(v.begin(), v.end(), 2);  // 2

count_if(v.begin(), v.end(), [](int x) { return x > 1; }); // 3
```

### find / find_if — 찾기

```cpp
auto it = find(v.begin(), v.end(), 3);
if (it != v.end()) {
    cout << "찾음! 인덱스: " << it - v.begin() << "\n";
}

auto it2 = find_if(v.begin(), v.end(), [](int x) { return x % 2 == 0; });
```

### fill — 채우기

```cpp
vector<int> v(10);
fill(v.begin(), v.end(), -1);

// 2차원 배열 초기화
int arr[100][100];
fill(&arr[0][0], &arr[0][0] + 100*100, -1);
```

### accumulate — 합계

```cpp
#include <numeric>

vector<int> v = {1, 2, 3, 4, 5};
int sum = accumulate(v.begin(), v.end(), 0);        // 15
long long sum2 = accumulate(v.begin(), v.end(), 0LL); // 오버플로 방지

int product = accumulate(v.begin(), v.end(), 1, multiplies<>());
```

### partial_sum — 누적합 (Prefix Sum)

구간 합 문제에서 매우 중요!

```cpp
#include <numeric>

vector<int> v = {1, 2, 3, 4, 5};
vector<int> prefix(v.size());
partial_sum(v.begin(), v.end(), prefix.begin());
// prefix = {1, 3, 6, 10, 15}

// 구간 [l, r]의 합 = prefix[r] - (l > 0 ? prefix[l-1] : 0)
```

### gcd / lcm — 최대공약수 / 최소공배수 (C++17)

```cpp
#include <numeric>

gcd(12, 8);   // 4
lcm(12, 8);   // 24

// 여러 수의 GCD
vector<int> v = {12, 8, 16};
int g = accumulate(v.begin(), v.end(), 0, [](int a, int b) {
    return gcd(a, b);
});
// g = 4
```

### nth_element — n번째 원소 찾기

전체 정렬 없이 O(n)에 n번째로 작은 원소를 찾음.

```cpp
vector<int> v = {5, 3, 1, 4, 2};
nth_element(v.begin(), v.begin() + 2, v.end());
// v[2] = 3 (보장), 나머지는 순서 미보장
```

### rotate — 회전

```cpp
vector<int> v = {1, 2, 3, 4, 5};
rotate(v.begin(), v.begin() + 2, v.end());
// {3, 4, 5, 1, 2}
```

### swap

```cpp
int a = 1, b = 2;
swap(a, b); // a=2, b=1

vector<int> v1 = {1,2,3}, v2 = {4,5,6};
swap(v1, v2); // O(1)
```

### all_of / any_of / none_of — 조건 검사

```cpp
vector<int> v = {2, 4, 6, 8};

all_of(v.begin(), v.end(), [](int x) { return x % 2 == 0; });  // true
any_of(v.begin(), v.end(), [](int x) { return x > 7; });       // true
none_of(v.begin(), v.end(), [](int x) { return x < 0; });      // true
```

### iota — 연속 값 채우기

```cpp
#include <numeric>

vector<int> v(5);
iota(v.begin(), v.end(), 1); // {1, 2, 3, 4, 5}
```

---

## 3. 유용한 C++17 기능들

### 구조화 바인딩 (Structured Bindings)

```cpp
for (auto& [key, value] : myMap) {
    cout << key << " " << value << "\n";
}

// BFS 활용
queue<tuple<int,int,int>> q;
q.push({0, 0, 0});
while (!q.empty()) {
    auto [x, y, dist] = q.front();
    q.pop();
}
```

### optional (C++17)

값이 있을 수도, 없을 수도 있는 경우.

```cpp
#include <optional>

optional<int> findValue(vector<int>& v, int target) {
    for (int i = 0; i < v.size(); i++)
        if (v[i] == target) return i;
    return nullopt;
}

auto result = findValue(v, 5);
if (result) cout << *result << "\n";
```

### clamp (C++17)

값을 범위 내로 제한.

```cpp
#include <algorithm>

clamp(15, 0, 10);   // 10
clamp(-5, 0, 10);   // 0
clamp(7, 0, 10);    // 7
```

---

## 4. 자주 쓰는 패턴 정리

### 입출력 속도 향상 (거의 필수!)

```cpp
ios::sync_with_stdio(false);
cin.tie(nullptr);
// printf/scanf와 혼용 금지!
```

### endl 대신 "\n"

`endl`은 매번 버퍼를 비워서 느림. `"\n"`을 사용할 것.

### INF 값 설정

```cpp
const int INF = 1e9;
const long long LLINF = 1e18;
```

### 범위 기반 for문

```cpp
for (int x : v) cout << x << " ";       // 복사
for (int& x : v) x *= 2;               // 참조로 수정
for (const auto& x : v) cout << x;     // 읽기만 (가장 효율적)
```

---

## 5. 상황별 요약

| 상황 | 사용할 STL |
|------|-----------|
| 데이터 저장 | `vector`, 앞뒤 모두 필요하면 `deque` |
| 탐색 (BFS) | `queue` + `vector<bool> visited` |
| 탐색 (DFS) | 재귀 또는 `stack` |
| 최단 경로 (다익스트라) | `priority_queue` (최소 힙) |
| 중복 제거 + 정렬 | `set` 또는 `sort` + `unique` + `erase` |
| 빠른 존재 확인 | `unordered_set` / `unordered_map` |
| 키-값 매핑 | `map` 또는 `unordered_map` |
| 정렬 | `sort` + 람다 |
| 이진 탐색 | `lower_bound` / `upper_bound` |
| 완전 탐색 (순열) | `next_permutation` |
| 구간 합 | `partial_sum` 또는 prefix sum 배열 |
| 최대공약수 | `gcd` (C++17) |