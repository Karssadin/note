---
up:
  - "[[贪心]]"
---
- Huffman树，每次取最小的两个合并，之后成为新的一堆，再从n-2+1堆中取最小的两堆进行合并

---

```C
\#include <bits/stdc++.h>
using namespace std;
int main(){
    int n;
    scanf("%d", &n);

    priority_queue<int, vector<int>, greater<int>> heap;
    while (n -- ){
        int x;
        scanf("%d", &x);
        heap.push(x);
    }

    int res = 0;
    while (heap.size() > 1){
        int a = heap.top(); heap.pop();
        int b = heap.top(); heap.pop();
        res += a + b;
        heap.push(a + b);
    }
    printf("%d\n", res);
    return 0;
}
```
