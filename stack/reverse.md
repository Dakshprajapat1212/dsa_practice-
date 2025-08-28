stack mai element reverse krdo

approch   [9,7,4,3]
recursio n use kremhgelenge 

ek ko nikalo 9 {7,4,3] ko reverse krdooo 9 ko bottom mai daldo 
fir next step mai 7 ko nikaldo 4 , 3 ko recursion krdo 7 ko bottom mai daldena

fir 4 ko nika do 3 ko reverse krdenaa



abh sbse phle 4 bottom mai fir 7 bottom mai 9 fir 3 bottm mai


mai  
mtlb [3,4,6,9]

Here’s how you can reverse a stack using recursion in **C++**, along with a dry run explanation to help you understand the flow.

---

### 🧑‍💻 C++ Code

```cpp
#include <iostream>
#include <stack>
using namespace std;

// Function to insert an element at the bottom of a stack
void insertAtBottom(stack<int>& st, int item) {
    if (st.empty()) {
        st.push(item);
    } else {
        int top = st.top();
        st.pop();
        insertAtBottom(st, item);
        st.push(top);
    }
}

// Function to reverse the stack using recursion
void reverseStack(stack<int>& st) {
    if (!st.empty()) {
        int top = st.top();
        st.pop();
        reverseStack(st);
        insertAtBottom(st, top);
    }
}

// Utility function to print stack contents
void printStack(stack<int> st) {
    while (!st.empty()) {
        cout << st.top() << " ";
        st.pop();
    }
    cout << endl;
}

int main() {
    stack<int> st;
    st.push(9);
    st.push(7);
    st.push(4);
    st.push(3);

    cout << "Original Stack (top to bottom): ";
    printStack(st);

    reverseStack(st);

    cout << "Reversed Stack (top to bottom): ";
    printStack(st);

    return 0;
}
```

---

### 🧪 Dry Run (Stack: `[9, 7, 4, 3]`)

- `reverseStack()` pops `3`, then `4`, then `7`, then `9`.
- Base case reached when stack is empty.
- Then `insertAtBottom()` starts inserting:
  - `9` → `[9]`
  - `7` → `[9, 7]`
  - `4` → `[9, 7, 4]`
  - `3` → `[9, 7, 4, 3]`

But since we’re inserting at the bottom each time, the final stack becomes:  
**Top to bottom: `3 4 7 9`**

---

Would you like a visual diagram or want to try this with a custom input?

Absolutely! Let’s walk through a **detailed dry run** of reversing a stack using recursion in **C++**, based on your example stack:  
`[9, 7, 4, 3]` (top is on the right)

---

### 🧠 Goal

Reverse the stack so that the bottom becomes the top:  
**Input:** `[9, 7, 4, 3]`  
**Output:** `[3, 4, 7, 9]`

---

### 🔁 Functions Involved

1. `reverseStack(stack<int>& st)`  
   - Pops the top element.
   - Recursively reverses the rest.
   - Inserts the popped element at the bottom.

2. `insertAtBottom(stack<int>& st, int item)`  
   - Inserts `item` at the bottom of the stack using recursion.

---

### 🧪 Dry Run Step-by-Step

Let’s simulate the function calls and stack state at each step.

#### Initial Stack:  
`st = [9, 7, 4, 3]` (top is `3`)

---

#### 🔄 Step 1: `reverseStack(st)`
- Pop `3`, call `reverseStack([9, 7, 4])`

#### 🔄 Step 2: `reverseStack([9, 7, 4])`
- Pop `4`, call `reverseStack([9, 7])`

#### 🔄 Step 3: `reverseStack([9, 7])`
- Pop `7`, call `reverseStack([9])`

#### 🔄 Step 4: `reverseStack([9])`
- Pop `9`, call `reverseStack([])`

#### 🔄 Step 5: `reverseStack([])`
- Stack is empty → base case reached → return

---

### 🔁 Now we backtrack and insert each popped item at the bottom:

#### 🔙 Backtrack 1: insert `9` at bottom of `[]`  
- Stack becomes `[9]`

