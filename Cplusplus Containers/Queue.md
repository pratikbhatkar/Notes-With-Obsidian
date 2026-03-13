### `std::queue` (FIFO Adaptor)

- **`std::queue`** is an **adaptor container** that provides a **First-In, First-Out (FIFO)** data structure. Think of it like a waiting line or a queue at a store: the first person to join the line is the first one to be served.
- It doesn't store elements directly but **adapts** an underlying container (like `std::deque` by default, or `std::list`) to provide queue functionality.
- It offers a restricted interface: you can only **push** elements onto the back, **pop** elements from the front, and **access** the front and back elements.

#### **Underlying Mechanism:**

`std::queue` is not a standalone container. It wraps another sequential container, typically `std::deque` by default. It can also use `std::list`. It exposes only a subset of the underlying container's functionality (`push_back`, `pop_front`, `front`, `back`) to strictly enforce the FIFO principle.

#### **Advantages:**

- **FIFO Semantics**: Perfectly models scenarios where FIFO behavior is required (e.g., breadth-first search, task scheduling, print spoolers).
- **Simplicity**: Its restricted interface (`push`, `pop`, `front`, `back`) makes it straightforward to use and prevents operations that would violate the queue's nature.
- **Efficiency**: Operations (`push`, `pop`, `front`, `back`, `empty`, `size`) typically have **O(1) time complexity** because they only involve operations at the ends of the underlying container.
- **Adaptability**: Can be built on different underlying containers (`std::deque` by default, `std::list`) allowing for flexibility. `std::vector` is generally not a good choice for `std::queue` because `pop_front()` would be O(n).

#### **Disadvantages:**

- **Limited Access**: You can only access the **front and back elements**. There's no way to access elements in the middle of the queue without dequeuing elements.
- **No Iterators**: `std::queue` does not provide iterators. You cannot traverse the queue elements directly. If you need to iterate, you must dequeue elements, process them, and then re-queue them or use the underlying container directly (which goes against the `std::queue` interface).
- **No Random Access**: Elements cannot be accessed by an index.
- **Memory Overhead (Inherited)**: Inherits memory overhead characteristics from its underlying container.

#### **Declaration of `queue`:**

C++

```
#include <iostream>
#include <queue>      // Required for std::queue
#include <deque>      // Default underlying container for std::queue
#include <list>       // For using std::list as underlying container
#include <string>

using namespace std;

int main() {

    // Default initialization (uses std::deque as underlying container)
    queue<int> myQueue;

    // Explicitly using std::list as the underlying container
    queue<string, list<string>> myStringQueue;

}
```

#### **Insertion & Accessing in Queue:**

C++

```
#include <iostream>
#include <queue>
#include <string>

using namespace std;

int main() {
    queue<int> myQueue;

    // Push elements onto the back of the queue
    myQueue.push(10); // Queue: [10]
    myQueue.push(20); // Queue: [10, 20]
    myQueue.push(30); // Queue: [10, 20, 30] (10 is front, 30 is back)

    cout << "Front element: " << myQueue.front() << endl; // Output: 10
    cout << "Back element: " << myQueue.back() << endl;   // Output: 30
    cout << "Queue size: " << myQueue.size() << endl;     // Output: 3

    // Push another element
    myQueue.push(40); // Queue: [10, 20, 30, 40] (10 is front, 40 is back)
    cout << "New back element: " << myQueue.back() << endl; // Output: 40

    // Note: Direct traversal with a loop is not possible with std::queue's interface
    // To 'see' all elements, you must dequeue them.
    cout << "Dequeuing elements to demonstrate FIFO: ";
    while (!myQueue.empty()) {
        cout << myQueue.front() << " "; // Access front
        myQueue.pop();                        // Remove front
    }
    cout << endl; // Output: 10 20 30 40

    return 0;
}
```

#### **Deletion in Queue:**

C++

```
#include <iostream>
#include <queue>

using namespace std;

int main() {
    queue<int> myQueue;
    myQueue.push(10);
    myQueue.push(20);
    myQueue.push(30);
    myQueue.push(40); // 10 is front

    cout << "Initial queue size: " << myQueue.size() << endl; // Output: 4
    cout << "Front element: " << myQueue.front() << endl;     // Output: 10

    // Pop an element from the front
    myQueue.pop(); // Removes 10
    cout << "After first pop, new front: " << myQueue.front() << endl; // Output: 20
    cout << "Queue size: " << myQueue.size() << endl; // Output: 3

    // Pop another element
    myQueue.pop(); // Removes 20
    cout << "After second pop, new front: " << myQueue.front() << endl; // Output: 30
    cout << "Queue size: " << myQueue.size() << endl; // Output: 2

    // Clear the queue (by repeatedly popping)
    while (!myQueue.empty()) {
        myQueue.pop();
    }

    cout << "Queue size after clear: " << myQueue.size() << endl; // Output: 0
    cout << "Is queue empty? " << (myQueue.empty() ? "Yes" : "No") << endl; // Output: Yes

    return 0;
}
```

#### **Member functions:**

- `push(element)`: Adds an `element` to the back of the queue. O(1) time complexity.
    
- `pop()`: Removes the front element from the queue. It does not return the element. O(1) time complexity.
    
- `front()`: Returns a reference to the front element of the queue. **Does not remove the element.** O(1) time complexity. Be careful not to call on an empty queue.
    
- `back()`: Returns a reference to the back element of the queue. **Does not remove the element.** O(1) time complexity. Be careful not to call on an empty queue.
    
- `empty()`: Checks if the queue is empty. Returns `true` if empty, `false` otherwise. O(1) time complexity.
    
- `size()`: Returns the number of elements in the queue. O(1) time complexity.