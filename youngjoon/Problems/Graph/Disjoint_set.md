# Disjoint Set (서로소 집합)

서로 겹치지 않는 집합들의 모임. 원소가 정확히 하나의 집합에만 속한다.

> Union-Find는 Disjoint Set을 구현하는 대표적인 자료구조.  
> 구현 방법은 [Union-Find](Union-Find.md) 참고.

## 동작 원리 예시

```
초기: {0} {1} {2} {3} {4}    각 노드가 독립된 집합
      parent = [0, 1, 2, 3, 4]

unite(0, 1):  parent[1] = 0  →  {0,1} {2} {3} {4}
unite(1, 2):  find(1)=0, parent[2] = 0  →  {0,1,2} {3} {4}
unite(3, 4):  parent[4] = 3  →  {0,1,2} {3,4}

connected(0, 2)?  find(0)=0, find(2)=0  →  true  (같은 집합)
connected(0, 3)?  find(0)=0, find(3)=3  →  false (다른 집합)
```

Union-Find 구현은 [Union-Find](Union-Find.md) 참고.

## 개념

```
초기 상태: {0} {1} {2} {3} {4}   (각자 독립된 집합)

unite(0,1) → {0,1} {2} {3} {4}
unite(1,2) → {0,1,2} {3} {4}
unite(3,4) → {0,1,2} {3,4}
```

## Array 기반 표현

```cpp
// parent[i] = i의 부모 노드 (루트이면 parent[i] == i)
vector<int> parent(n);
iota(parent.begin(), parent.end(), 0);
```

## 시간복잡도

| 연산 | 기본 | Path Compression + Union by Rank |
|------|------|----------------------------------|
| find | O(N) | O(α(N)) ≈ O(1) |
| union | O(N) | O(α(N)) ≈ O(1) |

## 관련 알고리즘

- [Union-Find](Union-Find.md)
- [MST](MST.md) — Kruskal
- [Cycle](Cycle.md) — 사이클 탐지
