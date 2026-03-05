---
up:
  - "[[图]]"
---
- 最短树问题通常使用广搜。最短树问题是指在一个带权重的无向连通图中找到一棵包含所有节点的生成树，使得生成树中各边的权重之和最小。最常见的解法是使用Kruskal算法或Prim算法，它们都是基于广度优先搜索的贪心算法。Kruskal算法使用并查集维护连通性，按照边权从小到大排序，依次将边加入生成树中，同时判断是否形成环路。Prim算法使用堆维护节点距离，从一个任意节点开始，按照节点距离从小到大选择下一个节点，同时更新其他节点的距离和父节点。这两个算法都是基于广度优先搜索的贪心算法，可以在较短的时间内找到最优解
- 稠密图使用朴素Prim、稀疏图使用Kruskal、堆优化的Prim不常用

### 朴素Prim

- Prim算法是一种用于求解最小生成树的贪心算法，其基本思路是从一个点开始，每次选择与当前已经选定的点集最近的一个点，并将其加入到点集中，重复该过程直到所有的点都被加入到点集中为止。
- 具体来说，Prim算法的步骤如下：
- 初始化一个空的点集和边集。
- 随机选择一个起始点，将其加入点集中。
- 遍历所有与点集中的点相邻的边，将它们加入边集中。
- 从边集中选择一条最小的边，将其连接的点加入点集中。
- 重复步骤3和4，直到所有的点都被加入到点集中为止

---

```C
\#include <iostream>
\#include <cstring>
using namespace std;
const int N=510;
const int INF=0x3f3f3f3f;
int graph[N][N];
int n,m;
int dist[N];//点到集合的距离，这里取1为初始集合，dist[1]=0，其他的都为INF
bool used[N];//点是否在集合中

int prim(){
    int res=0;
    dist[1]=0;
    for(int i=0;i<n;++i){
        int t=-1;
        for(int j=1;j<=n;++j){
            if(!used[j]&&(t==-1||dist[t]>dist[j]))
                t=j;
        }
        if(dist[t]==INF) return INF;

        res+=dist[t];
        for(int j=1;j<=n;++j)
            if(!used[j])
                dist[j]=min(dist[j],graph[t][j]);

        used[t]=true;
    }

    return res;
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    cin>>n>>m;
    memset(graph,0x3f,sizeof(graph));
    memset(dist,0x3f,sizeof(dist));
    while(m--){
        int a,b,c;
        cin>>a>>b>>c;
        graph[a][b]=graph[b][a]=min(c,graph[a][b]);
    }
    int t=prim();
    if(t==INF)
        cout<<"impossible"<<endl;
    else
        cout<<t<<endl;
    return 0;
}
```

### Kruskal

- Kruskal算法是一种用于解决最小生成树问题的贪心算法
- 其基本思想是将所有的边按照权值从小到大进行排序，然后依次选取最小的边
- 如果这条边所连接的两个节点不在同一个连通块中，则加入这条边，直到所有的节点都在同一个连通块中为止

---

```C
\#include <iostream>
\#include <vector>
\#include <cstring>
\#include <algorithm>
using namespace std;

const int N=1e5+5;

int parent[N];//每个点的祖先节点，初始为自己，判断是否在同一个连通块中

struct edge {
    int u, v, w;
};

//从小到大排序，
bool cmp(const edge &a, const edge &b) {
    return a.w < b.w;
}

int find(int x){
    //路径压缩
    if(parent[x]!=x)
        parent[x]=find(parent[x]);
    return parent[x];
}

void merge(int x,int y){
    //不做按秩合并了
    parent[find(x)]=find(y);

}


int main() {
    int n, m;
    cin >> n >> m;

    vector<edge> e(m);
    for (int i = 0; i < m; i++) {
        cin >> e[i].u >> e[i].v >> e[i].w;
    }

    sort(e.begin(), e.end(), cmp);

    for (int i = 1; i <= n; i++)
        parent[i] = i;

    int cnt = 0, ans = 0;
    for (int i = 0; i < m; i++) {
        //在同一个连通块中就结束
        if (find(e[i].u) == find(e[i].v)) continue;

        merge(e[i].u, e[i].v);
        ans += e[i].w;
        cnt++;

    }

    if(cnt<n-1)
        cout<<"impossible"<<endl;
    else
        cout << ans << endl;
    return 0;
}
```
