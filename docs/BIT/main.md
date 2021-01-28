---
hide:
  - toc
---

# 树状数组

- 前置知识 :

1. 差分&前缀和

2. 位运算

3. 树的基本概念和定理

### 1. 什么是树状数组?

> 树状数组(Binary Indexed Tree(B.I.T), Fenwick Tree)是一个查询和修改复杂度都为Log(N)的数据结构。主要用于查询任意两位之间的所有元素之和，但是每次只能修改一个元素的值；经过简单修改可以在Log(N)的复杂度下进行范围修改，但是这时只能查询其中一个元素的值(如果加入多个辅助数组则可以实现区间修改与区间查询)。——百度百科

~~SMG~~

### 其实基本的树状数组就是实现这样一个问题
##### ——求
1. 数组中一段区间的和
2. 修改数组中某一个数


## 小明:"不就是前缀和可以搞定的嘛!"

$ \large\texttt{嗖---} $

```cpp
#include<iostream>
using namespace std;
int a[1000000];
int sum[1000000];
int main()
{
    int n, ask;
    cin >> n >> ask;
    while (ask --)
    {
        char type;
        cin >> type;
        if (type == 'A') //查询
        {
            int x;
            cin >> x;
            cout << sum[x] << endl;
        }
        else if (type == 'B') //修改
        {
            int x, p;
            cin >> x >> p;
            for (int i = x; i <= n; i ++) sum[i] += p;
        }
    }
    return 0;
}
```

$ \large\texttt{提交} $

![](https://cdn.luogu.com.cn/upload/image_hosting/qnzuqtri.png)

### ~~跑得好快呀!~~

## 那么这时我们的树状数组就该出场啦~

首先,树状数组,顾名思义长得像树的数组

所以树状数组长这个样

![](https://cdn.luogu.com.cn/upload/image_hosting/mn6ar5j5.png)

上面是树,下面是数组

## 首先是求区间和

通过这幅图,应该不难看出,一个数到最前面的距离就等于刚好包含这个区间的几个横条的和:

#### 比如区间1~7为:

![](https://cdn.luogu.com.cn/upload/image_hosting/sephhjdz.png)

绿色条的和

知道这个,那么任意两个数之间的区间也就容易了,
只需要把两个区间到1的区间和相减就可以了。

## 那有了区间和还不够,还要修改呢

修改也简单,只需要每次修改包含自己的横条就好了(~~废话~~)

#### 比如修改5的值:

![](https://cdn.luogu.com.cn/upload/image_hosting/jo2wacek.png)

把蓝色横条全改了就好啦!

## 这么说两个功能都准备好了,那就到代码实现了

首先引入一个概念——Lowbit

指一个数在二进制下最末尾的1所对应的值

| 十进制数 | 二进制数               | Lowbit值 |
| :------- | :--------------------- | :------- |
| 1        | $\large\texttt{1}$     | 1        |
| 2        | $\large\texttt{1}$ 0   | 2        |
| 3        | 1$\large\texttt{1}$    | 1        |
| 4        | $\large\texttt{1}$00   | 4        |
| 5        | 10 $\large\texttt{1}$  | 1        |
| 6        | 1 $\large\texttt{1}$ 0 | 2        |
| 7        | 11 $\large\texttt{1}$  | 1        |
| 8        | $\large\texttt{1}$ 000 | 8        |
| 9        | 100$\large\texttt{1}$  | 1        |
| 10       | 10$\large\texttt{1}$0  | 2        |
| ...      | ...                    | ...      |

发现了吗,跟它在树状数组中的高度是一模一样的

![](https://cdn.luogu.com.cn/upload/image_hosting/cqndn1tv.png)

首先二进制负数就是原数的反码(把原数的0变为1,1变为0)+1(也称原数的补码)

也就是说我们用原数按位与(&)原数的补码

就是最后一个1的位置所表示的值,也就是Lowbit值

### **综上,Lowbit(x) = x&(-x)**

**如果没看懂也不要紧,记下来就好了。**

### 根据上面的结论,我们可以搞定两个东西
#### 1. 一个位置左边的横条在 x - Lowbit(x)
#### 2. 一个位置上面的横条在 x + Lowbit(x)

### (随便找一个点在上图中康康就知道啦)

&nbsp;

### 有了这几个结论,我们就可以开始搞事情了

## 首先是区间查询
只需要一直往左边找。(上面的1号结论)

代码很简单:
```cpp
// tree表示横条的值
int get_sum(int x) // 求前x个数的和
{
    int sum = 0;
    while (x > 0)
    {
        ans += tree[x]; // 加上当前点的值
        x -= lowbit(x); // 继续找左边的一个横条
    }
    return ans;
}
```

## 接着是修改点的值
只需要一直找上一个横条。(上面的2号结论)

代码:
```cpp
// 这个是加一个数,可以有别的修改
void add(int x, int p)
{
    while (x <= n)
    {
        tree[x] += p; //修改改当前点
        x += lowbit(x); // 继续找上面的横条
    }
}
```

## 树状数组get!

## 那它的复杂度呢?

- ### 区间查询

从之前所说来看,我们只要往左找,越到左边层数越高,最左边是最高的,最高点的高度最多为这颗二叉树树的高度

### 时间复杂度: $ \text{O}(\log_2N) $

- ### 单点修改

修改单点指需要找到包含自己的所有横条,也就是一直往树根跳,小于树的高度

### 时间复杂度: $ \text{O}(\log_2N) $

## 再看看前缀和的复杂度:

- 区间查询: 时间复杂度: $ \text{O}(1) $

- 单点修改: 时间复杂度: $ \text{O}(N) $

## 小明:"怪不得T飞了!"

### 模板切掉!
#### [洛谷P3374【模板】树状数组1](https://www.luogu.org/problem/P3374)
```cpp
#include<iostream>
#define endl '\n'
using namespace std;
int a[500010];
int n, m;
int lowbit(int x) //求Lowbit
{
    return x & -x;
}
void add(int x, int k) //修改
{
    while (x <= n)
    {
        a[x] += k;
        x += lowbit(x);
    }
}
int sum(int k) // 求和
{
    int sum = 0;
    while (k > 0)
    {
        sum += a[k];
        k -= lowbit(k);
    }
    return sum;
}
int main()
{
    ios::sync_with_stdio(0);  // 玄学输入输出优化
    cin.tie(0);
    cout.tie(0);
    cin >> n >> m;
    for (int i = 1; i <= n; i ++)
    {
        int tmp;
        cin >> tmp;
        add(i, tmp); // 初始边
    }
    for (int i = 1; i <= m; i ++)
    {
        int type, x, k;
        cin >> type >> x >> k;
        if (type == 1) add(x, k); // 加边
        else cout << sum(k) - sum(x - 1) << endl; // 输出区间和
    }
    return 0;
}
```

#### 温馨提示:
1. 树状数组的题输入量大,注意用 $\large\texttt{快读}$ 或 $\large\texttt{scanf, printf}$
2. 树状数组的题喜欢爆int,不要没开 $\large\texttt{long long}$ 见祖宗

### 会了模板,这几题可以切了!

#### [ Loj P10116 清点人数 ](https://loj.ac/problem/10116)

#### [ Loj P10117 简单题 ](https://loj.ac/problem/10117)