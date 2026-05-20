# Day 23 Coding Questions and Answers

# 1. Warehouse Package Rearrangement System

## Problem Statement
A warehouse management system stores package priority values in a sequence.

- Non-zero values represent active packages ready for shipment.
- Value 0 represents an empty storage slot.

Move all zeros to the end while maintaining the order of active packages.

---

## Input Format
- First line contains integer N
- Second line contains N space-separated integers

## Output Format
Print rearranged sequence.

---

## Sample Input
```text
5
0 1 0 3 12
```

## Sample Output
```text
1 3 12 0 0
```

---

## C++ Code
```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int N;
    cin >> N;

    vector<int> arr(N);

    for (int i = 0; i < N; i++)
        cin >> arr[i];

    vector<int> result;
    int zeroCount = 0;

    for (int x : arr) {
        if (x != 0)
            result.push_back(x);
        else
            zeroCount++;
    }

    while (zeroCount--)
        result.push_back(0);

    for (int x : result)
        cout << x << " ";

    return 0;
}
```

---

# 2. Customer Purchase Verification System

## Problem Statement
Detect duplicates and print unique product IDs in insertion order.

---

## Sample Input
```text
6
10 20 10 30 40 20
```

## Sample Output
```text
Duplicate Found
Unique Products: 10 20 30 40
```

---

## C++ Code
```cpp
#include <iostream>
#include <unordered_set>
#include <vector>
using namespace std;

int main() {
    int N;
    cin >> N;

    vector<int> arr(N);
    unordered_set<int> seen;
    vector<int> uniqueProducts;

    bool duplicate = false;

    for (int i = 0; i < N; i++) {
        cin >> arr[i];

        if (seen.count(arr[i])) {
            duplicate = true;
        } else {
            seen.insert(arr[i]);
            uniqueProducts.push_back(arr[i]);
        }
    }

    if (duplicate)
        cout << "Duplicate Found\n";
    else
        cout << "No Duplicate Found\n";

    cout << "Unique Products: ";

    for (int x : uniqueProducts)
        cout << x << " ";

    return 0;
}
```

---

# 3. Conference Room Booking Optimizer

## Sample Input
```text
4
1 3
2 6
8 10
15 18
```

## Sample Output
```text
1 6
8 10
15 18
```

---

## C++ Code
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    int N;
    cin >> N;

    vector<pair<int, int>> intervals(N);

    for (int i = 0; i < N; i++)
        cin >> intervals[i].first >> intervals[i].second;

    sort(intervals.begin(), intervals.end());

    vector<pair<int, int>> merged;
    merged.push_back(intervals[0]);

    for (int i = 1; i < N; i++) {
        int start = intervals[i].first;
        int end = intervals[i].second;

        if (start <= merged.back().second)
            merged.back().second = max(merged.back().second, end);
        else
            merged.push_back({start, end});
    }

    for (auto p : merged)
        cout << p.first << " " << p.second << endl;

    return 0;
}
```

---

# 4. Employee Attendance Streak Analyzer

## Sample Input
```text
6
100 4 200 1 3 2
```

## Sample Output
```text
4
```

---

## C++ Code
```cpp
#include <iostream>
#include <unordered_set>
using namespace std;

int main() {
    int N;
    cin >> N;

    unordered_set<int> s;

    for (int i = 0; i < N; i++) {
        int x;
        cin >> x;
        s.insert(x);
    }

    int longest = 0;

    for (int num : s) {
        if (!s.count(num - 1)) {
            int current = num;
            int streak = 1;

            while (s.count(current + 1)) {
                current++;
                streak++;
            }

            longest = max(longest, streak);
        }
    }

    cout << longest;

    return 0;
}
```

---

# 5. Department Salary Analysis Report

## SQL Query
```sql
SELECT 
    department,
    SUM(salary) AS total_salary
FROM EmployeeDetails
GROUP BY department
HAVING SUM(salary) > 100000;
```

---

# 6. Document Signature Verification System

## Sample Input
```text
listen
silent
```

## Sample Output
```text
Character Frequency Comparison:
e -> 1 | 1
i -> 1 | 1
l -> 1 | 1
n -> 1 | 1
s -> 1 | 1
t -> 1 | 1

Valid Match
```

---

## C++ Code
```cpp
#include <iostream>
#include <map>
using namespace std;

