# 🚀 NERDX-WEEKLY ASSESSMENT 2026  
## 📅 Day 10 – Coding Questions & Solutions (C++)

---

## 🔹 1. Top K Frequent Hashtags
```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    int n; cin >> n;
    vector<string> arr(n);
    for (int i = 0; i < n; i++) cin >> arr[i];

    int k; cin >> k;

    unordered_map<string, int> freq;
    for (auto &s : arr) freq[s]++;

    vector<pair<string,int>> v(freq.begin(), freq.end());

    sort(v.begin(), v.end(), [](auto &a, auto &b) {
        if (a.second != b.second)
            return a.second > b.second;
        return a.first < b.first;
    });

    for (int i = 0; i < k; i++)
        cout << v[i].first << " ";
}
```

---

## 🔹 2. Live Median Calculation
```cpp
#include <iostream>
#include <queue>
#include <iomanip>
using namespace std;

priority_queue<int> maxH;
priority_queue<int, vector<int>, greater<int>> minH;

int main() {
    int q; cin >> q;

    while (q--) {
        string op; cin >> op;

        if (op == "add") {
            int x; cin >> x;

            if (maxH.empty() || x <= maxH.top()) maxH.push(x);
            else minH.push(x);

            if (maxH.size() > minH.size() + 1) {
                minH.push(maxH.top()); maxH.pop();
            } else if (minH.size() > maxH.size()) {
                maxH.push(minH.top()); minH.pop();
            }
        } else {
            if (maxH.size() == minH.size())
                cout << fixed << setprecision(1)
                     << (maxH.top() + minH.top()) / 2.0 << "\n";
            else
                cout << fixed << setprecision(1)
                     << maxH.top() * 1.0 << "\n";
        }
    }
}
```

---

## 🔹 3. Jewels and Stones
```cpp
#include <iostream>
#include <unordered_set>
using namespace std;

int main() {
    string j, s;
    cin >> j >> s;

    unordered_set<char> st(j.begin(), j.end());

    int count = 0;
    for (char c : s)
        if (st.count(c)) count++;

    cout << count;
}
```

---

## 🔹 4. Stack Using Queue
```cpp
#include <iostream>
#include <queue>
using namespace std;

queue<int> q;

int main() {
    int n; cin >> n;

    while (n--) {
        string op; cin >> op;

        if (op == "push") {
            int x; cin >> x;
            q.push(x);

            int sz = q.size();
            while (sz-- > 1) {
                q.push(q.front());
                q.pop();
            }
        }
        else if (op == "pop") {
            cout << q.front() << endl;
            q.pop();
        }
        else if (op == "top") {
            cout << q.front() << endl;
        }
        else {
            if (q.empty())
                cout << "Stack is Empty\n";
            else
                cout << "Stack is Not Empty\n";
        }
    }
}
```

---

## 🔹 5. Kth Largest Element
```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
    int n; cin >> n;
    int arr[n];

    for (int i = 0; i < n; i++) cin >> arr[i];

    int k; cin >> k;

    priority_queue<int, vector<int>, greater<int>> pq;

    for (int i = 0; i < n; i++) {
        pq.push(arr[i]);
        if (pq.size() > k) pq.pop();
    }

    cout << pq.top();
}
```

---

## 🔹 6. Symmetric Tree + Preorder
```cpp
#include <iostream>
#include <queue>
using namespace std;

struct Node {
    int val;
    Node *left, *right;
    Node(int x): val(x), left(NULL), right(NULL) {}
};

Node* build(int arr[], int n) {
    if (n == 0 || arr[0] == -1) return NULL;

    Node* root = new Node(arr[0]);
    queue<Node*> q;
    q.push(root);

    int i = 1;

    while (!q.empty() && i < n) {
        Node* cur = q.front(); q.pop();

        if (arr[i] != -1) {
            cur->left = new Node(arr[i]);
            q.push(cur->left);
        }
        i++;

        if (i < n && arr[i] != -1) {
            cur->right = new Node(arr[i]);
            q.push(cur->right);
        }
        i++;
    }
    return root;
}

void preorder(Node* root) {
    if (!root) return;
    cout << root->val << " ";
    preorder(root->left);
    preorder(root->right);
}

bool mirror(Node* a, Node* b) {
    if (!a && !b) return true;
    if (!a || !b) return false;

    return a->val == b->val &&
           mirror(a->left, b->right) &&
           mirror(a->right, b->left);
}

int main() {
    int n; cin >> n;

    int arr[n];
    for (int i = 0; i < n; i++) cin >> arr[i];

    Node* root = build(arr, n);

    preorder(root);
    cout << endl;

    if (mirror(root, root))
        cout << "Symmetric";
    else
        cout << "Not Symmetric";
}
```

