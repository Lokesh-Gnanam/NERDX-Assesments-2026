# 🚀 NERDX WEEKLY ASSESSMENT 2026
# 📅 DAY 17 – COMPLETE SOLUTIONS (C++)

---

# 1. Smart Family Tree Analyzer

## Input
```text
7
1 2 3 4 5 6 7
4 6
```

## Output
```text
COUSINS 2
```

## C++ Code
```cpp
#include <iostream>
#include <queue>
#include <map>
#include <vector>
using namespace std;

struct Node{
    int val;
    Node *left,*right;
    Node(int x){
        val=x;
        left=right=NULL;
    }
};

map<int,int> depth,parentNode;

Node* buildTree(vector<int>& arr){
    if(arr.empty()) return NULL;

    Node* root=new Node(arr[0]);
    queue<Node*> q;
    q.push(root);

    int i=1;

    while(!q.empty() && i<arr.size()){
        Node* cur=q.front();
        q.pop();

        if(arr[i]!=-1){
            cur->left=new Node(arr[i]);
            q.push(cur->left);
        }
        i++;

        if(i<arr.size() && arr[i]!=-1){
            cur->right=new Node(arr[i]);
            q.push(cur->right);
        }
        i++;
    }
    return root;
}

void dfs(Node* root,int d,int par){
    if(!root) return;

    depth[root->val]=d;
    parentNode[root->val]=par;

    dfs(root->left,d+1,root->val);
    dfs(root->right,d+1,root->val);
}

int main(){
    int n;
    cin>>n;

    vector<int> arr(n);
    for(int i=0;i<n;i++) cin>>arr[i];

    int x,y;
    cin>>x>>y;

    Node* root=buildTree(arr);

    dfs(root,0,-1);

    if(parentNode[x]==parentNode[y]){
        cout<<"SIBLINGS "<<depth[x];
    }
    else if(depth[x]==depth[y]){
        cout<<"COUSINS "<<depth[x];
    }
    else{
        cout<<"NOT RELATED "<<depth[x]<<" "<<depth[y];
    }
}
```

---

# 2. Warehouse Load Balancing System

## Input
```text
4
1 2 4 8
10
```

## Output
```text
11
```

## C++ Code
```cpp
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int main() {
    int n;
    cin >> n;

    vector<int> nums(n);

    for(int i=0;i<n;i++)
        cin >> nums[i];

    int target;
    cin >> target;

    sort(nums.begin(), nums.end());

    int ans = nums[0] + nums[1] + nums[2];

    for(int i=0;i<n-2;i++) {
        int l=i+1, r=n-1;

        while(l<r) {
            int sum = nums[i] + nums[l] + nums[r];

            if(abs(target-sum) < abs(target-ans))
                ans = sum;

            if(sum < target)
                l++;
            else
                r--;
        }
    }

    cout << ans;
}
```

---

# 3. Detect the Faulty Network Cable

## Input
```text
3
1 2
1 3
2 3
```

## Output
```text
2 3
```

## C++ Code
```cpp
#include <iostream>
#include <vector>
using namespace std;

vector<int> parent(1005);

int find(int x){
    if(parent[x]==x) return x;
    return parent[x]=find(parent[x]);
}

bool unite(int a,int b){
    int pa=find(a);
    int pb=find(b);

    if(pa==pb) return false;

    parent[pa]=pb;
    return true;
}

int main(){
    int n;
    cin>>n;

    for(int i=0;i<=1000;i++)
        parent[i]=i;

    int u,v;
    vector<int> ans;

    for(int i=0;i<n;i++){
        cin>>u>>v;

        if(!unite(u,v))
            ans={u,v};
    }

    cout<<ans[0]<<" "<<ans[1];
}
```

---

# 4. Build a Fast Student Record System

## Input
```text
6
put 1 10
put 2 20
get 1
get 3
put 2 30
get 2
```

## Output
```text
10
-1
30
```

## C++ Code
```cpp
#include <iostream>
#include <unordered_map>
using namespace std;

int main() {
    int q;
    cin >> q;

    unordered_map<int,int> mp;

    while(q--) {
        string op;
        cin >> op;

        if(op=="put") {
            int k,v;
            cin >> k >> v;
            mp[k]=v;
        }
        else if(op=="get") {
            int k;
            cin >> k;

            if(mp.count(k))
                cout << mp[k] << endl;
            else
                cout << -1 << endl;
        }
        else {
            int k;
            cin >> k;
            mp.erase(k);
        }
    }
}
```

---

# 5. Network Signal Propagation in a Smart City

## Input
```text
4 3 2
2 1 1
2 3 1
3 4 1
```

## Output
```text
2
```

