### `std::set` (Ordered Unique Elements)

- **`set`** is an STL container used to store **unique values**.
- Values are always in an **ordered state** (ascending by default, or descending if specified).
- There is **no indexing** in sets like other data structures such as arrays or linked lists. Every element is identified by its own value.
- Once a value is inserted into a set, it **cannot be modified** directly. To "modify" an element, you must erase the old one and insert a new one.

#### **Underlying Mechanism:**

`set` is typically implemented using a **self-balancing binary search tree**, most commonly a Red-Black Tree. This data structure ensures that elements remain sorted and allows for efficient logarithmic time complexity for most operations.

#### **Advantages:**

- **Unique & Sorted Elements**: A set automatically eliminates duplicate elements, ensuring that all elements are unique and maintained in a sorted order.
- **Efficient Operations**:
    - **Insertion (`insert()`):** Average O(logn) time complexity.
    - **Deletion (`erase()`):** Average O(logn) time complexity.
    - **Fast Lookup (`find()`):** Average O(logn) time complexity.
- **Ordered Traversal**: Iterating through a set always yields elements in sorted order.
- **Bidirectional Iterators**: Supports efficient traversal of elements in both forward and backward directions.

#### **Disadvantages:**

- **Elements Must Be Comparable**: Elements in a set must be comparable using a strict weak ordering (e.g., the `<` operator or a custom comparator). This can be a constraint for complex custom data types.
- **Logarithmic Time Complexity for Basic Operations**: While O(logn) is efficient for many scenarios, it's slower than the average O(1) of hash-based containers (`unordered_set`) for insertion, deletion, and lookup.
- **No Random Access**: Although sets provide iterators, they **do not support random access** using an index (e.g., `mySet[i]`), making direct access to an element by its position impossible.
- **Higher Memory Usage**: Sets typically use more memory than simpler data structures like `vector` or arrays because they store additional metadata (pointers, color information for tree nodes) to maintain the sorted order and tree structure.
- **Limited Flexibility**: Sets are designed for specific use cases (unique, sorted data) and might not be the best choice for scenarios that require frequent random access or in-place modification of elements.

#### **Declaration of `set`:**

C++

```
#include <iostream>
#include <set>
#include <functional> // For std::greater

using namespace std; // Using namespace std

int main(){

    // Default initialization in increasing order (ascending)
    set <int> setName; // declaration of set with integer datatype

    // Declaration with some initial values
    set <int> setName2 = {1, 5, 2, 4, 1};  // Will store {1, 2, 4, 5}

    // Initialization of set in decreasing order (descending)
    set <int , greater<int>> decreasingSetName; // Stores elements in descending order

}
```

#### **Insertion & Traversing in Set:**

C++

```
#include <iostream>
#include <set>

using namespace std; // Using namespace std

int main() {
    set<int> mySet;
    mySet.insert(5);
    mySet.insert(2);
    mySet.insert(8);
    mySet.insert(5); // Duplicate, will be ignored

    cout << "Elements in the set (using iterator): ";
    set<int>::iterator itr;  // Creating iterator

    for(itr = mySet.begin() ; itr != mySet.end() ; itr++){
        cout << *itr << " "; // Output: 2 5 8 (always sorted)
    }
    cout << endl;

    // Using range-based for loop (recommended for simplicity)
    // 'const auto&' is used to avoid copying elements and ensure they are not modified.
    cout << "Elements in the set (using range-based for loop): ";
    for (const auto& element : mySet) {
        cout << element << " "; // Output: 2 5 8 (always sorted)
    }
    cout << endl;

    return 0;
}
```

#### **Deletion in Set:**

C++

```
#include <iostream>
#include <set>

using namespace std; // Using namespace std

int main() {
    set<int> mySet;
    mySet.insert(5);
    mySet.insert(2);
    mySet.insert(8);
    mySet.insert(3);

    cout << "Elements in the set: ";
    for (auto element : mySet) {
        cout << element << " "; // Output: 2 3 5 8
    }
    cout << endl;

    // Delete an element by value
    // Returns the number of elements removed (0 or 1)
    size_t num_erased = mySet.erase(2);
    cout << "Elements '2' erased: " << num_erased << endl;

    cout << "Elements in the set after deletion by value: ";
    for (auto element : mySet) {
        cout << element << " "; // Output: 3 5 8
    }
    cout << endl;

    // Delete using an iterator
    auto itr = mySet.find(3); // Find the element 3
    if (itr != mySet.end()) {
        mySet.erase(itr); // Erase using the iterator
    }

    cout << "Elements in the set after deletion using iterator: ";
    for (auto element : mySet) {
        cout << element << " "; // Output: 5 8
    }
    cout << endl;

    // Clear the set
    mySet.clear();

    cout << "Set size after clear: " << mySet.size() << endl; // Output: 0

    return 0;
}
```

#### **Member functions:**

- `insert(element)`: Adds an `element` to the set, automatically eliminating duplicates. Returns a `pair<iterator, bool>`, where `bool` is true if insertion occurred. O(logn) time complexity.

- `find(element)`: Returns an iterator to the `element` if it exists in the set, or `end()` if not found. O(logn) time complexity.

- `erase(element)`: Removes the specified `element` from the set. Returns the number of elements removed (0 or 1). O(logn) time complexity.

- `erase(iterator)`: Removes the element pointed to by `iterator`. O(logn) time complexity.

- `size()`: Returns the number of elements in the set.

- `empty()`: Checks if the set is empty.

- `begin()` and `end()`: Return `const_iterator`s to the first (smallest) and one-past-the-last elements, respectively. Allow iteration over the set in sorted order.

- `rbegin()` and `rend()`: Return `const_reverse_iterator`s for reverse iteration.

- `count(element)`: Returns 1 if the `element` is in the set, otherwise 0 (since elements are unique). O(logn) time complexity.

- `lower_bound(element)`: Returns an iterator pointing to the first element that is **not less than** the specified `element`. O(logn) time complexity.

- `upper_bound(element)`: Returns an iterator pointing to the first element that is **greater than** the specified `element`. O(logn) time complexity.