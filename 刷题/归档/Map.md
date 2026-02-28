---
up: 
  - "[[刷题/归档/哈希表]]"
down: 
relation: 
题目:
---
- [697. 数组的度](https://leetcode.cn/problems/degree-of-an-array/)
- 遍历，利用map(int, vector<int>)存储数字跟数字出现次数、首位、末尾，判断更新max_len与max_sum

---

- [560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k)
- [LCR 010. 和为 K 的子数组](https://leetcode.cn/problems/QTMn0o/)
- 使用unorder_map存储前缀和，以及前缀和的个数num，判断当前前缀和 - k的值在哈希表中有没有，如果有的话，就找到num个结果，res + num；

---

- [383. 赎金信](https://leetcode.cn/problems/ransom-note)
- 使用map来存储后者的数据，之后前者的字符--，如果value小于0，就说明不可以
- 可以使用26个int数组来存储，这样空间较小

---

- [454. 四数相加 II](https://leetcode.cn/problems/4sum-ii/description/)
- 前两个数组先相加存入map中，value是出现的次数。遍历后面两个数组，判断是否存在对应的值，存在就将value加到count上，之后返回count

---

- [1. 两数之和](https://leetcode.cn/problems/two-sum/description)
- 使用哈希判断当前target - nums[i]是否之前出现过，如果有就返回，如果没有就把当前这个值存入map中，先判断再存入，避免出现同一个索引值*2 == target

---

- [49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams)
- 多个相同的字符串使用`vector<string>`来存储，在key值相同的时候进行`push_back`或者`emplace_back`，使用排序判断字符串中字符是否相同

---

- [438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string)
- 使用滑动窗口来做，map来存储对应target和窗口中的字母哈希值

---

- [242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram)
- 使用同一个`unordered_map<char, int>`进行存储两个字符串中的所有字符，第一个字符串值相同的的进行++，第二个进行--，之后判断second是否为0，不为0就返回false，