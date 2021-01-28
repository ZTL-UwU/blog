# [ 模板 ] 替罪羊树

```cpp
#include <iostream>
#include <vector>
using std::cin;
using std::cout;
using std::vector;
namespace tzy
{
    struct node
    {
        node *left;
        node *right;
        int val;
        int size;
        int real_size;
        bool deleted;
        static const double alpha;
        bool is_negative();
        void push_up();
        node(int x)
        {
            this->left = nullptr;
            this->right = nullptr;
            this->val = x;
            this->size = 1;
            this->real_size = 1;
            this->deleted = false;
        }
    };
    const double node::alpha = 0.75;
    bool node::is_negative()
    {
        if (this->left == nullptr && this->right == nullptr)
            return false;
        else if (this->left == nullptr || this->right == nullptr)
            return true;
        if (this->size * alpha < this->left->size || this->size < this->right->size)
            return true;
        return false;
    }
    void node::push_up()
    {
        this->real_size = !deleted + this->left->real_size + this->right->real_size;
        this->size = 1 + this->left->size + this->right->size;
    }
    void get_subtree(node *u, vector<node *> &nodes)
    {
        if (u == nullptr)
            return;
        get_subtree(u->left, nodes);
        if (!u->deleted)
            nodes.push_back(u);
        get_subtree(u->right, nodes);
        if (u->deleted)
            delete u;
    }
    node *build(int l, int r, vector<node *> nodes)
    {
        if (l >= r)
            return nullptr;
        int mid = (l + r) / 2;
        node *u = nodes[mid];
        u->left = build(l, mid, nodes);
        u->right = build(mid + 1, r, nodes);
        u->push_up();
        return u;
    }
    void rebuild(node *&root)
    {
        vector<node *> nodes;
        get_subtree(root, nodes);
        root = build(0, nodes.size(), nodes);
    }
    void insert(node *&tree, int val)
    {
        if (tree == nullptr)
        {
            tree = new node(val);
            return;
        }
        if (val >= tree->val)
            insert(tree->left, val);
        else
            insert(tree->right, val);
        if (tree->is_negative())
            rebuild(tree);
    }
    void erase(node *&tree, int rank)
    {
        if (!tree->deleted && rank == tree->left->real_size + 1)
        {
            tree->deleted = true;
            tree->size--;
            return;
        }
        if (rank <= tree->left->real_size + !tree->deleted)
            erase(tree->left, rank);
        else
            erase(tree->right, rank - (tree->left->real_size + !tree->deleted));
    }
    int rank(node *u, int val)
    {
        int ans = 1;
        while (u != nullptr)
        {
            if (u->val <= val)
                u = u->left;
            else
            {
                ans += u->left->real_size + !u->deleted;
                u = u->right;
            }
        }
        return ans;
    }
    int find(node *u, int rank)
    {
        while (u != nullptr)
        {
            if (!u->deleted && u->left->real_size + 1 == rank)
                return u->val;
            if (u->left->real_size >= rank)
                u = u->left;
            else
            {
                rank -= u->left->real_size + !u->deleted;
                u = u->right;
            }
        }
        return -1;
    }
}; // namespace tzy
using namespace tzy;
int main()
{
    int n;
    cin >> n;
    node *root = nullptr;
    for (int i = 0; i < n; i++)
    {
        int type, x;
        cin >> type >> x;
        if (type == 1) insert(root, x);
        if (type == 2) erase(root, rank(root, x));
        if (type == 3) cout << rank(root, x) << "\n";
        if (type == 4) cout << find(root, x) << "\n";
        if (type == 5) cout << find(root, rank(root, x) - 1) << "\n";
        if (type == 6) cout << find(root, rank(root, x + 1)) << "\n";
    }
    return 0;
}

```