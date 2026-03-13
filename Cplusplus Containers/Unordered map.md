### `std::unordered_map` (Unordered Key-Value Pairs)

- **`std::unordered_map`** is an associative container that stores elements formed by a combination of a **key value** and a **mapped value** (i.e., **key-value pairs**).
- Each key is **unique**, similar to `std::map`.
- Unlike `std::map`, elements are **not stored in any particular sorted order**; their arrangement depends on the hash function.
- It provides very fast average-case lookup, insertion, and deletion based on keys.

#### **Underlying Mechanism:**

`std::unordered_map` is implemented using a **hash table**. When a key-value pair is inserted, the key is passed through a hash function, which computes an index (or hash value) that determines where the pair should be stored (in a "bucket"). This allows for average O(1) time complexity for operations. Collision resolution (when different keys hash to the same bucket) is handled internally (e.g., using chaining with linked lists).

#### **Advantages:**

- **Very Fast Average-Case Operations**:
    - **Lookup (`find()`, `operator[]`, `at()`):** Average O(1) time complexity.
    - **Insertion (`insert()`, `operator[]`):** Average O(1) time complexity.
    - **Deletion (`erase()`):** Average O(1) time complexity.
- **No Ordering Overhead**: Since elements are not sorted, there's no overhead associated with maintaining a specific order, which contributes to its speed.
- **Suitable for Large Datasets**: The near-constant average time complexity for core operations makes it highly suitable for applications requiring high performance on large amounts of data.

#### **Disadvantages:**

- **No Ordering**: Elements are not stored in any particular order. If you need sorted data or ordered traversal, use `std::map`.
- **Worst-Case Time Complexity**: In the worst-case scenario (e.g., poor hash function leading to many collisions, or all keys hashing to the same bucket), operations can degrade to **O(n) time complexity**. This is less common with good hash functions provided by the standard library.
- **Higher Memory Usage**: Hash tables typically use more memory than tree-based maps (`std::map`) due to the need for managing buckets and potentially handling collisions.
- **Keys Must Be Hashable**: Keys must be hashable, meaning there must be a way to compute a hash value for them. For custom types, you might need to provide a custom hash function.
- **No `lower_bound`/`upper_bound`**: These functions, which rely on sorted order, are not available.

#### **Declaration of `unordered_map`:**

C++

```
#include <iostream>
#include <unordered_map>
#include <string>
#include <functional> // For std::hash (implicitly used)

using namespace std;

int main() {

    // Default initialization (empty unordered_map)
    unordered_map<int, string> studentNames; // int key, string value

    // Declaration with some initial key-value pairs
    unordered_map<string, double> itemPrices = {
        {"Apple", 1.20},
        {"Banana", 0.75},
        {"Orange", 1.50}
    }; // Order is not guaranteed

    // For custom key types, you'd need to provide a custom hash and equality function
    // Example (conceptual):
    // struct MyCustomKey { int id; string name; };
    // struct MyCustomKeyHasher {
    //     size_t operator()(const MyCustomKey& k) const {
    //         return hash<int>()(k.id) ^ hash<string>()(k.name);
    //     }
    // };
    // unordered_map<MyCustomKey, int, MyCustomKeyHasher> myCustomMap;

}
```

#### **Insertion & Accessing in `unordered_map`:**

C++

```
#include <iostream>
#include <unordered_map>
#include <string>

using namespace std;

int main() {
    unordered_map<int, string> studentNames;

    // Method 1: Using insert() with make_pair or pair
    studentNames.insert(make_pair(101, "Alice"));
    studentNames.insert(pair<int, string>(103, "Charlie"));

    // Method 2: Using operator[] (convenient, inserts or modifies)
    studentNames[102] = "Bob";
    studentNames[101] = "Alice Johnson"; // Modifies value if key 101 already exists

    // Method 3: Using emplace (efficient for complex types)
    studentNames.emplace(104, "David");

    cout << "Elements in the unordered_map (Key-Value pairs):" << endl;
    // Using range-based for loop for traversal
    // The order will be arbitrary, not sorted by key
    for (const auto& pair : studentNames) {
        cout << "Key: " << pair.first << ", Value: " << pair.second << endl;
    }
    cout << endl;

    // Accessing elements
    cout << "Name for student 102: " << studentNames[102] << endl; // Output: Bob
    // Using .at() for bounds checking (throws out_of_range if key not found)
    try {
        cout << "Name for student 104: " << studentNames.at(104) << endl; // Output: David
        // cout << studentNames.at(999) << endl; // This would throw an exception
    } catch (const out_of_range& e) {
        cerr << "Error accessing: " << e.what() << endl;
    }

    // Checking if a key exists using find()
    if (studentNames.find(103) != studentNames.end()) {
        cout << "Student 103 found! Name: " << studentNames.find(103)->second << endl;
    } else {
        cout << "Student 103 not found." << endl;
    }

    return 0;
}
```

