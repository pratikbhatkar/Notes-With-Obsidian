### `std::map` (Ordered Key-Value Pairs)

- **`std::map`** is an associative container that stores elements formed by a combination of a **key value** and a **mapped value** (i.e., **key-value pairs**).
- Each key is **unique**, and these pairs are always stored in a **sorted order** based on their keys (ascending by default).
- It provides a way to efficiently look up, insert, and delete elements based on their keys.

#### **Underlying Mechanism:**

`std::map` is typically implemented using a **self-balancing binary search tree**, most commonly a Red-Black Tree. This structure ensures that elements (key-value pairs) are always kept in sorted order based on their keys and allows for efficient logarithmic time complexity for most operations. Each node in the tree stores a key-value pair.

#### **Advantages:**

- **Unique & Sorted Keys**: Automatically ensures that all keys are unique and maintains elements in a sorted order based on keys.
- **Efficient Lookup, Insertion, Deletion**: All these operations have an average time complexity of **O(logn)** because of the balanced tree structure.
- **Ordered Traversal**: Iterating through a `map` always yields elements in sorted order of their keys.
- **Bidirectional Iterators**: Supports efficient traversal of elements in both forward and backward directions.
- **Key-Value Association**: Ideal for scenarios where you need to associate a value with a unique identifier (e.g., dictionary, symbol table).

#### **Disadvantages:**

- **Logarithmic Time Complexity**: While O(logn) is efficient for many cases, it's slower than the average O(1) of hash-based containers (`unordered_map`) for lookup, insertion, and deletion.
- **Higher Memory Usage**: Typically uses more memory than `unordered_map` or simpler containers due to the overhead of storing tree node pointers and color information.
- **No Random Access by Index**: You cannot access elements by an integer index (e.g., `myMap[0]`). Access is always by key.
- **Keys Must Be Comparable**: Keys must support a strict weak ordering (e.g., the `<` operator or a custom comparator).

#### **Declaration of `map`:**

C++

```
#include <iostream>
#include <map>
#include <string>
#include <functional> // For std::greater

using namespace std;

int main() {

    // Default initialization (empty map, keys in ascending order)
    map<int, string> studentGrades; // int key, string value

    // Declaration with some initial key-value pairs
    map<string, int> cityPopulations = {
        {"New York", 8000000},
        {"Los Angeles", 4000000},
        {"Chicago", 3000000}
    }; // Keys automatically sorted: Chicago, Los Angeles, New York

    // Map with keys in decreasing order
    map<int, string, greater<int>> reverseOrderedMap;

}
```

#### **Insertion & Traversing in Map:**

C++

```
#include <iostream>
#include <map>
#include <string>

using namespace std;

int main() {
    map<int, string> studentGrades;

    // Method 1: Using insert() with make_pair or pair
    studentGrades.insert(make_pair(101, "Alice"));
    studentGrades.insert(pair<int, string>(103, "Charlie"));

    // Method 2: Using operator[] (convenient, but inserts default if key not found)
    studentGrades[102] = "Bob";
    studentGrades[101] = "Alice Smith"; // Modifies value if key exists

    // Method 3: Using emplace (efficient for complex types)
    studentGrades.emplace(104, "David");

    cout << "Elements in the map (Key-Value pairs):" << endl;
    // Using range-based for loop for easy traversal
    // 'const auto&' is good practice for reading elements
    for (const auto& pair : studentGrades) {
        cout << "Key: " << pair.first << ", Value: " << pair.second << endl;
    }
    // Expected Output (sorted by key):
    // Key: 101, Value: Alice Smith
    // Key: 102, Value: Bob
    // Key: 103, Value: Charlie
    // Key: 104, Value: David
    cout << endl;

    // Accessing elements
    cout << "Grade for student 102: " << studentGrades[102] << endl; // Output: Bob
    // Using .at() for bounds checking (throws out_of_range if key not found)
    try {
        cout << "Grade for student 104: " << studentGrades.at(104) << endl; // Output: David
        // cout << studentGrades.at(999) << endl; // This would throw an exception
    } catch (const out_of_range& e) {
        cerr << "Error accessing: " << e.what() << endl;
    }

    // Checking if a key exists using find()
    if (studentGrades.find(103) != studentGrades.end()) {
        cout << "Student 103 found!" << endl;
    } else {
        cout << "Student 103 not found." << endl;
    }

    return 0;
}
```

#### **Deletion in Map:**

C++

```
#include <iostream>
#include <map>
#include <string>

using namespace std;

int main() {
    map<int, string> myMap = {
        {1, "One"},
        {2, "Two"},
        {3, "Three"},
        {4, "Four"},
        {5, "Five"}
    };

    cout << "Initial map:" << endl;
    for (const auto& pair : myMap) {
        cout << pair.first << ": " << pair.second << endl;
    }
    cout << endl;

    // Delete by key
    // Returns the number of elements removed (0 or 1 for map)
    size_t num_erased = myMap.erase(3);
    cout << "Key '3' erased: " << num_erased << endl;

    cout << "Map after erasing key 3:" << endl;
    for (const auto& pair : myMap) {
        cout << pair.first << ": " << pair.second << endl;
    }
    cout << endl; // Output will not include 3:Three

    // Delete using an iterator
    // First, find the element
    auto it_to_erase = myMap.find(2);
    if (it_to_erase != myMap.end()) {
        myMap.erase(it_to_erase); // Erase using the iterator
    }

    cout << "Map after erasing using iterator (key 2):" << endl;
    for (const auto& pair : myMap) {
        cout << pair.first << ": " << pair.second << endl;
    }
    cout << endl; // Output will not include 2:Two

    // Clear the map
    myMap.clear();

    cout << "Map size after clear: " << myMap.size() << endl; // Output: 0
    cout << "Is map empty after clear? " << (myMap.empty() ? "Yes" : "No") << endl;

    return 0;
}
```

#### **Member functions:**

- `insert(pair)`: Inserts a key-value `pair` into the map. If the key already exists, the insertion fails. O(logn) time complexity.
    
- `operator[](key)`: Provides direct access to the mapped value associated with `key`. If `key` is not found, it's inserted with a default-constructed value. O(logn) time complexity.
    
- `at(key)`: Returns a reference to the mapped value associated with `key`. Throws `out_of_range` if `key` is not found. O(logn) time complexity.
    
- `find(key)`: Returns an iterator to the element with the specified `key`, or `end()` if the key is not found. O(logn) time complexity.
    
- `erase(key)`: Removes the element with the specified `key`. Returns the number of elements removed (0 or 1). O(logn) time complexity.
    
- `erase(iterator)`: Removes the element pointed to by `iterator`. O(logn) time complexity.
    
- `size()`: Returns the number of elements (key-value pairs) in the map.
    
- `empty()`: Checks if the map is empty.
    
- `clear()`: Removes all elements from the map.
    
- `begin()` and `end()`: Return iterators to the first (smallest key) and one-past-the-last elements.
    
- `rbegin()` and `rend()`: Return reverse iterators.
    
- `count(key)`: Returns 1 if the `key` is in the map, otherwise 0 (since keys are unique). O(logn) time complexity.
    
- `lower_bound(key)`: Returns an iterator pointing to the first element whose key is **not less than** the specified `key`. O(logn) time complexity.
    
- `upper_bound(key)`: Returns an iterator pointing to the first element whose key is **greater than** the specified `key`. O(logn) time complexity.