#### 🔙 Backtrack 2: insert `7` at bottom of `[9]`  
- Pop `9`, insert `7` into `[]` → `[7]`, push `9` back → `[7, 9]`

#### 🔙 Backtrack 3: insert `4` at bottom of `[7, 9]`  
- Pop `9`, pop `7`, insert `4` into `[]` → `[4]`, push `7`, `9` back → `[4, 7, 9]`

#### 🔙 Backtrack 4: insert `3` at bottom of `[4, 7, 9]`  
- Pop `9`, `7`, `4`, insert `3` into `[]` → `[3]`, push `4`, `7`, `9` back → `[3, 4, 7, 9]`

---

### ✅ Final Stack

```
Top → 3
       4
       7
Bottom → 9
```

---

### 🧩 Stack Trace Summary

| Call Stack Depth | Function Call         | Stack Before | Stack After      |
|------------------|------------------------|---------------|------------------|
| 1                | reverseStack([9,7,4,3]) | [9,7,4,3]     | [3,4,7,9]        |
| 2                | reverseStack([9,7,4])   | [9,7,4]       | [4,7,9]          |
| 3                | reverseStack([9,7])     | [9,7]         | [7,9]            |
| 4                | reverseStack([9])       | [9]           | [9]              |
| 5                | reverseStack([])        | []            | []               |

---
Chalo stack reversal ke **Time Complexity (TC)** aur **Space Complexity (SC)** ko ekdum detail mein samjhte hain, step-by-step, Hindi + technical style mein 😄

---

## 🔁 Stack Reversal Using Recursion – Deep Dive

### 📌 Problem Statement
Ek stack diya gaya hai:  
`[9, 7, 4, 3]` → Top pe 9 hai, bottom pe 3.

Goal: Is stack ko reverse karna hai using recursion. Final stack hona chahiye:  
`[3, 4, 7, 9]` → Top pe 3, bottom pe 9.

---

## 🧠 Algorithm Overview

### 🔹 Function 1: `reverseStack(stack<int>& st)`
- Top element ko pop karo.
- Baaki stack ko recursively reverse karo.
- Us popped element ko bottom mein daal do using `insertAtBottom()`.

### 🔹 Function 2: `insertAtBottom(stack<int>& st, int item)`
- Agar stack empty hai → item ko push karo.
- Nahi toh top ko pop karo, `insertAtBottom()` call karo, fir top ko wapas push karo.

---

## ⏱️ Time Complexity (TC)

### 🔍 Breakdown:

#### 1. `reverseStack()`:
- Stack ke har element ke liye ek recursive call hoti hai.
- Total **n** calls (n = number of elements).

#### 2. `insertAtBottom()`:
- Har element ko bottom mein daalne ke liye stack ko traverse karna padta hai.
- Worst case mein har insert ke liye **O(n)** time lagta hai.

### 🧮 Total Time:
- First insert: O(1)
- Second insert: O(2)
- Third insert: O(3)
- ...
- Last insert: O(n)

Sum = `1 + 2 + 3 + ... + n = n(n+1)/2 = O(n²)`

✅ **Final Time Complexity: `O(n²)`**

---

## 🧠 Space Complexity (SC)

### 🔍 Breakdown:

#### 1. `reverseStack()`:
- Recursion depth = n → uses **O(n)** space

#### 2. `insertAtBottom()`:
- Har insert ke liye bhi recursion depth = n (but not simultaneously)

So, maximum stack depth at any time = **O(n)**

✅ **Final Space Complexity: `O(n)`**

---

## 📊 Summary Table

| Function            | Time Complexity | Space Complexity |
|---------------------|------------------|------------------|
| `reverseStack()`    | O(n²)            | O(n)             |
| `insertAtBottom()`  | O(n) per insert  | O(n)             |

---

Agar tu chahe toh main iska **iterative version** bhi samjha sakta hoon, jo faster hota hai (O(n) time). Ya fir iska **C++ code** likh ke step-by-step dry run bhi kar sakta hoon. Batao, kis direction mein chalein? 😄
Would you 

like me to visualize this with a diagram or animate the stack changes step-by-step?
