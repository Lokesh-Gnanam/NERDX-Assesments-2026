# Day 24 Coding Questions and Answers

# 1. Secure Transaction Template Validator with Matched Pair Analysis

## Problem Statement
A fintech company uses automated transaction approval templates to process secure payment workflows.

A transaction template is valid only when:
- Every opening marker has a corresponding closing marker
- Security markers are closed in the correct order
- No unmatched markers exist

The system must also count total matched pairs.

---

## Input Format
A single string containing:
- ()
- {}
- []

---

## Output Format

If valid:
Transaction Approved  
Matched Pairs: X

If invalid:
Transaction Rejected  
Matched Pairs: X

---

## Sample Input
```text
{([])}
```

## Sample Output
```text
Transaction Approved
Matched Pairs: 3
```

---

## C++ Code

```cpp
#include <iostream>
#include <stack>
using namespace std;

int main() {
    string s;
    cin >> s;

    stack<char> st;
    int pairs = 0;
    bool valid = true;

    for(char ch : s) {
        if(ch == '(' || ch == '{' || ch == '[') {
            st.push(ch);
        }
        else {
            if(st.empty()) {
                valid = false;
                break;
            }

            char top = st.top();

            if((ch == ')' && top == '(') ||
               (ch == '}' && top == '{') ||
               (ch == ']' && top == '[')) {
                st.pop();
                pairs++;
            }
            else {
                valid = false;
                pairs = 0;
                break;
            }
        }
    }

    if(!st.empty()) {
        valid = false;
        pairs = 0;
    }

    if(valid)
        cout << "Transaction Approved\nMatched Pairs: " << pairs;
    else
        cout << "Transaction Rejected\nMatched Pairs: " << pairs;

    return 0;
}
```

---

# 2. Future Weather Improvement Forecast System

## Sample Input
```text
5
40 42 41 50 45
```

## Sample Output
```text
1 2 1 0 0
```

## C++ Code

```cpp
#include <iostream>
#include <vector>
#include <stack>
using namespace std;

int main() {
    int n;
    cin >> n;

    vector<int> temp(n), ans(n, 0);

    for(int i = 0; i < n; i++)
        cin >> temp[i];

    stack<int> st;

    for(int i = 0; i < n; i++) {
        while(!st.empty() && temp[i] > temp[st.top()]) {
            int idx = st.top();
            st.pop();
            ans[idx] = i - idx;
        }
        st.push(i);
    }

    for(int i = 0; i < n; i++)
        cout << ans[i] << " ";

    return 0;
}
```

---

# 3. Maximum Warehouse Storage Allocation System

## Sample Input
```text
6
2 1 5 6 2 3
```

## Sample Output
```text
10
```

## C++ Code

```cpp
#include <iostream>
#include <vector>
#include <stack>
using namespace std;

int main() {
    int n;
    cin >> n;

    vector<long long> h(n);

    for(int i = 0; i < n; i++)
        cin >> h[i];

    stack<int> st;
    long long maxArea = 0;
    int i = 0;

    while(i < n) {
        if(st.empty() || h[st.top()] <= h[i]) {
            st.push(i++);
        }
        else {
            int top = st.top();
            st.pop();

            long long area = h[top] * (st.empty() ? i : i - st.top() - 1);

            maxArea = max(maxArea, area);
        }
    }

    while(!st.empty()) {
        int top = st.top();
        st.pop();

        long long area = h[top] * (st.empty() ? i : i - st.top() - 1);

        maxArea = max(maxArea, area);
    }

    cout << maxArea;

    return 0;
}
```

---

# 4. Future Sales Opportunity Analyzer

## Sample Input
```text
5
1 5 2 8 3
```

## Sample Output
```text
5 8 8 -1 -1
```

## C++ Code

```cpp
#include <iostream>
#include <vector>
#include <stack>
using namespace std;

int main() {
    int n;
    cin >> n;

    vector<long long> arr(n), ans(n, -1);

    for(int i = 0; i < n; i++)
        cin >> arr[i];

    stack<int> st;

    for(int i = 0; i < n; i++) {
        while(!st.empty() && arr[i] > arr[st.top()]) {
            ans[st.top()] = arr[i];
            st.pop();
        }
        st.push(i);
    }

    for(int i = 0; i < n; i++)
        cout << ans[i] << " ";

    return 0;
}
```

