Got it — here’s a **complete, structured set of notes** for the LRU Cache lecture you’re watching, with **problem statement, dry run, diagrams, pseudo-code, helper functions, and time/space complexity** all in one place.  

---

# 📒 LRU Cache – Full Lecture Notes

## 1️⃣ Problem Statement
Design a data structure that implements the **Least Recently Used (LRU) Cache** with:
- **Constructor**: `LRUCache(capacity)` → sets max capacity.
- **get(key)** → returns value if present, else `-1`. Marks the key as most recently used.
- **put(key, value)** → inserts/updates key-value. If full, evicts least recently used key.
- **Constraint**: All operations in **O(1)** average time.

---

## 2️⃣ Core Idea
We need:
- **Fast lookup** → `HashMap` (key → node reference).
- **Fast reordering** → `Doubly Linked List` (most recent at head, least recent at tail).

---

## 3️⃣ Data Structures

### Doubly Linked List Node
```
Node {
    key
    value
    prev
    next
}
```

### Supporting Structures
- **HashMap**: `{ key → Node* }`
- **Dummy Head & Dummy Tail**:  
  ```
  head <-> ... <-> tail
  ```
  - Head’s next = most recent
  - Tail’s prev = least recent

---

## 4️⃣ Diagram – Initial State

```
head(-1,-1) <-> tail(-1,-1)
Map: {}
Capacity: N
```

---

## 5️⃣ Operations

### **get(key)**
1. If key not in map → return `-1`.
2. Else:
   - Get node from map.
   - Remove node from current position.
   - Insert node right after head.
   - Return node.value.

---

### **put(key, value)**
1. If key exists:
   - Update node.value.
   - Move node to front (after head).
2. Else:
   - If cache full:
     - Remove tail.prev (LRU node) from DLL.
     - Remove its key from map.
   - Create new node.
   - Insert after head.
   - Add to map.

---

## 6️⃣ Helper Functions

### deleteNode(node)
```pseudo
prevNode = node.prev
nextNode = node.next
prevNode.next = nextNode
nextNode.prev = prevNode
```

---

### insertAfterHead(node)
```pseudo
currAfterHead = head.next
head.next = node
node.prev = head
node.next = currAfterHead
currAfterHead.prev = node
```

---

## 7️⃣ Pseudo-code – Full LRUCache

```pseudo
class LRUCache:
    map: HashMap<int, Node*>
    capacity: int
    head: Node
    tail: Node

    constructor(capacity):
        this.capacity = capacity
        head = new Node(-1, -1)
        tail = new Node(-1, -1)
        head.next = tail
        tail.prev = head

    get(key):
        if key not in map:
            return -1
        node = map[key]
        deleteNode(node)
        insertAfterHead(node)
        return node.value

    put(key, value):
        if key in map:
            node = map[key]
            node.value = value
            deleteNode(node)
            insertAfterHead(node)
        else:
            if map.size == capacity:
                lru = tail.prev
                deleteNode(lru)
                map.remove(lru.key)
            newNode = new Node(key, value)
            insertAfterHead(newNode)
            map[key] = newNode
```

---

## 8️⃣ Dry Run Example

Capacity = 4  
Operations:
```
put(2,6) → [2:6]
put(4,7) → [4:7, 2:6]
put(8,11) → [8:11, 4:7, 2:6]
put(7,10) → [7:10, 8:11, 4:7, 2:6]
get(2) → move 2 to front → [2:6, 7:10, 8:11, 4:7]
get(8) → move 8 to front → [8:11, 2:6, 7:10, 4:7]
put(5,6) → evict 4:7 → [5:6, 8:11, 2:6, 7:10]
get(7) → move 7 to front → [7:10, 5:6, 8:11, 2:6]
put(5,7) → update value, move to front → [5:7, 7:10, 8:11, 2:6]
```

---

## 9️⃣ Time & Space Complexity

| Operation | Time Complexity | Reason |
|-----------|----------------|--------|
| get       | O(1)            | HashMap lookup + DLL pointer changes |
| put       | O(1)            | HashMap insert/remove + DLL pointer changes |
| deleteNode / insertAfterHead | O(1) | Constant pointer updates |

**Space Complexity**:  
- O(capacity) for HashMap + O(capacity) for DLL nodes → **O(capacity)** total.

---

✅ **Key Takeaways**:
- **HashMap + Doubly Linked List** is the standard LRU pattern.
- Dummy head/tail simplify edge cases.
- Always reuse nodes when moving to front — don’t create new ones unnecessarily.
- Eviction is always `tail.prev`.

---

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
