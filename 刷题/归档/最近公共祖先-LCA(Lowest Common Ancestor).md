---
up:
  - "[[树]]"
---
- 求最小公共祖先，需要从底向上遍历，那么二叉树，只能通过后序遍历（即：回溯）实现从底向上的遍历方式。

---

- [236.二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)
- [剑指 offer 68 - I1.二叉树的最近公共祖先](https://leetcode.cn/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof)

```C
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if(root == NULL || root == q || root == p)
        return root;
    //采用后序遍历的方法，只要当前根节点是p和q中的任意一个，就返回（因为不能比这个更深了，再深p和q中的一个就没了）
    TreeNode* left = lowestCommonAncestor(root->left, p, q);
    TreeNode* right = lowestCommonAncestor(root->right, p, q);
//根节点不是p和q中的任意一个，那么就继续分别往左子树和右子树找p和q

    if(q == NULL && p == NULL)
        return NULL;//pq都没有找到，就没有
    if(left == NULL)
        return right;//没在左子树，就返回右子树
    if(right == NULL)
        return left;//没在右子树，就返回左子树
    return root;//如果两个都找到，那就说明p和q分别在左右两个子树上，所以此时的最近公共祖先就是root
}
```

---

- [剑指 offer 68 - 1.二叉搜索树的最近公共祖先](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof)
- [235.二叉搜索树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree)
- 从上向下去递归遍历，第一次遇到 cur节点是数值在[p, q]区间中，那么cur就是 p和q的最近公共祖先。

---

- 面试题 04.08.首个共同祖先
- 865.具有所有最深节点的最小子树
- 1123.最深叶节点的最近公共祖先
- 2096.从二叉树一个节点到另一个节点每一步的方向
