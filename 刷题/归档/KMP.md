---
up: 
  - "[[刷题/归档/字符串]]"
down: 
relation: 
题目:
---
[![](https://cdn.nlark.com/yuque/0/2023/jpg/25992891/1698381803502-b368599d-7ef6-4859-acbe-b98b33b26fa0.jpg)](https://cdn.nlark.com/yuque/0/2023/jpg/25992891/1698381803502-b368599d-7ef6-4859-acbe-b98b33b26fa0.jpg)

- [28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)
- 实现next数组
- next是前缀表，每个位置的数字代表[0, 当前位置]的最大相等前缀后缀长度
- 有三种前缀表，1、普通前缀表；2、整体-1；3、前缀表右移，0位置为-1

```C
void getNext(int* next, const string& s) {
    int j = 0;
    next[0] = 0;
    //第一位的最大相等前缀后缀长度是0

    //j是前缀结束位置，j同时也是是[0, i]的最大相等前缀后缀长度
    //i是后缀结束位置
    for(int i = 1; i < s.size(); i++) {
        //i和j不相等的时候，需要向前回退到next[j - 1]，现在就相当于是s 和s进行匹配，i指向的是主串，需要重新进行回退到前一位指向的索引
        while (j > 0 && s[i] != s[j]) {
            j = next[j - 1];
        }
        //如果s[i]
        if (s[i] == s[j]) {
            j++;
        }
        next[i] = j;
    }
}
```

- 利用前缀表进行快速匹配
- 当发现不匹配的时候，1、j进行跳转到next[j - 1]；2、j进行跳转到next[j - 1] + 1；3、j进行跳转到next[j]

```C
int strStr(string haystack, string needle) {
    if (needle.size() == 0) {
        return 0;
    }
    int next[needle.size()];
    getNext(next, needle);
    int j = 0;
    for (int i = 0; i < haystack.size(); i++) {
        while(j > 0 && haystack[i] != needle[j]) {
            j = next[j - 1];
        }
        if (haystack[i] == needle[j]) {
            j++;
        }
        if (j == needle.size() ) {
            return (i - needle.size() + 1);
        }
    }
    return -1;
}
```

---

- [459. 重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/)
- 如果字符串的结尾的前缀表不为0，也就是整个字符串有相等的前后缀，而且，字符串的长度len可以整除len - next[len - 1]，这样就说明有重复的子字符串可以实现串