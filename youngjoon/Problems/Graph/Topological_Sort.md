# Topological Sort (위상 정렬)

DAG(방향 비순환 그래프)에서 노드를 **선후관계**에 맞게 일렬로 나열.  
사이클이 있으면 위상 정렬 불가능.

## 동작 원리 예시

<img src="../../../imgs/topo_sort.svg" alt="Topological Sort" width="560"/>

indegree가 0인 노드부터 Queue에 넣고, 처리할 때마다 이웃 노드의 indegree를 감소시킨다. indegree가 0이 된 노드를 새로 Queue에 추가하며 반복.

---

## 알고리즘 종류

| 방법 | 기반 | 특징 |
|------|------|------|
| Kahn's Algorithm | BFS + Indegree | 사이클 탐지 가능, 직관적 |
| DFS 방식 | DFS + Stack | 재귀 구현 간단 |

## 시간복잡도

- `O(V + E)` (V: 노드 수, E: 간선 수)

## 코드 템플릿 (C++ - Kahn's Algorithm)

```cpp
#include <vector>
#include <queue>
using namespace std;

vector<int> topoSort(int n, vector<vector<int>>& graph) {
    vector<int> indegree(n, 0);
    for (int u = 0; u < n; u++)
        for (int v : graph[u])
            indegree[v]++;

    queue<int> q;
    for (int i = 0; i < n; i++)
        if (indegree[i] == 0) q.push(i);

    vector<int> order;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        order.push_back(u);
        for (int v : graph[u]) {
            if (--indegree[v] == 0) q.push(v);
        }
    }

    // order.size() < n 이면 사이클 존재
    return order;
}
```

## 코드 템플릿 (C++ - DFS)

```cpp
#include <vector>
#include <stack>
#include <functional>
using namespace std;

vector<int> topoSortDFS(int n, vector<vector<int>>& graph) {
    vector<bool> visited(n, false);
    stack<int> st;

    function<void(int)> dfs = [&](int u) {
        visited[u] = true;
        for (int v : graph[u])
            if (!visited[v]) dfs(v);
        st.push(u);
    };

    for (int i = 0; i < n; i++)
        if (!visited[i]) dfs(i);

    vector<int> order;
    while (!st.empty()) {
        order.push_back(st.top()); st.pop();
    }
    return order;
}
```

## 특징

- 결과가 유일하지 않을 수 있음 (여러 개의 유효한 순서 존재)
- Kahn's Algorithm에서 `order.size() < n`이면 사이클 존재 → 사이클 탐지 가능
- 선수 과목, 작업 스케줄링, 빌드 시스템 등에 활용

## 관련 알고리즘

- [DAG](DAG.md)
- [Cycle](Cycle.md)

## Related Problems

- [207. Course Schedule](207.%20Course%20Schedule.md)
