### `std::list` (Doubly Linked List)

- **`std::list`** is a sequence container that implements a **doubly linked list**.
- Each element (node) stores its value, a pointer to the **next** element, and a pointer to the **previous** element.
- Elements are **not stored in contiguous memory locations**.
- Offers **efficient insertion and deletion** anywhere in the list.
- Supports **bidirectional iteration** (forward and backward).

#### **Underlying Mechanism:**

`std::list` is composed of individual nodes, dynamically allocated in non-contiguous memory. Each node contains the actual data, and two pointers: one pointing to the next node in the sequence and another pointing to the previous node. This structure allows for constant-time modifications (insertions/deletions) once the position (iterator) is known, because only a few pointers need to be updated.

#### **Advantages:**

- **Efficient Insertion and Deletion Anywhere**: Inserting or deleting an element at _any position_ (beginning, middle, or end) takes **O(1) time complexity** (assuming you have an iterator to that position). This is the primary advantage over `std::vector`.
- **No Reallocations**: Unlike `std::vector`, `std::list` does not need to reallocate its entire contents and copy elements when it grows, as nodes are individually allocated.
- **Iterator Stability**: Iterators (and references/pointers) to elements remain valid even after insertions or deletions elsewhere in the list (unless the element itself is deleted).
- **Efficient `splice()` Operations**: The `splice()` operation, which transfers elements from one list to another, is very efficient (O(1)) as it only involves pointer re-patching, not copying elements.

#### **Disadvantages:**

- **No Random Access**: Elements cannot be accessed directly by index (e.g., `myList[i]`). To access an element, you must traverse the list from the beginning or end, resulting in **O(n) time complexity** for random access.
- **Higher Memory Usage**: Each element requires extra memory for two pointers (next and previous), increasing memory consumption compared to `std::vector` (which only stores the data itself contiguously).
- **Poor Cache Performance**: Due to non-contiguous memory allocation, elements are scattered in memory, leading to poor cache locality. This can make sequential iteration slower than `std::vector` in practice, despite having O(n) complexity for both.
- **Slower Traversal**: While O(n), the lack of cache locality can make iterating through a large `std::list` significantly slower than iterating through a `std::vector`.

#### **Declaration of `list`:**

C++

```
#include <iostream>
#include <list>
#include <string>

using namespace std;

int main() {

    // Default initialization (empty list)
    list<int> myList; // declaration of list with integer datatype

    // Declaration with some initial values
    list<string> myStringList = {"apple", "banana", "cherry"};

    // Declaration with initial size and value
    list<double> myDoubleList(5, 3.14); // list of 5 doubles, all 3.14

}
```

#### **Insertion & Traversing in List:**

C++

```
#include <iostream>
#include <list>
#include <algorithm> // For std::advance, std::find

using namespace std;

int main() {
    list<int> myList;

    // Insertion at the end (efficient)
    myList.push_back(10);
    myList.push_back(30);

    // Insertion at the front (efficient)
    myList.push_front(5); // myList now: {5, 10, 30}

    // Insertion at a specific position (efficient if iterator is known)
    auto it = myList.begin();
    advance(it, 2); // Move iterator 2 positions forward, to 30
    myList.insert(it, 20); // Insert 20 before 30. myList now: {5, 10, 20, 30}

    cout << "Elements in the list (using iterator): ";
    list<int>::iterator listItr;
    for(listItr = myList.begin() ; listItr != myList.end() ; listItr++){
        cout << *listItr << " "; // Output: 5 10 20 30
    }
    cout << endl;

    // Using range-based for loop (recommended for simplicity)
    cout << "Elements using range-based for loop: ";
    for (const auto& element : myList) {
        cout << element << " "; // Output: 5 10 20 30
    }
    cout << endl;

    return 0;
}
```

#### **Deletion in List:**

C++

```
#include <iostream>
#include <list>
#include <algorithm> // For std::find

using namespace std;

int main() {
    list<int> myList = {10, 20, 30, 40, 50};

    cout << "Initial list: ";
    for (int val : myList) {
        cout << val << " ";
    }
    cout << endl;

    // Delete from the front (efficient)
    myList.pop_front(); // Removes 10
    cout << "After pop_front: ";
    for (int val : myList) {
        cout << val << " ";
    }
    cout << endl;

    // Delete from the end (efficient)
    myList.pop_back(); // Removes 50
    cout << "After pop_back: ";
    for (int val : myList) {
        cout << val << " "; // Output: 20 30 40
    }
    cout << endl;

    // Delete at a specific position using an iterator
    // Find element 30
    auto it_to_erase = find(myList.begin(), myList.end(), 30);
    if (it_to_erase != myList.end()) {
        myList.erase(it_to_erase); // Removes 30. myList now: {20, 40}
    }
    cout << "After finding and erasing 30: ";
    for (int val : myList) {
        cout << val << " ";
    }
    cout << endl;

    // Clear the list
    myList.clear();
    cout << "List size after clear: " << myList.size() << endl; // Output: 0

    return 0;
}
```

