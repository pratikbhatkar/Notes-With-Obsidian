### `std::stack` (LIFO Adaptor)

- **`std::stack`** is an **adaptor container** that provides a **Last-In, First-Out (LIFO)** data structure. Think of it like a stack of plates: the last plate you put on is the first one you take off.
- It doesn't store elements directly but **adapts** an underlying container (like `std::deque` by default, or `std::vector`, `std::list`) to provide stack functionality.
- It offers a restricted interface: you can only **push** elements onto the top, **pop** elements from the top, and **access** the top element.

#### **Underlying Mechanism:**

`std::stack` is not a standalone container. Instead, it wraps another sequential container. By default, it uses `std::deque` as its underlying container. You can explicitly specify `std::vector` or `std::list` if needed. It exposes only a subset of the underlying container's functionality to enforce the LIFO principle.

#### **Advantages:**

- **LIFO Semantics**: Perfectly models scenarios where LIFO behavior is required (e.g., function call stack, undo/redo features, parsing expressions).
- **Simplicity**: Its restricted interface (`push`, `pop`, `top`) makes it simple to use and prevents misuse of the underlying container's more flexible operations.
- **Efficiency**: Operations (`push`, `pop`, `top`, `empty`, `size`) have **O(1) time complexity** because they only involve operations at one end of the underlying container (usually `push_back`, `pop_back`, `back` for `std::deque` or `std::vector`).
- **Adaptability**: Can be built on different underlying containers (`std::deque` by default, `std::vector`, `std::list`) allowing for flexibility in performance characteristics (e.g., `std::vector` for better cache locality, `std::list` if middle insertions/deletions were conceptually needed on the _underlying_ container, though `stack` itself doesn't expose this).

#### **Disadvantages:**

- **Limited Access**: You can only access the **top element**. There's no way to access elements in the middle or at the bottom of the stack without popping elements off.
- **No Iterators**: `std::stack` does not provide iterators. You cannot traverse the stack elements directly. If you need to iterate, you must pop elements, store them, and then push them back, or use the underlying container directly (defeating the purpose of `std::stack`).
- **No Random Access**: Elements cannot be accessed by an index.
- **Memory Overhead (Inherited)**: Inherits memory overhead characteristics from its underlying container. For example, if using `std::vector`, reallocations can occur.

#### **Declaration of `stack`:**

C++

```
#include <iostream>
#include <stack>
#include <vector>     // For using std::vector as underlying container
#include <deque>      // Default underlying container for std::stack
#include <list>       // For using std::list as underlying container
#include <string>

using namespace std;

int main() {

    // Default initialization (uses std::deque as underlying container)
    stack<int> myStack;

    // Explicitly using std::vector as the underlying container
    stack<string, vector<string>> myStringStack;

    // Explicitly using std::list as the underlying container
    stack<char, list<char>> myCharStack;

}
```

#### **Insertion & Accessing in Stack:**

C++

```
#include <iostream>
#include <stack>
#include <string>

using namespace std;

int main() {
    stack<int> myStack;

    // Push elements onto the stack
    myStack.push(10); // Stack: [10]
    myStack.push(20); // Stack: [10, 20]
    myStack.push(30); // Stack: [10, 20, 30] (30 is top)

    cout << "Top element: " << myStack.top() << endl; // Output: 30
    cout << "Stack size: " << myStack.size() << endl; // Output: 3

    // Push another element
    myStack.push(40); // Stack: [10, 20, 30, 40] (40 is top)
    cout << "New top element: " << myStack.top() << endl; // Output: 40

    // Note: Direct traversal with a loop is not possible with std::stack's interface
    // To 'see' all elements, you must pop them off.
    cout << "Popping elements to demonstrate LIFO: ";
    while (!myStack.empty()) {
        cout << myStack.top() << " "; // Access top
        myStack.pop();                      // Remove top
    }
    cout << endl; // Output: 40 30 20 10

    return 0;
}
```

#### **Deletion in Stack:**

C++

```
#include <iostream>
#include <stack>

using namespace std;

int main() {
    stack<int> myStack;
    myStack.push(10);
    myStack.push(20);
    myStack.push(30);
    myStack.push(40); // 40 is at the top

    cout << "Initial stack size: " << myStack.size() << endl; // Output: 4
    cout << "Top element: " << myStack.top() << endl;         // Output: 40

    // Pop an element from the top
    myStack.pop(); // Removes 40
    cout << "After first pop, new top: " << myStack.top() << endl; // Output: 30
    cout << "Stack size: " << myStack.size() << endl; // Output: 3

    // Pop another element
    myStack.pop(); // Removes 30
    cout << "After second pop, new top: " << myStack.top() << endl; // Output: 20
    cout << "Stack size: " << myStack.size() << endl; // Output: 2

    // Clear the stack (by repeatedly popping)
    while (!myStack.empty()) {
        myStack.pop();
    }

    cout << "Stack size after clear: " << myStack.size() << endl; // Output: 0
    cout << "Is stack empty? " << (myStack.empty() ? "Yes" : "No") << endl; // Output: Yes

    return 0;
}
```

#### **Member functions:**

- `push(element)`: Adds an `element` to the top of the stack. O(1) time complexity.
    
- `pop()`: Removes the top element from the stack. It does not return the element. O(1) time complexity.
    
- `top()`: Returns a reference to the top element of the stack. **Does not remove the element.** O(1) time complexity. Be careful not to call on an empty stack.
    
- `empty()`: Checks if the stack is empty. Returns `true` if empty, `false` otherwise. O(1) time complexity.
    
- `size()`: Returns the number of elements in the stack. O(1) time complexity.