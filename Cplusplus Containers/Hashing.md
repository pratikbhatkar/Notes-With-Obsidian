## Hashing (C++)

### What is Hashing (DSA Context)?

In DSA, hashing is primarily about creating a super-efficient way to store and retrieve data. Think of it like organizing a massive collection of items (data) so you can find any specific item almost instantly, no matter how many items there are.

The core idea is to take a **key** (the thing you want to look up, like a name or ID) and use a special **hash function** to turn it into an **index** (a number) where you can store or find its associated **value**.

**Analogy:** Imagine a huge apartment building. Each apartment has a number (index), and a hash function is like a magical calculator that, given a person's name (key), tells you exactly their apartment number.

### The Power of Hashing: Hash Tables!

The main data structure that uses hashing is called a **Hash Table** (also known as a **Hash Map**, **Dictionary**, or **Associative Array**).

- **What it does:** Stores data as `(key, value)` pairs.
- **Superpower:** Allows for **average O(1) time complexity** for insertions, deletions, and lookups. This means it's incredibly fast, regardless of how much data you have! (Almost like magic).

**In C++:**

- You'll primarily use `std::unordered_map` for key-value pairs.
- For storing unique items quickly (like a set), you'll use `std::unordered_set`.

### Components of a Hash Table:

1. **Hash Function:**
    - Takes a `key` as input.
    - Computes an `index` (or hash value) where the key-value pair should be stored in an array (often called a "bucket array").
    - **Goal:** Distribute keys evenly to minimize collisions.
2. **Bucket Array:**
    - An underlying array (or vector in C++) where the actual data `(key, value)` pairs are stored.
    - Each position in this array is called a "bucket."
3. **Collision Handling Mechanism:**
    - What happens if two different keys produce the same index (i.e., want to go into the same bucket)? This is a **collision**.
    - Since collisions are inevitable (especially with many items), hash tables need a way to deal with them.

### Common Hash Function Methods:

While C++'s `std::unordered_map` and `std::unordered_set` have highly optimized internal hash functions, understanding these basic methods helps grasp how a hash function converts a key into an index.

1. **Division Method:**
    
    - **Idea:** The simplest and most common method. The hash index is found by taking the remainder of the key divided by the size of the hash table.
    - **Formula:** `h(key) = key % TableSize`
    - **Best Practice:** Choose `TableSize` (the number of buckets) as a **prime number**. This helps distribute keys more evenly and reduces collisions, especially if keys have common factors.
    - **Example:** If `TableSize = 10` and `key = 25`, `h(25) = 25 % 10 = 5`. If `TableSize = 7` (prime) and `key = 25`, `h(25) = 25 % 7 = 4`.
2. **Mid-Square Method:**
    
    - **Idea:** Square the key, then extract a certain number of middle digits from the result to form the hash index.
    - **Steps:**
        1. Square the key (`key * key`).
        2. Take the middle `r` digits (where `r` determines the size of your hash index).
    - **Example:** If `key = 85`, `TableSize = 100` (so we need 2 digits for index).
        1. `85^2 = 7225`
        2. Take middle two digits: `22`. So, `h(85) = 22`.
    - **Note:** The quality of this method depends heavily on the chosen digits and original key distribution.
3. **Digit Folding (or Folding Method):**
    
    - **Idea:** Divide the key into several parts, then combine these parts (usually by adding them up) to form the hash index.
    - **Steps:**
        1. Divide the key into segments (e.g., 2 or 3 digits per segment).
        2. Add these segments together.
        3. If the sum is too large, you might apply the division method again (`sum % TableSize`).
    - **Example:** If `key = 123456789`, `TableSize = 1000`.
        1. Divide into 3-digit segments: `123`, `456`, `789`.
        2. Add them: `123 + 456 + 789 = 1368`.
        3. Apply modulo: `1368 % 1000 = 368`. So, `h(123456789) = 368`.
    - **Variation (Fold Shifting):** Add the segments, but reverse every other segment before adding. (e.g., `123 + 654 + 789`).
