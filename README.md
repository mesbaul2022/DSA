C++ code to handle **insertion** (at beginning, at end, after a specific element) and **deletion** (at beginning, at end, a specific element) in an array.

Since arrays in C++ are of fixed size, we normally handle this by keeping track of the **current number of elements** (`n`) and shifting elements as needed.

Here’s a simple C++ example:

```cpp
#include <iostream>
using namespace std;

#define MAX 100   // maximum size of array

// Function to display array
void display(int arr[], int n) {
    cout << "Array: ";
    for (int i = 0; i < n; i++) cout << arr[i] << " ";
    cout << endl;
}

// Insert at beginning
void insertAtBeginning(int arr[], int &n, int x) {
    if (n >= MAX) { cout << "Overflow!\n"; return; }
    for (int i = n; i > 0; i--) arr[i] = arr[i-1];  // shift right
    arr[0] = x;
    n++;
}

// Insert at end
void insertAtEnd(int arr[], int &n, int x) {
    if (n >= MAX) { cout << "Overflow!\n"; return; }
    arr[n] = x;
    n++;
}

// Insert after specific element (first occurrence)
void insertAfterElement(int arr[], int &n, int key, int x) {
    if (n >= MAX) { cout << "Overflow!\n"; return; }
    for (int i = 0; i < n; i++) {
        if (arr[i] == key) {
            for (int j = n; j > i+1; j--) arr[j] = arr[j-1]; // shift right
            arr[i+1] = x;
            n++;
            return;
        }
    }
    cout << "Element not found!\n";
}

// Delete from beginning
void deleteFromBeginning(int arr[], int &n) {
    if (n <= 0) { cout << "Underflow!\n"; return; }
    for (int i = 0; i < n-1; i++) arr[i] = arr[i+1]; // shift left
    n--;
}

// Delete from end
void deleteFromEnd(int arr[], int &n) {
    if (n <= 0) { cout << "Underflow!\n"; return; }
    n--;  // just reduce size
}

// Delete specific element (first occurrence)
void deleteElement(int arr[], int &n, int key) {
    if (n <= 0) { cout << "Underflow!\n"; return; }
    for (int i = 0; i < n; i++) {
        if (arr[i] == key) {
            for (int j = i; j < n-1; j++) arr[j] = arr[j+1]; // shift left
            n--;
            return;
        }
    }
    cout << "Element not found!\n";
}

int main() {
    int arr[MAX] = {10, 20, 30};
    int n = 3;

    display(arr, n);

    insertAtBeginning(arr, n, 5);
    display(arr, n);

    insertAtEnd(arr, n, 40);
    display(arr, n);

    insertAfterElement(arr, n, 20, 25);
    display(arr, n);

    deleteFromBeginning(arr, n);
    display(arr, n);

    deleteFromEnd(arr, n);
    display(arr, n);

    deleteElement(arr, n, 25);
    display(arr, n);

    return 0;
}
```

### Output (step by step):

```
Array: 10 20 30 
Array: 5 10 20 30 
Array: 5 10 20 30 40 
Array: 5 10 20 25 30 40 
Array: 10 20 25 30 40 
Array: 10 20 25 30 
Array: 10 20 30 
```

---



1. A **2D matrix** that is sparse (mostly zeros).
2. C++ program to:

   * Display the matrix in **row order** (all elements in a single row).
   * Display the matrix in **column order** (all elements in a single row).
   * Convert it into a **sparse matrix representation** (triplet form: row, col, value).

---

Here’s a clean **C++ program**:

