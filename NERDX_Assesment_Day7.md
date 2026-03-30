# NERX-ASSESMENT MARCH 2026
# Day 7

## C1. In Chennai, an auto-rickshaw company uses a coded pattern system to assign routes to drivers.



Each route is described using:

A pattern string
A sequence of space-separated location names


Each character in the pattern corresponds to one word in the sequence.



To validate the route:

The number of characters in the pattern must be equal to the number of words in the sequence
Each character must map to exactly one word
Each word must map to exactly one character


If at any position:

A character is already mapped to a different word, or
A word is already mapped to a different character, 
then the pattern is invalid.

Input format :
First line: a string pattern
Second line: a string s containing space-separated words
Output format :
Print "Pattern Matches" if the pattern is valid
Otherwise, print "Pattern Does Not Match"
Code constraints :
1 ≤ pattern.length ≤ 300
1 ≤ s.length ≤ 3000
Pattern contains only lowercase English letters
Words in s are separated by a single space

| Case | Pattern | Word Sequence | Logic Result | Output |
| :--- | :--- | :--- | :--- | :--- |
| **1** | `abba` | `dog cat cat fish` | Mapping failure at index 3 | `Pattern Does Not Match` |
| **2** | `abba` | `dog cat cat dog` | Consistent 1:1 mapping | `Pattern Matches` |


```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    string pattern, s;
    cin >> pattern;
    cin.ignore();
    getline(cin, s);

    vector<string> words;
    stringstream ss(s);
    string word;
    while (ss >> word) words.push_back(word);

    if (pattern.size() != words.size()) {
        cout << "Pattern Does Not Match";
        return 0;
    }

    unordered_map<char, string> m1;
    unordered_map<string, char> m2;

    for (int i = 0; i < pattern.size(); i++) {
        char c = pattern[i];
        string w = words[i];

        if (m1.count(c) && m1[c] != w) {
            cout << "Pattern Does Not Match";
            return 0;
        }
        if (m2.count(w) && m2[w] != c) {
            cout << "Pattern Does Not Match";
            return 0;
        }

        m1[c] = w;
        m2[w] = c;
    }

    cout << "Pattern Matches";
}

```


-----------------------------------------------------------
-----------------------------------------------------------

## C2. Revenue Path Validation in Chennai Delivery Network



A logistics company in Chennai manages a delivery route system structured as a binary tree.



Each node represents a checkpoint, and the value at each node indicates the revenue earned at that checkpoint.



The company wants to verify if there exists any delivery route from the main hub (root) to a final delivery point (leaf) such that the total revenue collected equals a given target.



Your task is to determine whether there exists a root-to-leaf path such that the sum of node values along the path equals target.

Input format :
First line: Integer N (number of nodes)
Second line: Level-order traversal of the tree (-1 for NULL nodes)
Third line: Integer targetSum
Output format :
"Path Exists" if such a path is found
"No Path Found" otherwise
Code constraints :
0 ≤ N ≤ 1000
-1000 ≤ Node values ≤ 1000
-1000 ≤ targetSum ≤ 1000
| Case | Total Nodes | Level-Order Tree Input | Target Sum | Output |
| :--- | :--- | :--- | :--- | :--- |
| **1** | `5` | 1, 2, 3 | `5` | `No Path Found` |
| **2** | `9` | 5, 4, 8, 11, -1, 13, 4, 7, 2 | `22` | `Path Exists` |

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Node {
    int val;
    Node* left;
    Node* right;
    Node(int x) : val(x), left(NULL), right(NULL) {}
};

Node* buildTree(vector<int>& arr) {
    if (arr.empty()) return NULL;

    Node* root = new Node(arr[0]);
    queue<Node*> q;
    q.push(root);
    int i = 1;

    while (!q.empty() && i < arr.size()) {
        Node* curr = q.front(); q.pop();

        if (i < arr.size()) {
            curr->left = new Node(arr[i++]);
            q.push(curr->left);
        }
        if (i < arr.size()) {
            curr->right = new Node(arr[i++]);
            q.push(curr->right);
        }
    }
    return root;
}

