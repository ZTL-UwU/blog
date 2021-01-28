# [ 模板 ] [ 字符串 ] Manacher (马拉车)

```cpp
#include <iostream>
using namespace std;
const int MAXN = 1e6 + 10;
// Manacher

// Radius of palindromes with mid point on i
// ** This array should be 2 * MAXN cause we added '#' between chars
int ra[2 * MAXN];
string init_str(string str)
{
    /* New string which will be returned
     * Add "$" and "\0" to prevent runtime error
     */
    string n_str = "$" + str + str + "#\0";
    /*
     * Add '#' between two chars
     * In ordered to make both odd and even palindromes the same
     */
    for (int i = 1; i < n_str.size(); i++)
        n_str[i] = i % 2 ? '#' : str[i / 2 - 1];
    return n_str;
}
int Manacher(string str)
{
    // Init str
    str = init_str(str);
    // mx is the right border of a palindromes with id to be the mid point
    int mx = 0;
    int id = 0;
    // Variable to find the maximum palindrome
    // max_len = -INF
    int max_len = -0x7fffffff;
    for (int i = 0; i < str.size(); i++)
    {
        /* 
         * (2 * id - i) is the symmetry point of i with j in the middle
         *
         *     mx'   j      id      i      mx
         * ----^-----^------^-------^------^--------
         *        ---=---     ------=------
         *     -------------=---------------
         * 
         */
        if (i < mx)
            ra[i] = min(ra[2 * id - i], mx - i);
        /* 
         * Extend from i
         * If str[i - ra[i]] = str[i + ra[i]] means the palindrome can be larger
         * 
         * x-----i-----x
         * ^           ^
         * i - ra[i]   i + ra[i]
         */
        while (str[i - ra[i]] == str[i + ra[i]])
            ra[i]++;
        /*
         * If i + ra[i] > mx
         * Means palindrome with i in mid is larger than one with id in mid
         * Change id into i, mx to i + ra[i]
         */
        if (mx < i + ra[i])
        {
            mx = i + ra[i];
            id = i;
        }
        // Because we added '#' between every two chars, so ra[i] =  2 * (the palindrome) / 2 - 1;
        //                                                  ^ this is the RADIUS
        max_len = max(max_len, ra[i] - 1);
    }
    return max_len;
}
int main()
{
    string str;
    cin >> str;
    cout << Manacher(str) << "\n";
    return 0;
}
```