4. **Multiplication Method:**
    
    - **Idea:** Multiply the key by a constant `A` (where `0 < A < 1`), take the fractional part of the result, then multiply by the `TableSize`.
    - **Formula:** `h(key) = floor(TableSize * (key * A mod 1))`
        - `key * A mod 1` means the fractional part of `key * A`.
    - **Best Practice:** Often suggested that `A` be a value derived from the golden ratio (e.g., `A = (sqrt(5) - 1) / 2`).
    - **Example:** If `key = 123`, `TableSize = 100`, `A = 0.618034`.
        1. `key * A = 123 * 0.618034 = 75.908182`
        2. Fractional part (`0.908182`)
        3. `TableSize * fractional_part = 100 * 0.908182 = 90.8182`
        4. `floor(90.8182) = 90`. So, `h(123) = 90`.
    - **Note:** This method is often used in situations where the key might be very large or non-integer.

### Collision Handling Strategies (In Detail):

This is crucial for a robust hash table!

#### 1. Separate Chaining

- **Core Idea:** Each slot (bucket) in the hash table's underlying array doesn't hold just one item. Instead, it holds a **pointer to a linked list** (or a `std::vector`/`std::list` in C++).
- also known as `closed addressing` and `open hashing`. 
    
- **How it Works:**
    
    1. When you want to insert a `(key, value)` pair, you calculate its hash to get an `index`.
    2. You go to that `index` in the bucket array.
    3. You then simply **add the `(key, value)` pair to the linked list at that specific bucket**.
    4. If multiple keys hash to the same index, they all get added to the same linked list.
- **Retrieval:**
    
    1. Hash the `key` to get the `index`.
    2. Go to that `index` in the bucket array.
    3. **Traverse the linked list** at that bucket, comparing each item's key with the key you're searching for until you find a match or reach the end of the list.
- **Deletion:**
    
    1. Hash the `key` to get the `index`.
    2. Go to that `index` and remove the item from the linked list (standard linked list deletion).
- **Advantages:**
    
    - **Simple to implement.**
    - **Handles many collisions gracefully.** The table can effectively hold more elements than the number of buckets.
    - **Deletion is straightforward.**
    - Performance degrades gracefully as the load factor increases (just longer linked lists).
- **Disadvantages:**
    
    - **Uses more memory** due to linked list pointers/nodes.
    - Can have **cache performance issues** if linked lists are very long (items not stored contiguously in memory).
- **C++:** `std::unordered_map` and `std::unordered_set` are typically implemented using separate chaining.
    
    C++
    
    ```
    // Conceptual idea for a bucket in separate chaining
    // An array where each element is a list of (Key, Value) pairs
    // std::vector<std::list<std::pair<KeyType, ValueType>>> buckets;
    
    // Example: Adding to a bucket
    // int index = hashFunction(myKey);
    // buckets[index].push_back({myKey, myValue});
    ```
    

#### 2. Open Addressing

- **Core Idea:** All `(key, value)` pairs are stored directly within the main hash table array itself. If a hash function generates an index that is already occupied, the system **"probes"** for another empty slot in the array.
    
- **No linked lists!** This means better cache performance (data is contiguous).
    
- **How it Works (General Steps):**
    
    1. Calculate the initial `index` using the hash function.
    2. If the slot at `index` is empty, insert the item there.
    3. If the slot is occupied (a collision), use a **probing sequence** to find the next available empty slot.
- **Retrieval:**
    
    1. Calculate the initial `index`.
    2. Start probing along the same sequence used for insertion until you find the key you're looking for, or an empty slot (meaning the key isn't in the table), or you've checked all possible slots.
- **Deletion:** More complex! If you simply remove an item, it might create a "gap" that breaks the probe sequence for other items that were placed _after_ the deleted item.
    
    - Often handled with **"lazy deletion"**: Mark the slot as "deleted" (a special "tombstone" marker) instead of truly emptying it. This means lookups can still pass through it, but new insertions can overwrite it.
- **Advantages:**
    
    - **Better cache performance** because elements are stored contiguously.
    - **Less memory overhead** (no pointers for linked lists).
- **Disadvantages:**
    
    - **More complex deletion.**
    - **"Clustering"** can be a major problem (see below). This can severely degrade performance, potentially to O(N) even on average.
    - The table **cannot hold more elements than its number of buckets**. The load factor must always be less than 1 (typically much lower, like 0.5-0.7).
