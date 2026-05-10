
# DAY 16 - Complete C++ Coding Solutions

---

# 1. Customer Support Ticket Manager

## Problem Statement
A customer support system processes incoming support tickets using queue behavior implemented internally with stacks.

## Sample Input
```text
4
1 10
1 20
3
2
```

## Sample Output
```text
10
10
```

## C++ Code
```cpp
#include <iostream>
#include <stack>
using namespace std;

class TicketManager {
    stack<int> s1, s2;

public:

    void addTicket(int x) {
        s1.push(x);
    }

    void shiftStacks() {

        if (s2.empty()) {

            while (!s1.empty()) {
                s2.push(s1.top());
                s1.pop();
            }
        }
    }

    void resolveTicket() {

        shiftStacks();

        cout << s2.top() << endl;
        s2.pop();
    }

    void viewTicket() {

        shiftStacks();

        cout << s2.top() << endl;
    }

    void checkEmpty() {

        if (s1.empty() && s2.empty())
            cout << "true" << endl;
        else
            cout << "false" << endl;
    }
};

int main() {

    int q;
    cin >> q;

    TicketManager tm;

    while (q--) {

        int op;
        cin >> op;

        if (op == 1) {

            int x;
            cin >> x;

            tm.addTicket(x);
        }

        else if (op == 2) {
            tm.resolveTicket();
        }

        else if (op == 3) {
            tm.viewTicket();
        }

        else if (op == 4) {
            tm.checkEmpty();
        }

        else {
            cout << -1 << endl;
        }
    }

    return 0;
}
```

---

# 2. Fraudulent Transaction Pair Detection

## Sample Input
```text
4
1 3 4 2
6
```

## Sample Output
```text
Fraudulent transactions found at indices: 2 and 3
```

## C++ Code
```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
using namespace std;

int main() {

    int n;
    cin >> n;

    vector<int> arr(n);

    for (int i = 0; i < n; i++)
        cin >> arr[i];

    int target;
    cin >> target;

    unordered_map<int, int> mp;

    for (int i = 0; i < n; i++) {

        int need = target - arr[i];

        if (mp.find(need) != mp.end()) {

            cout << "Fraudulent transactions found at indices: "
                 << mp[need] << " and " << i << endl;

            return 0;
        }

        mp[arr[i]] = i;
    }

    return 0;
}
```

---

# 3. Secure Surveillance Tower Placement

## Sample Input
```text
4
```

## Sample Output
```text
.T..
...T
T...
..T.
```

## C++ Code
```cpp
#include <iostream>
#include <vector>
using namespace std;

vector<vector<string>> ans;

bool isSafe(vector<string>& board, int row, int col, int n) {

    for (int i = 0; i < row; i++) {
        if (board[i][col] == 'T')
            return false;
    }

    for (int i = row - 1, j = col - 1;
         i >= 0 && j >= 0;
         i--, j--) {

        if (board[i][j] == 'T')
            return false;
    }

    for (int i = row - 1, j = col + 1;
         i >= 0 && j < n;
         i--, j++) {

        if (board[i][j] == 'T')
            return false;
    }

    return true;
}

void solve(int row, vector<string>& board, int n) {

    if (row == n) {
        ans.push_back(board);
        return;
    }

    for (int col = 0; col < n; col++) {

        if (isSafe(board, row, col, n)) {

            board[row][col] = 'T';

            solve(row + 1, board, n);

            board[row][col] = '.';
        }
    }
}

int main() {

    int n;
    cin >> n;

    vector<string> board(n, string(n, '.'));

    solve(0, board, n);

    if (ans.empty()) {
        cout << "No valid surveillance layouts possible";
    }

    else {

        cout << "Valid surveillance layouts:" << endl;

        for (auto config : ans) {

            for (auto row : config)
                cout << row << endl;

            cout << endl;
        }
    }

    return 0;
}
```

---

# 4. Warehouse Label Locator System

## Sample Input
```text
3 4
A B C E
S F C S
A D E E
SEE
```

## Sample Output
```text
Label found starting at position: (1, 3)
```

## C++ Code
```cpp
#include <iostream>
#include <vector>
using namespace std;

int r, c;
vector<vector<char>> grid;
string word;

bool dfs(int x, int y, int idx, vector<vector<int>>& vis) {

    if (idx == word.size())
        return true;

    if (x < 0 || y < 0 || x >= r || y >= c)
        return false;

    if (vis[x][y] || grid[x][y] != word[idx])
        return false;

    vis[x][y] = 1;

    bool found =
        dfs(x + 1, y, idx + 1, vis) ||
        dfs(x - 1, y, idx + 1, vis) ||
        dfs(x, y + 1, idx + 1, vis) ||
        dfs(x, y - 1, idx + 1, vis);

    vis[x][y] = 0;

    return found;
}

int main() {

    cin >> r >> c;

    grid.resize(r, vector<char>(c));

    for (int i = 0; i < r; i++) {
        for (int j = 0; j < c; j++) {
            cin >> grid[i][j];
        }
    }

    cin >> word;

    for (int i = 0; i < r; i++) {

        for (int j = 0; j < c; j++) {

            vector<vector<int>> vis(r, vector<int>(c, 0));

            if (dfs(i, j, 0, vis)) {

                cout << "Label found starting at position: ("
                     << i << ", " << j << ")";

                return 0;
            }
        }
    }

    cout << "Label not found in warehouse";

    return 0;
}
```

---

# 5. Smart Browser Navigation System

## Sample Input
```text
a.com
5
visit b.com
visit c.com
back 2
forward 1
back 1
```

## Sample Output
```text
Current page after operation: a.com
Current page after operation: b.com
Current page after operation: a.com
```

## C++ Code
```cpp
#include <iostream>
#include <stack>
using namespace std;

stack<string> backStack, forwardStack;
string currentPage;

void visit(string url) {

    if (!currentPage.empty())
        backStack.push(currentPage);

    currentPage = url;

    while (!forwardStack.empty())
        forwardStack.pop();
}

string goBack(int steps) {

    while (steps-- && !backStack.empty()) {

        forwardStack.push(currentPage);

        currentPage = backStack.top();
        backStack.pop();
    }

    return currentPage;
}

string goForward(int steps) {

    while (steps-- && !forwardStack.empty()) {

        backStack.push(currentPage);

        currentPage = forwardStack.top();
        forwardStack.pop();
    }

    return currentPage;
}

int main() {

    string homepage;
    cin >> homepage;

    visit(homepage);

    int q;
    cin >> q;

    while (q--) {

        string op;
        cin >> op;

        if (op == "visit") {

            string url;
            cin >> url;

            visit(url);
        }

        else if (op == "back") {

            int steps;
            cin >> steps;

            cout << "Current page after operation: "
                 << goBack(steps) << endl;
        }

        else {

            int steps;
            cin >> steps;

            cout << "Current page after operation: "
                 << goForward(steps) << endl;
        }
    }

    return 0;
}
```
