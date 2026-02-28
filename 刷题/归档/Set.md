---
up: 
  - "[[刷题/归档/哈希表]]"
down: 
relation: 
题目:
---
- [202. 快乐数](https://leetcode.cn/problems/happy-number/)
- 使用set，判断sum是否重复出现，如果重复了就return false，否则就一直循环到1

---

- [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays)
- 使用unordered_set来存储第一个数组中的元素，之后遍历第二个数组中的元素是否在set中存在，之后结果也存放到一个set中来去重，最后利用迭代器转换为vector。

---

- [剑指 Offer 03. 数组中重复的数字](https://leetcode.cn/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)
- 使用unordered_set存储值，遍历时查看是否有对应的值，如果有直接返回

---

- [0～n-1中缺失的数字](https://leetcode.cn/problems/que-shi-de-shu-zi-lcof/)
- 使用unordered_set来存储数组中的所有元素，再从0 ~ n - 1遍历，判断哪个元素没有在哈希中存在，就返回

---

- [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence)
- 使用unordered_set 存储值，在遍历判断元素时判断该元素是否有前缀（也就是只判断元素为序列首位才开始判断），之后判断是否存在后续即可