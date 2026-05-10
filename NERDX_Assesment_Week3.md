# WEEK-3 NERDX ASSESSMENT - C++ Solutions

---

# 1. Generate Valid Service Messages

## Sample Input
```text
catsanddog
5
cat cats and sand dog
```

## Sample Output
```text
cat sand dog
cats and dog
```

## C++ Code
```cpp
#include <iostream>
#include <vector>
#include <unordered_set>
using namespace std;

vector<string> result;

void solve(string s, unordered_set<string>& dict,
           string path) {

    if (s.empty()) {
        result.push_back(path.substr(1));
        return;
    }

    for (int i = 1; i <= s.size(); i++) {

        string word = s.substr(0, i);

        if (dict.count(word)) {
            solve(s.substr(i), dict,
                  path + " " + word);
        }
    }
}

int main() {

    string s;
    cin >> s;

    int n;
    cin >> n;

    unordered_set<string> dict;

    for (int i = 0; i < n; i++) {
        string word;
        cin >> word;
        dict.insert(word);
    }

    solve(s, dict, "");

    if (result.empty()) {
        cout << -1;
    }

    else {
        for (string str : result)
            cout << str << endl;
    }

    return 0;
}
```

---

# 2. Course Completion Order Planning

## Sample Input
```text
4
3
1 0
2 1
3 2
```

## Sample Output
```text
0 1 2 3
```

## C++ Code
```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int main() {

    int n, m;
    cin >> n >> m;

    vector<vector<int>> graph(n);
    vector<int> indegree(n, 0);

    for (int i = 0; i < m; i++) {

        int a, b;
        cin >> a >> b;

        graph[b].push_back(a);
        indegree[a]++;
    }

    queue<int> q;

    for (int i = 0; i < n; i++) {
        if (indegree[i] == 0)
            q.push(i);
    }

    vector<int> order;

    while (!q.empty()) {

        int node = q.front();
        q.pop();

        order.push_back(node);

        for (int next : graph[node]) {

            indegree[next]--;

            if (indegree[next] == 0)
                q.push(next);
        }
    }

    if (order.size() != n) {
        cout << "Not Possible - Circular Dependency Found";
    }

    else {

        for (int x : order)
            cout << x << " ";
    }

    return 0;
}
```

---

# 3. Security Camera Placement in Facility Network

## Sample Input
```text
7
0 0 0 0 0 0 0
```

## Sample Output
```text
2
```

## C++ Code
```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

struct Node {

    int val;
    Node *left, *right;

    Node(int v) {
        val = v;
        left = right = NULL;
    }
};

Node* buildTree(vector<int>& arr) {

    if (arr.empty() || arr[0] == -1)
        return NULL;

    Node* root = new Node(arr[0]);

    queue<Node*> q;
    q.push(root);

    int i = 1;

    while (!q.empty() && i < arr.size()) {

        Node* curr = q.front();
        q.pop();

        if (i < arr.size() && arr[i] != -1) {

            curr->left = new Node(arr[i]);
            q.push(curr->left);
        }

        i++;

        if (i < arr.size() && arr[i] != -1) {

            curr->right = new Node(arr[i]);
            q.push(curr->right);
        }

        i++;
    }

    return root;
}

int cameras = 0;

int dfs(Node* root) {

    if (!root)
        return 1;

    int left = dfs(root->left);
    int right = dfs(root->right);

    if (left == 2 || right == 2) {

        cameras++;
        return 0;
    }

    if (left == 0 || right == 0)
        return 1;

    return 2;
}

int minCameraCover(Node* root) {

    if (dfs(root) == 2)
        cameras++;

    return cameras;
}

int main() {

    int n;
    cin >> n;

    vector<int> arr(n);

    for (int i = 0; i < n; i++)
        cin >> arr[i];

    Node* root = buildTree(arr);

    cout << minCameraCover(root);

    return 0;
}
```

---

# 4. Service Request Peak Analyzer

## Sample Input
```text
8
1 3 5 4 7 8 9 2
```

## Sample Output
```text
4
```

## C++ Code
```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {

    int n;
    cin >> n;

    vector<int> arr(n);

    for (int i = 0; i < n; i++)
        cin >> arr[i];

    int ans = 1;
    int count = 1;

    for (int i = 1; i < n; i++) {

        if (arr[i] > arr[i - 1]) {
            count++;
        }

        else {
            count = 1;
        }

        ans = max(ans, count);
    }

    cout << ans;

    return 0;
}
```

---

# 5. Missing Shipment ID in Warehouse Tracking

## Sample Input
```text
3
3 0 1
```

## Sample Output
```text
2
```

## C++ Code
```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {

    int n;
    cin >> n;

    vector<int> arr(n);

    int sum = 0;

    for (int i = 0; i < n; i++) {
        cin >> arr[i];
        sum += arr[i];
    }

    int total = n * (n + 1) / 2;

    cout << total - sum;

    return 0;
}
```

---

# 6. Reverse Service Dependency Structure

## Sample Input
```text
7
1 2 3 4 -1 -1 5
```

## Sample Output
```text
1 3 2 5 4
```

## C++ Code
```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

struct Node {

    int val;
    Node *left, *right;

    Node(int v) {
        val = v;
        left = right = NULL;
    }
};

Node* buildTree(vector<int>& arr) {

    if (arr.empty() || arr[0] == -1)
        return NULL;

    Node* root = new Node(arr[0]);

    queue<Node*> q;
    q.push(root);

    int i = 1;

    while (!q.empty() && i < arr.size()) {

        Node* curr = q.front();
        q.pop();

        if (i < arr.size() && arr[i] != -1) {

            curr->left = new Node(arr[i]);
            q.push(curr->left);
        }

        i++;

        if (i < arr.size() && arr[i] != -1) {

            curr->right = new Node(arr[i]);
            q.push(curr->right);
        }

        i++;
    }

    return root;
}

void invert(Node* root) {

    if (!root)
        return;

    swap(root->left, root->right);

    invert(root->left);
    invert(root->right);
}

void levelOrder(Node* root) {

    if (!root)
        return;

    queue<Node*> q;
    q.push(root);

    while (!q.empty()) {

        Node* curr = q.front();
        q.pop();

        cout << curr->val << " ";

        if (curr->left)
            q.push(curr->left);

        if (curr->right)
            q.push(curr->right);
    }
}

int main() {

    int n;
    cin >> n;

    vector<int> arr(n);

    for (int i = 0; i < n; i++)
        cin >> arr[i];

    Node* root = buildTree(arr);

    invert(root);

    levelOrder(root);

    return 0;
}
```
