# MST (Minimum Spanning Tree, 최소 신장 트리)

N개의 노드를 **N-1개의 간선**으로 연결하되, 간선의 가중치 합이 최소인 트리.  
사이클 없이 모든 노드를 연결한다.

## 알고리즘 비교

| 알고리즘 | 방식 | 시간복잡도 | 적합한 경우 |
|----------|------|------------|-------------|
| Kruskal | 간선 정렬 + Union-Find | O(E log E) | 간선이 적을 때 |
| Prim | 우선순위 큐 + Greedy | O((V+E) log V) | 간선이 많을 때 |

---

## 1. Kruskal's Algorithm

간선을 가중치 오름차순으로 정렬 후, 사이클이 생기지 않는 간선만 선택.

```cpp
#include <vector>
#include <algorithm>
#include <numeric>
using namespace std;

struct Edge {
    int u, v, w;
};

struct UnionFind {
    vector<int> parent, rank;
    UnionFind(int n) : parent(n), rank(n, 0) {
        iota(parent.begin(), parent.end(), 0);
    }
    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);
        return parent[x];
    }
    bool unite(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return false;
        if (rank[px] < rank[py]) swap(px, py);
        parent[py] = px;
        if (rank[px] == rank[py]) rank[px]++;
        return true;
    }
};

int kruskal(int n, vector<Edge>& edges) {
    sort(edges.begin(), edges.end(), [](const Edge& a, const Edge& b) {
        return a.w < b.w;
    });
    UnionFind uf(n);
    int totalCost = 0;
    for (auto& [u, v, w] : edges) {
        if (uf.unite(u, v)) totalCost += w;
    }
    return totalCost;
}
```

---

## 2. Prim's Algorithm

시작 노드에서 연결 가능한 간선 중 최소 가중치를 greedy하게 선택해 트리 확장.

```cpp
#include <vector>
#include <queue>
#include <climits>
using namespace std;

// graph[u] = {{v, weight}, ...}
int prim(int n, vector<vector<pair<int,int>>>& graph) {
    vector<bool> inMST(n, false);
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    pq.push({0, 0}); // {weight, node}

    int totalCost = 0;
    while (!pq.empty()) {
        auto [w, u] = pq.top(); pq.pop();
        if (inMST[u]) continue;
        inMST[u] = true;
        totalCost += w;
        for (auto [v, weight] : graph[u]) {
            if (!inMST[v]) pq.push({weight, v});
        }
    }
    return totalCost;
}
```

---

## 특징

- MST는 유일하지 않을 수 있음 (같은 가중치 간선이 여러 개인 경우)
- 네트워크 설계, 도로/전선 최소 비용 연결 등에 활용

## 관련 알고리즘

- [Union-Find](Union-Find.md)
- [Disjoint Set](Disjoint_set.md)
- [Tree](Tree.md)
