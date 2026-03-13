### `std::multimap` (Ordered Key-Value Pairs with Duplicate Keys)

- **`std::multimap`** is an associative container that stores elements formed by a combination of a **key value** and a **mapped value** (i.e., **key-value pairs**).
- Unlike `std::map`, it **allows duplicate keys**. This means multiple elements can have the same key, each with its own associated value.
- All elements are always stored in a **sorted order** based on their keys (ascending by default).
- It is useful when you need to store multiple pieces of information associated with the same key, while maintaining a sorted order.

#### **Underlying Mechanism:**

`std::multimap` is typically implemented using a **self-balancing binary search tree**, most commonly a Red-Black Tree, just like `std::map`. The tree structure ensures that elements are always kept in sorted order based on their keys. When duplicate keys are inserted, they are stored as separate nodes in the tree, maintaining the overall sorted order.

#### **Advantages:**

- **Allows Duplicate Keys**: This is its primary distinction from `std::map`, enabling one-to-many relationships where a single key can map to multiple values.
- **Unique & Sorted Keys**: Automatically ensures that all keys are unique and maintains elements in a sorted order based on keys.
- **Efficient Lookup, Insertion, Deletion**: All these operations have an average time complexity of **O(logn)** because of the balanced tree structure.
- **Ordered Traversal**: Iterating through a `multimap` always yields elements in sorted order of their keys. If there are duplicate keys, their relative order is stable (insertion order might be preserved for equal keys, depending on implementation).
- **Bidirectional Iterators**: Supports efficient traversal of elements in both forward and backward directions.

#### **Disadvantages:**

- **Logarithmic Time Complexity**: While O(logn) is efficient for many cases, it's slower than the average O(1) of hash-based containers (`unordered_multimap`) for lookup, insertion, and deletion.
- **Higher Memory Usage**: Typically uses more memory than `unordered_multimap` or simpler containers due to the overhead of storing tree node pointers and color information.
- **No Random Access by Index**: You cannot access elements by an integer index (e.g., `myMultimap[0]`). Access is always by key or iterator.
- **Keys Must Be Comparable**: Keys must support a strict weak ordering (e.g., the `<` operator or a custom comparator).

#### **Declaration of `multimap`:**

C++

```
#include <iostream>
#include <map> // multimap is in the <map> header
#include <string>
#include <functional> // For std::greater

using namespace std;

int main() {

    // Default initialization (empty multimap, keys in ascending order)
    multimap<string, int> studentScores; // string key, int value

    // Declaration with some initial key-value pairs
    multimap<string, string> courseEnrollment = {
        {"Math", "Alice"},
        {"Physics", "Bob"},
        {"Math", "Charlie"}, // Duplicate key "Math" is allowed
        {"Chemistry", "Alice"}
    }; // Keys automatically sorted: Chemistry, Math, Math, Physics

    // Multimap with keys in decreasing order
    multimap<int, string, greater<int>> descendingScores;

}
```

#### **Insertion & Traversing in Multimap:**

C++

```
#include <iostream>
#include <map> // For multimap
#include <string>

using namespace std;

int main() {
    multimap<string, int> studentScores;

    // Insertion with make_pair or pair
    studentScores.insert(make_pair("Alice", 95));
    studentScores.insert(pair<string, int>("Bob", 88));
    studentScores.insert(make_pair("Alice", 90)); // Duplicate key "Alice"
    studentScores.insert(make_pair("Charlie", 75));
    studentScores.insert(make_pair("Alice", 100)); // Another duplicate key "Alice"

    cout << "Elements in the multimap (Key-Value pairs):" << endl;
    // Using range-based for loop for easy traversal
    for (const auto& pair : studentScores) {
        cout << "Key: " << pair.first << ", Value: " << pair.second << endl;
    }
    // Expected Output (sorted by key, order of duplicates might vary slightly but often insertion order for equal keys):
    // Key: Alice, Value: 95
    // Key: Alice, Value: 90
    // Key: Alice, Value: 100
    // Key: Bob, Value: 88
    // Key: Charlie, Value: 75
    cout << endl;

    // Accessing elements with a specific key (returns a range of iterators)
    cout << "Scores for Alice:" << endl;
    auto range = studentScores.equal_range("Alice"); // Returns a pair of iterators
    for (auto it = range.first; it != range.second; ++it) {
        cout << "  " << it->second << endl; // Output: 95, 90, 100 (or other order for duplicates)
    }
    cout << endl;

    // Count occurrences of a key
    cout << "Number of scores for Alice: " << studentScores.count("Alice") << endl; // Output: 3
    cout << "Number of scores for David: " << studentScores.count("David") << endl; // Output: 0

    return 0;
}
```