int main() {
    string s1, s2;
    cin >> s1 >> s2;

    map<char, int> freq1, freq2;

    for (char c : s1)
        freq1[c]++;

    for (char c : s2)
        freq2[c]++;

    cout << "Character Frequency Comparison:\n";

    bool valid = true;

    for (char c = 'a'; c <= 'z'; c++) {
        if (freq1[c] > 0 || freq2[c] > 0) {
            cout << c << " -> "
                 << freq1[c] << " | "
                 << freq2[c] << endl;

            if (freq1[c] != freq2[c])
                valid = false;
        }
    }

    cout << endl;

    if (valid)
        cout << "Valid Match";
    else
        cout << "Invalid Match";

    return 0;
}
```

---

# 7. Employee Performance Ranking System

## SQL Query
```sql
SELECT
    employee_name,
    department,
    performance_score,

    RANK() OVER (ORDER BY performance_score DESC) AS rank_value,

    DENSE_RANK() OVER (ORDER BY performance_score DESC) AS dense_rank_value

FROM EmployeePerformance
ORDER BY performance_score DESC;
```

---

# 8. Peak Traffic Monitoring System

## Sample Input
```text
8
1 3 -1 -3 5 3 6 7
3
```

## Sample Output
```text
3 3 5 5 6 7
```

---

## C++ Code
```cpp
#include <iostream>
#include <vector>
#include <deque>
using namespace std;

int main() {
    int N;
    cin >> N;

    vector<int> arr(N);

    for (int i = 0; i < N; i++)
        cin >> arr[i];

    int K;
    cin >> K;

    deque<int> dq;

    for (int i = 0; i < N; i++) {

        while (!dq.empty() && dq.front() <= i - K)
            dq.pop_front();

        while (!dq.empty() && arr[dq.back()] < arr[i])
            dq.pop_back();

        dq.push_back(i);

        if (i >= K - 1)
            cout << arr[dq.front()] << " ";
    }

    return 0;
}
```

---

# 9. Trending Product Analytics System

## Sample Input
```text
6
1 1 1 2 2 3
2
```

## Sample Output
```text
1 2
```

---

## C++ Code
```cpp
#include <iostream>
#include <unordered_map>
#include <queue>
using namespace std;

int main() {
    int N;
    cin >> N;

    unordered_map<int, int> freq;

    for (int i = 0; i < N; i++) {
        int x;
        cin >> x;
        freq[x]++;
    }

    int K;
    cin >> K;

    priority_queue<pair<int, int>> pq;

    for (auto x : freq)
        pq.push({x.second, x.first});

    while (K--) {
        cout << pq.top().second << " ";
        pq.pop();
    }

    return 0;
}
```

---

# 10. Smart Inventory Access Manager

## Sample Input
```text
2
6
PUT 1 10
PUT 2 20
GET 1
PUT 3 30
GET 2
GET 3
```

## Sample Output
```text
10
-1
30
```

---

## C++ Code
```cpp
#include <iostream>
#include <list>
#include <unordered_map>
using namespace std;

class LRUCache {

    int capacity;

    list<pair<int, int>> cache;

    unordered_map<int, list<pair<int, int>>::iterator> mp;

public:

    LRUCache(int cap) {
        capacity = cap;
    }

    int get(int key) {

        if (mp.find(key) == mp.end())
            return -1;

        auto it = mp[key];

        int value = it->second;

        cache.erase(it);

        cache.push_front({key, value});

        mp[key] = cache.begin();

        return value;
    }

    void put(int key, int value) {

        if (mp.find(key) != mp.end()) {
            cache.erase(mp[key]);
        }

        else if (cache.size() == capacity) {

            auto last = cache.back();

            mp.erase(last.first);

            cache.pop_back();
        }

        cache.push_front({key, value});

        mp[key] = cache.begin();
    }
};

int main() {

    int capacity;
    cin >> capacity;

    int Q;
    cin >> Q;

    LRUCache lru(capacity);

    while (Q--) {

        string op;
        cin >> op;

        if (op == "PUT") {

            int key, value;
            cin >> key >> value;

            lru.put(key, value);
        }

        else if (op == "GET") {

            int key;
            cin >> key;

            cout << lru.get(key) << endl;
        }
    }

    return 0;
}
```
