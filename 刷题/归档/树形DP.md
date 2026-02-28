---
up: 
  - "[[动态规划]]"
  - "[[树]]"
down: 
relation: 
题目:
---
### 没有上司的舞会

Ural 大学有 N 名职员，编号为 1∼N。他们的关系就像一棵以校长为根的树，父节点就是子节点的直接上司。每个职员有一个快乐指数，用整数 Hi 给出，其中 1≤i≤N。现在要召开一场周年庆宴会，不过，没有职员愿意和直接上司一起参会。在满足这个条件的前提下，主办方希望邀请一部分职员参会，使得所有参会职员的快乐指数总和最大，求这个最大值。

---

- 状态表示：max
- f[u][0]:以u为根的子树中选择，不选u这个点的方案
- f[u][1]:以u为根的子树中选择，选u这个点的方案
- 状态转移：
- f[u][0] 有每个儿子的状态，f[u][0],f[u][1] max，因为不选当前树根节点，子树就可以有两种选择，选或者不选子树根节点
- f[u][1] f[u][0] max 因为选当前树根节点，子树只有一个根节点

```C
\#include<bits/stdc++.h>
using namespace std;
const int N = 10010;
int e[N],ne[N],h[N],idx;//单链表，<公司人际关系>
int happy[N];
int n;
bool has_fa[N];//某人有没有上司
int f[N][2];

void add(int a,int b){
    //e[idx]存储值，ne[idx]存储当前点的下一个点也就是指向，
    e[idx]=b,ne[idx]=h[a],h[a]=idx,idx++;
}
//每次输入的a和b就是节点的编号，编号用e[i]数组存储
//节点的下标指节点在数组中的位置索引，数组之间的关系就是通过下标来建立连接，下标用idx来记录。idx范围从0开始，如果idx==-1表示空
//ne[idx]的值是下标，是下标为idx的节点的next节点的下标，ne[idx]=-1表示该表已被遍历完
//h[a]存储的是下标，是编号为a的节点的next节点的下标idx，表示以a为头节点的子树

void dfs(int u){
    f[u][1]=happy[u];
    for(int i=h[u];i!=-1;i=ne[i]){
        int j=e[i];//取出点的编号
        dfs(j);//用该值开始新遍历
        f[u][1]+=f[j][0];
        f[u][0]+=max(f[j][0],f[j][1]);
    }
}
int main(){
    scanf("%d",&n);
    for(int i=1;i<=n;i++)
        cin>>happy[i];
    memset(h,-1,sizeof h);//头节点数组初始化为-1,每个点都无相连接的点）
    for(int i=n+1;i<2*n;i++){
        int a,b;//a是b的上司
        scanf("%d %d",&b,&a);
        has_fa[b]=true;//b有上司
        add(a,b);//建树
    }
    int root=1;//赋头节点初值

    while (has_fa[root])root++;//没有上司的节点即为根节点

    dfs(root);//从根节点开始dfs遍历
    printf("%d\n", max(f[root][0],f[root][1]));//比较头节点的两种方案

    return 0;
}
```

---

- [968. 监控二叉树](https://leetcode.cn/problems/binary-tree-cameras/)
- 从低到上，先给叶子节点父节点放个摄像头，然后隔两个节点放一个摄像头，直至到二叉树头结点
- 0：该节点无覆盖、1：本节点有摄像头、2：本节点有覆盖
- 情况1：// 左右节点都有覆盖`if (left == 2 && right == 2) return 0;`
- 情况2：//左右节点至少有一个没有覆盖`if (left == 0 || right == 0) { result++; return 1;}`
- 情况3：//左右节点至少有一个摄像头 `if (left == 1 || right == 1) return 2;`
- 情况4：头结点没有覆盖

  

**具体来说，对于每一个节点，我们需要维护三种状态：**

**0：该节点无覆盖1：本节点有摄像头2：本节点有覆盖**

```C
int result;
int traversal(TreeNode* cur) {
    // 空节点，该节点有覆盖
    if (cur == NULL) return 2;
    int left = traversal(cur->left);    // 左
    int right = traversal(cur->right);  // 右
    // 情况1
    // 左右节点都有覆盖
    if (left == 2 && right == 2) return 0;
    // 情况2
    if (left == 0 || right == 0) {
        result++;
        return 1;
    }
    // 情况3
    if (left == 1 || right == 1) return 2;
    return -1;
}

int minCameraCover(TreeNode* root) {
    result = 0;
    // 情况4
    if (traversal(root) == 0)// root 无覆盖
        result++;
    return result;
}
```