#### **Deletion in Multimap:**

C++

```
#include <iostream>
#include <map> // For multimap
#include <string>

using namespace std;

int main() {
    multimap<string, int> myMultimap = {
        {"A", 10}, {"B", 20}, {"A", 15}, {"C", 30}, {"A", 5}
    };

    cout << "Initial multimap:" << endl;
    for (const auto& pair : myMultimap) {
        cout << pair.first << ": " << pair.second << endl;
    }
    cout << endl; // Sorted by key

    // Delete all elements with a specific key
    // Returns the number of elements removed
    size_t num_erased_A = myMultimap.erase("A");
    cout << "Elements with key 'A' erased: " << num_erased_A << endl; // Output: 3

    cout << "Multimap after erasing all 'A's:" << endl;
    for (const auto& pair : myMultimap) {
        cout << pair.first << ": " << pair.second << endl;
    }
    cout << endl; // Output will only have B and C

    // Re-add some elements for iterator erase example
    myMultimap.insert({"D", 40});
    myMultimap.insert({"D", 45});
    myMultimap.insert({"E", 50});
    cout << "Multimap after re-adding for iterator erase:" << endl;
    for (const auto& pair : myMultimap) {
        cout << pair.first << ": " << pair.second << endl;
    }
    cout << endl; // {B:20, C:30, D:40, D:45, E:50}

    // Delete using an iterator
    // Find the first occurrence of "D"
    auto it_to_erase = myMultimap.find("D");
    if (it_to_erase != myMultimap.end()) {
        myMultimap.erase(it_to_erase); // Erase just that specific element (D:40)
    }

    cout << "Multimap after erasing first 'D' (40):" << endl;
    for (const auto& pair : myMultimap) {
        cout << pair.first << ": " << pair.second << endl;
    }
    cout << endl; // {B:20, C:30, D:45, E:50}

    // Clear the multimap
    myMultimap.clear();

    cout << "Multimap size after clear: " << myMultimap.size() << endl; // Output: 0
    cout << "Is multimap empty after clear? " << (myMultimap.empty() ? "Yes" : "No") << endl;

    return 0;
}
```

#### **Member functions:**

- `insert(pair)`: Inserts a key-value `pair` into the multimap. Always successful as duplicate keys are allowed. O(logn) time complexity.
    
- `find(key)`: Returns an iterator to the first element with the specified `key`, or `end()` if the key is not found. O(logn) time complexity.
    
- `count(key)`: Returns the number of elements with the specified `key`. O(logn) time complexity + O(k) where k is the number of occurrences.
    
- `equal_range(key)`: Returns a `pair` of iterators defining the range of elements with the specified `key`. The first iterator points to the first occurrence, the second to one past the last occurrence. Useful for iterating over all values for a duplicate key. O(logn) time complexity + O(k) where k is the count.
    
- `erase(key)`: Removes all elements with the specified `key`. Returns the number of elements removed. O(logn) time complexity + O(k) where k is the count.
    
- `erase(iterator)`: Removes the element pointed to by `iterator`. O(logn) time complexity.
    
- `size()`: Returns the number of elements (key-value pairs) in the multimap.
    
- `empty()`: Checks if the multimap is empty.
    
- `clear()`: Removes all elements from the multimap.
    
- `begin()` and `end()`: Return iterators to the first (smallest key) and one-past-the-last elements.
    
- `rbegin()` and `rend()`: Return reverse iterators.
    
- `lower_bound(key)`: Returns an iterator pointing to the first element whose key is **not less than** the specified `key`. For duplicate keys, this points to the first one. O(logn) time complexity.
    
- `upper_bound(key)`: Returns an iterator pointing to the first element whose key is **greater than** the specified `key`. For duplicate keys, this points one past the last one. O(logn) time complexity.
    

---

### `std::unordered_multimap` (Unordered Key-Value Pairs with Duplicate Keys)

