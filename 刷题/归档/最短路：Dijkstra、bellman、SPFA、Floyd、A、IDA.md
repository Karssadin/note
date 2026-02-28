---
up: 
  - "[[图]]"
down: 
relation: 
题目:
---
朴素Dijkstra——稠密图

堆优化Dijkstra——稀疏图

bellman-ford

SPFA

SPFA求负环

多源汇最短路——Floyd

A*

IDA*（迭代加深A*算法）

- 最短路模型问题通常使用广搜算法进行解决。深搜不适用于最短路问题，因为深搜会优先访问深度较大的结点，但这并不一定能够获得最短路径。相反，广搜能够在保证访问结点层数的前提下，逐层扩展到相邻结点，直到找到最短路径
- 单源最短路：
- 边的权重都是正数：朴素Dijkstra（O(n^2)与边数没关系，稠密图比较好）、堆优化的Dijkstra
- 存在负权重边：bellman-ford(O(nm))、SPFA(O(m))
- 多源汇最短路：Floyd(O(n^3))

### 朴素Dijkstra——稠密图

- S：已经确定是最短距离的点
- dis[1]=0、dis[i]=+∞
- 循环n次，i从0-n循环。找到不在s中距离最近的点t，将t加入S中，用t更新其他点的距离，dis[x]>dis[t]+x;
- 稠密图用邻接矩阵来存储

---

```C
\#include <iostream>
\#include <cstring>
\#include <algorithm>
using namespace std;

const int N = 510;
int n,m;
int graph[N][N];
int dis[N];
bool st[N];
int dijkstra(){
    memset(dis,0x3f,sizeof(dis));
    dis[1]=0;
    for(int i=0;i<n;i++){
        int t=-1;
        for(int j=1;j<=n;j++)
        //当前没有确定最短路长度的，距离起点最近的,找dis最小的点
            if(!st[j]&&(t==-1||dis[t]>dis[j]))
                t=j;
        st[t]=true;
        for(int j=1;j<=n;j++)
            dis[j]=min(dis[j],dis[t]+graph[t][j]);
    }
    if(dis[n]==0x3f3f3f3f)
        return -1;
    return dis[n];
}
int main(){
    memset(graph,0x3f,sizeof(graph));
    cin>>n>>m;
    while(m--){
        int a,b,c;
        cin>>a>>b>>c;

        graph[a][b]=min(graph[a][b],c);
    }
    cout<<dijkstra();
    return 0;
}
```

