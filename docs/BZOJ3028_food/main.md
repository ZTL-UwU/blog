---
hide:
  - toc
---

# BZOJ3028 食物

??? Quote "题面"

    ## 题面

    ### 链接

    [BZOJ](https://www.lydsy.com/JudgeOnline/problem.php?id=3028)

    [dark BZOJ](https://darkbzoj.tk/problem/3028)

    ### 题目描述

    明明这次又要出去旅游了，和上次不同的是，他这次要去宇宙探险！我们暂且不讨论他有多么 NC，他又幻想了他应该带一些什么东西。理所当然的，你当然要帮他计算携带 $N$ 件物品的方案数。他这次又准备带一些受欢迎的食物，如：蜜桃多啦，鸡块啦，承德汉堡等等当然，他又有一些稀奇古怪的限制：每种食物的限制如下：

    > 承德汉堡：偶数个
    > 
    > 可乐：$0$个或$1$个
    > 
    > 鸡腿：$0$个，$1$个或$2$个
    > 
    > 蜜桃多：奇数个
    > 
    > 鸡块：$4$的倍数个
    > 
    > 包子：$0$个，$1$个，$2$个或$3$个
    > 
    > 土豆片炒肉：不超过一个。
    > 
    > 面包：$3$的倍数个

    **注意**，这里我们懒得考虑明明对于带的食物该怎么搭配着吃，也认为每种食物都是以 `个` 为单位（反正是幻想嘛），只要总数加起来是 $N$ 就算一种方案。因此，对于给出的 $N$，你需要计算出方案数，并对 $10007$ 取模。

    ### 输入

    输入一个数字 $N, 1 \leq n \leq 10^{500}$

    ### 输出

    如题

    ### 输入样例

    `5`

    ### 输出样例

    `35`

## 题解

生成函数入门题

### 推导

1. 承德汉堡 $f_1$

    > 承德汉堡：偶数个

    即为数列 $\langle 0, 2, 4, 6, \cdots \rangle$ 的生成函数

    $$
        f_1(x) = \sum_{n \geq 0} x^{2n}
    $$

    求出其封闭形式

    $$
      \begin{align}
        f_1(x) \cdot x^2 + x^0 &= \sum_{n \geq 0} x^{2(n + 1)} + x^0 \\
                               &= \sum_{n \geq 1} x^{2n} + x^0       \\
                               &= f_1(x)
      \end{align}
    $$
    
    $$
        \therefore f_1(x) \cdot (x^2 - 1) = -1 \\
        \therefore f_1(x) = \frac{1}{1 - x^2}
    $$
    
2. 可乐 $f_2$

    > 可乐：0个或1个

    即为数列 $\langle 0, 1 \rangle$ 的生成函数

    $$
        f_2(x) = \sum_{n \in \{0, 1\}} x^n
    $$

    其封闭形式为

    $$
        f_2(x) = x^0 + x^1 = 1 + x
    $$

3. 鸡腿 $f_3$

    > 鸡腿：0个，1个或2个

    同 `2.可乐` 理

    即为数列 $\langle 0, 1, 2 \rangle$ 的生成函数

    $$
        f_3(x) = 1 + x + x^2
    $$

4. 蜜桃多 $f_4$

    > 蜜桃多：奇数个

    即为数列 $\langle 1, 3, 5, 7 \cdots \rangle$ 的生成函数

    $$
        f_4(x) = \sum_{n \geq 0} x^{2n + 1}
    $$

    求出其封闭形式

    $$
      \begin{align}
        f_4(x) \cdot x^2 + x &= \sum_{n \geq 0} x^{2(n + 1) + 1} + x \\
                             &= \sum_{n \geq 1} x^{2n + 1} + x       \\
                             &= f_4(x)
      \end{align}
    $$

    $$
        \therefore f_4(x) \cdot (x^2 - 1) = -x \\
        \therefore f_4(x) = \frac{x}{1 - x^2}
    $$

5. 鸡块 $f_5$

    > 鸡块：4的倍数个

    即为数列 $\langle 0, 4, 8, 12, \cdots \rangle$ 的生成函数

    $$
    f_5(x) = \sum_{n \geq 0} x^{4n}
    $$

    求出其封闭形式

    $$
      \begin{align}
         f_5(x) \cdot x^4 + x^0 &= \sum_{n \geq 0} x^{4(n + 1)} + x^0 \\
                                &= \sum_{n \geq 1} x^{4n} + x^0       \\
                                &= f_5(x)
      \end{align}
    $$

    $$
        \therefore f_5(x) \cdot (x^4 - 1) = -1 \\
        \therefore f_5(x) = \frac{1}{1 - x^4}
    $$

6. 包子 $f_6$

    > 包子：0个，1个，2个或3个

    同 `2.可乐` 理

    即为数列 $\langle 0, 1, 2, 3 \rangle$ 的生成函数

    $$
        f_6(x) = 1 + x + x^2 + x^3\\
    $$

    因式分解，得

    $$
        f_6(x) = (1 + x)(1 + x^2)
    $$

7. 土豆片炒肉 $f_7$

    > 土豆片炒肉：不超过一个。

    同 `2.可乐`

    $$
        f_7(x) = f_2(x) = 1 + x
    $$

8. 面包 $f_8$

    > 面包：3的倍数个

    $$
        f_8(x) = \sum_{n \geq 0} x^{3n}
    $$

    求其封闭形式

    $$
    \begin{align}
        f_8(x) \cdot x^3 + x^0 &= \sum_{n \geq 0} x^{3(n + 1)} + x^0 \\
                               &= \sum_{n \geq 1} x^{3n} + x^0       \\
                               &= f_8(x)
      \end{align}
    $$

    $$
        \therefore f_8(x) \cdot (x^3 - 1) = -1 \\
        \therefore f_8(x) = \frac{1}{1 - x^3}
    $$

综上

$$
    f_1(x) = \frac{1}{1 - x^2} \\
    f_2(x) = 1 + x             \\
    f_3(x) = 1 + x + x^2       \\
    f_4(x) = \frac{x}{1 - x^2} \\
    f_5(x) = \frac{1}{1 - x^4} \\
    f_6(x) = (1 + x)(1 + x^2)  \\
    f_7(x) = 1 + x             \\
    f_8(x) = \frac{1}{1 - x^3} \\
$$

乘积为

$$
    \prod_{i=1}^8 f_i(x) = \frac{1}{1 - x^2} \cdot (1 + x) \cdot (1 + x + x^2) \cdot \frac{x}{1 - x^2} \cdot \frac{1}{1 - x^4} \cdot (1 + x)(1 + x^2) \cdot (1 + x) \cdot \frac{1}{1 - x^3}
$$

展开，得

$$
    \prod_{i=1}^8 f_i(x) = \frac{1}{(1 + x)(1 - x)} \cdot (1 + x) \cdot (1 + x + x^2) \cdot \frac{x}{(1 + x)(1 - x)} \cdot \frac{1}{(1 + x^2)(1 + x)(1 - x)} \cdot (1 + x)(1 + x^2) \cdot (1 + x) \cdot \frac{1}{(1 - x)(1 + x + x^2)}
$$

化简，得

$$
    \prod_{i=1}^8 f_i(x) = \frac{x}{(1 - x)^4}\\
    = x(1 - x)^{-4}
$$

由二项式定理

$$
    (x + y)^n = \sum_{k = 0}^n \binom{n}{k} x^{n - k}y^k
$$

得：

$$
    x(1 - x)^{-4} = x \sum_{i \geq 0} \binom{-4}{i}(-x)^i
$$

第 $n$ 项对应 $i = n - 1$，其系数即为：

$$
    \binom{-4}{n - 1} (-1)^{n - 1}
$$

反转上指标[^反转上指标]

$$
    \binom{a}{b} = (-1)^b \binom{b - a - 1}{b} \ (a < 0)
$$

$$
    \therefore \binom{-4}{n - 1} (-1)^{n - 1} = \binom{n + 2}{n - 1} = \binom{n + 2}{3} = \frac{n(n + 1)(n + 2)}{6}
$$

即方案数为

$$
    \frac{n(n + 1)(n + 2)}{6}
$$

### 细节

1. 由于输入的 $n$ 范围为 $10^{500}$ 需要在输入时取模

    $$
        \because \overline{n_1 n_2 n_3 \cdots n_{f - 1} n_f} = 10 \overline{n_1 n_2 n_3 \cdots n_{f - 1}} + n_f\\
        \therefore \overline{n_1 n_2 n_3 \cdots n_{f - 1} n_f} = 10(10(\cdots (10n_1 + n_2) \cdots) + n_{f - 1}) + n_f \\
        \therefore \overline{n_1 n_2 n_3 \cdots n_{f - 1} n_f} \equiv (10((10((\cdots (10n_1 + n_2)\ mod\ M \cdots)\ mod\ M) + n_{f - 1})\ mod\ M) + n_f)\ mod\ M (mod\ M)
    $$

    **伪代码**

    ```cpp
    /*
        输入的数为 n
        n 的各位为 n[1 ... f]
        ans = n % MOD
    */

    Function Read()
        for i <- 1 to f do
            ans <- (ans * 10 + n[i]) % MOD
        end

        Return ans
    ```

2. 要求 $\frac{n(n + 1)(n + 2)\ mod\ M}{6}$ 需要用到费马小定理求出乘法逆元

    $$
        \frac{n(n + 1)(n + 2)\ mod\ M}{6} = (n(n + 1)(n + 2)\ mod\ M) \cdot (6^{M - 2}\ mod\ M)
    $$

    这里可以选择写带取模快速幂，也可以直接算出 $6^{M - 2}\ mod\ M$

    当 $M = 10007$ 时：
    $$
        6^{M - 2}\ mod\ M = 1668
    $$

## 代码

=== "C++"

    ```cpp
    #include <iostream>
    #include <cstdio>

    const int MOD = 10007;

    inline int read()
    {
        char ch;
        int res = 0;

        ch = std::getchar();
        while (ch >= '0' and ch <= '9')
        {
            res = (res * 10 + ch - '0') % MOD;
            ch = std::getchar();
        }

        return res;
    }

    int main()
    {
        int n;
        n = read();

        std::cout << ((n * (n + 1) % MOD) * (n + 2) % MOD) * 1668 % MOD;
        return 0;
    }
    ```

=== "Python3"

    ```python
    MOD = 10007

    n = int(input())
    print(n * (n + 1) * (n + 2) * 1668 % MOD)
    ```

## 参考

[^反转上指标]: [[知乎] 当组合里出现负数怎么运算？](https://www.zhihu.com/question/268134502/answer/1358799081)

[BZOJ3028 食物](https://blog.csdn.net/As_A_Kid/article/details/85766917)

[[BZOJ3028] [生成函数] 食物](https://blog.csdn.net/qq_43346903/article/details/87925793)

[[百度百科] 二项式定理](https://baike.baidu.com/item/%E4%BA%8C%E9%A1%B9%E5%BC%8F%E5%AE%9A%E7%90%86)

[本题数据](https://darkbzoj.tk/data/3028.zip)
