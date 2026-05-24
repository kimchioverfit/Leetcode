# BFS (Breadth-First Search)

너비 우선 탐색. 시작 노드에서 가까운 노드부터 순서대로 탐색한다.  
Queue를 사용하며, **최단 경로** 탐색에 적합하다.

## 구조

```
시작 노드 → 인접 노드들을 Queue에 삽입 → Queue에서 꺼내며 반복
```

## 동작 원리 예시

<img src="../../../imgs/bfs_example.svg" alt="BFS 동작 원리" width="520"/>

아래 그래프에서 노드 1부터 BFS 탐색:

```
    1
   / \
  2   3
 / \    \
4   5    6
```

| 단계 | 처리 노드 | Queue 상태 | 방문 완료 |
|------|-----------|------------|-----------|
| 초기 | -         | [1]        | {1}       |
| 1    | 1         | [2, 3]     | {1,2,3}   |
| 2    | 2         | [3, 4, 5]  | {1,2,3,4,5} |
| 3    | 3         | [4, 5, 6]  | {1,2,3,4,5,6} |
| 4    | 4         | [5, 6]     | 동일      |
| 5    | 5         | [6]        | 동일      |
| 6    | 6         | []         | 동일      |

탐색 순서: **1 → 2 → 3 → 4 → 5 → 6** (레벨 단위로 좌→우)

같은 거리(depth)의 노드를 먼저 전부 방문한 뒤 다음 레벨로 넘어가는 것이 핵심.

## 시간복잡도

- `O(V + E)` (V: 노드 수, E: 간선 수)

## 코드 템플릿 (C++)

```cpp
#include <vector>
#include <queue>
#include <unordered_set>
using namespace std;

void bfs(vector<vector<int>>& graph, int start) {
    unordered_set<int> visited;
    queue<int> q;
    visited.insert(start);
    q.push(start);

    while (!q.empty()) {
        int node = q.front(); q.pop();
        for (int neighbor : graph[node]) {
            if (!visited.count(neighbor)) {
                visited.insert(neighbor);
                q.push(neighbor);
            }
        }
    }
}
```

## 특징

- 레벨(depth) 단위로 탐색
- 가중치 없는 그래프에서 최단 경로 보장
- DFS보다 메모리를 더 사용할 수 있음

## Related Problems

- [200. Number of Islands](200.%20Number%20of%20Islands.md)
- [130. Surrounded Regions](130.%20Surrounded%20Regions.md)
- [909. Snakes and Ladders](909.%20Snakes%20and%20Ladders.md)
- [547. Number of Provinces](547.%20Number%20of%20Provinces.md)
