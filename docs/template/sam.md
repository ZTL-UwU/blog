# [ 模板 ] [ 字符串 ] SAM (后缀自动机)

```cpp
#include <iostream>
#include <string>
#include <map>
using namespace std;
const int MAXN = 2e5 + 10;
map<int, int> trans[MAXN]; // Transform
int max_len[MAXN];         // Max-length
int link[MAXN];            // link
long long ans;
int size, last;
void build(int add)
{
    int z = ++size; // New status
    int p = last;   // Pointer on link-path
    max_len[z] = max_len[last] + 1;
    for (; p != -1 && !trans[p].count(add); p = link[p])
        trans[p][add] = z;
    /*
     * Condition 1
     * v belongs to link-path(u -> S)
     * trans[v][] = NULL
     * -> link[z] = S
     */
    if (p == -1)     // All NULL in the link-path
        link[z] = 0; // Link to head
    /*
     * Condition 2
     * v belongs to link-path(u -> S)
     * found !NULL in trans[v][]
     */
    else
    {
        int x = trans[p][add]; // Next status
        /*
         * Condition 2.1
         * max-len[x] = max-len[v] + 1
         * -> link[z] = x
         */
        if (max_len[x] == max_len[p] + 1)
            link[z] = x;
        /*
         * Condition 2.2
         * max-len[x] > max-len[v] + 1
         * -> make new status
         */
        else
        {
            int y = ++size; // New status
            max_len[y] = max_len[p] + 1;
            // Copy x's data to y
            trans[y] = trans[x];
            link[y] = link[x];
            for (; p != -1 && trans[p][add] == x; p = link[p])
                trans[p][add] = y;
            link[x] = link[z] = y;
        }
    }
    last = z;
    ans += max_len[z] - max_len[link[z]];
}
int main()
{
    link[0] = -1;
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i++)
    {
        int x;
        scanf("%d", &x);
        build(x);
        printf("%lld\n", ans);
    }
    return 0;
}

```