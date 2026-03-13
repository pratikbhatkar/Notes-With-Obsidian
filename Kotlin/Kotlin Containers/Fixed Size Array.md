## Kotlin Fixed-Size Arrays: Primitive vs. Boxed

In Kotlin, arrays are invariant and fixed in size. However, there is a massive performance and memory difference between using a **Primitive Array** (`IntArray`, `DoubleArray`, etc.) and a **Boxed Array** (`Array<T>`).
### Imports
- **`IntArray`**: **No import needed.** It is part of the `kotlin` package which is imported by default in every single file.
    
- **`Array<Int>`**: **No import needed.** This is also a core part of the language.

### Support in KMP
	Array<Int> and IntArray both are 100% percent supported in Kotlin Multiplatform
	
## 1. `IntArray` (The Primitive Array)

This is the Kotlin equivalent of a raw Java `int[]`. It is optimized for performance and memory efficiency.

- **Contiguous Memory**: All numbers are stored right next to each other in one solid block of memory.
    
- **No Overhead**: It stores the raw binary representation of the number (e.g., `5` is just `0101`).
    
- **Cache Friendly**: It is extremely fast because the CPU can predict where the next number is (spatial locality), reducing "cache misses."
    
- **C++ Equivalent**: `int arr[5];`

___

#### 1. The Standard Initializers (Fixed Size)

These are the most common ways to create an array when you know the size upfront.

- **Default (All Zeros):** `val arr = IntArray(5)`
	_(Creates: `[0, 0, 0, 0, 0]`)_
    
- **With a Custom Default Value:** `val arr = IntArray(5) { 10 }`
    _(Creates: `[10, 10, 10, 10, 10]`)_
    
- **Using the Index (Lambda):** `val arr = IntArray(5) { it * 2 }`
    _(Creates: `[0, 2, 4, 6, 8]`. Here, `it` is the index.)_
    

___

#### 2. The Factory Functions (Known Elements)

Use these when you already know the specific numbers you want to store.

- **`intArrayOf` (Most Common):** 
    `val arr = intArrayOf(1, 2, 3, 4, 5)`
- **From a Collection:** 
    `val list = listOf(1, 2, 3)`
    `val arr = list.toIntArray()`

___

#### 3. The "Empty" Declarations

Sometimes you need to declare the variable now but initialize it later (common in Android Fragments or Activities).

- **Empty Array:** `var arr = intArrayOf()`
    
    _(Size is 0; useful as a placeholder.)_
    
- **Late Initialization (`lateinit` does NOT work for IntArray):** Because `IntArray` is a primitive-like type in Kotlin, you cannot use `lateinit`. You must use:
    
    `var arr: IntArray? = null`

___

#### Some functions & Properties of IntArray

```
fun main() {
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 1. DECLARATION & INITIALIZATION
    
    val nums = intArrayOf(5, 2, 9, 1, 5, 6)
    
    val emptyTable = IntArray(10) { 0 } // Size 10, all zeros
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 2. CORE PROPERTIES
    
    println("Size: ${nums.size}")               // 6
    
    println("Last Index: ${nums.lastIndex}")    // 5
    
    println("Indices Range: ${nums.indices}")   // 0..5
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 3. SEARCHING & CHECKING
    
    val hasFive = 5 in nums                     // true
    
    val firstIdx = nums.indexOf(9)              // 2
    
    val lastIdx = nums.lastIndexOf(9)           // 2 
    
    val exists = nums.any { it > 10 }           // false (any element > 10?)
    
    val allPositive = nums.all { it >= 0 }      // true (all elements >= 0?)
    
    val countFives = nums.count { it == 5 }     // 2
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 4. TRANSFORMATIONS (Returns NEW collections)
    
    val doubled = nums.map { it * 2 }           // [10, 4, 18, 2, 10, 12]
    
    val evens = nums.filter { it % 2 == 0 }     // [2, 6]
    
    val unique = nums.distinct()                // [5, 2, 9, 1, 6]
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 5. IN-PLACE UTILITIES (Modifies the original array)
    
    nums.sort()                                 // [1, 2, 5, 5, 6, 9]
    
    // nums.sortDescending()
    
    // nums.reverse()
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 6. CALCULATIONS
    
    val maxVal = nums.maxOrNull()               // 9
    
    val minVal = nums.minOrNull()               // 1
    
    val total = nums.sum()                      // 28
    
    val average = nums.average()                // 4.66...
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 7. SLICING & COPYING
    
    val firstThree = nums.take(3)               // [1, 2, 5]
    
    val copy = nums.copyOf()                    // New IntArray instance
    
    val part = nums.copyOfRange(1, 4)           // [2, 5, 5] (exclusive of end)
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 8. ITERATION IDIOMS
    // Traditional
    
    for (n in nums) { /* ... */ }
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // Functional with Index (Best for DSA)
    
    nums.forEachIndexed { index, value ->
        println("Index $index has value $value")
    }
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 9. ADVANCED: PREFIX SUMS (Great for LeetCode)
    // Computes running totals: [1, 1+2, 1+2+5...]
    
    val prefixSum = nums.scan(0) { acc, n -> acc + n }
}
```

