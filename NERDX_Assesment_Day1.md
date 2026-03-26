# NERDX-ASSESMENT MARCH 2026

## Day 1

---

## 🔹 Question 1: Locate a Product Code in a Rotated Shelf Index

### Problem

Given a rotated sorted array, find the index of a target element. If not found, return -1.

### Sample Input / Output

| Input                       | Output |
| --------------------------- | ------ |
| 7 <br> 4 5 6 7 0 1 2 <br> 0 | 4      |
| 7 <br> 4 5 6 7 0 1 2 <br> 3 | -1     |

### C++ Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, target;
    cin >> n;
    vector<int> a(n);
    for(int i=0;i<n;i++) cin>>a[i];
    cin >> target;

    int l=0,r=n-1;
    while(l<=r){
        int m=(l+r)/2;
        if(a[m]==target){ cout<<m; return 0; }

        if(a[l]<=a[m]){
            if(target>=a[l] && target<a[m]) r=m-1;
            else l=m+1;
        } else {
            if(target>a[m] && target<=a[r]) l=m+1;
            else r=m-1;
        }
    }
    cout<<-1;
}
```

---

## 🔹 Question 2: Reverse the Maintenance Code Sequence

### Problem

Reverse a character array in-place.

### Sample Input / Output

| Input            | Output    |
| ---------------- | --------- |
| 5 <br> h e l l o | o l l e h |

### C++ Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<char> s(n);
    for(int i=0;i<n;i++) cin>>s[i];

    reverse(s.begin(), s.end());

    for(char c:s) cout<<c<<" ";
}
```

---

## 🔹 Question 3: Merge Two Sorted Patient Queues

### Problem

Merge two sorted arrays.

### Sample Input / Output

| Input                       | Output                    |
| --------------------------- | ------------------------- |
| 3 <br>1 2 4 <br>3 <br>1 3 4 | 1 1 2 3 4 4 <br> Count: 6 |

### C++ Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n,m; cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    cin>>m;
    vector<int>b(m);
    for(int i=0;i<m;i++) cin>>b[i];

    vector<int>res;
    merge(a.begin(),a.end(),b.begin(),b.end(),back_inserter(res));

    for(int x:res) cout<<x<<" ";
    cout<<"\nCount: "<<res.size();
}
```

---

## 🔹 Question 4: Reverse Tokens in Groups

### Problem

Reverse array in groups of size k.

### C++ Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n,k; cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];
    cin>>k;

    for(int i=0;i<n;i+=k){
        if(i+k<=n)
            reverse(a.begin()+i,a.begin()+i+k);
    }

    for(int x:a) cout<<x<<" ";
    cout<<"\nCount: "<<n;
}
```

---

## 🔹 Question 5: Spiral Matrix Traversal

### C++ Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int m,n; cin>>m>>n;
    vector<vector<int>> a(m,vector<int>(n));

    for(int i=0;i<m;i++)
        for(int j=0;j<n;j++)
            cin>>a[i][j];

    int top=0,bot=m-1,l=0,r=n-1;

    while(top<=bot && l<=r){
        for(int i=l;i<=r;i++) cout<<a[top][i]<<" ";
        top++;
        for(int i=top;i<=bot;i++) cout<<a[i][r]<<" ";
        r--;
        if(top<=bot){
            for(int i=r;i>=l;i--) cout<<a[bot][i]<<" ";
            bot--;
        }
        if(l<=r){
            for(int i=bot;i>=top;i--) cout<<a[i][l]<<" ";
            l++;
        }
    }
}
```

---

## 🔹 Question 6: Duplicate Array

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    for(int i=0;i<2*n;i++)
        cout<<a[i%n]<<" ";
}
```

---

## 🔹 Question 7: Range Sum Query

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    vector<int>a(n),pre(n+1,0);

    for(int i=0;i<n;i++){
        cin>>a[i];
        pre[i+1]=pre[i]+a[i];
    }

    int q; cin>>q;
    while(q--){
        int l,r; cin>>l>>r;
        cout<<pre[r+1]-pre[l]<<"\n";
    }
}
```

---

## 🔹 Question 8: Fibonacci

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n; cin>>n;
    int a=0,b=1;

    for(int i=2;i<=n;i++){
        int c=a+b;
        a=b; b=c;
    }

    cout<<(n==0?0:b);
}
```

---

## 🔹 Question 9: Valid Anagram

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    string s,t;
    cin>>s>>t;

    vector<int>a(26,0),b(26,0);
    for(char c:s) a[c-'a']++;
    for(char c:t) b[c-'a']++;

    cout<<(a==b?"Valid Anagram\n":"Invalid Anagram\n");

    cout<<"S: ";
    for(int i=0;i<26;i++)
        if(a[i]) cout<<char(i+'a')<<":"<<a[i]<<" ";

    cout<<"\nT: ";
    for(int i=0;i<26;i++)
        if(b[i]) cout<<char(i+'a')<<":"<<b[i]<<" ";
}
```

---

## 🔹 Question 10: Search Insert Position

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n,target;
    cin>>n;

    vector<int>a(n);
    for(int i=0;i<n;i++) cin>>a[i];
    cin>>target;

    int l=0,r=n-1,pos=n;
    bool found=false;

    while(l<=r){
        int m=(l+r)/2;
        if(a[m]==target){ pos=m; found=true; break;}
        else if(a[m]<target) l=m+1;
        else { pos=m; r=m-1; }
    }

    cout<<"Position: "<<pos<<"\n";
    cout<<"Status: "<<(found?"Present":"Insert");
}
```

---

## ✅ Day 1 Completed