```cpp
#include <iostream>
using namespace std;

#define MAX 100   // maximum non-zero elements for sparse representation

// Function to display matrix in row order
void displayRowOrder(int mat[][4], int rows, int cols) {
    cout << "Row Order: ";
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            cout << mat[i][j] << " ";
        }
    }
    cout << endl;
}

// Function to display matrix in column order
void displayColOrder(int mat[][4], int rows, int cols) {
    cout << "Column Order: ";
    for (int j = 0; j < cols; j++) {
        for (int i = 0; i < rows; i++) {
            cout << mat[i][j] << " ";
        }
    }
    cout << endl;
}

// Function to convert into sparse matrix (triplet form)
int convertToSparse(int mat[][4], int rows, int cols, int sparse[][3]) {
    int k = 1; // index for sparse matrix (0th row will store metadata)

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (mat[i][j] != 0) {
                sparse[k][0] = i;
                sparse[k][1] = j;
                sparse[k][2] = mat[i][j];
                k++;
            }
        }
    }

    // first row of sparse matrix = [rows, cols, non-zero count]
    sparse[0][0] = rows;
    sparse[0][1] = cols;
    sparse[0][2] = k - 1;

    return k; // total rows in sparse matrix
}

// Function to display sparse matrix
void displaySparse(int sparse[][3], int count) {
    cout << "\nSparse Matrix (row, col, value):\n";
    for (int i = 0; i < count; i++) {
        cout << sparse[i][0] << " " << sparse[i][1] << " " << sparse[i][2] << endl;
    }
}

int main() {
    int mat[4][4] = {
        {0, 0, 3, 0},
        {22, 0, 0, 0},
        {0, 7, 0, 0},
        {0, 0, 0, 5}
    };

    int rows = 4, cols = 4;
    displayRowOrder(mat, rows, cols);
    displayColOrder(mat, rows, cols);

    int sparse[MAX][3];
    int count = convertToSparse(mat, rows, cols, sparse);

    displaySparse(sparse, count);

    return 0;
}
```

---

### ✅ Example Output

For the above `4x4` matrix:

```
Row Order: 0 0 3 0 22 0 0 0 0 7 0 0 0 0 0 5 
Column Order: 0 22 0 0 0 0 7 0 3 0 0 0 0 0 0 5 

Sparse Matrix (row, col, value):
4 4 4
0 2 3
1 0 22
2 1 7
3 3 5
```

---

🔎 Explanation of sparse format:

* First row = `rows cols nonZeroCount` → `4 4 4`
* Then each row = `row col value`

---



---

### 🔹 C++ Code (Infix → Postfix Conversion)

```cpp
#include <iostream>
#include <stack>
#include <string>
using namespace std;

// Check if character is operand (a-z)
bool isOperand(char c) {
    return (c >= 'a' && c <= 'z');
}

// Get precedence of operators
int getPrecedence(char op) {
    if (op == '+' || op == '-') return 1;   // lowest precedence
    if (op == '*' || op == '/') return 2;   // medium precedence
    if (op == '^') return 3;                // highest precedence
    return -1; // Not an operator
}

// Convert infix expression to postfix
void convertToPostfix(string infix) {
    stack<char> st;

    for (char c : infix) {
        // If operand → print directly
        if (isOperand(c)) {
            cout << c;
        }
        // If '(' → push to stack
        else if (c == '(') {
            st.push(c);
        }
        // If ')' → pop until '('
        else if (c == ')') {
            while (!st.empty() && st.top() != '(') {
                cout << st.top();
                st.pop();
            }
            if (!st.empty()) st.pop(); // remove '('
        }
        // If operator → pop higher/equal precedence operators first
        else {
            while (!st.empty() && getPrecedence(st.top()) >= getPrecedence(c)) {
                cout << st.top();
                st.pop();
            }
            st.push(c);
        }
    }

    // Pop remaining operators
    while (!st.empty()) {
        cout << st.top();
        st.pop();
    }
}

int main() {
    string infix;
    cout << "Enter the infix expression: ";
    cin >> infix;

    cout << "Postfix expression: ";
    convertToPostfix(infix);
    cout << endl;

    return 0;
}
```

---

### 🔹 Example Run

**Input:**

```
a+b*c
```

**Output:**

```
Postfix expression: abc*+
```

---

✅ **Logic Summary**:

1. Operands (`a-z`) → directly output.
2. `(` → push to stack.
3. `)` → pop operators until matching `(`.
4. Operators (`+ - * / ^`) → pop from stack while precedence of top ≥ current operator, then push current operator.
5. At end, pop remaining operators.

---



1. Builds a **Binary Search Tree (BST)** with user input.
2. Implements **insertion** and **deletion** (covering all three cases).
3. Displays the tree in **Preorder, Inorder, and Postorder traversals**.
4. Lets user see execution of **all deletion cases (leaf, single child, two children)**.

---

### 🔹 C++ Program: BST with Insert + Delete + Traversals

