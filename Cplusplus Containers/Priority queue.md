### `std::priority_queue` (Priority-based Adaptor)

- **`std::priority_queue`** is an **adaptor container** that provides a **max-heap** data structure by default. This means the element with the highest value is always at the top.
- It functions like a queue, but instead of strict FIFO, elements are ordered based on their **priority** (their value). The "highest priority" element is always the one you can access and remove.
- Like `std::stack` and `std::queue`, it's an **adaptor**, meaning it doesn't store elements directly but uses an underlying container (by default, `std::vector`) to achieve its functionality.

#### **Underlying Mechanism:**

`std::priority_queue` is typically implemented using a **max-heap** data structure, which itself is often built on top of a `std::vector`. A heap is a specialized tree-based data structure that satisfies the heap property: for a max-heap, the value of each parent node is greater than or equal to the value of its children. This property ensures that the largest element is always at the root (top) of the heap. When elements are pushed or popped, the heap structure is re-balanced to maintain this property.

#### **Advantages:**

- **Efficient Retrieval of Highest Priority Element**: Accessing the top (highest priority) element is an **O(1)** operation.
- **Efficient Insertion**: Adding an element has a time complexity of **O(logn)** as it involves inserting and then potentially "bubbling up" the element to maintain the heap property.
- **Efficient Deletion (of top element)**: Removing the top element (highest priority) has a time complexity of **O(logn)** as it involves removing the root and then "sifting down" a new root to maintain the heap property.
- **Guaranteed Top Element**: Always provides direct access to the element with the highest priority.
- **Adaptability**: Can use various underlying containers (`std::vector` is default and common, `std::deque` can also be used). You can also provide a custom comparator to make it a min-heap (smallest element at the top).

#### **Disadvantages:**

- **No Random Access**: You can only access the **top element**. There's no way to access or iterate over other elements without removing the top ones.
- **No Iterators**: `std::priority_queue` does not provide iterators. You cannot traverse the elements directly. If you need to inspect all elements, you must pop them off.
- **Limited Flexibility**: Its interface is very restricted, tailored specifically for priority queue operations. You cannot perform operations like finding an arbitrary element, deleting a specific element (other than the top), or modifying elements.
- **Memory Overhead (Inherited)**: Inherits memory characteristics from its underlying container.
- **Logarithmic Time for Push/Pop**: While efficient, O(logn) is slower than the O(1) of stack/queue operations, but it's the cost of maintaining priority order.

#### **Declaration of `priority_queue`:**

C++

```
#include <iostream>
#include <queue>      // Required for std::priority_queue
#include <vector>     // Default underlying container
#include <functional> // For std::greater (to create a min-priority_queue)

using namespace std;

int main() {

    // Default max-priority_queue (largest element at top, uses vector by default)
    priority_queue<int> max_pq;

    // Min-priority_queue (smallest element at top)
    // Uses vector as underlying container, with std::greater as comparator
    priority_queue<int, vector<int>, greater<int>> min_pq;

    // priority_queue with a different underlying container (e.g., deque)
    priority_queue<double, deque<double>> double_pq;

}
```

#### **Insertion & Accessing in `priority_queue`:**

C++

```
#include <iostream>
#include <queue> // For priority_queue

using namespace std;

int main() {
    priority_queue<int> my_pq; // Max-priority_queue by default

    // Push elements onto the priority_queue
    my_pq.push(10); // PQ: {10}
    my_pq.push(30); // PQ: {30, 10} (30 is top)
    my_pq.push(20); // PQ: {30, 10, 20} (internal order, 30 still top)
    my_pq.push(50); // PQ: {50, 30, 20, 10} (50 is top)

    cout << "Top element: " << my_pq.top() << endl; // Output: 50
    cout << "Priority Queue size: " << my_pq.size() << endl; // Output: 4

    // You can only access the top element directly.
    // To see other elements, you must pop the top ones.
    cout << "Popping elements to demonstrate priority order: ";
    while (!my_pq.empty()) {
        cout << my_pq.top() << " "; // Access top element
        my_pq.pop();               // Remove top element
    }
    cout << endl; // Output: 50 30 20 10 (always in decreasing order for max-pq)

    return 0;
}
```

#### **Deletion in `priority_queue`:**

C++

```
#include <iostream>
#include <queue> // For priority_queue

using namespace std;

int main() {
    priority_queue<int> my_pq;
    my_pq.push(10);
    my_pq.push(20);
    my_pq.push(5);
    my_pq.push(30); // PQ: {30, 20, 10, 5} (30 is top)

    cout << "Initial priority_queue size: " << my_pq.size() << endl; // Output: 4
    cout << "Top element: " << my_pq.top() << endl;                 // Output: 30

    // Pop the top element
    my_pq.pop(); // Removes 30
    cout << "After first pop, new top: " << my_pq.top() << endl; // Output: 20
    cout << "Priority Queue size: " << my_pq.size() << endl;     // Output: 3

    // Pop another element
    my_pq.pop(); // Removes 20
    cout << "After second pop, new top: " << my_pq.top() << endl; // Output: 10
    cout << "Priority Queue size: " << my_pq.size() << endl;     // Output: 2

    // Clear the priority_queue (by repeatedly popping)
    while (!my_pq.empty()) {
        my_pq.pop();
    }

    cout << "Priority Queue size after clear: " << my_pq.size() << endl; // Output: 0
    cout << "Is priority_queue empty? " << (my_pq.empty() ? "Yes" : "No") << endl; // Output: Yes

    return 0;
}
```

#### **Member functions:**

- `push(element)`: Adds an `element` to the `priority_queue`. The element is placed in its correct sorted position based on priority. O(logn) time complexity.
    
- `pop()`: Removes the top (highest priority) element from the `priority_queue`. It does not return the element. O(logn) time complexity.
    
- `top()`: Returns a `const` reference to the top (highest priority) element. **Does not remove the element.** O(1) time complexity. Be careful not to call on an empty `priority_queue`.
    
- `empty()`: Checks if the `priority_queue` is empty. Returns `true` if empty, `false` otherwise. O(1) time complexity.
    
- `size()`: Returns the number of elements in the `priority_queue`. O(1) time complexity.