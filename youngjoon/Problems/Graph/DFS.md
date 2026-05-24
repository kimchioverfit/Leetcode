# DFS (Depth-First Search)

깊이 우선 탐색. 한 방향으로 끝까지 탐색한 뒤 되돌아오는 방식.  
Stack 또는 재귀를 사용한다.

## 구조

```
시작 노드 → 인접 노드로 재귀/스택 진입 → 더 갈 곳 없으면 backtrack
```

## 동작 원리 예시

<img src="../../../imgs/dfs_example.svg" alt="DFS 동작 원리" width="520"/>

아래 그래프에서 노드 1부터 DFS 탐색 (인접 리스트: 1→[2,3], 2→[4,5], 3→[6]):

```
    1
   / \
  2   3
 / \    \
4   5    6
```

재귀 호출 흐름:

```
dfs(1)
 └─ dfs(2)
     ├─ dfs(4)  ← 더 갈 곳 없음, return
     └─ dfs(5)  ← 더 갈 곳 없음, return
 └─ dfs(3)
     └─ dfs(6)  ← 더 갈 곳 없음, return
```

탐색 순서: **1 → 2 → 4 → 5 → 3 → 6** (한 방향으로 끝까지 내려간 뒤 backtrack)

BFS와 달리 레벨을 무시하고 한 경로를 끝까지 파고든다.

## 시간복잡도

- `O(V + E)` (V: 노드 수, E: 간선 수)

## 코드 템플릿 (C++)

```cpp
#include <vector>
#include <unordered_set>
#include <functional>
using namespace std;

// 재귀 방식
void dfs(vector<vector<int>>& graph, int node, unordered_set<int>& visited) {
    visited.insert(node);
    for (int neighbor : graph[node]) {
        if (!visited.count(neighbor)) {
            dfs(graph, neighbor, visited);
        }
    }
}
```

```cpp
// 반복(Stack) 방식
#include <stack>

void dfs_iter(vector<vector<int>>& graph, int start) {
    unordered_set<int> visited;
    stack<int> st;
    st.push(start);

    while (!st.empty()) {
        int node = st.top(); st.pop();
        if (!visited.count(node)) {
            visited.insert(node);
            for (int neighbor : graph[node]) {
                if (!visited.count(neighbor)) {
                    st.push(neighbor);
                }
            }
        }
    }
}
```

## 특징

- 경로 추적, 사이클 탐지, 연결 요소 탐색에 유용
- 재귀 깊이 제한 주의 (스택 오버플로우 가능, 반복 방식 고려)
- 최단 경로 보장 X (BFS 사용할 것)

## Related Problems

- [200. Number of Islands](200.%20Number%20of%20Islands.md)
- [130. Surrounded Regions](130.%20Surrounded%20Regions.md)
- [695. Max Area of Island](695.%20Max%20Area%20of%20Island.md)
- [133. Clone Graph](133.%20Clone%20Graph.md)
