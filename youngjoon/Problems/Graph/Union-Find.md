# Union-Find (DSU, Disjoint Set Union)

원소들이 어느 집합에 속하는지 효율적으로 관리하는 자료구조.  
`find` / `union` 두 연산으로 구성된다.

<img src="../../imgs/Union_find-1.png" alt="Union_find-1" width="400"/>

<img src="../../imgs/Union_find-2.png" alt="Union_find-2" width="400"/>

## 핵심 연산

| 연산 | 설명 |
|------|------|
| `find(x)` | x가 속한 집합의 루트 반환 |
| `unite(x, y)` | x와 y의 집합을 합침 |
| `connected(x, y)` | 같은 집합인지 확인 |

## 최적화

- **Path Compression**: `find` 시 경로상의 모든 노드를 루트에 직접 연결
- **Union by Rank**: 작은 트리를 큰 트리 아래에 붙여 높이 최소화

두 최적화를 적용하면 사실상 `O(α(N))` (inverse Ackermann, 거의 O(1))

## 코드 템플릿 (C++)

```cpp
#include <vector>
#include <numeric>
using namespace std;

struct UnionFind {
    vector<int> parent, rank;

    UnionFind(int n) : parent(n), rank(n, 0) {
        iota(parent.begin(), parent.end(), 0);
    }

    int find(int x) {
        if (parent[x] != x)
            parent[x] = find(parent[x]); // path compression
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

    bool connected(int x, int y) {
        return find(x) == find(y);
    }
};
```

## 사용 예시

```cpp
UnionFind uf(5); // 노드 0~4
uf.unite(0, 1);
uf.unite(1, 2);
uf.connected(0, 2); // true
uf.connected(0, 3); // false
```

## 관련 알고리즘

- [Connected Component](Connected_component.md) — 연결 요소 탐색
- [Cycle](Cycle.md) — 사이클 탐지 (Undirected)
- [MST](MST.md) — Kruskal 알고리즘

## Related Problems

- [305. Number of Islands II](305.%20Number%20of%20Islands%20II.md)
- [547. Number of Provinces](547.%20Number%20of%20Provinces.md)
