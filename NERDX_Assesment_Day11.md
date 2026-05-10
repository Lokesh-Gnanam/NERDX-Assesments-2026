# 🚀 NERDX WEEKLY ASSESSMENT 2026  
## 📅 DAY 11 – FULL SOLUTIONS (C++)

---

## 🔹 1. Delivery Route Mapping (Root to Leaf Paths)

### Input
5
1 2 3 null 5

### Output
1->2->5
1->3

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

struct Node {
    string val;
    Node *left, *right;
    Node(string x): val(x), left(NULL), right(NULL) {}
};

Node* build(vector<string>& arr) {
    if (arr[0] == "null") return NULL;

    Node* root = new Node(arr[0]);
    queue<Node*> q;
    q.push(root);

    int i = 1;
    while (!q.empty() && i < arr.size()) {
        Node* cur = q.front(); q.pop();

        if (arr[i] != "null") {
            cur->left = new Node(arr[i]);
            q.push(cur->left);
        }
        i++;

        if (i < arr.size() && arr[i] != "null") {
            cur->right = new Node(arr[i]);
            q.push(cur->right);
        }
        i++;
    }
    return root;
}

void dfs(Node* root, string path) {
    if (!root) return;

    if (!path.empty()) path += "->";
    path += root->val;

    if (!root->left && !root->right) {
        cout << path << endl;
        return;
    }

    dfs(root->left, path);
    dfs(root->right, path);
}

