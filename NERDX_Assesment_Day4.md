# NERDX-ASSESMENT MARCH 2026

## Day 4

---

## 🔹 Question 1: Insert into BST & Level Order

```cpp id="d4q1f"
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int val;
    Node *l,*r;
    Node(int x):val(x),l(NULL),r(NULL){}
};

Node* insert(Node* root,int val){
    if(!root) return new Node(val);
    if(val<root->val) root->l=insert(root->l,val);
    else root->r=insert(root->r,val);
    return root;
}

int main(){
    int n; cin>>n;
    Node* root=NULL;

    for(int i=0;i<n;i++){
        int x; cin>>x;
        root=insert(root,x);
    }

    int val; cin>>val;
    root=insert(root,val);

    queue<Node*>q;
    q.push(root);

    while(!q.empty()){
        Node* cur=q.front(); q.pop();
        cout<<cur->val;
        if(!q.empty()||cur->l||cur->r) cout<<" ";

        if(cur->l) q.push(cur->l);
        if(cur->r) q.push(cur->r);
    }
}
```

---

## 🔹 Question 2: Flood Fill

```cpp id="d4q2f"
#include <bits/stdc++.h>
using namespace std;

int main(){
    int m,n; cin>>m>>n;
    vector<vector<int>>a(m,vector<int>(n));

    for(int i=0;i<m;i++)
        for(int j=0;j<n;j++)
            cin>>a[i][j];

    int sr,sc,color;
    cin>>sr>>sc>>color;

    int orig=a[sr][sc];
    if(orig==color){
        for(auto &r:a){
            for(int x:r) cout<<x<<" ";
            cout<<"\n";
        }
        return 0;
    }

    queue<pair<int,int>>q;
    q.push({sr,sc});
    a[sr][sc]=color;

    int d[4][2]={{1,0},{-1,0},{0,1},{0,-1}};

    while(!q.empty()){
        auto [r,c]=q.front(); q.pop();

        for(auto &x:d){
            int nr=r+x[0],nc=c+x[1];
            if(nr>=0&&nr<m&&nc>=0&&nc<n&&a[nr][nc]==orig){
                a[nr][nc]=color;
                q.push({nr,nc});
            }
        }
    }

    for(auto &r:a){
        for(int x:r) cout<<x<<" ";
        cout<<"\n";
    }
}
```

---

## 🔹 Question 3: Binary Search

```cpp id="d4q3f"
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    int target; cin>>target;

    int l=0,r=n-1;

    while(l<=r){
        int m=(l+r)/2;
        if(a[m]==target){
            cout<<m; return 0;
        }
        else if(a[m]<target) l=m+1;
        else r=m-1;
    }
    cout<<-1;
}
```

---

## 🔹 Question 4: Course Feasibility

```cpp id="d4q4f"
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n,p; cin>>n>>p;

    vector<vector<int>>adj(n);
    vector<int>ind(n,0);

    while(p--){
        int a,b; cin>>a>>b;
        adj[b].push_back(a);
        ind[a]++;
    }

    queue<int>q;
    for(int i=0;i<n;i++)
        if(ind[i]==0) q.push(i);

    int count=0;

    while(!q.empty()){
        int u=q.front(); q.pop();
        count++;

        for(int v:adj[u]){
            if(--ind[v]==0)
                q.push(v);
        }
    }

    cout<<(count==n?"Possible":"Not Possible");
}
```

---

## 🔹 Question 5: Retrieve Subtree

```cpp id="d4q5f"
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int val;
    Node *l,*r;
    Node(int x):val(x),l(NULL),r(NULL){}
};

Node* build(vector<int>&a){
    if(a.empty()||a[0]==-1) return NULL;

    Node* root=new Node(a[0]);
    queue<Node*>q;
    q.push(root);

    int i=1;
    while(!q.empty() && i<a.size()){
        Node* cur=q.front(); q.pop();

        if(i<a.size() && a[i]!=-1){
            cur->l=new Node(a[i]);
            q.push(cur->l);
        }
        i++;

        if(i<a.size() && a[i]!=-1){
            cur->r=new Node(a[i]);
            q.push(cur->r);
        }
        i++;
    }
    return root;
}

Node* search(Node* root,int val){
    if(!root||root->val==val) return root;
    if(val<root->val) return search(root->l,val);
    return search(root->r,val);
}

int main(){
    int n; cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    int val; cin>>val;

    Node* root=build(a);
    Node* t=search(root,val);

    if(!t){ cout<<"[]"; return 0; }

    queue<Node*>q;
    q.push(t);

    while(!q.empty()){
        Node* cur=q.front(); q.pop();
        cout<<cur->val<<" ";

        if(cur->l) q.push(cur->l);
        if(cur->r) q.push(cur->r);
    }
}
```