- Dijkstra算法的基本思想是从起点开始逐步向外扩展，在此过程中维护起点到已探索顶点的最短路径。具体而言，每次选取距离起点最近的未探索顶点作为下一个探索对象，通过该顶点更新与之相邻的顶点的最短距离。这个过程重复执行，直到到达终点或者所有可达的顶点都被探索过。
- 如果有一个负权路径指向某个已经探索过的点，是不会进入迭代中考虑这个路径的。这个是贪心，只考虑眼前的最优解
    
    [![](https://cdn.nlark.com/yuque/0/2023/jpeg/25992891/1698507092734-912c65c5-65a1-4b86-9a1d-0267ed691b82.jpeg)](https://cdn.nlark.com/yuque/0/2023/jpeg/25992891/1698507092734-912c65c5-65a1-4b86-9a1d-0267ed691b82.jpeg)
    

### 堆优化Dijkstra——稀疏图

- 稀疏图的话，朴素Dijkstra可能会RTL
- 连线很多的时候，对应的就是稠密图，显然易见，稠密图的路径太多了，所以就用点来找，也就是抓重点；
- 点很多，但是连线不是很多的时候，对应的就是稀疏图，稀疏图的路径不多，所以按照连接路径找最短路，这个过程运用优先队列，能确保每一次查找保留到更新到队列里的都是最小的，同时还解决了两个点多条路选择最短路的问题；

---

```C
\#include <iostream>
\#include <cstring>
\#include <queue>

using namespace std;

typedef pair<int,int> PII;

const int N = 150010;
int n,m;
int h[N],e[N],ne[N],w[N],idx;
int dist[N];
bool st[N];

void add(int a, int b, int c)  // 添加一条边a->b，边权为c
{
        // 有重边也不要紧，假设1->2有权重为2和3的边，再遍历到点1的时候2号点的距离会更新两次放入堆中
    // 这样堆中会有很多冗余的点，但是在弹出的时候还是会弹出最小值2+x（x为之前确定的最短路径），
    // 并标记st为true，所以下一次弹出3+x会continue不会向下执行。
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
    //e[idx]=b存储的是指向哪个点，w[idx]为a->这个点的权重，h[a]存储的是指向的idx序列，e[idx]才是点
}

int dijkstra(){
    memset(dist,0x3f,sizeof(dist));
    dist[1]=0;
    priority_queue<PII,vector<PII>,greater<PII>> heap;
    heap.push(make_pair(0,1)); // 这个顺序不能倒，pair排序时是先根据first，再根据second，
                         // 这里显然要根据距离排序
    while(heap.size()){
        auto t=heap.top(); // 取不在集合S中距离最短的点
        heap.pop();

        int ver=t.second,distance=t.first;
        if(st[ver])
            continue;
        st[ver]=true;
        for(int i=h[ver];i!=-1;i=ne[i]){
            int j=e[i];
            if(dist[j]>distance+w[i]){
                //j到起点的距离，和w[i](i点到j的距离)和distance(i到起点的距离)
                dist[j]=distance+w[i];
                heap.push(make_pair(dist[j],j));
            }
        }

    }
    if(dist[n]==0x3f3f3f3f)
        return -1;
    return dist[n];
}

int main(){
    memset(h,-1,sizeof(h));
    cin>>n>>m;
    while(m--){
        int a,b,c;
        cin>>a>>b>>c;
        add(a, b, c);
    }
    cout<<dijkstra();
    return 0;
}
```

### bellman-ford

- 存储边的方法是随便的
- n次，备份dis、循环所有边a、b、w
- 更新路径dist[b]=mid(dist[b],dist[a]+w)——松弛操作
- 所有边的距离，一定满足dist[b]=mid(dist[b],dist[a]+w)——三角不等式
- 迭代K次，表示不超过K条边到这个点的最短路径。**如果第N次迭代之后发现有更新，说明有存在负环**

---

```C
\#include <cstring>
\#include <iostream>
\#include <algorithm>

using namespace std;

const int N = 510,M=10010;
int n,m,k;
int dist[N],backup[N];
struct Edge{
    int a,b,w;
}edges[M];
void bellman_ford(){
    memset(dist,0x3f,sizeof(dist));
    dist[1]=0;
    for(int i=0;i<k;i++){
        //更新时候只使用上次迭代的结果，而不是用这次前段循环迭代的结果
        memcpy(backup,dist,sizeof(dist));
        for(int j=0;j<m;j++){
            int a=edges[j].a;
            int b=edges[j].b;
            int w=edges[j].w;

            dist[b]=min(dist[b],backup[a]+w);
        }
    }
}
int main(){

    cin>>n>>m>>k;
    for(int i=0;i<m;i++){
        int a,b,w;
        cin>>a>>b>>w;
        edges[i]={a,b,w};
    }
    bellman_ford();
    if(dist[n]>0x3f3f3f3f/2)
        cout<<"impossible";
    else
        cout<<dist[n];
    return 0;
}
```

### SPFA

- 只要没有负环使用SPFA，正权使用Dijkstra、负权使用SPFA
- 可以解决负权问题，正权问题，但是会被卡
- bellman-ford每次遍历所有边，SPFA对其做优化,只有前面的点变小了，才能更新后面的点
- 使用队列，将起点放入队列
- 只要队列不空，队列中存放的是dis[]变小的结点。
- 取t点，更新t的所有出边，加入队列

---

```C
\#include <cstring>
\#include <iostream>
\#include <queue>

using namespace std;

const int N = 1e5 + 10;

int n, m;
int h[N], e[N], ne[N], w[N], idx;
bool st[N];
int dist[N];

void add(int a, int b, int c)
{
    e[idx] = b;
    w[idx] = c;
    ne[idx] = h[a];
    h[a] = idx++;
}

int spfa()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    queue<int> q;
    q.push(1);

    st[1] = true;  //判重数组， 队列中有重复的点没有意义
    //st队列判断当前队列是否有该点，如果有就不用入队

    //只要队列中有变小的点，就进行迭代
    //遍历所有该点的出边
    //遍历，变小之后加入队列
    while (q.size()) {
        int t = q.front();
        q.pop();

        st[t] = false;

        for (int i = h[t]; i != -1; i = ne[i]) {
            int j = e[i];
            if (dist[j] > dist[t] + w[i]) {
                dist[j] = dist[t] + w[i];
                if (!st[j]) {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
    if (dist[n] == 0x3f3f3f3f) {
        return -1;
    }
    return dist[n];
}

int main()
{
    cin >> n >> m;

    memset(h, -1, sizeof h);

    for (int i = 0; i < m; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
    }
    spfa();
    if(dist[n] == 0x3f3f3f3f)
        cout << "impossible" << endl;
    else
        cout << dist[n] << endl;
    return 0;
}
```

### SPFA求负环

- dist表示最短距离，cnt数组表示最短路边的数量

```C
\#include <cstring>
\#include <iostream>
\#include <queue>

using namespace std;

const int N = 1e5 + 10;

int n, m;
int h[N], e[N], ne[N], w[N], idx;
bool st[N];
int dist[N],cnt[N];

void add(int a, int b, int c)
{
    e[idx] = b;
    w[idx] = c;
    ne[idx] = h[a];
    h[a] = idx++;
}

bool spfa()
{
    queue<int> q;
    //负环不一定从1开始，需要把所有点都放入队列中
    for(int i=1;i<=n;i++){
        q.push(i);
        st[i]=true;
    }
    while (q.size()) {
        int t = q.front();
        q.pop();
        st[t] = false;

        for (int i = h[t]; i != -1; i = ne[i]) {
            int j = e[i];
            if (dist[j] > dist[t] + w[i]) {
                dist[j] = dist[t] + w[i];
                //dist表示1到当前点的最小距离
                //cnt表示，1到当前点的边数，如果>=n就说明有负环了，经历了至少n+1个点
                cnt[j]=cnt[t]+1;
                if(cnt[j]>=n)
                    return true;

                if (!st[j]) {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
    return false;
}

int main()
{
    cin >> n >> m;
    memset(h, -1, sizeof h);

    for (int i = 0; i < m; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
    }
    spfa();

    if(spfa())
        cout << "Yes" << endl;
    else
        cout << "No" << endl;

    return 0;
}
```

### 多源汇最短路——Floyd

- 多源BFS（Multiple Source Breadth First Search）是一种基于广度优先搜索（BFS）的算法，用于在一个有向或无向图中从多个源点开始搜索到所有的可达节点，并求出每个可达节点与所有源点的最短距离。
- 多源BFS的思想是将所有源点一起放入队列中，然后以队列中的节点为起点，向外扩展，直到队列为空。每次扩展到一个新节点时，将其距离所有源点的距离更新，并将其邻居节点加入队列。由于每个节点只会被遍历一次，因此多源BFS算法的时间复杂度为O(|V| + |E|)，其中|V|是图中节点的数量，|E|是边的数量。
- 多源BFS算法可以用于解决多源最短路问题、多源可达性问题等。比如，在一个网格图中，多源BFS可以用于求出每个点到若干个障碍物的最短距离，或者求出每个点是否能够到达若干个障碍物
- Floyd算法是一种基于动态规划的算法，用于解决任意两点之间的最短路径问题
- d[i][j]存储所有的边

1、k:1->n2、i:1->n3、j:1->n;d[i][j]=min(d[i][j],d[i][k]+d[k][k])：遍历图中的每个节点k，依次考虑所有节点对(i,j)之间的最短路径，更新dis[i][j]的值

---

```C
\#include <iostream>
\#include <cstring>
using namespace std;

const int N = 210, M = 2e+10;

int n, m, k, x, y, z;
int d[N][N];

void floyd() {
    for(int k = 1; k <= n; k++)
        for(int i = 1; i <= n; i++)
            for(int j = 1; j <= n; j++)
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
}

int main() {
    cin >> n >> m >> k;
    memset(d,0x3f,sizeof d);
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= n; j++)
            if(i == j) d[i][j] = 0;
    while(m--) {
        cin >> x >> y >> z;
        d[x][y] = min(d[x][y], z);
        //注意保存最小的边
    }
    floyd();
    while(k--) {
        cin >> x >> y;
        if(d[x][y] > 0x3f3f3f3f/2) puts("impossible");
        //d[i][k]<0,d[k][j] 不可达，但是取min会取这个INF+d[i][j]会变小
        //由于有负权边存在所以约大过INF/2也很合理
        else cout << d[x][y] << endl;
    }
    return 0;
}
```

### A*

### IDA*（迭代加深A*算法）