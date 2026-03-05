---
tags:
  - 算法
up:
  - "[[搜索]]"
down:
  - "[[组合]]"
  - "[[图路径问题]]"
  - "[[洪水灌溉问题]]"
  - "[[排列]]"
relation:
  - "[[图的遍历算法]]"
---
- [[组合]]
- 图的搜索
    - [[图路径问题]]
    - [[洪水灌溉问题]]
- [二分图](https://www.notion.so/3226e8cbddff4bedb3d22bf7fe7128f6?pvs=21) 】】
- 二分图
- 欧拉回路
    - 2097.合法重新排列数对
- 暴力枚举
    - 2160.拆分数位后四位数字的最小和
    - 剑指 Offer 49.丑数
    - 22.括号生成
    - 869.重新排序得到 2 的幂
    - 949.给定数字能组成的最大时间
    - 17.电话号码的字母组合
    - 522.最长特殊序列I1
    - 1780.判断一个数字是否可以表示成三的幂的和
    - 剑指 offer I1 087.复原 IP
    - 93.复原IP 地址
    - 769.最多能完成排序的块
    - 2002.两个回文子序列长度的最大乘积
    - 1457.二又树中的伪回文路径
- 二叉树的搜索
    - 二叉搜索树的搜索

  

```C++
vector<vector<int>> adjList;
vector<bool> visited(adjList.size(), false);
void DFS(int curr, vector<bool> &visited) {
    visited[curr] = true;
    cout << curr << " ";
    for (int neighbor : adjList[curr]) {
        if (!visited[neighbor]) {
            DFS(neighbor, visited);
        }
    }
}
```

# 树的DFS

# 图的DFS

# 回溯

# 剪枝
