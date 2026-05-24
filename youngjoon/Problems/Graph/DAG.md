# DAG (Directed Acyclic Graph)

<img src="../../imgs/Recover Binary Search Tree_2.jpg" alt="Recover Binary Search Tree_2" width="300"/>

방향 비순환 그래프, 선후 관계 표현에 사용된다.

## 동작 원리 예시

```
A → B → D
↓       ↑
C ──────┘

- 방향(Directed): 간선에 방향이 있음 (A→B, A→C, B→D, C→D)
- 비순환(Acyclic): 어떤 노드에서 출발해도 자기 자신으로 돌아오는 경로 없음
- 선후 관계: A를 먼저 처리해야 B, C를 처리할 수 있음
```

DAG에서는 **Topological Sort**로 노드를 선후 순서에 맞게 일렬 나열할 수 있고,  
**DP**로 최장/최단 경로를 O(V+E)에 구할 수 있다.

Related Algorithm 

1. Kahn
2. DFS
3. DP
4. Cycle detection
5. minimun/maximum length