#### **Member functions:**

- `push_front(element)`: Adds an `element` to the beginning of the list. O(1) time complexity.
    
- `push_back(element)`: Adds an `element` to the end of the list. O(1) time complexity.
    
- `pop_front()`: Removes the first element. O(1) time complexity.
    
- `pop_back()`: Removes the last element. O(1) time complexity.
    
- `insert(position, element)`: Inserts `element` at the specified `position` (iterator). O(1) time complexity.
    
- `erase(position)`: Removes the element at the specified `position` (iterator). O(1) time complexity.
    
- `erase(first, last)`: Removes elements in the range `[first, last)`. O(n) time complexity (where n is the number of elements in the range).
    
- `front()`: Returns a reference to the first element.
    
- `back()`: Returns a reference to the last element.
    
- `size()`: Returns the number of elements in the list. O(1) time complexity.
    
- `empty()`: Checks if the list is empty.
    
- `clear()`: Removes all elements from the list.
    
- `remove(value)`: Removes all occurrences of `value` from the list. O(n) time complexity.
    
- `remove_if(predicate)`: Removes all elements for which `predicate` returns true. O(n) time complexity.
    
- `sort()`: Sorts the elements in the list. O(nlogn) time complexity.
    
- `merge(other_list)`: Merges two sorted lists into one, maintaining sorted order. O(n+m) time complexity (n, m are sizes of lists).
    
- `splice(position, other_list)`: Moves elements from `other_list` into `this` list at `position`. Extremely efficient (O(1) for moving entire lists or sub-ranges).
    
- `unique()`: Removes consecutive duplicate elements. O(n) time complexity.
    
- `reverse()`: Reverses the order of elements. O(n) time complexity.
    
- `begin()` and `end()`: Return iterators to the first and one-past-the-last elements.
    
- `rbegin()` and `rend()`: Return reverse iterators to the last and one-before-the-first elements.
    

---

### `std::forward_list` (Singly Linked List)

- **`std::forward_list`** is a sequence container that implements a **singly linked list**.
- Each element (node) stores its value and only a pointer to the **next** element.
- It is designed to be a more **memory-efficient** alternative to `std::list` when bidirectional iteration is not required.
- Elements are **not stored in contiguous memory locations**.

#### **Underlying Mechanism:**

`std::forward_list` uses nodes that each contain data and a single pointer to the next node. It lacks a pointer to the previous node and typically does not store its size directly, to minimize memory overhead. Operations often require access to the _previous_ element's pointer to correctly modify the list (e.g., insertion _after_ an element).

#### **Advantages:**

- **Most Memory-Efficient Linked List**: Uses less memory per node than `std::list` because it only stores one pointer per node.
- **Extremely Fast Insertion/Deletion at the Beginning**: `push_front()` and `pop_front()` are O(1) operations.
- **Efficient Insertion/Deletion _After_ an Element**: `insert_after()` and `erase_after()` are O(1) operations (if you have an iterator to the element _before_ the insertion/deletion point).
- **Iterator Stability**: Similar to `std::list`, iterators remain valid upon insertions or deletions elsewhere.

#### **Disadvantages:**

- **No Backward Iteration**: Due to being singly linked, you can only traverse the list forward.
- **No Random Access**: Like `std::list`, elements cannot be accessed by index (O(n) traversal required).
- **No `size()` Member Function (by default)**: To maintain optimal efficiency and memory footprint, `std::forward_list` typically does not store the size. Calculating the size requires O(n) traversal.
- **Difficult Insertion/Deletion _Before_ an Element**: To insert or delete _before_ a specific element, you usually need to traverse from the beginning to find the _preceding_ element, making these operations O(n). It provides `before_begin()` as a special iterator to facilitate operations at the true beginning of the list.
- **Poor Cache Performance**: Similar to `std::list`, non-contiguous memory can lead to cache misses.

#### **Declaration of `forward_list`:**

C++

