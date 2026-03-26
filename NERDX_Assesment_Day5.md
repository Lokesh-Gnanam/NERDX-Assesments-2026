# NERDX-ASSESMENT MARCH 2026

## Day 5

---

## 🔹 Question 1: Longest Increasing Subsequence

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    vector<int>dp;

    for(int x:a){
        auto it=lower_bound(dp.begin(),dp.end(),x);
        if(it==dp.end()) dp.push_back(x);
        else *it=x;
    }

    cout<<"Length of Longest Increasing Subsequence: "<<dp.size();
}
```

---

## 🔹 Question 2: Merge Intervals

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<pair<int,int>>v(n);

    for(int i=0;i<n;i++)
        cin>>v[i].first>>v[i].second;

    sort(v.begin(),v.end());

    vector<pair<int,int>>res;

    for(auto &p:v){
        if(res.empty() || res.back().second < p.first)
            res.push_back(p);
        else
            res.back().second = max(res.back().second, p.second);
    }

    for(auto &p:res)
        cout<<"["<<p.first<<", "<<p.second<<"]\n";
}
```

---

## 🔹 Question 3: Assign Cookies (Greedy)

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<int>g(n);
    for(int i=0;i<n;i++) cin>>g[i];

    int m; cin>>m;
    vector<int>s(m);
    for(int i=0;i<m;i++) cin>>s[i];

    sort(g.begin(),g.end());
    sort(s.begin(),s.end());

    int i=0,j=0,count=0;

    while(i<n && j<m){
        if(s[j]>=g[i]){
            count++; i++; j++;
        } else j++;
    }

    cout<<count;
}
```

---

## 🔹 Question 4: Sort Array

```cpp
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

## 🔹 Question 5: Sorted Array → Balanced BST

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int val;
    Node *l,*r;
    Node(int x):val(x),l(NULL),r(NULL){}
};

Node* build(vector<int>&a,int l,int r){
    if(l>r) return NULL;

    int m=(l+r)/2;
    Node* root=new Node(a[m]);

    root->l=build(a,l,m-1);
    root->r=build(a,m+1,r);

    return root;
}

void print(Node* root){
    if(!root) return;

    cout<<root->val<<" -> (left: ";
    if(root->l) cout<<root->l->val;
    else cout<<"null";

    cout<<", right: ";
    if(root->r) cout<<root->r->val;
    else cout<<"null";

    cout<<")\n";

    print(root->l);
    print(root->r);
}

int main(){
    int n; cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    Node* root=build(a,0,n-1);
    print(root);
}
```

---

## 🔹 Question 6: Middle of Linked List

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int val;
    Node* next;
    Node(int x):val(x),next(NULL){}
};

int main(){
    int n; cin>>n;

    Node *head=NULL,*tail=NULL;

    for(int i=0;i<n;i++){
        int x; cin>>x;
        Node* node=new Node(x);

        if(!head) head=tail=node;
        else{
            tail->next=node;
            tail=node;
        }
    }

    Node *slow=head,*fast=head;

    while(fast && fast->next){
        slow=slow->next;
        fast=fast->next->next;
    }

    while(slow){
        cout<<slow->val;
        slow=slow->next;
        if(slow) cout<<" -> ";
    }
}
```

---

## 🔹 Question 7: Permutations (FIXED ORDER)

```cpp
#include <bits/stdc++.h>
using namespace std;

void backtrack(vector<int>&nums, vector<bool>&used, vector<int>&curr){
    if(curr.size()==nums.size()){
        cout<<"[";
        for(int i=0;i<curr.size();i++){
            cout<<curr[i];
            if(i!=curr.size()-1) cout<<", ";
        }
        cout<<"]\n";
        return;
    }

    for(int i=0;i<nums.size();i++){
        if(used[i]) continue;

        used[i]=true;
        curr.push_back(nums[i]);

        backtrack(nums,used,curr);

        curr.pop_back();
        used[i]=false;
    }
}

int main(){
    int n; cin>>n;
    vector<int>nums(n);

    for(int i=0;i<n;i++) cin>>nums[i];

    sort(nums.begin(),nums.end()); // IMPORTANT

    cout<<"All possible permutations:\n";

    vector<bool>used(n,false);
    vector<int>curr;

    backtrack(nums,used,curr);
}
```

---

## 🔹 Question 8: Climbing Stairs

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;

    if(n<=2){
        cout<<"Total number of ways to climb "<<n<<" steps: "<<n;
        return 0;
    }

    int a=1,b=2,c;

    for(int i=3;i<=n;i++){
        c=a+b;
        a=b;
        b=c;
    }

    cout<<"Total number of ways to climb "<<n<<" steps: "<<b;
}
```

---

## 🔹 Question 9: Subsets

```cpp
#include <bits/stdc++.h>
using namespace std;

void solve(int i, vector<int>&nums, vector<int>&curr){
    if(i==nums.size()){
        cout<<"[";
        for(int j=0;j<curr.size();j++){
            cout<<curr[j];
            if(j!=curr.size()-1) cout<<", ";
        }
        cout<<"]\n";
        return;
    }

    curr.push_back(nums[i]);
    solve(i+1,nums,curr);

    curr.pop_back();
    solve(i+1,nums,curr);
}

int main(){
    int n; cin>>n;
    vector<int>nums(n);

    for(int i=0;i<n;i++) cin>>nums[i];

    cout<<"All possible subsets:\n";

    vector<int>curr;
    solve(0,nums,curr);
}
```

---

## 🔹 Question 10: Sliding Window Maximum

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    int k; cin>>k;

    deque<int>dq;

    cout<<"Maximum values in each sliding window: ";

    for(int i=0;i<n;i++){
        while(!dq.empty() && dq.front()<=i-k)
            dq.pop_front();

        while(!dq.empty() && a[dq.back()]<=a[i])
            dq.pop_back();

        dq.push_back(i);

        if(i>=k-1)
            cout<<a[dq.front()]<<" ";
    }
}
```

---

## ✅ Day 5 Completed
