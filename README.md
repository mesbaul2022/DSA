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

