# Read-Only Collections (Immutable)

These collections do not have `add`, `remove`, or `clear` methods. They are thread-safe and ideal for passing data between different screens in an **Android** app.

## List

A generic ordered collection of elements. Elements can be accessed by their indices.

- **Factory**: `listOf(1, 2, 3)`
    
- **C++ Equivalent**: `const std::vector<int>`
    
- **Use Case**: Displaying a static list of items in a UI.
    

## Set

A collection that contains only unique elements.

- **Factory**: `setOf(1, 2, 2, 3)` (Result: `[1, 2, 3]`)
    
- **C++ Equivalent**: `const std::unordered_set<int>`
    
- **Use Case**: Checking if a specific ID exists in a collection.
    

## Map

A collection of key-value pairs where keys are unique.

- **Factory**: `mapOf("key" to 10)`
    
- **C++ Equivalent**: `const std::unordered_map<string, int>`
    
- **Use Case**: Passing configuration settings or user profiles.
    

---

# Mutable Collections

These collections allow you to modify the contents after creation. You will use these for almost every **LeetCode** problem where you are building a result dynamically.

## MutableList

A list that supports adding and removing elements.

- **Implementation**: Defaults to `ArrayList`.
    
- **Factory**: `mutableListOf<Int>()`
    
- **Common Functions**: `add()`, `removeAt()`, `set(index, value)`.
    
- **C++ Equivalent**: `std::vector<int>`
    

___

### Functions & Properties of MutableList

```
fun main() {
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 1. DECLARATION & INITIALIZATION
    
    val nums = mutableListOf(5, 2, 9, 1, 5, 6)
    
    val emptyList = mutableListOf<Int>()
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 2. CORE PROPERTIES
    
    println("Size: ${nums.size}")                // 6
    
    println("Is Empty: ${nums.isEmpty()}")      // false
    
    println("Indices: ${nums.indices}")          // 0..5
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 3. MODIFICATION (The "Mutable" Part)
    
    nums.add(10)                                // [5, 2, 9, 1, 5, 6, 10]
    
    nums.add(0, 99)                             // Insert 99 at index 0
    
    nums.remove(5)                              // Removes the FIRST occurrence of 5
    
    nums.removeAt(2)                            // Removes element at index 2
    
    nums[1] = 7                                 // Update index 1 to 7
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 4. SEARCHING & CHECKING
    
    val hasNine = 9 in nums                     // true
    
    val firstIdx = nums.indexOf(5)              // Returns index of first 5
    
    val lastIdx = nums.lastIndexOf(5)           // Returns index of last 5
    
    val allEven = nums.all { it % 2 == 0 }      // false
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 5. TRANSFORMATIONS (Returns NEW collections)
    
    val doubled = nums.map { it * 2 }           // [10, 4, 18...] (New List)
    
    val filtered = nums.filter { it > 5 }       // Elements > 5 (New List)
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 6. IN-PLACE UTILITIES (Modifies original)
    
    nums.sort()                                 // Sorts in ascending order
    
    nums.subList(1, 4).sort()                   // subList(fromIndex, toIndex)
    
    nums.shuffle()                              // Randomizes the order
    
    nums.clear()                                // Removes all elements (Size becomes 0)
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 7. CALCULATIONS
    
    val max = nums.maxOrNull()                  // Highest value
    
    val total = nums.sum()                      // Sum of all elements
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 8. CONVERSIONS
    
    val readOnlyList = nums.toList()            // Returns an Immutable List
    
    val primitiveArray = nums.toIntArray()      // Converts to IntArray (for performance)
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 9. ITERATION IDIOMS
    
    nums.forEach { println(it) }
    
    nums.removeIf { it < 2 }                // Removes all elements less than 2
}
```

___

## MutableSet

A set that supports adding and removing unique elements.

- **Implementation**: Defaults to `LinkedHashSet` (preserves insertion order).
    
- **Factory**: `mutableSetOf<Int>()`
    
- **Common Functions**: `add()`, `remove()`.
    
- **C++ Equivalent**: `std::unordered_set<int>`
    
### Functions & Properties of MutableSet

