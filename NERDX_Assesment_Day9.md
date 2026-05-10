# 🚀 NERDX-WEEKLY ASSESSMENT 2026  
## 📅 Day 9 – Complete Solutions (C++)

---

## 🔹 1. Longest Balanced Activity Window
Input:
4
0 1 0 1

Output:
4

```cpp
#include <iostream>
#include <unordered_map>
using namespace std;

int main() {
    int n; cin >> n;
    int arr[n];
    for (int i = 0; i < n; i++) cin >> arr[i];

    unordered_map<int,int> mp;
    mp[0] = -1;

    int sum = 0, maxLen = 0;

    for (int i = 0; i < n; i++) {
        sum += (arr[i] == 1 ? 1 : -1);

        if (mp.count(sum))
            maxLen = max(maxLen, i - mp[sum]);
        else
            mp[sum] = i;
    }

    cout << maxLen;
}
```

---

## 🔹 2. Rotate Linked List
Input:
5
1 2 3 4 5
2

Output:
4 5 1 2 3 

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
};

int main() {
    int n; cin >> n;
    if (n == 0) return 0;

    Node *head=NULL, *tail=NULL;

    for (int i = 0; i < n; i++) {
        int x; cin >> x;
        Node* temp = new Node{x,NULL};
        if (!head) head = tail = temp;
        else {
            tail->next = temp;
            tail = temp;
        }
    }

    int k; cin >> k;
    tail->next = head;

    k = k % n;
    int steps = n - k;

    Node* newTail = head;
    for (int i = 1; i < steps; i++)
        newTail = newTail->next;

    Node* newHead = newTail->next;
    newTail->next = NULL;

    while (newHead) {
        cout << newHead->data << " ";
        newHead = newHead->next;
    }
}
```

---

## 🔹 3. Rotten Oranges
Input:
3 3
2 1 1
1 1 0
0 1 1

Output:
4

```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
    int m,n; cin>>m>>n;
    int grid[m][n];
    queue<pair<int,int>> q;
    int fresh=0;

    for(int i=0;i<m;i++)
        for(int j=0;j<n;j++){
            cin>>grid[i][j];
            if(grid[i][j]==2) q.push({i,j});
            if(grid[i][j]==1) fresh++;
        }

    int time=0;
    int dir[4][2]={{1,0},{-1,0},{0,1},{0,-1}};

    while(!q.empty() && fresh){
        int sz=q.size();
        time++;

        while(sz--){
            auto [x,y]=q.front(); q.pop();
            for(auto &d:dir){
                int nx=x+d[0], ny=y+d[1];
                if(nx>=0&&ny>=0&&nx<m&&ny<n&&grid[nx][ny]==1){
                    grid[nx][ny]=2;
                    fresh--;
                    q.push({nx,ny});
                }
            }
        }
    }

    cout<<(fresh? -1:time);
}
```

---

## 🔹 4. Reverse Words
Input:
the sky is blue

Output:
blue is sky the

```cpp
#include <iostream>
#include <sstream>
using namespace std;

int main() {
    string s;
    getline(cin,s);

    stringstream ss(s);
    string word,res="";

    while(ss>>word){
        if(res=="") res=word;
        else res=word+" "+res;
    }

    cout<<res;
}
```

---

## 🔹 5. Detect Keyword
Input:
hello
ll

Output:
2

```cpp
#include <iostream>
using namespace std;

int main() {
    string h,n;
    cin>>h>>n;

    int pos=h.find(n);
    cout<<(pos==string::npos?-1:pos);
}
```

---

## 🔹 6. Best City
Input:
4
4
0 1 3
1 2 1
2 3 4
0 3 7
4

Output:
3

```cpp
#include <iostream>
using namespace std;

int main(){
    int n,m; cin>>n>>m;
    int dist[101][101];

    for(int i=0;i<n;i++)
        for(int j=0;j<n;j++)
            dist[i][j]=(i==j?0:1e9);

    for(int i=0;i<m;i++){
        int u,v,w; cin>>u>>v>>w;
        dist[u][v]=dist[v][u]=w;
    }

    int t; cin>>t;

    for(int k=0;k<n;k++)
        for(int i=0;i<n;i++)
            for(int j=0;j<n;j++)
                dist[i][j]=min(dist[i][j],dist[i][k]+dist[k][j]);

    int res=-1,minC=1e9;

    for(int i=0;i<n;i++){
        int c=0;
        for(int j=0;j<n;j++)
            if(i!=j && dist[i][j]<=t) c++;

        if(c<=minC){
            minC=c;
            res=i;
        }
    }

    cout<<res;
}
```

---

## 🔹 7. Defang IP
Input:
1.1.1.1

Output:
1[.]1[.]1[.]1

```cpp
#include <iostream>
using namespace std;

int main(){
    string s; cin>>s;
    for(char c:s){
        if(c=='.') cout<<"[.]";
        else cout<<c;
    }
}
```

---

## 🔹 8. Intersection
Input:
5
4 9 5 8 4
5
9 4 9 8 4

Output:
4 8 9 

```cpp
#include <iostream>
#include <set>
using namespace std;

int main(){
    int n; cin>>n;
    set<int> s1;

    for(int i=0;i<n;i++){
        int x; cin>>x;
        s1.insert(x);
    }

    int m; cin>>m;
    set<int> res;

    for(int i=0;i<m;i++){
        int x; cin>>x;
        if(s1.count(x)) res.insert(x);
    }

    for(int x:res) cout<<x<<" ";
}
```

---

## 🔹 9. Path Exists
Input:
2
1
0 1
0 1

Output:
Path Exists

```cpp
#include <iostream>
#include <vector>
using namespace std;

vector<int> adj[200001];
bool vis[200001];

void dfs(int u){
    vis[u]=true;
    for(int v:adj[u])
        if(!vis[v]) dfs(v);
}

int main(){
    int n,m; cin>>n>>m;

    for(int i=0;i<m;i++){
        int u,v; cin>>u>>v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    int s,d; cin>>s>>d;
    dfs(s);

    cout<<(vis[d]?"Path Exists":"No Path Exists");
}
```

---

## 🔹 10. Max Points on Line
Input:
3
1 1
2 2
3 3

Output:
3

```cpp
#include <iostream>
#include <map>
using namespace std;

int main(){
    int n; cin>>n;

    if(n<=2){
        cout<<n;
        return 0;
    }

    pair<int,int> p[n];
    for(int i=0;i<n;i++)
        cin>>p[i].first>>p[i].second;

    int ans=0;

    for(int i=0;i<n;i++){
        map<pair<int,int>,int> mp;
        int same=1;

        for(int j=i+1;j<n;j++){
            int dx=p[j].first-p[i].first;
            int dy=p[j].second-p[i].second;

            if(dx==0 && dy==0){
                same++;
                continue;
            }

            int g=__gcd(dx,dy);
            dx/=g; dy/=g;

            mp[{dx,dy}]++;
            ans=max(ans,mp[{dx,dy}]+same);
        }

        ans=max(ans,same);
    }

    cout<<ans;
}
```

---

# ✅ Done
