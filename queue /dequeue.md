Nice one ✅ Now let’s move to Deque (Double Ended Queue).

A Deque (pronounced “deck”) is like a queue but more flexible:
	•	You can insert and delete elements from both front and rear.
	•	STL provides it in <deque> header.

⸻

Operations of Deque

Operation	Description
push_front(x)	Insert element at front
push_back(x)	Insert element at rear
pop_front()	Remove element from front
pop_back()	Remove element from rear
front()	Access first element
back()	Access last element
empty()	Check if deque is empty
size()	Number of elements


⸻

STL Deque Example

#include <iostream>
#include <deque>   // for deque
using namespace std;

int main() {
    deque<int> dq;

    // Insert at back
    dq.push_back(10);
    dq.push_back(20);

    // Insert at front
    dq.push_front(5);
    dq.push_front(1);

    cout << "Deque elements: ";
    for (int x : dq) cout << x << " ";  // prints 1 5 10 20
    cout << endl;

    cout << "Front element: " << dq.front() << endl; // 1
    cout << "Back element: " << dq.back() << endl;   // 20

    // Remove from front and back
    dq.pop_front(); // removes 1
    dq.pop_back();  // removes 20

    cout << "After pop operations: ";
    for (int x : dq) cout << x << " ";  // prints 5 10
    cout << endl;

    return 0;
}


⸻

Output

Deque elements: 1 5 10 20
Front element: 1
Back element: 20
After pop operations: 5 10


⸻

⚡ When to use Deque?
	•	If you need both stack and queue functionality in one container.
	•	Useful in sliding window problems, palindrome check, max/min in range problems.

⸻

👉 Do you want me to also explain how Deque is implemented internally (using a dynamic array of blocks) or just stick to STL usage?