- **`std::unordered_multimap`** is an associative container that stores elements formed by a combination of a **key value** and a **mapped value** (i.e., **key-value pairs**).
- It **allows duplicate keys**, meaning multiple elements can have the same key.
- Unlike `std::multimap`, elements are **not stored in any particular sorted order**; their arrangement depends on the hash function.
- It is the unordered counterpart to `std::multimap`, offering high average performance for insertion, deletion, and lookup when order is not a concern.

#### **Underlying Mechanism:**

`std::unordered_multimap` is implemented using a **hash table**, similar to `std::unordered_map`. When a key-value pair is inserted, the key is hashed to determine its bucket. Since duplicate keys are allowed, multiple elements might reside in the same bucket. Collision resolution is handled internally, typically using chaining (e.g., linked lists within buckets).

#### **Advantages:**

- **Allows Duplicate Keys**: Its primary feature, allowing multiple key-value pairs with the same key.
- **Very Fast Average-Case Operations**:
    - **Lookup (`find()`):** Average O(1) time complexity.
    - **Insertion (`insert()`):** Average O(1) time complexity.
    - **Deletion (`erase()`):** Average O(1) time complexity.
- **No Ordering Overhead**: Since elements are not sorted, there's no overhead associated with maintaining a specific order, contributing to its speed.
- **Suitable for Large Datasets**: The near-constant average time complexity for core operations makes it highly suitable for applications requiring high performance on large amounts of data where order is irrelevant.

#### **Disadvantages:**

- **No Ordering**: Elements are not stored in any particular order. If you need sorted data or ordered traversal, use `std::multimap`.
- **Worst-Case Time Complexity**: In the worst-case scenario (e.g., poor hash function or many collisions), operations can degrade to **O(n) time complexity**. This is less common with good hash functions.
- **Higher Memory Usage**: Hash tables typically use more memory than tree-based multimaps due to the need for managing buckets and collision handling.
- **Keys Must Be Hashable**: Keys must be hashable. For custom types, you might need to provide a custom hash function.
- **No `lower_bound`/`upper_bound`**: These functions, which rely on sorted order, are not available.

#### **Declaration of `unordered_multimap`:**

C++

```
#include <iostream>
#include <unordered_map> // unordered_multimap is in the <unordered_map> header
#include <string>
#include <functional> // For std::hash

using namespace std;

int main() {

    // Default initialization (empty unordered_multimap)
    unordered_multimap<string, int> phoneBook; // string key (name), int value (phone number)

    // Declaration with some initial key-value pairs
    unordered_multimap<int, string> studentCourses = {
        {101, "Math"},
        {102, "Physics"},
        {101, "Chemistry"}, // Duplicate key 101 allowed
        {103, "Biology"}
    }; // Order is not guaranteed

}
```

#### **Insertion & Traversing in `unordered_multimap`:**

C++

```
#include <iostream>
#include <unordered_map> // For unordered_multimap
#include <string>

using namespace std;

int main() {
    unordered_multimap<string, string> studentHobbies;

    // Insertion with make_pair or pair
    studentHobbies.insert(make_pair("Alice", "Reading"));
    studentHobbies.insert(make_pair("Bob", "Gaming"));
    studentHobbies.insert(make_pair("Alice", "Hiking")); // Duplicate key "Alice"
    studentHobbies.insert(make_pair("Charlie", "Cooking"));
    studentHobbies.insert(make_pair("Alice", "Painting")); // Another duplicate key "Alice"

    cout << "Elements in the unordered_multimap (Key-Value pairs):" << endl;
    // Using range-based for loop for traversal
    // The order will be arbitrary, not sorted by key
    for (const auto& pair : studentHobbies) {
        cout << "Key: " << pair.first << ", Value: " << pair.second << endl;
    }
    cout << endl;

    // Accessing elements with a specific key (returns a range of iterators)
    cout << "Hobbies for Alice:" << endl;
    auto range = studentHobbies.equal_range("Alice"); // Returns a pair of iterators
    for (auto it = range.first; it != range.second; ++it) {
        cout << "  " << it->second << endl; // Order of duplicates is arbitrary
    }
    cout << endl;

    // Count occurrences of a key
    cout << "Number of hobbies for Alice: " << studentHobbies.count("Alice") << endl; // Output: 3
    cout << "Number of hobbies for David: " << studentHobbies.count("David") << endl; // Output: 0

    return 0;
}
```