- **Types of Probing (for finding the next empty slot):**
    
    - a. **Linear Probing:** * **Rule:** If `H(key)` is occupied, try `(H(key) + 1) % TableSize`, then `(H(key) + 2) % TableSize`, and so on. You probe linearly through the array. * **Problem:** **Primary Clustering.** If many keys hash to nearby initial indices, they create long contiguous blocks of occupied slots. This makes future insertions and lookups very slow, as you have to traverse these long blocks.
    
    - b. **Quadratic Probing:** * **Rule:** If `H(key)` is occupied, try `(H(key) + 1^2) % TableSize`, then `(H(key) + 2^2) % TableSize`, `(H(key) + 3^2) % TableSize`, etc. * **Improvement:** Reduces primary clustering by spreading out the probes more. * **Problem:** **Secondary Clustering.** Keys that hash to the _same_ initial index will still follow the _exact same_ probe sequence, leading to clusters among those specific keys. Also, it might not always find an empty slot if the table isn't at least half-empty and its size is a prime number.
    
    - c. **Double Hashing:** * **Rule:** Uses _two_ hash functions, `H1(key)` and `H2(key)`. * If `H1(key)` is occupied, the next probe is at `(H1(key) + 1 * H2(key)) % TableSize`, then `(H1(key) + 2 * H2(key)) % TableSize`, `(H1(key) + 3 * H2(key)) % TableSize`, etc. * **Improvement:** `H2(key)` provides a _different step size_ for each initial key. This effectively eliminates both primary and secondary clustering by creating unique probe sequences for almost every key. * **Best Open Addressing Method:** Generally considered the best strategy for open addressing.
    

### Time Complexity (Hash Tables):

|Operation|Average Case|Worst Case|
|:--|:--|:--|
|Insertion|O(1)|O(N)|
|Deletion|O(1)|O(N)|
|Lookup|O(1)|O(N)|


- **Why O(1) Average?** A good hash function distributes keys evenly, so most buckets have few items (separate chaining) or are found quickly (open addressing).
- **Why O(N) Worst Case?** If all keys hash to the _same_ bucket (bad hash function or adversarial input), it degenerates into a linked list (separate chaining) or a linear scan (open addressing), leading to O(N) time.

### Load Factor:

- **Definition:** `(Number of elements) / (Number of buckets)`
- **Importance:** A high load factor means more items per bucket (chaining) or more occupied slots (open addressing), increasing the chance of collisions and making operations slower (closer to worst-case O(N)).
- **Resizing/Rehashing:** When the load factor exceeds a certain threshold (e.g., 0.7 for chaining, 0.5-0.7 for open addressing), hash tables usually **rehash**. This means creating a larger bucket array and re-inserting all existing elements into the new, larger table. This operation is O(N) but happens infrequently.

### C++ Specifics: `std::unordered_map` and `std::unordered_set`

- **`std::unordered_map<Key, Value>`:** Stores `(Key, Value)` pairs.
- **`std::unordered_set<Key>`:** Stores unique `Key` values.

#### Common Member Functions for Problem Solving:

These are the methods you'll use most often with `unordered_map` and `unordered_set` in DSA problems:

- **`insert(element)`**: Inserts a new element. For `unordered_map`, this is a `std::pair<const Key, Value>`. For `unordered_set`, it's just the `Key`. Returns a `std::pair<iterator, bool>` indicating if insertion occurred.
    
    C++
    
    ```
    myMap.insert({key, value});
    mySet.insert(key);
    ```
    
- **`erase(key)` / `erase(iterator)`**: Removes element(s) associated with `key` or at `iterator`. Returns number of elements erased (1 or 0 for unique keys).
    
    C++
    
    ```
    myMap.erase(someKey);
    mySet.erase(someOtherKey);
    // Or with an iterator:
    // auto it = myMap.find(key_to_erase);
    // if (it != myMap.end()) myMap.erase(it);
    ```
    
- **`find(key)`**: Returns an iterator to the element with the specified `key`. If `key` is not found, returns `end()`. Useful when you need to access or modify the element directly _after_ finding it.
    
    C++
    
    ```
    auto it = myMap.find("apple");
    if (it != myMap.end()) {
        std::cout << "Value for apple: " << it->second << std::endl;
    }
    ```
    
- **`count(key)`**: Returns `1` if the `key` exists in the container, `0` otherwise. Useful for simply checking presence without getting an iterator.
    
    C++
    
    ```
    if (mySet.count(5)) {
        std::cout << "5 is in the set." << std::endl;
    }
    ```
    
