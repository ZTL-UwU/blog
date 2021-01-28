# [ 模板 ] [ 字符串 ] AC 自动机

```cpp
#include <iostream>
#include <queue>
using namespace std;
const int MAXN = 1e6 + 10;
const int MAXN_N = 1e4 + 10;
// Trie array
int trie[MAXN_N][30];
// End point of Trie
int endf[MAXN_N];
// Fail array in automata
int fail[MAXN_N];
// Number of nodes in Trie
int num;
// Trie
void insert(string str)
{
    int u = 0;
    for (int i = 0; i < str.size(); i++)
    {
        int ch = str[i] - 'a';
        /*
         *
         *   (u)     (u)  --+
         *   /  -->  /      | u
         * NULL    ++num  <-+
         * --or--
         *   (u)     (u) --+
         *   /  -->  /     | u
         * (ch)     (ch) <-+
         * 
         */
        if (!trie[u][ch])
            trie[u][ch] = ++num;
        u = trie[u][ch];
    }
    // End of a string
    endf[u] ++;
}
// AC Automata
void build(string str)
{
    queue<int> q;
    /*
     * Init
     *
     *      +-->  a  <--+
     * fail |    / \    | fail
     *      +-  b   c  -+ <<- trie[0][a..z]
     *         / \  |  
     *        d   e f  
     */
    for (int i = 0; i < 26; i++)
    {
        int v = trie[0][i];
        if (v)
        {
            fail[v] = 0;
            q.push(v);
        }
    }
    // BFS
    while (!q.empty())
    {
        int u = q.front();
        q.pop();
        /*
         * Get fail
         *                        ...
         *               fail[u]  /
         *               +-----> c   <- trie[fail[u]]
         *         ...   |      / \
         *          |    |     a   b <- trie[fail[u]][i]
         *     u -> a ---+         ↑
         *          |              |
         *     v -> b -------------+
         *     ↑         fail[v]
         * trie[u][i]
         */
        for (int i = 0; i < 26; i++)
        {
            int v = trie[u][i];
            if (v)
            {
                fail[v] = trie[fail[u]][i];
                q.push(v);
            }
            else
                trie[u][i] = trie[fail[u]][i];
        }
    }
}
int query(string str)
{
    int u = 0;
    int ans = 0;
    /*
     * Query
     * (f means fail)
     *               
     *              r
     *             / \
     *            /   \
     *           /     \
     *          a       b
     *         / \       \  
     *        /   \       \ 
     *       /     \       \
     *      c       d       e
     *      |       |          
     *      g       h     
     *      | 
     *      |          
     *      i 
     *           v final
     *              ↓
     *              r <---+
     *             / \    | f
     *            /   \   |
     *           /     \ /
     *          a  v3-> b <---+
     *         / \       \    |
     *        /   \       \   |
     *       /     \       \  |f
     *      c       d       e |
     *      |       |         |
     *      g  v2-> h --------+    
     *      |       ↑\ 
     *      |       | \
     * u -> i ------+  +--> end => ans += endf[v2];
     *      ↑   f
     *   v start
     */
    for (int i = 0; i < str.size(); i++)
    {
        int ch = str[i] - 'a';
        u = trie[u][ch];
        int v = u;
        while (v && endf[v] != -1)
        {
            // If it't not the end point -> endf = 0 -> ans += 0 (no changes)
            ans += endf[v];
            // visited
            endf[v] = -1;
            // Jump to next point
            v = fail[v];
        }
    }
    return ans;
}
int main()
{
    int n;
    cin >> n;
    for (int i = 0; i < n; i++)
    {
        string s;
        cin >> s;
        insert(s);
    }
    string str;
    cin >> str;
    build(str);
    cout << query(str) << "\n";
    return 0;
}
```