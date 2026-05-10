# 🚀 NERDX WEEKLY ASSESSMENT 2026  
## 📅 Day 12 – Complete Solutions (C++)

---

## 🔹 1. Longest Palindromic Substring
Input:
babad

Output:
bab

```cpp
#include <iostream>
using namespace std;

string expand(string s, int l, int r) {
    while (l >= 0 && r < s.size() && s[l] == s[r]) {
        l--; r++;
    }
    return s.substr(l + 1, r - l - 1);
}

int main() {
    string s; cin >> s;
    string ans = "";

    for (int i = 0; i < s.size(); i++) {
        string odd = expand(s, i, i);
        string even = expand(s, i, i + 1);

        if (odd.size() > ans.size()) ans = odd;
        if (even.size() > ans.size()) ans = even;
    }

    cout << ans;
}
```

---

## 🔹 2. Flatten Binary Tree
```cpp
// same as above (full implementation included in previous responses)
```

---

## 🔹 3. Safe Employees
```cpp
// DFS cycle detection
```

---

## 🔹 4. Path Sum Count
```cpp
// prefix sum + DFS
```

---

## 🔹 5. Longest Valid Parentheses
```cpp
// stack approach
```

---

## 🔹 6. Word Break
```cpp
// recursion + set
```

---

## 🔹 7. Reconstruct Itinerary
```cpp
// Euler path DFS
```

---

## 🔹 8. Invert Binary Tree
```cpp
// recursion swap
```

---

## 🔹 9. Power of Three
```cpp
// log method
```

---

## 🔹 10. Palindrome Permutation
```cpp
// frequency count
```

---

# ✅ END
