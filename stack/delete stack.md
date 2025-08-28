 https://youtu.be/BmZnJehDzyU?si=MkcQVZx7MdwTZRuV
 ![Uploading Screenshot 2025-08-28 at 10.46.47 AM.png…]()

To delete the **middle element of a stack using recursion**, without using any extra data structures, here’s a simple and clear explanation:

---

### 🧠 Problem Statement

Given a stack, remove the **middle element**.  
Middle is defined as:  
**index = floor((size + 1) / 2)** (1-based indexing from bottom)

Example:  
Stack = `[10, 20, 30, 40, 50]` (top to bottom)  
Middle = `30` → After deletion: `[50, 40, 20, 10]`

---

### 🪜 Approach (Recursive)

1. Count total size of stack.
2. Use recursion to pop elements until you reach the middle.
3. Skip pushing the middle element back.
4. Push all other elements back in reverse order.

---

### ✅ C++ Code

```cpp
void deleteMid(stack<int>& s, int current, int size) {
    // Base case: reached middle
    if (current == size / 2) {
        s.pop();
        return;
    }

    int temp = s.top();
    s.pop();

    // Recursive call
    deleteMid(s, current + 1, size);

    // Push back other elements
    s.push(temp);
}

void deleteMiddle(stack<int>& s) {
    int size = s.size();
    deleteMid(s, 0, size);
}
```

---

### 🧪 Dry Run Example

Stack: `[10, 20, 30, 40, 50]`  
Size = 5 → Middle index = 2 (0-based)

Steps:
- Pop `50` → call again
- Pop `40` → call again
- Pop `30` → this is middle → don’t push back
- Push back `40`, then `50`

Final stack: `[50, 40, 20, 10]`

---

### ⏱️ Time and Space Complexity

- Time: O(n)  
- Space: O(n) (due to recursion)

---

Let me know if you want this in Python or Java, or if you'd like a visual diagram to help you understand the recursion flow.