## C++ Code
```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int main() {
    int n,m,k;
    cin >> n >> m >> k;

    vector<vector<pair<int,int>>> adj(n+1);

    for(int i=0;i<m;i++) {
        int u,v,w;
        cin >> u >> v >> w;

        adj[u].push_back({v,w});
    }

    vector<int> dist(n+1,1e9);
    dist[k]=0;

    priority_queue<pair<int,int>,
    vector<pair<int,int>>,
    greater<pair<int,int>>> pq;

    pq.push({0,k});

    while(!pq.empty()) {
        auto cur=pq.top();
        pq.pop();

        int d=cur.first;
        int u=cur.second;

        for(auto &it:adj[u]) {
            int v=it.first;
            int w=it.second;

            if(dist[v]>d+w) {
                dist[v]=d+w;
                pq.push({dist[v],v});
            }
        }
    }

    int ans=0;

    for(int i=1;i<=n;i++) {
        if(dist[i]==1e9) {
            cout << -1;
            return 0;
        }

        ans=max(ans,dist[i]);
    }

    cout << ans;
}
```

---

# 6. Maximize Water Storage Between Vertical Walls

## C++ Code
```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n;
    cin >> n;

    vector<int> h(n);

    for(int i=0;i<n;i++)
        cin >> h[i];

    int l=0,r=n-1;
    int ans=0;

    while(l<r) {
        int area=min(h[l],h[r])*(r-l);

        ans=max(ans,area);

        if(h[l]<h[r])
            l++;
        else
            r--;
    }

    cout << ans;
}
```

---

# 7. Longest Uniform Signal After Replacement

## C++ Code
```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    string s;
    cin >> s;

    int k;
    cin >> k;

    vector<int> freq(26,0);

    int l=0,maxFreq=0,ans=0;

    for(int r=0;r<s.size();r++) {
        freq[s[r]-'A']++;

        maxFreq=max(maxFreq,freq[s[r]-'A']);

        while((r-l+1)-maxFreq>k) {
            freq[s[l]-'A']--;
            l++;
        }

        ans=max(ans,r-l+1);
    }

    cout << ans;
}
```

---

# 8. Drone Coverage Analysis System

## C++ Code
```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n;
    cin >> n;

    vector<int> nums(n);

    for(int i=0;i<n;i++)
        cin >> nums[i];

    int farthest=0;

    for(int i=0;i<n;i++) {
        if(i>farthest) break;

        farthest=max(farthest,i+nums[i]);
    }

    cout << min(farthest,n-1);
}
```

---

# 9. Reverse Level Order Traversal

## C++ Code
```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <algorithm>
using namespace std;

struct Node{
    int val;
    Node *left,*right;
    Node(int x){
        val=x;
        left=right=NULL;
    }
};

Node* build(vector<int>& arr){
    if(arr.empty()) return NULL;

    Node* root=new Node(arr[0]);

    queue<Node*> q;
    q.push(root);

    int i=1;

    while(!q.empty() && i<arr.size()){
        Node* cur=q.front();
        q.pop();

        if(arr[i]!=-1){
            cur->left=new Node(arr[i]);
            q.push(cur->left);
        }
        i++;

        if(i<arr.size() && arr[i]!=-1){
            cur->right=new Node(arr[i]);
            q.push(cur->right);
        }
        i++;
    }

    return root;
}

int main(){
    int n;
    cin>>n;

    vector<int> arr(n);

    for(int i=0;i<n;i++)
        cin>>arr[i];

    Node* root=build(arr);

    queue<Node*> q;
    q.push(root);

    vector<vector<int>> levels;

    while(!q.empty()){
        int sz=q.size();

        vector<int> level;

        for(int i=0;i<sz;i++){
            Node* cur=q.front();
            q.pop();

            level.push_back(cur->val);

            if(cur->left) q.push(cur->left);
            if(cur->right) q.push(cur->right);
        }

        levels.push_back(level);
    }

    reverse(levels.begin(),levels.end());

    for(auto &lvl:levels){
        for(int x:lvl)
            cout<<x<<" ";
        cout<<endl;
    }
}
```

---

# 10. Retail Demand Analyzer System

## C++ Code
```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    int n;
    cin >> n;

    vector<int> nums(n);

    for(int i=0;i<n;i++)
        cin >> nums[i];

    int k;
    cin >> k;

    unordered_map<int,int> freq;

    for(int x:nums)
        freq[x]++;

    vector<pair<int,int>> arr;

    for(auto &p:freq)
        arr.push_back({p.first,p.second});

    sort(arr.begin(),arr.end(),
    [](pair<int,int> a,pair<int,int> b){
        if(a.second==b.second)
            return a.first<b.first;

        return a.second>b.second;
    });

    for(int i=0;i<k;i++)
        cout<<arr[i].first<<" ";
}
```

---

# ✅ DAY 17 COMPLETED 🚀
