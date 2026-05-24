# Cycle Detection (사이클 탐지)

그래프 내에 순환 경로가 존재하는지 탐지한다.  
**Directed / Undirected** 그래프에서 방법이 다르다.

## 동작 원리 예시

<img src="../../../imgs/cycle_detection.svg" alt="Cycle Detection" width="540"/>

**Undirected**: DFS 중 이미 방문한 노드를 부모가 아닌 경로로 재방문 → 사이클  
**Directed**: DFS 중 현재 재귀 스택(GRAY)에 있는 노드를 재방문 → 사이클

---

## Undirected Graph

DFS 탐색 중 이미 방문한 노드를 **부모가 아닌 경로로** 다시 만나면 사이클.

```cpp
#include <vector>
#include <functional>
using namespace std;

bool hasCycleUndirected(vector<vector<int>>& graph, int n) {
    vector<bool> visited(n, false);

    function<bool(int, int)> dfs = [&](int node, int parent) -> bool {
        visited[node] = true;
        for (int neighbor : graph[node]) {
            if (!visited[neighbor]) {
                if (dfs(neighbor, node)) return true;
            } else if (neighbor != parent) {
                return true;
            }
        }
        return false;
    };

    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            if (dfs(i, -1)) return true;
        }
    }
    return false;
}
```

## Directed Graph

DFS 탐색 중 현재 경로(recursion stack)에 있는 노드를 다시 방문하면 사이클.

```cpp
bool hasCycleDirected(vector<vector<int>>& graph, int n) {
    // 0: WHITE, 1: GRAY, 2: BLACK
    vector<int> color(n, 0);

    function<bool(int)> dfs = [&](int node) -> bool {
        color[node] = 1;
        for (int neighbor : graph[node]) {
            if (color[neighbor] == 1) return true;
            if (color[neighbor] == 0 && dfs(neighbor)) return true;
        }
        color[node] = 2;
        return false;
    };

    for (int i = 0; i < n; i++) {
        if (color[i] == 0) {
            if (dfs(i)) return true;
        }
    }
    return false;
}
```

## Union-Find로 탐지 (Undirected)

```cpp
#include <numeric>

bool hasCycleUF(int n, vector<vector<int>>& edges) {
    vector<int> parent(n);
    iota(parent.begin(), parent.end(), 0);

    function<int(int)> find = [&](int x) -> int {
        while (parent[x] != x) {
            parent[x] = parent[parent[x]];
            x = parent[x];
        }
        return x;
    };

    for (auto& e : edges) {
        int pu = find(e[0]), pv = find(e[1]);
        if (pu == pv) return true;
        parent[pu] = pv;
    }
    return false;
}
```

## Related Problems

- [207. Course Schedule](207.%20Course%20Schedule.md)
- [DAG](DAG.md)
