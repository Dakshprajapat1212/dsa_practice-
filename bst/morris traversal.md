inorder


<img width="1470" height="956" alt="Screenshot 2025-09-16 at 4 10 20 PM" src="https://github.com/user-attachments/assets/023d0475-69da-43a0-a8ea-d31e56427aa7" />


Here’s a clean, well‑commented **C++ implementation** of **Morris Inorder Traversal** — the space‑efficient way to traverse a binary tree without recursion or a stack.

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

// Morris Inorder Traversal
void morrisInorder(Node* root) {
    Node* curr = root;
    while (curr != nullptr) {
        if (curr->left == nullptr) {
            // No left child → visit this node and move right
            cout << curr->data << " ";
            curr = curr->right;
        } else {
            // Find inorder predecessor (rightmost node in left subtree)
            Node* pred = curr->left;
            while (pred->right != nullptr && pred->right != curr) {
                pred = pred->right;
            }

            if (pred->right == nullptr) {
                // Make a temporary thread to the current node
                pred->right = curr;
                curr = curr->left;
            } else {
                // Thread already exists → remove it and visit current
                pred->right = nullptr;
                cout << curr->data << " ";
                curr = curr->right;
            }
        }
    }
}

int main() {
    /*
            1
           / \
          2   3
         / \
        4   5
    */
    Node* root = new Node(1);
    root->left = new Node(2);
    root->right = new Node(3);
    root->left->left = new Node(4);
    root->left->right = new Node(5);

    cout << "Morris Inorder Traversal: ";
    morrisInorder(root);
    cout << endl;

    return 0;
}
```

### How it works
- **If no left child** → print the node and move right.
- **If left child exists** → find the **inorder predecessor** (rightmost node in left subtree).
  - If predecessor’s `right` is `nullptr`, create a temporary link to `curr` and move left.
  - If predecessor’s `right` is `curr`, remove the link, print `curr`, and move right.

**Time Complexity:** O(n)  
**Space Complexity:** O(1) — no recursion or stack.

If you want, I can also give you the **Morris Preorder Traversal** version, which is just a small tweak of this code. Would you like me to add that?