bool hasPath(Node* root, int sum) {
    if (!root) return false;

    if (!root->left && !root->right)
        return sum == root->val;

    return hasPath(root->left, sum - root->val) ||
           hasPath(root->right, sum - root->val);
}

int main() {
    int n;
    cin >> n;

    if (n == 0) {
        cout << "No Path Found";
        return 0;
    }

    vector<int> arr(n);
    for (int i = 0; i < n; i++) cin >> arr[i];

    int target;
    cin >> target;

    Node* root = buildTree(arr);

    cout << (hasPath(root, target) ? "Path Exists" : "No Path Found");
}
```



-------------------
-------------------

## C3. Encrypted Message Decoder

A cybersecurity team in Chennai receives encoded messages from remote sensors. Each message follows a specific pattern where parts of the string are repeated multiple times. To analyze the data correctly, the system must decode these messages into their original readable format.

Input format :
A single line containing the encoded string s
Output format :
A single line containing the decoded string
Code constraints :
1 ≤ |s| ≤ 30
s consists of lowercase letters, digits, and brackets []
1 ≤ k ≤ 300
Input string is always valid and properly formatted



| Case | Input (Encoded) | Decoding Logic | Output (Decoded) |
| :--- | :--- | :--- | :--- |
| **1** | `3[a]2[bc]` | 3*'a' + 2*'bc' | `aaabcbc` |
| **2** | `3[a2[c]]` | 3*( 'a' + 2*'c' ) | `accaccacc` |


```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    string s;
    cin >> s;

    stack<int> counts;
    stack<string> resultStack;

    string curr = "";
    int k = 0;

    for (char c : s) {
        if (isdigit(c)) {
            k = k * 10 + (c - '0');
        } else if (c == '[') {
            counts.push(k);
            resultStack.push(curr);
            k = 0;
            curr = "";
        } else if (c == ']') {
            int count = counts.top(); counts.pop();
            string temp = resultStack.top(); resultStack.pop();

            for (int i = 0; i < count; i++)
                temp += curr;

            curr = temp;
        } else {
            curr += c;
        }
    }

    cout << curr;
}
```

---------------------------
---------------------------

## C4. Chennai Product Bundle Generator (Duplicate Handling)

A Chennai-based e-commerce warehouse prepares product bundles for delivery. Each product is identified by an integer ID, and duplicate IDs may appear due to multiple stock entries.



The system must generate all possible unique bundles (subsets) of products. Even if the input contains duplicate product IDs, duplicate subsets must not be printed.



Subsets should be generated in a way that:

Each element can be chosen or not chosen.
Duplicate subsets are avoided.
Subsets follow the order based on the sorted input.


The empty subset must also be included.

Input format :
First line: Integer n
Second line: n space-separated integers
Output format :
Print all unique subsets
Each subset should be printed in one line
Elements inside subset separated by space
Empty subset printed as: []
Code constraints :
1 ≤ n ≤ 10
-10 ≤ nums[i] ≤ 10

| Case | Size ($N$) | Input Elements | Output (Unique Subsets) |
| :--- | :--- | :--- | :--- |
| **1** | `3` | 1, 2, 2 | `[]`<br>`1`<br>`1 2`<br>`1 2 2`<br>`2`<br>`2 2` |
| **2** | `1` | 0 | `[]`<br>`0` |

```cpp