#### **Deletion in `unordered_multimap`:**

C++

```
#include <iostream>
#include <unordered_map> // For unordered_multimap
#include <string>

using namespace std;

int main() {
    unordered_multimap<int, string> myUnorderedMultimap = {
        {1, "Apple"}, {2, "Banana"}, {1, "Cherry"}, {3, "Date"}, {1, "Fig"}
    };

    cout << "Initial unordered_multimap:" << endl;
    for (const auto& pair : myUnorderedMultimap) {
        cout << pair.first << ": " << pair.second << endl;
    }
    cout << endl; // Order will be arbitrary

    // Delete all elements with a specific key
    // Returns the number of elements removed
    size_t num_erased_1 = myUnorderedMultimap.erase(1);
    cout << "Elements with key '1' erased: " << num_erased_1 << endl; // Output: 3

    cout << "Unordered_multimap after erasing all '1's:" << endl;
    for (const auto& pair : myUnorderedMultimap) {
        cout << pair.first << ": " << pair.second << endl;
    }
    cout << endl; // Output will not include any key '1' elements

    // Re-add some elements for iterator erase example
    myUnorderedMultimap.insert({4, "Grape"});
    myUnorderedMultimap.insert({4, "Honeydew"});
    myUnorderedMultimap.insert({5, "Kiwi"});
    cout << "Unordered_multimap after re-adding for iterator erase:" << endl;
    for (const auto& pair : myUnorderedMultimap) {
        cout << pair.first << ": " << pair.second << endl;
    }
    cout << endl; // {2:Banana, 3:Date, 4:Grape, 4:Honeydew, 5:Kiwi} (arbitrary order)

    // Delete using an iterator
    // Find the first occurrence of "4"
    auto it_to_erase = myUnorderedMultimap.find(4);
    if (it_to_erase != myUnorderedMultimap.end()) {
        myUnorderedMultimap.erase(it_to_erase); // Erase just that specific element (e.g., 4:Grape)
    }

    cout << "Unordered_multimap after erasing first '4':" << endl;
    for (const auto& pair : myUnorderedMultimap) {
        cout << pair.first << ": " << pair.second << endl;
    }
    cout << endl; // Will have one less '4' element

    // Clear the unordered_multimap
    myUnorderedMultimap.clear();

    cout << "Unordered_multimap size after clear: " << myUnorderedMultimap.size() << endl; // Output: 0
    cout << "Is unordered_multimap empty after clear? " << (myUnorderedMultimap.empty() ? "Yes" : "No") << endl;

    return 0;
}
```

#### **Member functions:**

- `insert(pair)`: Inserts a key-value `pair` into the `unordered_multimap`. Always successful. Average O(1) time complexity, worst-case O(n).
    
- `find(key)`: Returns an iterator to the first element with the specified `key` (arbitrary order), or `end()` if the key is not found. Average O(1) time complexity, worst-case O(n).
    
- `count(key)`: Returns the number of elements with the specified `key`. Average O(1) time complexity + O(k) where k is the number of occurrences, worst-case O(n).
    
- `equal_range(key)`: Returns a `pair` of iterators defining the range of elements with the specified `key`. Useful for iterating over all values for a duplicate key. Average O(1) time complexity + O(k) where k is the count, worst-case O(n).
    
- `erase(key)`: Removes all elements with the specified `key`. Returns the number of elements removed. Average O(1) time complexity + O(k) where k is the count, worst-case O(n).
    
- `erase(iterator)`: Removes the element pointed to by `iterator`. Average O(1) time complexity, worst-case O(n).
    
- `size()`: Returns the number of elements (key-value pairs) in the `unordered_multimap`.
    
- `empty()`: Checks if the `unordered_multimap` is empty.
    
- `clear()`: Removes all elements from the `unordered_multimap`.
    
- `begin()` and `end()`: Return iterators to the first and one-past-the-last elements in their current (arbitrary) order.
    
- `bucket_count()`: Returns the number of buckets in the hash table.
    
- `load_factor()`: Returns the average number of elements per bucket.
    
- `max_load_factor()`: Gets or sets the maximum load factor before the container rehashes.
    
- `rehash(n)`: Sets the number of buckets to `n` or more.
    
- `reserve(n)`: Sets the number of buckets to accommodate at least `n` elements without rehash.