---

## 🔹 Question 6: Power Function

```cpp id="d4q6f"
#include <bits/stdc++.h>
using namespace std;

double power(double x,long long n){
    if(n==0) return 1;

    double half=power(x,n/2);

    if(n%2==0) return half*half;
    else return n>0?x*half*half:(half*half)/x;
}

int main(){
    double x; long long n;
    cin>>x>>n;

    cout<<fixed<<setprecision(5)<<power(x,n);
}
```

---

## 🔹 Question 7: Tree Diameter

```cpp id="d4q7f"
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int val;
    Node *l,*r;
    Node(int x):val(x),l(NULL),r(NULL){}
};

int dia=0;

int height(Node* root){
    if(!root) return 0;

    int l=height(root->l);
    int r=height(root->r);

    dia=max(dia,l+r);
    return 1+max(l,r);
}

Node* build(vector<int>&a){
    if(a.empty()||a[0]==-1) return NULL;

    Node* root=new Node(a[0]);
    queue<Node*>q;
    q.push(root);

    int i=1;
    while(!q.empty()&&i<a.size()){
        Node* cur=q.front(); q.pop();

        if(a[i]!=-1){
            cur->l=new Node(a[i]);
            q.push(cur->l);
        }
        i++;

        if(i<a.size()&&a[i]!=-1){
            cur->r=new Node(a[i]);
            q.push(cur->r);
        }
        i++;
    }
    return root;
}

int main(){
    int n; cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    Node* root=build(a);
    height(root);

    cout<<dia;
}
```

---

## 🔹 Question 8: Validate BST

```cpp id="d4q8f"
#include <bits/stdc++.h>
using namespace std;

struct Node{
    long long val;
    Node *l,*r;
    Node(long long x):val(x),l(NULL),r(NULL){}
};

bool check(Node* root,long long mn,long long mx){
    if(!root) return true;
    if(root->val<=mn || root->val>=mx) return false;

    return check(root->l,mn,root->val) &&
           check(root->r,root->val,mx);
}

Node* build(vector<long long>&a){
    if(a.empty()||a[0]==-1) return NULL;

    Node* root=new Node(a[0]);
    queue<Node*>q;
    q.push(root);

    int i=1;
    while(!q.empty()&&i<a.size()){
        Node* cur=q.front(); q.pop();

        if(a[i]!=-1){
            cur->l=new Node(a[i]);
            q.push(cur->l);
        }
        i++;

        if(i<a.size()&&a[i]!=-1){
            cur->r=new Node(a[i]);
            q.push(cur->r);
        }
        i++;
    }
    return root;
}

int main(){
    int n; cin>>n;
    vector<long long>a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    Node* root=build(a);

    cout<<(check(root,LLONG_MIN,LLONG_MAX)?"Valid":"Invalid");
}
```

---

## 🔹 Question 9: LCA

```cpp id="d4q9f"
TreeNode* buildTree(vector<string>& nodes, int i){
    if(i>=nodes.size()||nodes[i]=="null") return NULL;

    TreeNode* root=new TreeNode(stoi(nodes[i]));
    root->left=buildTree(nodes,2*i+1);
    root->right=buildTree(nodes,2*i+2);
    return root;
}

TreeNode* lca(TreeNode* root,int p,int q){
    if(!root||root->val==p||root->val==q) return root;

    TreeNode* l=lca(root->left,p,q);
    TreeNode* r=lca(root->right,p,q);

    if(l&&r) return root;
    return l?l:r;
}
```

---

## 🔹 Question 10: Number of Islands

```cpp id="d4q10f"
#include <bits/stdc++.h>
using namespace std;

void dfs(vector<vector<int>>&g,int i,int j){
    int m=g.size(),n=g[0].size();

    if(i<0||j<0||i>=m||j>=n||g[i][j]==0) return;

    g[i][j]=0;

    dfs(g,i+1,j);
    dfs(g,i-1,j);
    dfs(g,i,j+1);
    dfs(g,i,j-1);
}

int main(){
    int m,n; cin>>m>>n;
    vector<vector<int>>g(m,vector<int>(n));

    for(int i=0;i<m;i++)
        for(int j=0;j<n;j++)
            cin>>g[i][j];

    int count=0;

    for(int i=0;i<m;i++)
        for(int j=0;j<n;j++)
            if(g[i][j]==1){
                dfs(g,i,j);
                count++;
            }

    cout<<count;
}
```

---

## ✅ Day 4 Completed
