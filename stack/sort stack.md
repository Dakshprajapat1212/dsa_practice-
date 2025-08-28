https://drive.google.com/file/d/1fwiLdh_Bs7mebGgAIXjSAEc9h_5ZbGOf/view



<img width="1470" height="956" alt="Screenshot 2025-08-28 at 4 31 25 PM" src="https://github.com/user-attachments/assets/30363904-dd2b-455f-b4eb-00df1ed62e52" />


<img width="1470" height="956" alt="Screenshot 2025-08-28 at 4 32 15 PM" src="https://github.com/user-attachments/assets/23fdff27-81eb-41ae-905d-1ec43fe6f59e" />


waps backing type sorted way mai aranage krna hai
<img width="1470" height="956" alt="Screenshot 2025-08-28 at 4 33 19 PM" src="https://github.com/user-attachments/assets/67a1d9e1-ec3e-4d05-9e4c-d8884ea3f19b" />
<img width="1470" height="956" alt="Screenshot 2025-08-28 at 4 34 12 PM" src="https://github.com/user-attachments/assets/dced006d-c6bd-4efd-8307-2d5f84f39661" />


dekh waps dalthe tym comapre krna hai 

phle ki jese hai side  mai krthe jana  haid compare kr krr

dekh chotha hai jaha dal de 

aur joph elemet ko loya tha usko waps dal the jan hai  
Here’s a complete solution to the **Sort a Stack** problem using recursion, along with a clear explanation, dry run, and time-space complexity analysis.

---

### ✅ Final Code

```cpp
class SortedStack {
public:
    stack<int> s;

    void sort() {
        if (s.empty()) return;

        int top = s.top();
        s.pop();

        sort();  // Recursively sort remaining stack

        insertSorted(top);  // Insert popped element in sorted order
    }

private:
    void insertSorted(int x) {
        if (s.empty() || s.top() < x) {
            s.push(x);
            return;
        }

        int temp = s.top();
        s.pop();

        insertSorted(x);  // Recursively find correct position

        s.push(temp);  // Push back the popped element
    }
};
```

---

### 🧠 Explanation

We use **two recursive functions**:

1. `sort()` — Pops all elements until the stack is empty, then inserts each one back in sorted order.
2. `insertSorted(x)` — Inserts an element `x` into its correct position in the already sorted stack.

This avoids using any extra data structures and relies only on the call stack.

---

### 🧪 Dry Run Example

Initial stack (top to bottom): `11, 2, 32, 3, 41`

#### Step 1: `sort()` pops all elements
- Pop `11`, `2`, `32`, `3`, `41` → stack becomes empty

#### Step 2: Start inserting back using `insertSorted`
- Insert `41` → stack: `41`
- Insert `3` → goes below `41` → stack: `41, 3`
- Insert `32` → goes between `41` and `3` → stack: `41, 32, 3`
- Insert `2` → bottom → stack: `41, 32, 3, 2`
- Insert `11` → between `32` and `3` → stack: `41, 32, 11, 3, 2`

✅ Final sorted stack (top to bottom): `41, 32, 11, 3, 2`

---

### ⏱️ Time and Space Complexity

| Metric              | Value     | Why? |
|---------------------|-----------|------|
| Time Complexity     | O(n²)     | Each insertSorted call may traverse the entire stack |
| Space Complexity    | O(n)      | Due to recursion stack |

---

### 📌 Notes from GeeksforGeeks

- You must sort the stack so that the **top has the greatest element**.
- You’re not allowed to use any extra data structures like arrays or vectors.
- This solution meets the expected constraints:  
  - Time: O(N²)  
  - Space: O(N) (auxiliary)

---

Let me know if you want to visualize the recursion flow or convert this logic to Python or Java.