```
fun main() {
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 1. DECLARATION & INITIALIZATION
    
    val nums = mutableSetOf(5, 2, 9, 1, 6)
    
    val emptySet = mutableSetOf<Int>()
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 2. CORE PROPERTIES
    
    println("Size: ${nums.size}")                // 5
    
    println("Is Empty: ${nums.isEmpty()}")      // false
    
    // Note: Sets do not have indices like Lists, but you can still iterate
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 3. MODIFICATION (The "Unique" Part)
    
    nums.add(10)                                // [5, 2, 9, 1, 6, 10]
    
    nums.add(5)                                 // Does nothing (5 already exists)
    
    nums.remove(2)                              // Removes the element 2
    
    val addedAll = nums.addAll(listOf(7, 8))    // Adds multiple elements at once
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 4. SEARCHING & CHECKING (Extremely Fast O(1))
    
    val hasNine = 9 in nums                     // true (much faster than List)
    
    val containsAll = nums.containsAll(setOf(1, 5)) // true
    
    val anyGreaterThanTen = nums.any { it > 10 }   // false
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 5. TRANSFORMATIONS (Returns NEW collections)
    
    val doubled = nums.map { it * 2 }           // Returns a LIST [10, 18, 2...]
    
    val filtered = nums.filter { it > 5 }       // Returns a LIST of elements > 5
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 6. SET OPERATIONS (Mathematics)
    
    val otherSet = setOf(1, 6, 100)
    
    val intersect = nums.intersect(otherSet)    // Elements in BOTH
    
    val union = nums.union(otherSet)            // Combined unique elements
    
    val subtract = nums.subtract(otherSet)      // Elements in nums NOT in otherSet
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 7. IN-PLACE UTILITIES
    
    nums.removeIf { it % 2 == 0 }               // Removes all even numbers
    
    nums.clear()                                // Size becomes 0
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 8. CONVERSIONS
    
    val list = nums.toList()                    // Converts to ordered List
    
    val array = nums.toIntArray()               // Converts to primitive IntArray
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 9. ITERATION IDIOMS
    
    for (element in nums) { println(element) }
    
    nums.forEach { println(it) }
}
```

___

## MutableMap

A map that supports adding, updating, and removing key-value pairs.

- **Implementation**: Defaults to `LinkedHashMap`.
    
- **Factory**: `mutableMapOf<String, Int>()`
    
- **Common Functions**: `put()`, `remove()`, `getOrPut()`.
    
- **C++ Equivalent**: `std::unordered_map<string, int>`

### Functions & Properties of MutableMap

```
fun main() {
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 1. DECLARATION & INITIALIZATION
    
    val scores = mutableMapOf("Alice" to 1426, "Bob" to 1200)
    
    val emptyMap = mutableMapOf<String, Int>()
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 2. CORE PROPERTIES
    
    println("Size: ${scores.size}")               // 2
    
    println("Keys: ${scores.keys}")               // [Alice, Bob] (A Set)
    
    println("Values: ${scores.values}")           // [1426, 1200]
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 3. MODIFICATION
    
    scores["Charlie"] = 1500                      // Add or Update
    
    scores.put("Dave", 1100)                      // Same as scores["Dave"] = 1100
    
    scores.remove("Bob")                          // Removes key and its value
    
    // THE BEST FUNCTION FOR DSA:
    // If "Eve" isn't there, add her with 1000 and return it
    val eveScore = scores.getOrPut("Eve") { 1000 } 
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 4. SEARCHING & DEFAULTING
    
    val myScore = scores["Alice"]                 // 1426 (Returns null if missing)
    
    val unknown = scores.getOrDefault("Zack", 0)  // Returns 0 because Zack is missing
    
    val hasKey = scores.containsKey("Alice")      // true
    
    val hasValue = scores.containsValue(1500)     // true (Warning: O(n) operation)
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 5. TRANSFORMATIONS (Returns NEW Map/List)
    
    val filtered = scores.filter { it.value > 1300 } 
    
    val boosted = scores.mapValues { it.value + 100 }
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 6. ITERATION IDIOMS
    
    // Destructuring in loops (Very clean!)
    for ((name, score) in scores) {
        println("$name has $score")
    }
    
    scores.forEach { (name, score) -> println(name) }
}
```