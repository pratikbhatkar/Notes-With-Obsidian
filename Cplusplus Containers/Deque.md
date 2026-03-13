### `std::deque` (Double-Ended Queue)

- **`std::deque`** (pronounced "deck" or "dee-queue") is a sequence container that allows **efficient insertion and deletion at both its beginning and its end**.
- It provides similar functionality to `std::vector` (dynamic array) by supporting **random access** to elements using an index.
- Unlike `std::vector` which stores elements in a single contiguous block, `std::deque` stores elements in **multiple, non-contiguous fixed-size blocks of memory**, managed by an internal map or array of pointers to these blocks.

#### **Underlying Mechanism:**

`std::deque` is implemented as a set of fixed-size contiguous memory blocks, with a map or array of pointers pointing to these blocks. This allows it to efficiently expand from both ends. When elements are added at the front or back, new blocks can be allocated as needed without reallocating the entire structure and copying all existing elements, unlike `std::vector`.

#### **Advantages:**

- **Efficient Insertion/Deletion at Both Ends**: `push_front()`, `pop_front()`, `push_back()`, and `pop_back()` operations all have **O(1) time complexity** (amortized for `push` operations). This is a significant advantage over `std::vector` for front operations.
- **Random Access**: Elements can be accessed by their index using `operator[]` or `at()`, providing **O(1) time complexity** for random access, similar to `std::vector`.
- **No Full Reallocations**: Unlike `std::vector`, `std::deque` does not need to reallocate its entire contents and copy all elements when it grows, as it allocates new blocks of memory. This avoids large, potentially costly single operations.
- **Iterator Stability (Partial)**: Iterators to elements remain valid after insertions or deletions at either end of the deque. However, inserting or deleting elements _in the middle_ invalidates all iterators, references, and pointers to elements beyond the insertion/deletion point.

#### **Disadvantages:**

- **Less Cache Locality than `std::vector`**: Because elements are stored in separate memory blocks, accessing sequential elements might incur more cache misses than with `std::vector`, which stores everything contiguously. This can make sequential iteration slightly slower in practice than `std::vector`.
- **More Memory Overhead**: It typically uses more memory than `std::vector` due to the overhead of managing multiple memory blocks and the map/array of pointers to these blocks.
- **Slower Middle Insertion/Deletion**: Inserting or deleting elements in the _middle_ of a `std::deque` still requires shifting elements within existing blocks and potentially across blocks, resulting in **O(n) time complexity**. This is similar to `std::vector` but worse than `std::list`.
- **No Guaranteed Contiguous Storage**: Unlike `std::vector`, you cannot rely on `deque::data()` to get a pointer to a single contiguous block of memory for all elements.

#### **Declaration of `deque`:**

C++

```
#include <iostream>
#include <deque>    // Required for std::deque
#include <string>

using namespace std;

int main() {

    // Default initialization (empty deque)
    deque<int> myDeque; // declaration of deque with integer datatype

    // Declaration with some initial values
    deque<string> myStringDeque = {"alpha", "beta", "gamma"};

    // Declaration with initial size and value
    deque<double> myDoubleDeque(4, 7.77); // deque of 4 doubles, all 7.77

}
```

#### **Insertion & Traversing in Deque:**

When you want to look at elements in a deque, you have a few ways to do it.

C++

```
#include <iostream>
#include <deque>
#include <algorithm> // For std::for_each

using namespace std;

int main() {
    deque<int> myDeque;

    // --- Insertion Examples ---
    // Adding elements to the back (right side)
    myDeque.push_back(30);   // Deque: {30}
    myDeque.push_back(40);   // Deque: {30, 40}

    // Adding elements to the front (left side)
    myDeque.push_front(20);  // Deque: {20, 30, 40}
    myDeque.push_front(10);  // Deque: {10, 20, 30, 40}

    // --- Traversing Examples ---

    // Method 1: Using index (like a regular array) - Good for specific positions
    cout << "Elements using index (myDeque[i]): ";
    for (size_t i = 0; i < myDeque.size(); ++i) {
        cout << myDeque[i] << " "; // Access element at position 'i'
    }
    cout << endl; // Output: 10 20 30 40

    // Method 2: Using iterators - Flexible for advanced operations (e.g., insert/erase)
    cout << "Elements using iterators: ";
    deque<int>::iterator itr; // Declare an iterator
    for(itr = myDeque.begin() ;    // Start 'itr' at the beginning of the deque
        itr != myDeque.end() ;     // Continue until 'itr' reaches the end (one past the last element)
        itr++){                    // Move 'itr' to the next element
        cout << *itr << " ";  // Dereference 'itr' to get the actual value
    }
    cout << endl; // Output: 10 20 30 40

    // Method 3: Using range-based for loop - Easiest and recommended for simple iteration
    cout << "Elements using range-based for loop: ";
    for (const auto& element : myDeque) { // 'const auto&' is efficient: reads element without copying it
        cout << element << " ";
    }
    cout << endl; // Output: 10 20 30 40

    // --- Insertion in the middle (less efficient - O(n)) ---
    // Finding the position: myDeque.begin() + 2 means 2 elements after the beginning.
    // Original: {10, 20, 30, 40}
    // Index:     0   1   2   3
    // myDeque.begin() + 2 points to element 30.
    myDeque.insert(myDeque.begin() + 2, 25); // Insert 25 BEFORE the element at index 2 (which was 30).
                                             // Deque now: {10, 20, 25, 30, 40}
    cout << "After middle insertion (25 at index 2): ";
    for (int val : myDeque) {
        cout << val << " ";
    }
    cout << endl; // Output: 10 20 25 30 40

    return 0;
}
```