___

#### Specialized Primitive Arrays

Just like `IntArray`, Kotlin provides specialized, fixed-size primitive arrays for every basic type to avoid the "boxing" overhead of `Array<T>`.

| **Kotlin Type**    | **Java Equivalent** | **C++ Equivalent**        |
| ------------------ | ------------------- | ------------------------- |
| **`BooleanArray`** | `boolean[]`         | `bool arr[N]`             |
| **`ByteArray`**    | `byte[]`            | `char arr[N]` or `int8_t` |
| **`CharArray`**    | `char[]`            | `char16_t arr[N]`         |
| **`LongArray`**    | `long[]`            | `long long arr[N]`        |
| **`DoubleArray`**  | `double[]`          | `double arr[N]`           |

___

## 2. `Array<Int>` (The Boxed Array)

This compiles down to a Java `Integer[]`. It is an array of objects rather than raw values.

- **Array of Pointers**: The array itself doesn't hold numbers; it holds "addresses" (pointers) to memory locations where the numbers are stored.
    
- **Object Overhead**: Each number is "Boxed" inside an `Integer` object. Every object has extra metadata (headers) that takes up significant space.
    
- **Memory Fragmentation**: The objects can be scattered anywhere in the heap. The CPU has to "jump" to a new memory location for every single element access (Pointer Chasing).
    
- **C++ Equivalent**: `Integer** arr = new Integer*[5];` (An array of pointers to `Integer` objects).

#### 1. The Standard Initializers

- **Lambda Initialization (Required):** Unlike `IntArray`, you **must** provide a lambda to initialize the objects :
	`val arr = Array<Int>(5) { 0 }` 
	_(Creates: `[0, 0, 0, 0, 0]` where each `0` is an object)_
    
- **Using the Index:**
	`val arr = Array(5) { it * it }` 
	_(Creates: `[0, 1, 4, 9, 16]`)_
    

---

#### 2. The Factory Functions

- **`arrayOf` (Most Common):** `val arr = arrayOf(1, 2, 3)`
    
- **From a Collection:** `val list = listOf(1, 2, 3)` `val arr = list.toTypedArray()`
    
- **Nullable Array:** This is a major use case for `Array<Int>`. `val arr = arrayOfNulls<Int>(5)` _(Creates: `[null, null, null, null, null]`)_
    

---

#### 3. The "Empty" Declarations

- **Empty Array:** `var arr = emptyArray<Int>()`
    
- **Late Initialization:** Because `Array<Int>` is an object type, you **can** use `lateinit`! `lateinit var arr: Array<Int>`
    

---

#### Functions & Properties of Array


```
fun main() {
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 1. DECLARATION & INITIALIZATION
    
    val nums = arrayOf(5, 2, 9, 1, 5, 6) // Inferred as Array<Int>
    
    val nulls = arrayOfNulls<Int>(3)     // [null, null, null]
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 2. CORE PROPERTIES (Identical to IntArray)
    
    println("Size: ${nums.size}")               // 6
    
    println("Last Index: ${nums.lastIndex}")    // 5
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 3. SEARCHING & CHECKING
    
    val firstIdx = nums.indexOf(9)              // 2
    
    val lastIdx = nums.lastIndexOf(5)           // 4
    
    val hasNull = nulls.any { it == null }      // true
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 4. TRANSFORMATIONS
    
    val asStrings = nums.map { "Value: $it" }   // List<String>
    
    val filtered = nums.filterNotNull()         // Removes nulls (if Array<Int?>)
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 5. IN-PLACE UTILITIES
    
    nums.sort()                                 // [1, 2, 5, 5, 6, 9]
    // Warning: sort() on Array<Int> uses object comparison (slightly slower)
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 6. CALCULATIONS (Extensions)
    
    val total = nums.sum()                      // 28
    // Note: sum() involves unboxing each Integer back to a primitive int.
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 7. CONVERSION (Critical for LeetCode)
    
    val primitive = nums.toIntArray()           // Converts Array<Int> -> IntArray
    
    val list = nums.toList()                    // Converts Array<Int> -> List<Int>
    
//–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
    // 8. ITERATION
    
    nums.forEach { n -> 
	    print("$n ")
	}
}
```
## _Comparison Table_

| **Feature**      | **IntArray (int[])**           | **Array<Int> (Integer[])**                 |
| ---------------- | ------------------------------ | ------------------------------------------ |
| **Storage**      | Raw values (inline)            | Pointers to objects                        |
| **Memory Usage** | Small (~4 bytes per int)       | Large (~16-24 bytes per element)           |
| **Performance**  | **Very Fast** (Cache friendly) | **Slower** (Pointer chasing + GC pressure) |
| **Nullability**  | Cannot be null (0 is default)  | Can be null (null is default)              |
| **Generics**     | Cannot be used in `<T>`        | Works with Generics/Collections            |