---

## 🔹 7. Generate Parentheses
```cpp
#include <iostream>
using namespace std;

void solve(int open, int close, string s) {
    if (open == 0 && close == 0) {
        cout << s << " ";
        return;
    }

    if (open > 0)
        solve(open - 1, close, s + "(");

    if (close > open)
        solve(open, close - 1, s + ")");
}

int main() {
    int n; cin >> n;
    solve(n, n, "");
}
```

---

## 🔹 8. Zigzag Traversal
```cpp
#include <iostream>
using namespace std;

int main() {
    int n; cin >> n;
    int arr[n];

    for (int i = 0; i < n; i++) cin >> arr[i];

    int level = 0, i = 0;

    while (i < n) {
        int size = 1 << level;

        if (level % 2 == 0) {
            for (int j = 0; j < size && i < n; j++)
                if (arr[i] != -1) cout << arr[i++] << " ";
                else i++;
        } else {
            int temp[size];
            int k = 0;

            for (int j = 0; j < size && i < n; j++)
                temp[k++] = arr[i++];

            for (int j = k - 1; j >= 0; j--)
                if (temp[j] != -1) cout << temp[j] << " ";
        }

        cout << endl;
        level++;
    }
}
```

---

## 🔹 9. Coin Change Combinations
```cpp
#include <iostream>
#include <vector>
using namespace std;

void solve(vector<int>& arr, int target, int idx, vector<int>& temp) {
    if (target == 0) {
        for (int x : temp) cout << x << " ";
        cout << endl;
        return;
    }

    for (int i = idx; i < arr.size(); i++) {
        if (arr[i] > target) continue;

        temp.push_back(arr[i]);
        solve(arr, target - arr[i], i, temp);
        temp.pop_back();
    }
}

int main() {
    int n; cin >> n;
    vector<int> arr(n);

    for (int i = 0; i < n; i++) cin >> arr[i];

    int target; cin >> target;

    vector<int> temp;
    solve(arr, target, 0, temp);
}
```

---

## 🔹 10. Construct Binary Tree from Traversals
```cpp
#include <iostream>
#include <unordered_map>
#include <queue>
using namespace std;

struct Node {
    int val;
    Node* left;
    Node* right;
    Node(int x): val(x), left(NULL), right(NULL) {}
};

Node* build(int pre[], int in[], int &idx, int l, int r, unordered_map<int,int>& mp) {
    if (l > r) return NULL;

    int val = pre[idx++];
    Node* root = new Node(val);

    int pos = mp[val];

    root->left = build(pre, in, idx, l, pos - 1, mp);
    root->right = build(pre, in, idx, pos + 1, r, mp);

    return root;
}

int main() {
    int n; cin >> n;

    int pre[n], in[n];
    for (int i = 0; i < n; i++) cin >> pre[i];
    for (int i = 0; i < n; i++) cin >> in[i];

    unordered_map<int,int> mp;
    for (int i = 0; i < n; i++) mp[in[i]] = i;

    int idx = 0;
    Node* root = build(pre, in, idx, 0, n - 1, mp);

    queue<Node*> q;
    q.push(root);

    while (!q.empty()) {
        int sz = q.size();

        while (sz--) {
            Node* cur = q.front(); q.pop();
            cout << cur->val << " ";

            if (cur->left) q.push(cur->left);
            if (cur->right) q.push(cur->right);
        }
        cout << endl;
    }
}
```

---

# ✅ Done 💯
