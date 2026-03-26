
# NERDX-ASSESMENT MARCH 2026

## Day 3

---

## 🔹 Question 1: Printing Server Layers (Level Order)

### Sample Input / Output

| Input                  | Output                                         |
| ---------------------- | ---------------------------------------------- |
| 6 <br> 1 2 3 4 -1 -1 5 | Level 0: 1 <br> Level 1: 2 3 <br> Level 2: 4 5 |

### C++ Code

```cpp id="0o1l9a"
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    queue<int>q;
    q.push(0);
    int level=0;

    while(!q.empty()){
        int sz=q.size();
        cout<<"Level "<<level++<<": ";

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

## 🔹 Question 2: Largest Rectangle in Histogram

```cpp id="c8r1py"
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<int>h(n);
    for(int i=0;i<n;i++) cin>>h[i];

    stack<int>st;
    int maxA=0;

    for(int i=0;i<=n;i++){
        int cur=(i==n?0:h[i]);

        while(!st.empty() && cur<h[st.top()]){
            int ht=h[st.top()];
            st.pop();
            int w=st.empty()?i:i-st.top()-1;
            maxA=max(maxA,ht*w);
        }
        st.push(i);
    }

    cout<<"Max Area: "<<maxA;
}
```

---

## 🔹 Question 3: Top K Frequent Elements

```cpp id="8k3v3m"
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    int k; cin>>k;

    unordered_map<int,int>mp;
    for(int x:a) mp[x]++;

    vector<pair<int,int>>v;
    for(auto &p:mp)
        v.push_back({p.second,p.first});

    sort(v.begin(),v.end(),[](auto &a,auto &b){
        if(a.first==b.first) return a.second>b.second;
        return a.first>b.first;
    });

    vector<int>res;
    for(int i=0;i<k;i++) res.push_back(v[i].second);

    sort(res.begin(),res.end());

    cout<<"Top K Frequent: [";
    for(int i=0;i<res.size();i++){
        cout<<res[i];
        if(i!=res.size()-1) cout<<", ";
    }
    cout<<"]";
}
```

---

## 🔹 Question 4: Group Anagrams

```cpp id="8p5f6c"
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<string>s(n);
    for(int i=0;i<n;i++) cin>>s[i];

    unordered_map<string,vector<string>>mp;

    for(string str:s){
        string key=str;
        sort(key.begin(),key.end());
        mp[key].push_back(str);
    }

    vector<vector<string>>groups;

    for(auto &p:mp){
        sort(p.second.begin(),p.second.end());
        groups.push_back(p.second);
    }

    sort(groups.begin(),groups.end(),[](auto &a,auto &b){
        return a[0]<b[0];
    });

    cout<<"Groups: "<<groups.size()<<"\n";

    for(auto &g:groups){
        for(int i=0;i<g.size();i++){
            cout<<g[i];
            if(i!=g.size()-1) cout<<" ";
        }
        cout<<"\n";
    }
}
```

---

## 🔹 Question 5: Reverse Linked List

```cpp id="kj3u9x"
Node* reverseList(Node* head){
    Node* prev=NULL;
    Node* curr=head;

    while(curr){
        Node* next=curr->next;
        curr->next=prev;
        prev=curr;
        curr=next;
    }
    return prev;
}
```

---

## 🔹 Question 6: Valid Parentheses

```cpp id="6cn8gk"
#include <bits/stdc++.h>
using namespace std;

int main(){
    string s; cin>>s;
    stack<char>st;

    for(char c:s){
        if(c=='('||c=='{'||c=='[')
            st.push(c);
        else{
            if(st.empty()){
                cout<<"Invalid"; return 0;
            }
            char t=st.top(); st.pop();

            if((c==')'&&t!='(')||
               (c=='}'&&t!='{')||
               (c==']'&&t!='[')){
                cout<<"Invalid"; return 0;
            }
        }
    }

    cout<<(st.empty()?"Valid":"Invalid");
}
```

---

## 🔹 Question 7: Queue using Stacks

```cpp id="db5r38"
#include <bits/stdc++.h>
using namespace std;

int main(){
    int q; cin>>q;
    stack<int>s1,s2;

    while(q--){
        int t; cin>>t;

        if(t==1){
            int x; cin>>x;
            s1.push(x);
            cout<<x<<" added\n";
        }
        else if(t==2){
            if(s1.empty()&&s2.empty()){
                cout<<"Queue is empty\n";
                continue;
            }
            if(s2.empty()){
                while(!s1.empty()){
                    s2.push(s1.top());
                    s1.pop();
                }
            }
            cout<<s2.top()<<" removed\n";
            s2.pop();
        }
        else if(t==3){
            if(s1.empty()&&s2.empty()){
                cout<<"Queue is empty\n";
                continue;
            }
            if(s2.empty()){
                while(!s1.empty()){
                    s2.push(s1.top());
                    s1.pop();
                }
            }
            cout<<s2.top()<<" at top\n";
        }
        else{
            cout<<((s1.empty()&&s2.empty())?"True\n":"False\n");
        }
    }
}
```

---

## 🔹 Question 8: Subarray Sum Equals K

```cpp id="3m5g3r"
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

    cout<<"Count: "<<count;
}
```

---

## 🔹 Question 9: Maximum Average Subarray

```cpp id="9o0r5k"
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    int k; cin>>k;

    long long sum=0;
    for(int i=0;i<k;i++) sum+=a[i];

    long long maxSum=sum;
    int start=0;

    for(int i=k;i<n;i++){
        sum+=a[i]-a[i-k];
        if(sum>maxSum){
            maxSum=sum;
            start=i-k+1;
        }
    }

    double avg=(double)maxSum/k;

    cout<<"Max Average: "<<avg<<"\n";
    cout<<"Subarray indices: "<<start<<" "<<start+k-1;
}
```

---

## 🔹 Question 10: Maximum Depth of Binary Tree

```cpp id="3k7x0t"
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int val;
    Node* l,*r;
    Node(int x):val(x),l(NULL),r(NULL){}
};

int main(){
    int n; cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    if(n==0){ cout<<"Depth: 0"; return 0; }

    queue<Node*>q;
    Node* root=new Node(a[0]);
    q.push(root);

    int i=1;
    while(!q.empty() && i<n){
        Node* cur=q.front(); q.pop();

        if(i<n && a[i]!=-1){
            cur->l=new Node(a[i]);
            q.push(cur->l);
        }
        i++;

        if(i<n && a[i]!=-1){
            cur->r=new Node(a[i]);
            q.push(cur->r);
        }
        i++;
    }

    int depth=0;
    queue<Node*>bfs;
    bfs.push(root);

    while(!bfs.empty()){
        int sz=bfs.size();
        depth++;

        while(sz--){
            Node* cur=bfs.front(); bfs.pop();
            if(cur->l) bfs.push(cur->l);
            if(cur->r) bfs.push(cur->r);
        }
    }

    cout<<"Depth: "<<depth;
}
```

---

## ✅ Day 3 Completed