#### **Deletion in Deque:**

Deleting elements from a deque can be done from either end very efficiently, or from the middle with more cost.

C++

```
#include <iostream>
#include <deque>
#include <algorithm> // For std::find

using namespace std;

int main() {
    deque<int> myDeque = {10, 20, 30, 40, 50};

    cout << "Initial deque: ";
    for (int val : myDeque) {
        cout << val << " ";
    }
    cout << endl;

    // Delete from the front (efficient - O(1))
    myDeque.pop_front(); // Removes 10
    cout << "After pop_front: ";
    for (int val : myDeque) {
        cout << val << " ";
    }
    cout << endl; // Output: 20 30 40 50

    // Delete from the end (efficient - O(1))
    myDeque.pop_back(); // Removes 50
    cout << "After pop_back: ";
    for (int val : myDeque) {
        cout << val << " "; // Output: 20 30 40
    }
    cout << endl;

    // Delete at a specific position using an iterator (less efficient - O(n))
    // myDeque.begin() + 1 points to the element at index 1 (which is 30 now)
    myDeque.erase(myDeque.begin() + 1); // Removes the element at index 1 (30).
                                        // Deque now: {20, 40}
    cout << "After erasing element at index 1 (value 30): ";
    for (int val : myDeque) {
        cout << val << " ";
    }
    cout << endl; // Output: 20 40

    // Re-initialize deque for range erase example
    myDeque = {1, 2, 3, 4, 5, 6, 7};
    cout << "Deque for range erase example: ";
    for (int val : myDeque) { cout << val << " "; }
    cout << endl; // Output: 1 2 3 4 5 6 7

    // Erase elements from index 2 (value 3) up to (but not including) index 5 (value 6).
    // This will remove 3, 4, 5.
    myDeque.erase(myDeque.begin() + 2, myDeque.begin() + 5);
    cout << "After erasing range [index 2, index 5): ";
    for (int val : myDeque) {
        cout << val << " ";
    }
    cout << endl; // Output: 1 2 6 7

    // Clear the deque
    myDeque.clear();
    cout << "Deque size after clear(): " << myDeque.size() << endl; // Output: 0
    cout << "Is deque empty after clear()? " << (myDeque.empty() ? "Yes" : "No") << endl;

    return 0;
}
```

#### **Member functions:**

- `push_front(element)`: Adds an `element` to the beginning of the deque. O(1) time complexity (amortized).
    
- `push_back(element)`: Adds an `element` to the end of the deque. O(1) time complexity (amortized).
    
- `pop_front()`: Removes the first element from the deque. O(1) time complexity.
    
- `pop_back()`: Removes the last element from the deque. O(1) time complexity.
    
- `insert(position, element)`: Inserts `element` at the specified `position` (iterator). O(n) time complexity.
    
- `erase(position)`: Removes the element at the specified `position` (iterator). O(n) time complexity.
    
- `erase(first, last)`: Removes elements in the range `[first, last)`. O(n) time complexity.
    
- `at(index)`: Returns a reference to the element at `index`, with bounds checking (throws `out_of_range` if `index` is invalid). O(1) time complexity.
    
- `operator[](index)`: Returns a reference to the element at `index`, without bounds checking (faster, but potentially unsafe). O(1) time complexity.
    
- `front()`: Returns a reference to the first element. O(1) time complexity.
    
- `back()`: Returns a reference to the last element. O(1) time complexity.
    
- `size()`: Returns the number of elements in the deque. O(1) time complexity.
    
- `capacity()`: Not applicable directly as in `std::vector`. There isn't a single 'capacity' for the entire deque.
    
- `empty()`: Checks if the deque is empty. O(1) time complexity.
    
- `clear()`: Removes all elements from the deque.
    
- `begin()` and `end()`: Return iterators to the first and one-past-the-last elements, respectively.
    
- `rbegin()` and `rend()`: Return reverse iterators.