```cpp
#include <iostream>
using namespace std;

// Structure of a node
struct Node {
    int info;
    Node* left;
    Node* right;
};

// Create new node
Node* createNode(int data) {
    Node* newNode = new Node();
    newNode->info = data;
    newNode->left = newNode->right = nullptr;
    return newNode;
}

// Insert a node in BST
Node* insert(Node* node, int data) {
    if (node == nullptr) {
        return createNode(data);
    }
    if (data < node->info) {
        node->left = insert(node->left, data);
    } else if (data > node->info) {
        node->right = insert(node->right, data);
    }
    return node;
}

// Inorder traversal (LNR)
void inorder(Node* root) {
    if (root == nullptr) return;
    inorder(root->left);
    cout << root->info << " ";
    inorder(root->right);
}

// Preorder traversal (NLR)
void preorder(Node* root) {
    if (root == nullptr) return;
    cout << root->info << " ";
    preorder(root->left);
    preorder(root->right);
}

// Postorder traversal (LRN)
void postorder(Node* root) {
    if (root == nullptr) return;
    postorder(root->left);
    postorder(root->right);
    cout << root->info << " ";
}

// Find minimum value node (used for deletion case III)
Node* minValueNode(Node* node) {
    Node* current = node;
    while (current && current->left != nullptr) {
        current = current->left;
    }
    return current;
}

// Delete a node
Node* deleteNode(Node* root, int key) {
    if (root == nullptr) return root;

    if (key < root->info) {
        root->left = deleteNode(root->left, key);
    } else if (key > root->info) {
        root->right = deleteNode(root->right, key);
    } else {
        // Case I: Leaf node
        if (root->left == nullptr && root->right == nullptr) {
            cout << "Deleting leaf node: " << root->info << endl;
            delete root;
            return nullptr;
        }
        // Case II: One child
        else if (root->left == nullptr) {
            cout << "Deleting node with one child: " << root->info << endl;
            Node* temp = root->right;
            delete root;
            return temp;
        } else if (root->right == nullptr) {
            cout << "Deleting node with one child: " << root->info << endl;
            Node* temp = root->left;
            delete root;
            return temp;
        }
        // Case III: Two children
        else {
            cout << "Deleting node with two children: " << root->info << endl;
            Node* temp = minValueNode(root->right);
            root->info = temp->info; // Copy inorder successor
            root->right = deleteNode(root->right, temp->info);
        }
    }
    return root;
}

int main() {
    Node* root = nullptr;
    int n, val;

    cout << "Enter number of nodes to insert: ";
    cin >> n;

    cout << "Enter " << n << " values:\n";
    for (int i = 0; i < n; i++) {
        cin >> val;
        root = insert(root, val);
    }

    cout << "\nPreorder Traversal: ";
    preorder(root);
    cout << "\nInorder Traversal: ";
    inorder(root);
    cout << "\nPostorder Traversal: ";
    postorder(root);
    cout << endl;

    // Demonstrating all delete cases
    cout << "\nEnter a node to delete (leaf case): ";
    cin >> val;
    root = deleteNode(root, val);

    cout << "\nInorder after deletion: ";
    inorder(root);
    cout << endl;

    cout << "\nEnter a node to delete (one child case): ";
    cin >> val;
    root = deleteNode(root, val);

    cout << "\nInorder after deletion: ";
    inorder(root);
    cout << endl;

    cout << "\nEnter a node to delete (two children case): ";
    cin >> val;
    root = deleteNode(root, val);

    cout << "\nInorder after deletion: ";
    inorder(root);
    cout << endl;

    return 0;
}
```

---

### 🔹 Explanation of Deletion Cases

* **Case I (Leaf node):** Node has no children → simply delete.
* **Case II (One child):** Replace node with its only child.
* **Case III (Two children):** Replace node with **inorder successor** (smallest value in right subtree), then delete that successor.

---

### 🔹 Example Run

```
Enter number of nodes to insert: 7
Enter 7 values:
50 30 70 20 40 60 80

Preorder Traversal: 50 30 20 40 70 60 80 
Inorder Traversal: 20 30 40 50 60 70 80 
Postorder Traversal: 20 40 30 60 80 70 50 

Enter a node to delete (leaf case): 20
Deleting leaf node: 20
Inorder after deletion: 30 40 50 60 70 80 

Enter a node to delete (one child case): 30
Deleting node with one child: 30
Inorder after deletion: 40 50 60 70 80 

Enter a node to delete (two children case): 50
Deleting node with two children: 50
Inorder after deletion: 40 60 70 80 
```

---