- **`operator[](key)`**:
    
    - **For `unordered_map` only.**
    - **Access:** If `key` exists, returns a reference to its `value`.
    - **Insertion:** If `key` does _not_ exist, inserts a new `(key, default_constructed_value)` pair and returns a reference to the newly inserted `value`. Be careful, as this can unexpectedly insert elements.
    
    C++
    
    ```
    myMap["newKey"] = 100; // Inserts if "newKey" doesn't exist, then assigns 100
    int value = myMap["existingKey"]; // Accesses value
    ```
    
- **`at(key)`**:
    
    - **For `unordered_map` only.**
    - Accesses the `value` associated with `key`. If `key` does not exist, it throws an `std::out_of_range` exception. Safer than `operator[]` for pure lookups if you expect the key to be present.
    
    C++
    
    ```
    try {
        std::cout << myMap.at("safeKey") << std::endl;
    } catch (const std::out_of_range& oor) {
        std::cerr << "Key not found: " << oor.what() << std::endl;
    }
    ```
    
- **`size()`**: Returns the number of elements in the container.
    
    C++
    
    ```
    std::cout << "Map size: " << myMap.size() << std::endl;
    ```
    
- **`empty()`**: Returns `true` if the container is empty, `false` otherwise.
    
    C++
    
    ```
    if (mySet.empty()) {
        std::cout << "Set is empty." << std::endl;
    }
    ```
    
- **`clear()`**: Removes all elements from the container.
    
    C++
    
    ```
    myMap.clear();
    ```
    
- **`begin()`, `end()`**: Return iterators to the first and one-past-the-last elements, respectively. Used for iterating through the container.
    
    C++
    
    ```
    for (const auto& pair : myMap) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }
    for (int num : mySet) {
        std::cout << num << " ";
    }
    ```
    

#### Custom Hash Functions (for custom objects):

If you want to use your own class as a `Key` in `unordered_map` or `unordered_set`, you need to tell C++ how to hash it and how to compare two objects for equality (`operator==`).

C++

```
#include <iostream>
#include <string>
#include <unordered_map>
#include <functional> // For std::hash

// 1. Define your custom struct/class
struct Point {
    int x, y;
    // 2. Define equality operator (REQUIRED for unordered_map/set)
    bool operator==(const Point& other) const {
        return x == other.x && y == other.y;
    }
};

// 3. Define a custom hash struct for Point
//    This must be a struct with an overloaded operator()
struct PointHash {
    std::size_t operator()(const Point& p) const {
        // A simple way to combine hashes (can be more sophisticated)
        // std::hash<T>()(value) calculates the default hash for type T
        // Combining hashes using XOR and bit shifts is a common practice.
        return std::hash<int>()(p.x) ^ (std::hash<int>()(p.y) << 1);
    }
};

int main() {
    // Use your custom Point with unordered_map
    // The third template argument is your custom hash functor
    std::unordered_map<Point, std::string, PointHash> myMap;

    myMap[{1, 2}] = "First Point";
    myMap[{3, 4}] = "Second Point";
    myMap[{1, 2}] = "Updated First Point"; // Updates existing

    if (myMap.count({1, 2})) {
        std::cout << "Point (1,2) found: " << myMap.at({1, 2}) << std::endl;
    }

    myMap.erase({3, 4});
    std::cout << "Map size after erase: " << myMap.size() << std::endl;

    return 0;
}
```

- **Explanation of `PointHash`:**
    - `std::hash<int>()(p.x)`: Gets the default hash for `p.x`.
    - `^`: Bitwise XOR, a common way to combine hash values.
    - `<< 1`: Left shift by 1 to make the `y` hash bits distinct from `x` hash bits, reducing simple collisions.

### When to Use Hashing in DSA:

- When you need **fast average-case lookups, insertions, and deletions**.
- Implementing caches.
- Counting frequencies of items (e.g., word counts).
- Checking for duplicates in a collection.
- Graph algorithms (e.g., storing visited nodes).

### When NOT to Use Hashing:

- When you need to keep data **sorted** (Hash tables don't preserve order). Use `std::map` (balanced binary search tree) instead for sorted data, but it's O(log N) for operations.
- When memory is extremely constrained (hash tables can have overhead for buckets and pointers).
- When worst-case performance is critical (though this is rare with good hash functions).