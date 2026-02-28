---
up: 
  - "[[搜索]]"
down: 
  - "[[层序遍历]]"
  - "[[最短路问题]]"
  - "[[最长路问题]]"
  - "[[连通性-染色问题]]"
  - "[[拓扑排序]]"
  - "[[二分+广搜]]"
  - "[[其他广搜]]"
  - "[[双向BFS和多源BFS]]"
  - "[[二分图：染色法、匈牙利算法]]"
relation: 
题目:
---
---

```C++
vector<vector<int>> adjList;

void BFS(int start) {
    queue<int> q;
    vector<bool> visited(adjList.size(), false);
    vector<int> distance(adjList.size(), INT_MAX);
    visited[start] = true;
    distance[start] = 0;
    q.push(start);

    while (!q.empty()) {
        int curr = q.front();
        q.pop();
        cout << curr << " ";
        //这里可以添加一个for循环，遍历当前队列中的所有结点，这样可以分层遍历
        for (int neighbor : adjList[curr]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                distance[neighbor] = distance[curr] + 1;
                //可以不是1，可以是当前结点的权重
                q.push(neighbor);
            }
        }
    }
}
```

[[层序遍历]]

[[最短路问题]]

[[最长路问题]]

[[连通性-染色问题]]

[[拓扑排序]]

[[二分+广搜]]

[[其他广搜]]

[[双向BFS和多源BFS]]