#### **Deletion in `unordered_map`:**

C++

```
#include <iostream>
#include <unordered_map>
#include <string>

using namespace std;

int main() {
    unordered_map<int, string> myMap = {
        {1, "One"},
        {2, "Two"},
        {3, "Three"},
        {4, "Four"},
        {5, "Five"}
    };

    cout << "Initial unordered_map:" << endl;
    for (const auto& pair : myMap) {
        cout << pair.first << ": " << pair.second << endl;
    }
    cout << endl; // Order will be arbitrary

    // Delete by key
    // Returns the number of elements removed (0 or 1 for unordered_map)
    size_t num_erased = myMap.erase(3);
    cout << "Key '3' erased: " << num_erased << endl;

    cout << "unordered_map after erasing key 3:" << endl;
    for (const auto& pair : myMap) {
        cout << pair.first << ": " << pair.second << endl;
    }
    cout << endl; // Order will be arbitrary, but 3 will be gone

    // Delete using an iterator
    // First, find the element
    auto it_to_erase = myMap.find(2);
    if (it_to_erase != myMap.end()) {
        myMap.erase(it_to_erase); // Erase using the iterator
    }

    cout << "unordered_map after erasing using iterator (key 2):" << endl;
    for (const auto& pair : myMap) {
        cout << pair.first << ": " << pair.second << endl;
    }
    cout << endl; // Order will be arbitrary, but 2 will be gone

    // Clear the unordered_map
    myMap.clear();

    cout << "unordered_map size after clear: " << myMap.size() << endl; // Output: 0
    cout << "Is unordered_map empty after clear? " << (myMap.empty() ? "Yes" : "No") << endl;

    return 0;
}
```

#### **Member functions:**

- `insert(pair)`: Inserts a key-value `pair` into the map. If the key already exists, the insertion fails. Average O(1) time complexity, worst-case O(n).
    
- `operator[](key)`: Provides direct access to the mapped value associated with `key`. If `key` is not found, it's inserted with a default-constructed value. Average O(1) time complexity, worst-case O(n).
    
- `at(key)`: Returns a reference to the mapped value associated with `key`. Throws `out_of_range` if `key` is not found. Average O(1) time complexity, worst-case O(n).
    
- `find(key)`: Returns an iterator to the element with the specified `key`, or `end()` if the key is not found. Average O(1) time complexity, worst-case O(n).
    
- `erase(key)`: Removes the element with the specified `key`. Returns the number of elements removed (0 or 1). Average O(1) time complexity, worst-case O(n).
    
- `erase(iterator)`: Removes the element pointed to by `iterator`. Average O(1) time complexity, worst-case O(n).
    
- `size()`: Returns the number of elements (key-value pairs) in the map.
    
- `empty()`: Checks if the map is empty.
    
- `clear()`: Removes all elements from the map.
    
- `begin()` and `end()`: Return iterators to the first and one-past-the-last elements in their current (arbitrary) order.
    
- `count(key)`: Returns 1 if the `key` is in the map, otherwise 0 (since keys are unique). Average O(1) time complexity, worst-case O(n).
    
- `bucket_count()`: Returns the number of buckets in the hash table.
    
- `load_factor()`: Returns the average number of elements per bucket.
    
- `max_load_factor()`: Gets or sets the maximum load factor before the container rehashes.
    
- `rehash(n)`: Sets the number of buckets to `n` or more.
    
- `reserve(n)`: Sets the number of buckets to accommodate at least `n` elements without rehash.