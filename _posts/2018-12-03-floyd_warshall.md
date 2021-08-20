---

title:  "7. Graph - Topological Sort, Floyd Warshall"
tag: DataStructure 
---
각 자료,Task 간의 의존관계가 존재할 때 수행가능한 작업 순서를 도식화하기위해 사용

#### 의존관계?(교과과정의 선수과목)

자 주어진 adj\_list 를 살펴보자

기본적으로 adj\_list 는 진출 차수를 가지고 있다.  
진출 차수는 의존 관계를 설정하는 자료라고 볼 수 있다.

adj\_list\[0\] 은 1 을 진출 차수로 가진다.  
여기서 1 은 0에 대해 의존한다고 볼 수 있다.  
1이 수행되기 위해선 0이 그 전에 수행되어야만 한다.  
그렇다면 0은 1보다 앞부분에 정렬되어야 한다.

그렇다면 empty list 인 adj\_list\[7\] 을 살펴보자.  
adj\_list\[7\] 은 8에 해당하는 진출 차수를 가진다.  
8이 empty 라는 것은 8의 수행을 필요로 하는 정점이 존재하지 않는다는 것이다.  
8은 누구에게도 영향을 미치지 않는다.  
그러므로 가장 마지막에 정렬(수행)되는 것이 적절하다.  
그렇다면 8은 누구에 의존성을 가지는가?  
이는 8이라는 값을 진출차수로 가진 값이 누구인지 찾아보면 된다.  
adj\_list\[6\] 은 8을 갖는다.  
따라서 정렬의 마지막은 \[.... 7, 8\]로 끝날 것이다

그렇다면 첫번째로 오게되는 정점은 누구일까?  
첫번째로 정렬이 되려면 어떤 정점의 진출 차수가 되서는 안된다.  
본인이 진출 차수를 가져서 의존성을 부여할 수 있겠지만 자기자신은  
어떤 의존성을 가질 수 없다.  
adj\_list 에 2라는 값은 찾을 수가 없다.  
이는 2가 모든 정점에 대해서 의존성을 갖지 않는다는 의미이다.  
그러므로 \[2, ....7, 8\] 의 형태로 첫번째에 2가 위치하게 될 것이다.

```

adj_list = [[1], [3, 4], [0, 1], [6], [5], [7], [7], [8], []]
# adj_list = [[2, 1], [3, 0], [3, 0], [9, 8, 2, 1],
#             [5], [7, 6, 4], [7, 5], [6, 5], [3], [3]]
N = len(adj_list)
visited = [False] * (N)
s = []

def dfs(v):
    visited[v] = True
    for w in adj_list[v]:
        if not visited[w]:
            dfs(w)
    s.append(v)  # 각 정점의 연결 정점들의 visited 가 모두 True 일 때 append 됨

for i in range(N):
    if not visited[i]:
        dfs(i)

# reverse() 전 list 는 각 정점에 대해 영향을 가장 적게 미치는 값부터 정렬되어있다.
# reverse() 후 list 는 가장 영향력이 강한 순서대로 정렬되게 된다. 
s.reverse()  # 진출차수의 리스트(adj_list)로 위상정렬을 수행하기위해서는 역순정렬이 필요
print('위상정렬: ')
print(s)

```

### Floyd Warshall

```

import sys

N = 5
INF = sys.maxsize
D = [[0, 4, 2, 5, INF],
     [INF, 0, 1, INF, 4],
     [1, 3, 0, 1, 2],
     [-2, INF, INF, 0, 2],
     [INF, -3, 3, 1, 0]]

for k in range(N):
    for i in range(N):
        for j in range(N):
            D[i][j] = min(D[i][j], D[i][k] + D[k][j])


for i in range(N):
    for j in range(N):
        print(f'{D[i][j]:3d}  ', end='')
    print()

if __name__ == '__main__':
    pass

```