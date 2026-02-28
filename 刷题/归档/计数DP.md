---
up: 
  - "[[动态规划]]"
down: 
relation: 
题目:
---
### 计数类DP

### 整数划分

一个正整数n可以表示成若干个正整数之和，形如：n=n1+n2+…+nk，其中n1≥n2≥…≥nk,k≥1。我们将这样的一种表示称为正整数n的一种划分。现在给定一个正整数n，请你求出n共有多少种不同的划分方法。

---

- 将1,n分别看作n个物体的体积，这些物体使用次数无限制。问恰好能装满n背包的方案数。完全背包变形
- 状态表示：
- f[i][j]是前i个整数恰好拼成j的方案数
- 状态转移：
- f[i][j] = f[i - 1][j] + f[i - 1][j - i] + f[i - 1][j - 2 * i] + ...;
- f[i][j - i] = f[i - 1][j - i] + f[i - 1][j - 2 * i] + ...;
- f[i][j]=f[i-1][j]+f[i][j-1]

```C
\#include <iostream>
using namespace std;
const int N = 1e3 + 7, mod = 1e9 + 7;
int f[N][N];

int main() {
    int n;
    cin >> n;

    f[0][0]=1;// 容量为0时，前 i 个物品全不选也是一种方案

    for (int i = 1; i <= n; i ++) {
        for (int j = 0; j <= n; j ++) {
            f[i][j] = f[i - 1][j] % mod; // 特殊 f[0][0] = 1
            //如果j>i的话，分成不选i和选 多个i。看上面推导
            if (j >= i) f[i][j] = (f[i - 1][j] + f[i][j - i]) % mod;
        }
    }

    cout << f[n][n] << endl;
}
```

### 计数问题

给出两个数字a、b，求a、b中0-9数字出现的次数

---

- 分情况讨论：
- 求从1-n中x出现的次数
- count(n,x)：计算1-n中x出现的次数
- count(b,x)-count(a-1,x)，类似前缀和，[a,b]

```C
# include <iostream>
# include <cmath>
using namespace std;

int dgt(int n) // 计算整数n有多少位
{
    int res = 0;
    while (n) ++ res, n /= 10;
    return res;
}

int cnt(int n, int i) // 计算从1到n的整数中数字i出现多少次
{
    int res = 0, d = dgt(n);
    for (int j = 1; j <= d; ++ j) // 从右到左第j位上数字i出现多少次
    {
        // l和r是第j位左边和右边的整数 (视频中的abc和efg); dj是第j位的数字
        // 443309  j=2 ,dj=0 ,l=4433 r=9
        int p = pow(10, j - 1), l = n / p / 10, r = n % p, dj = n / p % 10;


        //分情况讨论，在第j位上i出现的次数，也就是1-n中数字第j位是i的情况
        // 第一种情况
        //第j位左边的数字xxx，可以变化，也就是xxx!=l的情况（xxx = 000 ~ l - 1）
        //l可以取（000~l-1）中任何数字，r可以取(0~p之间所有数字
        //在这种情况下，小于n的数字中，i在第j位上有l*p个
        if (i) res += l * p;

        //第二种情况，如果i = 0, 左边高位不能全为0(视频中xxx = 001 ~ abc - 1)
        else if (!i && l) res += (l - 1) * p;

        //第三种情况
        //第j左边的数字不可以变化，如果dj<i的话，lir>ldjr，这样不在cnt函数的考虑范围内，为0，这里不考虑这点
        //如果dj==i，r就可以取(0~r),有r+1个数字
        //如果dj>i ，r就可以取(0~p)，有p个数字
        if ( (dj > i) && (i || l) ) res += p;
        //(i || l)表示i=0时，i不能出现在最高位（即l不能为0），如果高位都是0算0出现的次数是没有意义的
        if ( (dj == i) && (i || l) ) res += r + 1;
    }
    return res;
}
int main(){
    int a, b;
    while (cin >> a >> b , a)
    {
        if (a > b) swap(a, b);
        for (int i = 0; i <= 9; ++ i) cout << cnt(b, i) - cnt(a - 1, i) << ' ';
        cout << endl;
    }
    return 0;
}
```