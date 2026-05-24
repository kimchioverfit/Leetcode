# Tree

사이클이 없는 연결 그래프 (Undirected Acyclic Connected Graph).  
`n`개의 노드가 있을 때 간선은 정확히 `n-1`개.

## 특징

- 임의의 두 노드 사이 경로가 유일
- 루트를 정하면 계층 구조(부모-자식 관계) 형성
- 이진 트리, 일반 트리, 트라이 등 다양한 형태 존재

## 그래프와의 관계

```
Tree ⊂ DAG ⊂ Graph
```

| 조건 | 이름 |
|------|------|
| 연결 + 비순환 (무방향) | Tree |
| 방향 + 비순환 | DAG |
| 방향 + 순환 가능 | 일반 Graph |

## 트리 순회

| 순회 | 순서 |
|------|------|
| Preorder | Root → Left → Right |
| Inorder | Left → Root → Right |
| Postorder | Left → Right → Root |
| Level-order | BFS |

## 코드 템플릿 (C++ - 인접리스트 트리)

```cpp
#include <vector>
#include <functional>
using namespace std;

vector<vector<int>> buildTree(int n, vector<vector<int>>& edges) {
    vector<vector<int>> graph(n);
    for (auto& e : edges) {
        graph[e[0]].push_back(e[1]);
        graph[e[1]].push_back(e[0]);
    }
    return graph;
}

void dfs(int node, int parent, vector<vector<int>>& graph) {
    for (int child : graph[node]) {
        if (child != parent) {
            dfs(child, node, graph);
        }
    }
}
```

## Related Algorithm

1. BFS / DFS 순회
2. LCA (Lowest Common Ancestor)
3. 트리 DP
4. Spanning Tree (MST)

## Related Problems

- [DAG](DAG.md)
- [MST](MST.md)