```

------------------------
------------------------

## C5.  At a busy Chennai bus depot, conductors continuously record ticket fares collected throughout the day. These fares are managed using a stack-like system, where the most recently added fare is always the first to be removed.



To assist the depot manager in monitoring operations efficiently, you are required to simulate this system. The system must support standard stack operations along with an additional requirement: the ability to instantly determine the minimum fare present in the stack at any given time.



You need to process a sequence of operations that manipulate the stack and produce outputs for specific queries.



The supported operations are:

push x: Add an integer fare x to the top of the stack.
pop: Remove the element at the top of the stack.
top: Retrieve and print the element currently at the top of the stack.
getMin: Retrieve and print the minimum element present in the stack.


The stack is guaranteed to have at least one element whenever pop, top, or getMin operations are performed.

Input format :
The first line contains an integer q — the total number of operations.
Each of the next q lines contains a single operation in one of the following formats:
push x — where x is an integer
pop
top
getMin
Output format :
For every top operation, output the value at the top of the stack.
For every getMin operation, output the minimum value currently present in the stack.
Each result should be printed on a new line.
Code constraints :
1 ≤ Q ≤ 30,000
-2³¹ ≤ x ≤ 2³¹ - 1
pop, top, and getMin will always be called on a non-empty stack


| Operation | Main Stack | Min Stack | Output/Result |
| :--- | :--- | :--- | :--- |
| `push -2` | [-2] | [-2] | |
| `push 0` | [-2, 0] | [-2] | |
| `push -3` | [-2, 0, -3] | [-2, -3] | |
| `getMin` | [-2, 0, -3] | [-2, -3] | **-3** |
| `pop` | [-2, 0] | [-2] | |
| `top` | [-2, 0] | [-2] | **0** |

```cpp

```

-----------------------
-----------------------

## C6. Chennai Logistics Zero Balance Shipment Combinations

A logistics company in Chennai manages shipments across four warehouses. Each warehouse provides a list of load values (which may be positive or negative depending on adjustments).



You are given four arrays A, B, C, and D, each containing n integers representing load values from the four warehouses.



Engineers need to determine how many combinations of shipments—selecting exactly one value from each of the four warehouses—result in a total net load of zero.



In other words, count the number of tuples (i, j, k, l) such that:

A[i] + B[j] + C[k] + D[l] = 0

Input format :
First line: Integer n
Next 4 lines: Each contains n space-separated integers representing arrays A, B, C, and D
Output format :
Print a single integer → number of valid tuples
Code constraints :
1 ≤ n ≤ 200
-2²⁸ ≤ values ≤ 2²⁸


| Case | $N$ | Logic Formula | Key Pairs ($A+B$) | Output |
| :--- | :--- | :--- | :--- | :--- |
| **1** | 2 | $A[i]+B[j]+C[k]+D[l]=0$ | (1,-2), (2,-1) | `2` |
| **2** | 3 | All $C, D$ are 0 | (1,-1), (2,-2), (3,-3) | `27` |


```cpp

```


---------------------------
---------------------------

## C7. Binary Tree Message Encoding System



In a communication system, hierarchical data is structured as a binary tree and transmitted as a string. At the receiving end, the system must rebuild the tree and process it for further use.



You are given a string representing a binary tree in level-order traversal, where:

Each value is either an integer or "null"
"null" indicates a missing node


Your task is:

Reconstruct (Deserialize) the binary tree from the given string
Re-encode (Serialize) the tree using level-order traversal:
Include "null" for missing nodes
Include all nodes encountered during traversal
Display the tree using level-order traversal:
Print only non-null node values


Special Case:



If n == 0, output:
(empty)
(empty)
Input format :
First line: Integer n — number of elements
Second line: A string containing space-separated values representing the tree
Values are either integers or "null"
Output format :
First line: Serialized tree string (space-separated, including "null"), or "(empty)" if tree is empty
Second line: Level-order traversal of only non-null nodes, or "(empty)" if tree is empty
Refer sample output.
Code constraints :
0 ≤ n ≤ 10⁴
-1000 ≤ Node.val ≤ 1000
Input tree is valid

| Case | Total Nodes | Level-Order Input | Full Structure Output | Clean Traversal Output |
| :--- | :--- | :--- | :--- | :--- |
| **1** | `7` | `1 2 3 null null 4 5` | `1 2 3 null null 4 5 null null null null` | `1 2 3 4 5` |
| **2** | `0` | `(empty)` | `(empty)` | `(empty)` |


```cpp


