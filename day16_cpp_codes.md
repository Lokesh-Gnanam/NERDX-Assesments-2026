# DAY 16 - C++ Coding Solutions

# 1. Customer Support Ticket Manager

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

    void shift() {
        if (s2.empty()) {
            while (!s1.empty()) {
                s2.push(s1.top());
                s1.pop();
            }
        }
    }

    void resolveTicket() {
        shift();
        cout << s2.top() << endl;
        s2.pop();
    }

    void viewTicket() {
        shift();
        cout << s2.top() << endl;
    }

    void isEmpty() {
        cout << ((s1.empty() && s2.empty()) ? "true" : "false") << endl;
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
            tm.isEmpty();
        }
        else {
            cout << -1 << endl;
        }
    }

    return 0;
}
```

# 2. Fraudulent Transaction Pair Detection

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

# 3. Secure Surveillance Tower Placement

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

    for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
        if (board[i][j] == 'T')
            return false;
    }

    for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
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
        cout << "Valid surveillance layouts:\n";

        for (auto config : ans) {
            for (auto row : config)
                cout << row << endl;
            cout << endl;
        }
    }

    return 0;
}
```

# 4. Warehouse Label Locator System

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
