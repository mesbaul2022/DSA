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
