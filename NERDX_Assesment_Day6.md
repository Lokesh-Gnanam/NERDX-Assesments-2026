# NERDX-ASSESMENT MARCH 2026

## Day 6

---

## 🔹 Question 1: Detect Duplicate Product IDs

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    unordered_set<long long>s;

    bool dup=false;

    for(int i=0;i<n;i++){
        long long x; cin>>x;
        if(s.count(x)) dup=true;
        s.insert(x);
    }

    cout<<(dup?"Duplicate Found":"No Duplicates");
}
```

---

## 🔹 Question 2: Level Order Traversal (Line by Line)

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    queue<int>q;
    q.push(0);

    while(!q.empty()){
        int sz=q.size();

        while(sz--){
            int i=q.front(); q.pop();

            if(a[i]==-1) continue;

            cout<<a[i]<<" ";

            if(2*i+1<n) q.push(2*i+1);
            if(2*i+2<n) q.push(2*i+2);
        }
        cout<<"\n";
    }
}
```

---

## 🔹 Question 3: N-Queens

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<vector<string>>res;

bool isSafe(vector<string>&b,int r,int c,int n){
    for(int i=0;i<r;i++)
        if(b[i][c]=='T') return false;

    for(int i=r-1,j=c-1;i>=0&&j>=0;i--,j--)
        if(b[i][j]=='T') return false;

    for(int i=r-1,j=c+1;i>=0&&j<n;i--,j++)
        if(b[i][j]=='T') return false;

    return true;
}

void solve(int r,int n,vector<string>&b){
    if(r==n){
        res.push_back(b);
        return;
    }

    for(int c=0;c<n;c++){
        if(isSafe(b,r,c,n)){
            b[r][c]='T';
            solve(r+1,n,b);
            b[r][c]='.';
        }
    }
}

int main(){
    int n; cin>>n;

    vector<string>b(n,string(n,'.'));
    solve(0,n,b);

    if(res.empty()){
        cout<<"No valid configurations";
        return 0;
    }

    for(auto &grid:res){
        for(auto &row:grid)
            cout<<row<<"\n";
        cout<<"\n";
    }
}
```

---

## 🔹 Question 4: Coin Change

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<int>c(n);
    for(int i=0;i<n;i++) cin>>c[i];

    int amount; cin>>amount;

    vector<int>dp(amount+1,1e9);
    dp[0]=0;

    for(int coin:c){
        for(int i=coin;i<=amount;i++){
            dp[i]=min(dp[i],dp[i-coin]+1);
        }
    }

    cout<<(dp[amount]==1e9?-1:dp[amount]);
}
```

---

## 🔹 Question 5: Subarray Sum Count

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    int k; cin>>k;

    unordered_map<int,int>mp;
    mp[0]=1;

    int sum=0,count=0;

    for(int x:a){
        sum+=x;
        if(mp.count(sum-k))
            count+=mp[sum-k];
        mp[sum]++;
    }

    cout<<count;
}
```

---

## 🔹 Question 6: Edit Distance (Word-based)

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    string s1,s2;
    getline(cin,s1);
    getline(cin,s2);

    stringstream ss1(s1), ss2(s2);
    vector<string>a,b;
    string w;

    while(ss1>>w) a.push_back(w);
    while(ss2>>w) b.push_back(w);

    int n=a.size(), m=b.size();

    vector<vector<int>>dp(n+1,vector<int>(m+1));

    for(int i=0;i<=n;i++) dp[i][0]=i;
    for(int j=0;j<=m;j++) dp[0][j]=j;

    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            if(a[i-1]==b[j-1])
                dp[i][j]=dp[i-1][j-1];
            else
                dp[i][j]=1+min({dp[i-1][j],dp[i][j-1],dp[i-1][j-1]});
        }
    }

    cout<<dp[n][m];
}
```

---

## 🔹 Question 7: Jump Game

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<int>a(n);

    for(int i=0;i<n;i++) cin>>a[i];

    int reach=0;

    for(int i=0;i<n;i++){
        if(i>reach){
            cout<<"No";
            return 0;
        }
        reach=max(reach,i+a[i]);
    }

    cout<<"Yes";
}
```

---

## 🔹 Question 8: Cheapest Flight with K Stops

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n,m; cin>>n>>m;

    vector<vector<pair<int,int>>>adj(n);

    for(int i=0;i<m;i++){
        int u,v,w; cin>>u>>v>>w;
        adj[u].push_back({v,w});
    }

    int src,dst,k; cin>>src>>dst>>k;

    queue<tuple<int,int,int>>q;
    q.push({src,0,0});

    vector<int>cost(n,1e9);
    cost[src]=0;

    int ans=1e9;

    while(!q.empty()){
        auto [u,c,st]=q.front(); q.pop();

        if(st>k) continue;

        for(auto &[v,w]:adj[u]){
            if(c+w < cost[v] && st<=k){
                cost[v]=c+w;
                q.push({v,c+w,st+1});
            }
        }
    }

    cout<<(cost[dst]==1e9?-1:cost[dst]);
}
```

---

## 🔹 Question 9: Tree Traversals (Planning / Editing / Release)

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int val;
    int l,r;
};

vector<Node>nodes;

void preorder(int i){
    if(i==-1) return;
    cout<<nodes[i].val<<" ";
    preorder(nodes[i].l);
    preorder(nodes[i].r);
}

void inorder(int i){
    if(i==-1) return;
    inorder(nodes[i].l);
    cout<<nodes[i].val<<" ";
    inorder(nodes[i].r);
}

void postorder(int i){
    if(i==-1) return;
    postorder(nodes[i].l);
    postorder(nodes[i].r);
    cout<<nodes[i].val<<" ";
}

int main(){
    int n; cin>>n;
    nodes.resize(n);

    for(int i=0;i<n;i++){
        cin>>nodes[i].val>>nodes[i].l>>nodes[i].r;
    }

    cout<<"Planning: ";
    preorder(0);
    cout<<"\n";

    cout<<"Editing: ";
    inorder(0);
    cout<<"\n";

    cout<<"Release: ";
    postorder(0);
}
```

---

## 🔹 Question 10: Longest Common Subsequence

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    string a,b;
    cin>>a>>b;

    int n=a.size(),m=b.size();

    vector<vector<int>>dp(n+1,vector<int>(m+1,0));

    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            if(a[i-1]==b[j-1])
                dp[i][j]=1+dp[i-1][j-1];
            else
                dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
        }
    }

    cout<<dp[n][m];
}
```

---

## ✅ Day 6 Completed