```
#include <iostream>
#include <forward_list>
#include <string>

using namespace std;

int main() {

    // Default initialization (empty forward_list)
    forward_list<int> myForwardList;

    // Declaration with some initial values
    forward_list<string> myStringForwardList = {"red", "green", "blue"};

    // Declaration with initial size and value
    forward_list<char> myCharForwardList(4, 'x'); // forward_list of 4 'x' chars

}
```

#### **Insertion & Traversing in `forward_list`:**

C++

```
#include <iostream>
#include <forward_list>
#include <algorithm> // For std::advance

using namespace std;

int main() {
    forward_list<int> myForwardList;

    // Insertion at the front (most common and efficient for forward_list)
    myForwardList.push_front(30);
    myForwardList.push_front(20);
    myForwardList.push_front(10); // myForwardList: {10, 20, 30}

    // Insertion *after* a specific position (efficient if iterator is known)
    auto it = myForwardList.begin(); // 'it' points to 10
    advance(it, 1); // 'it' now points to 20
    myForwardList.insert_after(it, 25); // Insert 25 after 20. myForwardList: {10, 20, 25, 30}

    // Special iterator for inserting at the very beginning conceptually
    myForwardList.insert_after(myForwardList.before_begin(), 5); // Insert 5 at beginning.
                                                                // myForwardList: {5, 10, 20, 25, 30}

    cout << "Elements in the forward_list: ";
    for (const auto& element : myForwardList) {
        cout << element << " "; // Output: 5 10 20 25 30
    }
    cout << endl;

    return 0;
}
```

#### **Deletion in `forward_list`:**

C++

```
#include <iostream>
#include <forward_list>
#include <algorithm> // For std::find

using namespace std;

int main() {
    forward_list<int> myForwardList = {10, 20, 30, 40, 50};

    cout << "Initial forward_list: ";
    for (int val : myForwardList) {
        cout << val << " ";
    }
    cout << endl;

    // Delete from the front (efficient)
    myForwardList.pop_front(); // Removes 10
    cout << "After pop_front: ";
    for (int val : myForwardList) {
        cout << val << " ";
    }
    cout << endl; // Output: 20 30 40 50

    // Delete *after* a specific position (efficient if iterator to preceding element is known)
    auto it_before_erase = myForwardList.begin(); // 'it_before_erase' points to 20
    // We want to delete 30, which is after 20.
    myForwardList.erase_after(it_before_erase); // Removes 30. myForwardList: {20, 40, 50}
    cout << "After erasing after 20: ";
    for (int val : myForwardList) {
        cout << val << " ";
    }
    cout << endl;

    // Clear the forward_list
    myForwardList.clear();
    cout << "Forward_list size after clear (approx, no direct size()): ";
    // Can't directly call .size(), need to iterate if actual count is needed.
    // For checking emptiness:
    cout << (myForwardList.empty() ? "Empty" : "Not Empty") << endl;

    return 0;
}
```

#### **Member functions:**

- `push_front(element)`: Adds an `element` to the beginning. O(1) time complexity.
    
- `pop_front()`: Removes the first element. O(1) time complexity.
    
- `insert_after(position, element)`: Inserts `element` _after_ the specified `position` (iterator). O(1) time complexity.
    
- `erase_after(position)`: Removes the element _after_ the specified `position` (iterator). O(1) time complexity.
    
- `erase_after(first, last)`: Removes elements in the range `(first, last)`. O(n) time complexity.
    
- `before_begin()`: Returns a special iterator that refers to the element _before_ the first element. Useful for `insert_after` and `erase_after` at the true beginning of the list.
    
- `front()`: Returns a reference to the first element.
    
- `empty()`: Checks if the `forward_list` is empty.
    
- `clear()`: Removes all elements.
    
- `remove(value)`: Removes all occurrences of `value`. O(n) time complexity.
    
- `remove_if(predicate)`: Removes all elements for which `predicate` returns true. O(n) time complexity.
    
- `sort()`: Sorts the elements. O(nlogn) time complexity.
    
- `merge(other_forward_list)`: Merges two sorted `forward_list`s. O(n+m) time complexity.
    
- `splice_after(position, other_forward_list)`: Moves elements from `other_forward_list` into `this` list _after_ `position`. Very efficient.
    
- `unique()`: Removes consecutive duplicate elements. O(n) time complexity.
    
- `reverse()`: Reverses the order of elements. O(n) time complexity.
    
- `begin()` and `end()`: Return iterators to the first and one-past-the-last elements.
    
- `cbegin()` and `cend()`: Return `const_iterator` versions of `begin()` and `end()`.