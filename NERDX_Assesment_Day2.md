# NERDX-ASSESMENT MARCH 2026

## Day 2

---

## 🔹 Question 1: Merge Multiple Sorted Data Streams

### Problem

Merge k sorted lists into one sorted list.

### Sample Input / Output

| Input                                  | Output          |
| -------------------------------------- | --------------- |
| 3 <br> 3 1 4 5 <br> 3 1 3 4 <br> 2 2 6 | 1 1 2 3 4 4 5 6 |

### C++ Code

```cpp id="61y9s3"
#include <bits/stdc++.h>
using namespace std;

int main(){
    int k; cin>>k;
    vector<int> all;

    for(int i=0;i<k;i++){
        int n; cin>>n;
        for(int j=0;j<n;j++){
            int x; cin>>x;
            all.push_back(x);
        }
    }

    sort(all.begin(),all.end());

    for(int i=0;i<all.size();i++){
        cout<<all[i];
        if(i!=all.size()-1) cout<<" ";
    }
}
```

---

## 🔹 Question 2: LRU Cache

### C++ Code

```cpp id="mfglj5"
#include <bits/stdc++.h>
using namespace std;

class LRUCache {
    int cap;
    list<pair<int,int>> dll;
    unordered_map<int, list<pair<int,int>>::iterator> mp;

public:
    LRUCache(int capacity) { cap = capacity; }

    int get(int key) {
        if (!mp.count(key)) return -1;

        auto it = mp[key];
        int val = it->second;

        dll.erase(it);
        dll.push_front({key,val});
        mp[key] = dll.begin();

        return val;
    }

    void put(int key,int value){
        if(mp.count(key)){
            dll.erase(mp[key]);
        }
        else if(dll.size()==cap){
            auto last=dll.back();
            mp.erase(last.first);
            dll.pop_back();
        }

        dll.push_front({key,value});
        mp[key]=dll.begin();
    }
};

int main(){
    int cap,q; cin>>cap>>q;
    LRUCache cache(cap);

    while(q--){
        string op; cin>>op;
        if(op=="GET"){
            int k; cin>>k;
            cout<<cache.get(k)<<"\n";
        } else {
            int k,v; cin>>k>>v;
            cache.put(k,v);
        }
    }
}
```

---

## 🔹 Question 3: Longest Consecutive Sequence

```cpp id="6r8c53"
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    unordered_set<int>s;

    for(int i=0;i<n;i++){
        int x; cin>>x;
        s.insert(x);
    }

    int ans=0;

    for(int x:s){
        if(!s.count(x-1)){
            int cur=x,len=1;
            while(s.count(cur+1)){
                cur++; len++;
            }
            ans=max(ans,len);
        }
    }

    cout<<ans;
}
```

---

## 🔹 Question 4: Find Town Judge

```cpp id="gnt1mc"
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n,m; cin>>n>>m;
    vector<int> in(n+1,0),out(n+1,0);

    while(m--){
        int a,b; cin>>a>>b;
        out[a]++; in[b]++;
    }

    for(int i=1;i<=n;i++){
        if(in[i]==n-1 && out[i]==0){
            cout<<i; return 0;
        }
    }
    cout<<-1;
}
```

---

## 🔹 Question 5: Merge Sort

```cpp id="i7ssvh"
#include <bits/stdc++.h>
using namespace std;

void mergeSort(vector<int>&a,int l,int r){
    if(l>=r) return;
    int m=(l+r)/2;

    mergeSort(a,l,m);
    mergeSort(a,m+1,r);

    vector<int>temp;
    int i=l,j=m+1;

    while(i<=m && j<=r){
        if(a[i]<=a[j]) temp.push_back(a[i++]);
        else temp.push_back(a[j++]);
    }

    while(i<=m) temp.push_back(a[i++]);
    while(j<=r) temp.push_back(a[j++]);

    for(int k=0;k<temp.size();k++)
        a[l+k]=temp[k];
}

int main(){
    int n; cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    mergeSort(a,0,n-1);

    for(int x:a) cout<<x<<" ";
}
```

---

## 🔹 Question 6: Two Sum

```cpp id="qj5pda"
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n,target; cin>>n;
    vector<int>a(n);

    for(int i=0;i<n;i++) cin>>a[i];
    cin>>target;

    unordered_map<int,int>mp;

    for(int i=0;i<n;i++){
        int d=target-a[i];
        if(mp.count(d)){
            cout<<mp[d]<<" "<<i;
            return 0;
        }
        mp[a[i]]=i;
    }

    cout<<"-1 -1";
}
```

---

## 🔹 Question 7: Next Greater Element

```cpp id="4y0xqe"
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    unordered_map<int,int>nge;
    stack<int>st;

    for(int i=n-1;i>=0;i--){
        while(!st.empty() && st.top()<=a[i]) st.pop();
        nge[a[i]] = st.empty()?-1:st.top();
        st.push(a[i]);
    }

    int m; cin>>m;
    while(m--){
        int x; cin>>x;
        cout<<nge[x]<<" ";
    }
}
```

---

## 🔹 Question 8: Detect Cycle in Linked List

```cpp id="p4t7l2"
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int val;
    Node* next;
    Node(int x):val(x),next(NULL){}
};

Node* detect(Node* head){
    Node *slow=head,*fast=head;

    while(fast && fast->next){
        slow=slow->next;
        fast=fast->next->next;

        if(slow==fast){
            slow=head;
            while(slow!=fast){
                slow=slow->next;
                fast=fast->next;
            }
            return slow;
        }
    }
    return NULL;
}

int main(){
    int n; cin>>n;
    vector<Node*>nodes(n);

    for(int i=0;i<n;i++){
        int x; cin>>x;
        nodes[i]=new Node(x);
    }

    for(int i=0;i<n-1;i++)
        nodes[i]->next=nodes[i+1];

    int pos; cin>>pos;
    if(pos!=-1)
        nodes[n-1]->next=nodes[pos];

    Node* res=detect(nodes[0]);

    if(res) cout<<res->val;
    else cout<<-1;
}
```

---

## 🔹 Question 9: Sort Array

```cpp id="3krru5"
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<int>a(n);

    for(int i=0;i<n;i++) cin>>a[i];

    sort(a.begin(),a.end());

    for(int x:a) cout<<x<<" ";
}
```

---

## 🔹 Question 10: Course Schedule (Topological Sort)

```cpp id="rqz32k"
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n,m; cin>>n>>m;

    vector<vector<int>>adj(n);
    vector<int>ind(n,0);

    while(m--){
        int a,b; cin>>a>>b;
        adj[b].push_back(a);
        ind[a]++;
    }

    queue<int>q;
    for(int i=0;i<n;i++)
        if(ind[i]==0) q.push(i);

    vector<int>res;

    while(!q.empty()){
        int u=q.front(); q.pop();
        res.push_back(u);

        for(int v:adj[u]){
            if(--ind[v]==0) q.push(v);
        }
    }

    if(res.size()!=n) cout<<-1;
    else for(int x:res) cout<<x<<" ";
}
```

---

## ✅ Day 2 Completed
