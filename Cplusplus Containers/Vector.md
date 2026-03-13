### `std::vector` (Dynamic Array)

- `vector` is a sequence container that represents a **dynamic array**.
- It can **grow or shrink in size dynamically** as elements are added or removed, handling its own memory management.
- Elements are stored in **contiguous memory locations**, similar to a C-style array.
- Provides **random access** to elements using an index (like arrays).

#### **Underlying Mechanism:**

`vector` manages a dynamically-sized array. When the vector needs to grow beyond its current capacity, it typically allocates a new, larger block of contiguous memory, copies all existing elements to the new location, and then deallocates the old memory. This reallocation process is what makes it "dynamic."

#### **Advantages:**

- **Dynamic Size**: Can grow or shrink at runtime, eliminating the need to specify a fixed size at compile time.
- **Efficient Random Access**: Due to contiguous memory storage, elements can be accessed directly using an index in **O(1) time complexity**.
- **Cache Locality**: Storing elements contiguously improves cache performance, which can lead to faster overall execution, especially for algorithms that iterate sequentially.
- **Efficient Back Insertion/Deletion**: Adding or removing elements at the _end_ of the vector is typically very fast (average O(1) amortized).
- **Compatibility**: Can easily interoperate with C-style arrays and functions that expect raw pointers.

#### **Disadvantages:**

- **Expensive Middle Insertion/Deletion**: Inserting or deleting elements in the _middle_ or at the _beginning_ of the vector is **O(n) time complexity**. This is because all subsequent elements need to be shifted to maintain contiguity.
- **Reallocations**: When the vector's capacity is exceeded, a new, larger memory block is allocated, and all elements are copied. This reallocation can be a costly operation, especially for large vectors.
- **Memory Overhead**: While generally efficient, there can be some wasted memory if the vector allocates more capacity than immediately needed (to amortize the cost of reallocations).

#### **Declaration of `vector`:**

C++

```
#include <iostream>
#include <vector>
#include <string>

using namespace std;

int main() {

    // Default initialization (empty vector)
    vector<int> myVector; // declaration of vector with integer datatype

    // Declaration with initial size (all elements initialized to 0 for int)
    vector<double> myDoubleVector(5); // vector of 5 doubles, all initialized to 0.0

    // Declaration with initial size and value
    vector<string> myStringVector(3, "hello"); // vector of 3 strings, all "hello"

    // Declaration with initializer list
    vector<int> myInitializedVector = {1, 2, 3, 4, 5};

    // Copy constructor
    vector<int> anotherVector = myInitializedVector;

}
```

#### **Insertion & Traversing in Vector:**

C++

```
#include <iostream>
#include <vector>
#include <algorithm> // For std::for_each

using namespace std;

int main() {
    vector<int> myVector;

    // Insertion at the end (efficient)
    myVector.push_back(10);
    myVector.push_back(20);
    myVector.push_back(30);

    // Insertion at a specific position (less efficient - O(n))
    // myVector.begin() + 1 points to the element at index 1.
    // Inserts 15 BEFORE the element that was at index 1 (which was 20).
    myVector.insert(myVector.begin() + 1, 15); // myVector now: {10, 15, 20, 30}

    cout << "Elements in the vector (using index): ";
    for (size_t i = 0; i < myVector.size(); ++i) {
        cout << myVector[i] << " "; // Random access
    }
    cout << endl;

    cout << "Elements in the vector (using iterator): ";
    // Creating iterator
    vector<int>::iterator itr;
    for(itr = myVector.begin() ; itr != myVector.end() ; itr++){
        cout << *itr << " ";
    }
    cout << endl;

    // Using range-based for loop (recommended for simplicity and safety)
    cout << "Elements using range-based for loop: ";
    for (const auto& element : myVector) { // 'const auto&' is good practice
        cout << element << " ";
    }
    cout << endl;

    // Using std::for_each (from <algorithm>)
    cout << "Elements using std::for_each: ";
    for_each(myVector.begin(), myVector.end(), [](int val){
        cout << val << " ";
    });
    cout << endl;

    return 0;
}
```

#### **Deletion in Vector:**

C++

```
#include <iostream>
#include <vector>
#include <algorithm> // For std::find

using namespace std;

int main() {
    vector<int> myVector = {10, 20, 30, 40, 50};

    cout << "Initial vector: ";
    for (int val : myVector) {
        cout << val << " ";
    }
    cout << endl;

    // Delete from the end (efficient - O(1))
    myVector.pop_back(); // Removes 50
    cout << "After pop_back(): ";
    for (int val : myVector) {
        cout << val << " ";
    }
    cout << endl; // Output: 10 20 30 40

    // Delete at a specific position using an iterator (less efficient - O(n))
    // myVector.begin() + 1 points to the element at index 1 (which is 20 now).
    myVector.erase(myVector.begin() + 1); // Removes 20. myVector now: {10, 30, 40}
    cout << "After erasing element at index 1 (value 20): ";
    for (int val : myVector) {
        cout << val << " ";
    }
    cout << endl; // Output: 10 30 40

    // Find and erase an element (involves find (O(n)) + erase (O(n)))
    auto it_to_erase = find(myVector.begin(), myVector.end(), 40);
    if (it_to_erase != myVector.end()) {
        myVector.erase(it_to_erase); // Removes 40. myVector now: {10, 30}
    }
    cout << "After finding and erasing 40: ";
    for (int val : myVector) {
        cout << val << " ";
    }
    cout << endl; // Output: 10 30


    // Clear the vector
    myVector.clear();
    cout << "Vector size after clear(): " << myVector.size() << endl; // Output: 0
    cout << "Is vector empty after clear()? " << (myVector.empty() ? "Yes" : "No") << endl;

    return 0;
}
```

#### **Member functions:**

- `push_back(element)`: Adds an `element` to the end of the vector. Average O(1) time complexity (amortized).

- `pop_back()`: Removes the last element from the vector. O(1) time complexity.

- `insert(position, element)`: Inserts `element` at the specified `position` (iterator). O(n) time complexity.

- `erase(position)`: Removes the element at the specified `position` (iterator). O(n) time complexity.

- `erase(first, last)`: Removes elements in the range `[first, last)`. O(n) time complexity.

- `at(index)`: Returns a reference to the element at `index`, with bounds checking (throws `out_of_range` if `index` is invalid).

- `operator[](index)`: Returns a reference to the element at `index`, without bounds checking (faster, but potentially unsafe).

- `front()`: Returns a reference to the first element.

- `back()`: Returns a reference to the last element.

- `size()`: Returns the number of elements in the vector.

- `capacity()`: Returns the total number of elements the vector can hold without reallocating.

- `empty()`: Checks if the vector is empty.

- `clear()`: Removes all elements from the vector.

- `begin()` and `end()`: Return iterators to the first and one-past-the-last elements, respectively.

- `rbegin()` and `rend()`: Return reverse iterators.

- `reserve(n)`: Requests that the vector's capacity be at least `n`.

- `shrink_to_fit()`: Reduces memory usage by freeing unused memory (reduces capacity to size).