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
Absolutely — the **LRU (Least Recently Used) Cache** pattern is a classic in system design, and it shows up in many real-world systems where memory or speed is limited. Here are some compelling examples:

---

## 🖥️ 1. **Operating System Page Replacement**
- **Scenario**: When your computer runs multiple programs, it stores pages (chunks of memory) in RAM.
- **Problem**: RAM is limited. When it’s full, the OS must decide which page to evict.
- **Solution**: LRU is often used to remove the page that hasn’t been accessed in the longest time.
- **Why it works**: It assumes that recently used pages are more likely to be used again soon.

---

## 🌐 2. **Web Browser Cache**
- **Scenario**: Browsers cache images, scripts, and stylesheets to load websites faster.
- **Problem**: Cache size is limited.
- **Solution**: LRU helps evict the least recently viewed resources when space runs out.
- **Example**: If you haven’t visited a site in weeks, its cached assets may be removed to make room for newer ones.

---

## 📱 3. **Mobile App Image Caching**
- **Scenario**: Apps like Instagram or Twitter load lots of images.
- **Problem**: Storing all images would consume too much memory.
- **Solution**: LRU caches the most recently viewed images and discards older ones.
- **Bonus**: This keeps scrolling smooth without bloating memory.

---

## 🧠 4. **Database Query Caching**
- **Scenario**: Databases like Redis or Memcached store query results to speed up repeated access.
- **Problem**: Cache size is limited.
- **Solution**: LRU ensures that frequently accessed queries stay cached, while stale ones are removed.

---

## 🚗 5. **Navigation Systems (GPS)**
- **Scenario**: A GPS app stores recent routes and map tiles.
- **Problem**: Limited storage on the device.
- **Solution**: LRU keeps frequently traveled routes and discards old ones.

---

Want me to walk you through how one of these examples would be implemented in code or architecture diagrams? I can even simulate a cache eviction scenario for fun.
