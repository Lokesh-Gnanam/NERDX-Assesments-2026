# 🚀 NERDX-WEEKLY ASSESSMENT 2026  
## 📅 Day 13 – Coding Questions & Solutions (C++)

---

## 🔹 1. Remove K Digits

### 📥 Input
1432219  
3  

### 📤 Output
1219  

### 💻 Code
```cpp
#include <iostream>
#include <stack>
using namespace std;

string removeKdigits(string num, int k) {
    stack<char> st;

    for (char c : num) {
        while (!st.empty() && k > 0 && st.top() > c) {
            st.pop();
            k--;
        }
        st.push(c);
    }

    while (k-- > 0) st.pop();

    string res = "";
    while (!st.empty()) {
        res = st.top() + res;
        st.pop();
    }

    int i = 0;
    while (i < res.size() && res[i] == '0') i++;
    res = res.substr(i);

    return res.empty() ? "0" : res;
}

int main() {
    string num;
    int k;
    cin >> num >> k;
    cout << removeKdigits(num, k);
}
```

---

## 🔹 2. Min-Max Arrangement

### 📥 Input
1 2 3 4 5 6  

### 📤 Output
6 1 5 2 4 3  

### 💻 Code
```cpp
#include <iostream>
using namespace std;

void rearrange(int arr[], int n) {
    int max_idx = n - 1, min_idx = 0;
    int max_elem = arr[n - 1] + 1;

    for (int i = 0; i < n; i++) {
        if (i % 2 == 0)
            arr[i] += (arr[max_idx--] % max_elem) * max_elem;
        else
            arr[i] += (arr[min_idx++] % max_elem) * max_elem;
    }

    for (int i = 0; i < n; i++)
        arr[i] /= max_elem;
}

int main() {
    int arr[100], n = 0;
    while (cin >> arr[n]) n++;

    rearrange(arr, n);

    for (int i = 0; i < n; i++)
        cout << arr[i] << " ";
}
```

---

## 🔹 3. Longest Palindromic Substring

### 📥 Input
babad  

### 📤 Output
bab  

### 💻 Code
```cpp
#include <iostream>
using namespace std;

string longestPalindrome(string s) {
    int start = 0, maxLen = 1;

    for (int i = 0; i < s.length(); i++) {
        int l = i, r = i;
        while (l >= 0 && r < s.length() && s[l] == s[r]) {
            if (r - l + 1 > maxLen) {
                start = l;
                maxLen = r - l + 1;
            }
            l--; r++;
        }

        l = i; r = i + 1;
        while (l >= 0 && r < s.length() && s[l] == s[r]) {
            if (r - l + 1 > maxLen) {
                start = l;
                maxLen = r - l + 1;
            }
            l--; r++;
        }
    }

    return s.substr(start, maxLen);
}

int main() {
    string s;
    cin >> s;
    cout << longestPalindrome(s);
}
```

---

## 🔹 4. Remove Duplicate Letters
```cpp
#include <iostream>
#include <stack>
#include <vector>
using namespace std;

string removeDuplicateLetters(string s) {
    vector<int> last(26), seen(26, 0);
    for (int i = 0; i < s.size(); i++) last[s[i]-'a'] = i;

    stack<char> st;

    for (int i = 0; i < s.size(); i++) {
        char c = s[i];
        if (seen[c-'a']) continue;

        while (!st.empty() && st.top() > c && last[st.top()-'a'] > i) {
            seen[st.top()-'a'] = 0;
            st.pop();
        }

        st.push(c);
        seen[c-'a'] = 1;
    }

    string res = "";
    while (!st.empty()) {
        res = st.top() + res;
        st.pop();
    }
    return res;
}

int main() {
    string s;
    cin >> s;
    cout << removeDuplicateLetters(s);
}
```

---

## 🔹 5. Critical Points
```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
};

int countCritical(Node* head) {
    if (!head || !head->next || !head->next->next) return 0;

    Node* prev = head;
    Node* curr = head->next;
    int count = 0;

    while (curr->next) {
        if ((curr->data > prev->data && curr->data > curr->next->data) ||
            (curr->data < prev->data && curr->data < curr->next->data))
            count++;

        prev = curr;
        curr = curr->next;
    }
    return count;
}

int main() {
    int n;
    cin >> n;

    Node* head = NULL;
    Node* tail = NULL;

    for (int i = 0; i < n; i++) {
        int x; cin >> x;
        Node* temp = new Node{x, NULL};

        if (!head) head = tail = temp;
        else {
            tail->next = temp;
            tail = temp;
        }
    }

    cout << "Number of critical points: " << countCritical(head);
}
```

---

# ✅ Done