```
---------------
---------------

## C8. Chennai Weather Forecast Waiting System

In the Chennai Meteorological Department, daily temperature readings are recorded for operational monitoring.



Given a sequence of daily temperatures, for each day, engineers want to know how many days they must wait until a warmer temperature occurs.



For each day [i], find the number of days after it such that a future day has a strictly higher temperature than the current day.

If no such future day exists, the value for that day should be 0.

Input format :
First line: Integer n (number of days)
Second line: n space-separated integers (temperatures)
Output format :
Print n space-separated integers representing waiting days
Code constraints :
1 ≤ n ≤ 100000
30 ≤ temperatures[i] ≤ 100


| Case | Total Days ($N$) | Temperature Readings | Output (Wait Days) |
| :--- | :--- | :--- | :--- |
| **1** | `8` | 73, 74, 75, 71, 69, 72, 76, 73 | `1 1 4 2 1 1 0 0` |
| **2** | `3` | 30, 40, 50 | `1 1 0` |

```cpp

```

--------------------------
--------------------------

## C9. Network Reliability Analyzer (Maximum Signal Path)

A Chennai-based telecom company manages a network of signal towers arranged in a tree-like structure. Each tower is represented as a node and may have a left and a right child.



The network is given in level-order format as an array. A value of -1 indicates that no node exists at that position.



The tree is constructed as follows:



For each index i, if the value is not -1, a node is created.

Nodes are connected sequentially: for each valid node, the next available values in the array are assigned as its left and right children.

Positions with -1 are skipped (no node is created, and no children are assigned from them).



Each node has a signal strength (which can be positive or negative).



Engineers need to determine the maximum signal strength achievable along any path in the network.



A path is defined as any sequence of connected nodes, where:



The path can start and end at any node in the tree

The path must follow parent-child connections

A node can be included only once in the path

Input format :
First line: Integer n — number of elements in the level-order array
Second line: n space-separated integers representing the tree
-1 represents a NULL node
Output format :
Print a single integer → maximum path sum
Code constraints :
1 ≤ n ≤ 1000
-1000 ≤ Node values ≤ 1000
| Case | Total Nodes | Level-Order Tree Input | Output (Max Path Sum) |
| :--- | :--- | :--- | :--- |
| **1** | `7` | -10, 9, 20, -1001, -1001, 15, 7 | `42` |
| **2** | `3` | 1, 2, 3 | `6` |

```cpp

```

-------------------------------------
-------------------------------------

## C10. Chennai Metro CCTV Right View Monitoring

The Chennai Metro Rail system uses a hierarchical CCTV camera network arranged in a tree structure. Each camera is represented as a node and may have a left and a right child camera.



The network is provided as a level-order sequence of camera IDs. A value of -1 indicates that no camera exists at that position.



The tree is constructed sequentially from this level-order input:



Each valid camera node is assigned its left child from the next available value in the sequence, followed by its right child.

If a position contains -1, no node is created, and it is skipped during tree linking.



From the control room, engineers can only observe the rightmost camera at each level of the network due to obstruction and viewing angle limitations.



Your task is to determine and print the IDs of the cameras visible from the right side of the network.

Input format :
First line: An integer n — the number of values in the level-order sequence
Second line: n space-separated integers representing the camera network
A value of -1 represents a missing (NULL) camera
Output format :
Print space-separated integers representing the right side view
Code constraints :
0 ≤ n ≤ 1000
-100 ≤ Node value ≤ 100
Tree is constructed using level order input
Sample test cases :
Input 1 :
7
1 2 3 -1 5 -1 4
Output 1 :
1 3 4 
Input 2 :
6
1 2 3 4 5 6
Output 2 :
1 3 6 


------------------------------
------------------------------
------------------------------
