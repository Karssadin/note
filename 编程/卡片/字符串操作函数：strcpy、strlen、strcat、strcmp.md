---
tags: 
up: 
  - "[[C++]]"
down: 
relation:
  - "[[拷贝函数：strcpy、memcpy、sprintf]]"
---
- `char* strcpy(char* strDest, const char* strSrc)`:把从strSrc地址开始且含有'\0'结束符的字符串复制到以strDest开始的地址空间，返回值的类型为char*

```C
char* strcpy(char *dst,const char *src) {
    assert(dst != NULL && src != NULL);
    char *ret = dst;
    while ((*dst++=*src++)!='\0');
    return ret;
}
```

- `int strlen(const char* str)`：计算给定字符串的长度
- `char* strcat(char* dest, const char* src)`:把src所指字符串添加到dest结尾处
- `int strcmp(const char* str1, const char* str2)`：相等返回0，str1小返回负数