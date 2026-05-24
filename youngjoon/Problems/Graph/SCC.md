# SCC (Strongly Connected Component)

SCC는 directed graph에서 서로 도달 가능한 노드들의 그룹이다.

노드 `u`, `v`가 같은 SCC에 있으려면 아래 조건이 모두 성립해야 한다.

- `u`에서 `v`로 갈 수 있다.
- `v`에서 `u`로 갈 수 있다.

즉, 방향 그래프 안에서 cycle처럼 서로 왕복 가능한 묶음을 찾는 개념이다.

## 왜 필요한가

Directed graph에서는 connected component만으로는 부족하다. 무방향 그래프에서는 연결되어 있으면 같은 그룹으로 볼 수 있지만, 방향 그래프에서는 한쪽으로만 갈 수 있는 경우가 많다.

SCC를 찾으면 방향 그래프를 더 단순한 구조로 압축할 수 있다.

각 SCC를 하나의 노드로 압축하면 전체 그래프는 DAG가 된다. 이를 condensation graph라고 한다.

## 학습 위치

SCC는 아래 개념을 먼저 알고 보면 좋다.

1. DFS
2. Directed Graph
3. Cycle Detection
4. Reverse Graph

그래서 학습 순서는 보통 아래처럼 잡는다.

```text
DFS
→ Cycle
→ SCC
→ DAG
→ Topological Sort
```

## Kosaraju's Algorithm

Kosaraju는 DFS를 두 번 사용해서 SCC를 찾는 알고리즘이다.

핵심 아이디어:

1. 원래 그래프에서 DFS를 수행하면서 종료 순서를 기록한다.
2. 모든 edge 방향을 뒤집은 reverse graph를 만든다.
3. 종료 순서의 역순으로 reverse graph에서 DFS를 수행한다.
4. 한 번의 DFS로 방문되는 노드들이 하나의 SCC가 된다.

## 동작 순서

예를 들어 edge가 아래처럼 있다고 하자.

```text
0 -> 1
1 -> 2
2 -> 0
2 -> 3
3 -> 4
4 -> 3
```

SCC는 아래처럼 나뉜다.

```text
{0, 1, 2}
{3, 4}
```

`0, 1, 2`는 서로 도달 가능하고, `3, 4`도 서로 도달 가능하다. 하지만 `{0, 1, 2}`에서 `{3, 4}`로 갈 수는 있어도 반대로 돌아올 수는 없으므로 하나의 SCC가 아니다.

## 코드 템플릿 (C++)

```cpp
class Solution {
public:
    vector<vector<int>> getSCC(int n, vector<vector<int>>& edges) {
        vector<vector<int>> graph(n);
        vector<vector<int>> reversed(n);

        for (auto& edge : edges) {
            int u = edge[0];
            int v = edge[1];
            graph[u].push_back(v);
            reversed[v].push_back(u);
        }

        vector<bool> visited(n, false);
        vector<int> order;

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfsOrder(i, graph, visited, order);
            }
        }

        fill(visited.begin(), visited.end(), false);
        reverse(order.begin(), order.end());

        vector<vector<int>> sccList;

        for (int node : order) {
            if (!visited[node]) {
                vector<int> component;
                dfsCollect(node, reversed, visited, component);
                sccList.push_back(component);
            }
        }

        return sccList;
    }

private:
    void dfsOrder(int node, vector<vector<int>>& graph, vector<bool>& visited, vector<int>& order) {
        visited[node] = true;

        for (int next : graph[node]) {
            if (!visited[next]) {
                dfsOrder(next, graph, visited, order);
            }
        }

        order.push_back(node);
    }

    void dfsCollect(int node, vector<vector<int>>& graph, vector<bool>& visited, vector<int>& component) {
        visited[node] = true;
        component.push_back(node);

        for (int next : graph[node]) {
            if (!visited[next]) {
                dfsCollect(next, graph, visited, component);
            }
        }
    }
};
```

## 시간복잡도

```text
O(V + E)
```

DFS를 두 번 수행하고, reverse graph를 한 번 만들기 때문에 전체 시간복잡도는 `O(V + E)`다.

공간복잡도도 graph, reversed graph, visited, order를 사용하므로 `O(V + E)`다.

## Tarjan's Algorithm

Tarjan도 SCC를 찾는 알고리즘이다. Kosaraju와 달리 DFS 한 번으로 SCC를 찾는다.

다만 `disc`, `low-link`, stack 개념이 필요해서 Kosaraju보다 이해 난이도가 높다. 처음 SCC를 공부할 때는 Kosaraju를 먼저 보고, 이후 Tarjan을 보는 것이 좋다.

## 특징

- Directed graph에서만 의미가 있다.
- 서로 도달 가능한 노드 그룹을 찾는다.
- SCC를 압축하면 DAG가 된다.
- Topological Sort와 함께 방향 그래프 문제에서 자주 연결된다.

## Related Algorithm

- [DFS](DFS.md)
- [Cycle](Cycle.md)
- [DAG](DAG.md)
- [Topological Sort](Topological_Sort.md)
