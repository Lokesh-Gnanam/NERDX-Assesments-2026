# Day 25 Coding Questions and Answers

## 1. Automated Workflow Pattern Generator

### Sample Input
```text
2
```

### Sample Output
```text
(())
()()
```

### C++ Code

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

## 2. Secure Transaction Flow Validation

### Sample Input
```text
([]){}
```

### Sample Output
```text
3
```

### C++ Code

```cpp
#include <iostream>
#include <stack>
using namespace std;

int main() {

    string s;
    cin >> s;

    stack<char> st;
    int count = 0;

    for(char ch : s) {

        if(ch == '(' || ch == '{' || ch == '[') {
            st.push(ch);
        }
        else {

            if(st.empty()) {
                cout << -1;
                return 0;
            }

            char top = st.top();

            if((ch == ')' && top == '(') ||
               (ch == '}' && top == '{') ||
               (ch == ']' && top == '[')) {

                st.pop();
                count++;
            }
            else {
                cout << -1;
                return 0;
            }
        }
    }

    if(!st.empty())
        cout << -1;
    else
        cout << count;

    return 0;
}
```

---

## 3. Weather Alert Waiting Period

### Sample Input
```text
8
73 74 75 71 69 72 76 73
```

### Sample Output
```text
1 1 4 2 1 1 0 0
```

### C++ Code

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

## 4. Food Storage Contamination Tracker

### Sample Input
```text
3 3
2 1 1
1 1 0
0 1 1
```

### Sample Output
```text
4
```

### C++ Code

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

        auto cur = q.front();
        q.pop();

        int x = cur.first.first;
        int y = cur.first.second;
        int t = cur.second;

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

## 5. Renewable Energy Growth Forecast

### Sample Input
```text
7
```

### Sample Output
```text
13
```

### C++ Code

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

## 6. Warehouse Shipment Upgrade Analyzer

### Sample Input
```text
4
4 5 2 10
```

### Sample Output
```text
5 10 10 -1
```

### C++ Code

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

## 7. Warehouse Storage Optimization

### Sample Input
```text
6
2 1 5 6 2 3
```

### Sample Output
```text
10
```

### C++ Code

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

## 8. Student Course Enrollment Data Optimization

### SQL Query

```sql
CREATE TABLE Students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(100)
);

CREATE TABLE Courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100)
);

CREATE TABLE Enrollments (
    student_id INT,
    course_id INT,
    PRIMARY KEY(student_id, course_id),
    FOREIGN KEY(student_id) REFERENCES Students(student_id),
    FOREIGN KEY(course_id) REFERENCES Courses(course_id)
);
```

---

## 9. Bank Account Money Transfer System

### SQL Query

```sql
START TRANSACTION;

UPDATE BankAccounts
SET balance = balance - 10000
WHERE account_id = 101;

UPDATE BankAccounts
SET balance = balance + 10000
WHERE account_id = 102;

COMMIT;

SELECT * FROM BankAccounts;
```

---

## 10. Emergency Supply Container Manager

### Sample Input
```text
7
1 10
1 20
3
2
3
2
4
```

### Sample Output
```text
20
10
EMPTY
```

### C++ Code

```cpp
#include <iostream>
#include <queue>
using namespace std;

class StackUsingQueue {

    queue<int> q1, q2;

public:

    void push(int x) {

        q2.push(x);

        while(!q1.empty()) {
            q2.push(q1.front());
            q1.pop();
        }

        swap(q1, q2);
    }

    void pop() {

        if(q1.empty()) {
            cout << -1 << endl;
            return;
        }

        q1.pop();
    }

    void top() {

        if(q1.empty()) {
            cout << -1 << endl;
            return;
        }

        cout << q1.front() << endl;
    }

    void emptyCheck() {

        if(q1.empty())
            cout << "EMPTY" << endl;
        else
            cout << "NOT EMPTY" << endl;
    }
};

int main() {

    int n;
    cin >> n;

    StackUsingQueue s;

    while(n--) {

        int op;
        cin >> op;

        if(op == 1) {

            int x;
            cin >> x;

            s.push(x);
        }
        else if(op == 2) {
            s.pop();
        }
        else if(op == 3) {
            s.top();
        }
        else if(op == 4) {
            s.emptyCheck();
        }
    }

    return 0;
}
```
