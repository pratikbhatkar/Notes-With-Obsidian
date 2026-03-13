### `std::unordered_set` (Unordered Unique Elements)

- **`unordered_set`** is an STL container used to store **unique values**, like `std::set`, but it does _not_ maintain any specific order.
- Elements are stored in a way that allows for very fast average-case lookup, insertion, and deletion.
- It provides the functionality of a set but without the overhead of keeping elements sorted.

#### **Underlying Mechanism:**

`unordered_set` is implemented using a **hash table**. Elements are stored in buckets, and a hash function determines which bucket an element belongs to. This allows for near-constant time complexity for most operations, _on average_.

#### **Advantages:**

- **Very Fast Average-Case Operations**:
    - **Insertion (`insert()`):** Average O(1) time complexity.
    - **Deletion (`erase()`):** Average O(1) time complexity.
    - **Lookup (`find()`):** Average O(1) time complexity.
- **No Ordering Overhead**: Since elements are not sorted, there's no need to maintain a specific order, which can improve performance compared to `std::set`.
- **Suitable for Large Datasets**: The near-constant time complexity of operations makes it well-suited for large datasets where speed is critical.

#### **Disadvantages:**

- **No Ordering**: Elements are not stored in any particular order. If you need sorted data, use `std::set`.
- **Worst-Case Time Complexity**: In the worst-case scenario (e.g., many hash collisions), operations can degrade to O(n) time complexity, though this is rare with a good hash function and proper bucket management.
- **Higher Memory Usage**: Hash tables typically require more memory than balanced trees (used by `std::set`) to store the hash table itself and manage buckets.
- **Elements Must Be Hashable**: Elements must be hashable, meaning there must be a way to compute a hash value for each element. This usually means providing a suitable hash function.

#### **Declaration of `unordered_set`:**

C++

```
#include <iostream>
#include <unordered_set>
#include <string>

using namespace std;

int main() {

    // Default initialization (empty unordered_set)
    unordered_set<int> mySet; // declaration of unordered_set with integer datatype

    // Declaration with some initial values
    unordered_set<string> myStringSet = {"apple", "banana", "cherry"}; // Order is not guaranteed

    // Custom hash function (if needed)
    // Example: For a custom class, you'd need to define a hash function and equality operator.

}
```

#### **Insertion & Traversing in `unordered_set`:**

C++

```
#include <iostream>
#include <unordered_set>

using namespace std;

int main() {
    unordered_set<int> mySet;
    mySet.insert(5);
    mySet.insert(2);
    mySet.insert(8);
    mySet.insert(5); // Duplicate, will be ignored

    cout << "Elements in the unordered_set (using iterator): ";
    unordered_set<int>::iterator itr; // Creating iterator
    for (itr = mySet.begin(); itr != mySet.end(); itr++) {
        cout << *itr << " "; // Output: Elements in an arbitrary order
    }
    cout << endl;

    // Using range-based for loop (recommended for simplicity)
    cout << "Elements in the unordered_set (using range-based for loop): ";
    for (const auto& element : mySet) {
        cout << element << " "; // Output: Elements in an arbitrary order
    }
    cout << endl;

    return 0;
}
```

#### **Deletion in `unordered_set`:**

C++

```
#include <iostream>
#include <unordered_set>

using namespace std;

int main() {
    unordered_set<int> mySet;
    mySet.insert(5);
    mySet.insert(2);
    mySet.insert(8);
    mySet.insert(3);

    cout << "Elements in the unordered_set: ";
    for (int element : mySet) {
        cout << element << " "; // Output: Elements in an arbitrary order
    }
    cout << endl;

    // Delete an element by value
    size_t num_erased = mySet.erase(2);
    cout << "Elements '2' erased: " << num_erased << endl;

    cout << "Elements in the unordered_set after deletion by value: ";
    for (int element : mySet) {
        cout << element << " "; // Output: Remaining elements in an arbitrary order
    }
    cout << endl;

    // Delete using an iterator
    auto itr = mySet.find(3); // Find the element 3
    if (itr != mySet.end()) {
        mySet.erase(itr); // Erase using the iterator
    }

    cout << "Elements in the unordered_set after deletion using iterator: ";
    for (int element : mySet) {
        cout << element << " "; // Output: Remaining elements in an arbitrary order
    }
    cout << endl;

    // Clear the unordered_set
    mySet.clear();

    cout << "unordered_set size after clear: " << mySet.size() << endl; // Output: 0

    return 0;
}
```

#### **Member functions:**

- `insert(element)`: Adds an `element` to the `unordered_set`, automatically eliminating duplicates. Average O(1) time complexity.

- `find(element)`: Returns an iterator to the `element` if it exists, or `end()` if not found. Average O(1) time complexity.

- `erase(element)`: Removes the specified `element`. Average O(1) time complexity.

- `erase(iterator)`: Removes the element pointed to by `iterator`. Average O(1) time complexity.

- `size()`: Returns the number of elements.

- `empty()`: Checks if the `unordered_set` is empty.

- `begin()` and `end()`: Return iterators to the beginning and end of the `unordered_set`. Elements are not in any specific order.

- `clear()`: Removes all elements.

- `count(element)`: Returns 1 if the `element` is in the `unordered_set`, otherwise 0. Average O(1) time complexity.

- `bucket_count()`: Returns the number of buckets in the hash table.

- `load_factor()`: Returns the average number of elements per bucket.

- `rehash(n)`: Attempts to reorganize the hash table to have at least `n` buckets. Can improve performance if the load factor becomes too high.