---

# 5. Monthly Investment Growth Predictor

## Sample Input
```text
9
```

## Sample Output
```text
34
```

## C++ Code

```cpp
#include <iostream>
using namespace std;

int fib(int n) {
    if(n == 0)
        return 0;

    if(n == 1)
        return 1;

    return fib(n - 1) + fib(n - 2);
}

int main() {
    int n;
    cin >> n;

    cout << fib(n);

    return 0;
}
```

---

# 6. Emergency Parcel Retrieval Management System

## Sample Input
```text
9
ADD 50
ADD 60
ADD 70
REMOVE
ADD 80
TOP
SIZE
REMOVE
TOP
```

## Sample Output
```text
70
80
3
80
60
```

## C++ Code

```cpp
#include <iostream>
#include <queue>
using namespace std;

class StackUsingQueues {
    queue<int> q1, q2;

public:
    void add(int x) {
        q2.push(x);

        while(!q1.empty()) {
            q2.push(q1.front());
            q1.pop();
        }

        swap(q1, q2);
    }

    void remove() {
        if(q1.empty()) {
            cout << "No Parcels Available\n";
            return;
        }

        cout << q1.front() << "\n";
        q1.pop();
    }

    void top() {
        if(q1.empty()) {
            cout << "No Parcels Available\n";
            return;
        }

        cout << q1.front() << "\n";
    }

    void size() {
        cout << q1.size() << "\n";
    }
};

int main() {
    int n;
    cin >> n;

    StackUsingQueues s;

    while(n--) {
        string op;
        cin >> op;

        if(op == "ADD") {
            int x;
            cin >> x;
            s.add(x);
        }
        else if(op == "REMOVE") {
            s.remove();
        }
        else if(op == "TOP") {
            s.top();
        }
        else if(op == "SIZE") {
            s.size();
        }
    }

    return 0;
}
```

---

# 7. SQL Query – Students Scoring Above 75

```sql
SELECT UPPER(name) AS name, marks
FROM Student
WHERE marks > 75
ORDER BY name;
```

---

# 8. Malware Outbreak Containment in a Smart Data Center

## Sample Input
```text
3 3
2 1 1
1 1 0
0 1 1
```

## Sample Output
```text
4
```

## C++ Code

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int main() {
    int r, c;
    cin >> r >> c;

    vector<vector<int>> grid(r, vector<int>(c));
    queue<pair<pair<int,int>, int>> q;

    int fresh = 0;

    for(int i = 0; i < r; i++) {
        for(int j = 0; j < c; j++) {
            cin >> grid[i][j];

            if(grid[i][j] == 2)
                q.push({{i, j}, 0});

            else if(grid[i][j] == 1)
                fresh++;
        }
    }

    int time = 0;

    int dx[] = {-1, 1, 0, 0};
    int dy[] = {0, 0, -1, 1};

    while(!q.empty()) {
        auto current = q.front();
        q.pop();

        int x = current.first.first;
        int y = current.first.second;
        int t = current.second;

        time = max(time, t);

        for(int i = 0; i < 4; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];

            if(nx >= 0 && ny >= 0 && nx < r && ny < c && grid[nx][ny] == 1) {
                grid[nx][ny] = 2;
                fresh--;
                q.push({{nx, ny}, t + 1});
            }
        }
    }

    if(fresh > 0)
        cout << -1;
    else
        cout << time;

    return 0;
}
```

---

# 9. Secure Access Token Combination Generator

## Sample Input
```text
4
```

## Sample Output
```text
(((())))
((()()))
((())())
((()))()
(()(()))
(()()())
(()())()
(())(())
(())()()
()((()))
()(()())
()(())()
()()(())
()()()()
```

## C++ Code

```cpp
#include <iostream>
using namespace std;

void generate(int open, int close, string s, int n) {

    if(s.length() == 2 * n) {
        cout << s << endl;
        return;
    }

    if(open < n)
        generate(open + 1, close, s + "(", n);

    if(close < open)
        generate(open, close + 1, s + ")", n);
}

int main() {
    int n;
    cin >> n;

    generate(0, 0, "", n);

    return 0;
}
```

---

# 10. SQL Query – Employees from New York and San Francisco

```sql
SELECT name, city
FROM Employee
WHERE city IN ('New York', 'San Francisco');
```
