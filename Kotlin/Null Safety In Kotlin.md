# Kotlin Null Safety

## 1. Nullable Types

In Kotlin, the type system distinguishes between references that can hold `null` and those that cannot.

- `Int`: Cannot be null.
    
- `Int?`: Can be an integer **or** null.
    

Kotlin

```
val x1 = readln().toInt()          // Type: Int (May throw exception if input is invalid)
val x2 = readln().toIntOrNull()    // Type: Int? (Returns null if input is invalid)
```

---

## 2. Handling Nullable Values

### The Not-Null Assertion (`!!`)

The `!!` operator converts any value to a non-nullable type and **throws a NullPointerException (NPE)** if the value is null. Use this only when you are 100% sure the value isn't null.

Kotlin

```
// Will crash if x2 or x3 is null
val x4 = x2!! + x3!! 
println("Addition of x2 + x3 is : $x4")
```

### Safe Call Operator (`?.`)

Executes the call only if the variable is **not null**. Otherwise, it returns `null`.

Kotlin

```
val x7: Int? = readln().toIntOrNull()
val x8 = x7?.inc() // If x7 is 4, x8 is 5. If x7 is null, x8 is null.
```

### Elvis Operator (`?:`)

Provides a **default value** if the expression on the left is null.

Kotlin

```
val x5 = readln().toIntOrNull() ?: 0 // Returns 0 if input is invalid
```

> [!WARNING] Logical Error in Example
> 
> In your code: `val x6 = readln().toInt() ?: 0`
> 
> This is redundant because `.toInt()` throws an exception before the Elvis operator can even check for null. Use `.toIntOrNull()` instead.

---

## 3. Advanced Features

### Smart Casts

If you check a variable for null using an `if` statement, Kotlin automatically "casts" it to a non-nullable type inside that block.

Kotlin

```
if (x7 != null) {
    // No ?. or !! needed here; x7 is treated as Int
    val square = x7 * x7
}
```

### Safe Cast (`as?`)

Tries to cast a value to a specific type. If the cast is impossible, it returns `null` instead of crashing.

Kotlin

```
val input: Any = "Hello"
val safeInt: Int? = input as? Int // Returns null
```

### The `.let` Function

Used to execute a block of code **only** if the object is not null. It uses `it` to refer to the non-null value.

Kotlin

```
val email: String? = readln().takeIf { it.isNotBlank() }
email?.let {
    println("Sending verification to: $it")
}
```

### Nullability in Collections

You can filter out nulls from a list easily using `filterNotNull()`.

Kotlin

```
val numbersWithNulls: List<Int?> = listOf(10, null, 30)
val onlyNumbers = numbersWithNulls.filterNotNull() // Result: [10, 30]
```

---

## 📊 Summary Comparison Table

| **Operator** | **Name**      | **Logic (If Null)**    | **Logic (If Not Null)** | **Use Case**                 |
| ------------ | ------------- | ---------------------- | ----------------------- | ---------------------------- |
| `?.`         | **Safe Call** | Returns `null`         | Executes access         | Accessing properties/methods |
| `?:`         | **Elvis**     | Returns **Right** side | Returns **Left** side   | Providing default values     |
| `!!`         | **Assertion** | **Throws NPE (Crash)** | Unwraps value           | When 100% certain            |
| `as?`        | **Safe Cast** | Returns `null`         | Performs cast           | Safe type conversion         |
| `?.let {}`   | **Scope**     | Skips block            | Executes block          |                              |