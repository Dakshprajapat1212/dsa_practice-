Here’s a clean, fully working **C++ implementation** of the LRU Cache exactly as explained in the lecture — using a **HashMap + Doubly Linked List** for **O(1)** `get` and `put` operations.  

---

## 💻 C++ Code – LRU Cache

```cpp
#include <bits/stdc++.h>
using namespace std;

class Node {
public:
    int key, value;
    Node* prev;
    Node* next;
    Node(int _key, int _value) {
        key = _key;
        value = _value;
        prev = nullptr;
        next = nullptr;
    }
};

class LRUCache {
private:
    int capacity;
    unordered_map<int, Node*> mp;
    Node* head;
    Node* tail;

    // Remove a node from the DLL
    void deleteNode(Node* node) {
        Node* prevNode = node->prev;
        Node* nextNode = node->next;
        prevNode->next = nextNode;
        nextNode->prev = prevNode;
    }

    // Insert a node right after head
    void insertAfterHead(Node* node) {
        Node* currAfterHead = head->next;
        head->next = node;
        node->prev = head;
        node->next = currAfterHead;
        currAfterHead->prev = node;
    }

public:
    LRUCache(int _capacity) {
        capacity = _capacity;
        head = new Node(-1, -1);
        tail = new Node(-1, -1);
        head->next = tail;
        tail->prev = head;
    }

    int get(int key) {
        if (mp.find(key) == mp.end()) return -1;
        Node* node = mp[key];
        deleteNode(node);
        insertAfterHead(node);
        return node->value;
    }

    void put(int key, int value) {
        if (mp.find(key) != mp.end()) {
            Node* node = mp[key];
            node->value = value;
            deleteNode(node);
            insertAfterHead(node);
        } else {
            if ((int)mp.size() == capacity) {
                Node* lru = tail->prev;
                mp.erase(lru->key);
                deleteNode(lru);
                delete lru;
            }
            Node* newNode = new Node(key, value);
            mp[key] = newNode;
            insertAfterHead(newNode);
        }
    }
};

// Example usage
int main() {
    LRUCache cache(4);
    cache.put(2, 6);
    cache.put(4, 7);
    cache.put(8, 11);
    cache.put(7, 10);
    cout << cache.get(2) << "\n"; // 6
    cout << cache.get(8) << "\n"; // 11
    cache.put(5, 6);              // Evicts key 4
    cout << cache.get(7) << "\n"; // 10
    cache.put(5, 7);              // Updates value of key 5
    cout << cache.get(5) << "\n"; // 7
}
```

---

## ⚡ Key Points
- **`unordered_map<int, Node*>`** → O(1) key lookup.
- **Doubly Linked List** → O(1) insertion/removal from any position.
- **Dummy head/tail** → simplifies edge cases.
- **Eviction** → always remove `tail->prev` (LRU node).

---

If you want, I can also give you the **Java** or **Python** version of this exact implementation so you can match it to your preferred language.  
Do you want me to prepare those as well?