int main() {
    int n; cin >> n;
    vector<string> arr(n);
    for (int i = 0; i < n; i++) cin >> arr[i];

    Node* root = build(arr);
    dfs(root, "");
}
```

---

## 🔹 2. Efficient Randomized Set

```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
    int q; cin >> q;
    set<long long> st;

    while (q--) {
        string op; cin >> op;

        if (op == "insert") {
            long long x; cin >> x;
            if (st.count(x)) cout << "false
";
            else { st.insert(x); cout << "true
"; }
        }
        else if (op == "remove") {
            long long x; cin >> x;
            if (!st.count(x)) cout << "false
";
            else { st.erase(x); cout << "true
"; }
        }
        else {
            if (st.empty()) cout << -1 << endl;
            else cout << *st.begin() << endl;
        }
    }
}
```

---

## 🔹 3. Network Signal Propagation (Dijkstra)

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int main() {
    int n,m; cin>>n>>m;
    vector<vector<pair<int,int>>> adj(n+1);

    for(int i=0;i<m;i++){
        int u,v,w; cin>>u>>v>>w;
        adj[u].push_back({v,w});
    }

    int k; cin>>k;

    vector<int> dist(n+1,1e9);
    dist[k]=0;

    priority_queue<pair<int,int>,vector<pair<int,int>>,greater<>> pq;
    pq.push({0,k});

    while(!pq.empty()){
        auto [d,u]=pq.top(); pq.pop();

        for(auto &it:adj[u]){
            int v=it.first, w=it.second;

            if(dist[v]>d+w){
                dist[v]=d+w;
                pq.push({dist[v],v});
            }
        }
    }

    int ans=0;
    for(int i=1;i<=n;i++){
        if(dist[i]==1e9){ cout<<-1; return 0; }
        ans=max(ans,dist[i]);
    }
    cout<<ans;
}
```

---

## 🔹 4. Move Zeroes

```cpp
#include <iostream>
using namespace std;

int main() {
    int n; cin >> n;
    int arr[n];

    for (int i = 0; i < n; i++) cin >> arr[i];

    int j = 0;
    for (int i = 0; i < n; i++) {
        if (arr[i] != 0) {
            swap(arr[i], arr[j]);
            j++;
        }
    }

    for (int i = 0; i < n; i++) cout << arr[i] << " ";
}
```

---

## 🔹 5. Trapping Rain Water

```cpp
#include <iostream>
using namespace std;

int main() {
    int n; cin >> n;
    int arr[n];

    for (int i = 0; i < n; i++) cin >> arr[i];

    int l = 0, r = n-1;
    int leftMax = 0, rightMax = 0, water = 0;

    while (l < r) {
        if (arr[l] < arr[r]) {
            if (arr[l] >= leftMax) leftMax = arr[l];
            else water += leftMax - arr[l];
            l++;
        } else {
            if (arr[r] >= rightMax) rightMax = arr[r];
            else water += rightMax - arr[r];
            r--;
        }
    }

    cout << water;
}
```

---

## 🔹 6. Bipartite Graph

```cpp
#include <iostream>
#include <vector>
using namespace std;

vector<int> adj[105];
int color[105];

bool dfs(int u, int c) {
    color[u] = c;
    for (int v : adj[u]) {
        if (color[v] == -1) {
            if (!dfs(v, 1-c)) return false;
        } else if (color[v] == c) return false;
    }
    return true;
}

int main() {
    int n; cin >> n;

    for (int i = 0; i < n; i++) {
        int k; cin >> k;
        while (k--) {
            int x; cin >> x;
            adj[i].push_back(x);
        }
    }

    fill(color, color+n, -1);

    for (int i = 0; i < n; i++) {
        if (color[i] == -1 && !dfs(i,0)) {
            cout << -1 << endl << i;
            return 0;
        }
    }

    int sum=0;
    for(int i=0;i<n;i++) if(color[i]==0){ cout<<i<<" "; sum+=i; }
    cout<<endl;

    for(int i=0;i<n;i++) if(color[i]==1) cout<<i<<" ";
    cout<<endl;

    cout<<sum;
}
```

---

## 🔹 7. Container With Most Water

```cpp
#include <iostream>
using namespace std;

int main() {
    int n; cin >> n;
    int h[n];

    for (int i = 0; i < n; i++) cin >> h[i];

    int l = 0, r = n-1, ans = 0;

    while (l < r) {
        int area = (r - l) * min(h[l], h[r]);
        ans = max(ans, area);

        if (h[l] < h[r]) l++;
        else r--;
    }

    cout << ans;
}
```

---

## 🔹 8. Sum of Left Leaves

```cpp
#include <iostream>
#include <queue>
using namespace std;

struct Node {
    int val;
    Node *left,*right;
    Node(int x):val(x),left(NULL),right(NULL){}
};

Node* build(int arr[], int n) {
    if(arr[0]==-1) return NULL;

    Node* root=new Node(arr[0]);
    queue<Node*> q;
    q.push(root);

    int i=1;
    while(!q.empty() && i<n){
        Node* cur=q.front(); q.pop();

        if(arr[i]!=-1){
            cur->left=new Node(arr[i]);
            q.push(cur->left);
        }
        i++;

        if(i<n && arr[i]!=-1){
            cur->right=new Node(arr[i]);
            q.push(cur->right);
        }
        i++;
    }
    return root;
}

int solve(Node* root,bool isLeft){
    if(!root) return 0;

    if(!root->left && !root->right && isLeft)
        return root->val;

    return solve(root->left,true)+solve(root->right,false);
}

int main(){
    int n; cin>>n;
    int arr[n];

    for(int i=0;i<n;i++) cin>>arr[i];

    Node* root=build(arr,n);
    cout<<solve(root,false);
}
```

---

## 🔹 9. Frequency Tracker

```cpp
#include <iostream>
#include <map>
using namespace std;

int main() {
    int q; cin >> q;
    map<string,int> mp;

    while (q--) {
        string op,key;
        cin >> op;

        if (op=="inc"){
            cin>>key;
            mp[key]++;
        }
        else if(op=="dec"){
            cin>>key;
            if(mp.count(key)){
                mp[key]--;
                if(mp[key]==0) mp.erase(key);
            }
        }
        else if(op=="getMaxKey"){
            if(mp.empty()){ cout<<"EMPTY
"; continue; }

            int mx=0;
            for(auto &p:mp) mx=max(mx,p.second);

            string ans="";
            for(auto &p:mp)
                if(p.second==mx)
                    if(ans==""||p.first<ans)
                        ans=p.first;

            cout<<ans<<endl;
        }
        else{
            if(mp.empty()){ cout<<"EMPTY
"; continue; }

            int mn=1e9;
            for(auto &p:mp) mn=min(mn,p.second);

            string ans="";
            for(auto &p:mp)
                if(p.second==mn)
                    if(ans==""||p.first<ans)
                        ans=p.first;

            cout<<ans<<endl;
        }
    }
}
```

---

## 🔹 10. Minimum Vertices in DAG

```cpp
#include <iostream>
using namespace std;

int main() {
    int n,m;
    cin>>n>>m;

    int indeg[n]={0};

    for(int i=0;i<m;i++){
        int u,v; cin>>u>>v;
        indeg[v]++;
    }

    for(int i=0;i<n;i++)
        if(indeg[i]==0)
            cout<<i<<" ";
}
```

---

# ✅ DONE 💯
