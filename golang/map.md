# Maps

In Go (Golang), a **map** is a built-in data structure that associates keys with values, allowing efficient lookups, insertions, and deletions. It’s similar to dictionaries or hash maps in other languages. Here’s a concise overview:

### Key Characteristics
- **Unordered**: Maps do not maintain insertion order.
- **Key-Value Pairs**: Keys must be of a comparable type (e.g., strings, numbers, structs with comparable fields). Values can be of any type.
- **Reference Type**: Maps are reference types, meaning they are `nil` until initialized.
- **Dynamic**: Maps can grow or shrink as elements are added or removed.

### Syntax
```go
// Declare a map
var m map[KeyType]ValueType

// Initialize a map
m = make(map[KeyType]ValueType)

// Shorthand declaration and initialization
m := map[KeyType]ValueType{}

// Example
m := map[string]int{
    "apple":  5,
    "banana": 10,
}
```

### Basic Operations
1. **Insert/Update**:
   ```go
   m["key"] = value // Adds or updates the key-value pair
   ```

2. **Retrieve**:
   ```go
   value := m["key"] // Returns the value for the key, or the zero value if the key doesn’t exist
   ```

3. **Check Existence**:
   ```go
   value, exists := m["key"] // Returns value and a boolean indicating if the key exists
   if exists {
       fmt.Println("Key exists with value:", value)
   }
   ```

4. **Delete**:
   ```go
   delete(m, "key") // Removes the key-value pair
   ```

5. **Length**:
   ```go
   len(m) // Returns the number of key-value pairs
   ```

### Example Program
```go
package main

import "fmt"

func main() {
    // Initialize a map
    fruits := make(map[string]int)

    // Add key-value pairs
    fruits["apple"] = 5
    fruits["banana"] = 10

    // Update value
    fruits["apple"] = 7

    // Access value
    fmt.Println("Apples:", fruits["apple"]) // Output: Apples: 7

    // Check if key exists
    count, exists := fruits["orange"]
    if !exists {
        fmt.Println("Orange not found")
    }

    // Delete a key
    delete(fruits, "banana")

    // Iterate over map
    for key, value := range fruits {
        fmt.Printf("%s: %d\n", key, value)
    }

    // Length of map
    fmt.Println("Length:", len(fruits))
}
```

### Notes
- **Nil Maps**: A `nil` map cannot be used for assignments or lookups. Use `make()` or a map literal to initialize.
- **Concurrency**: Maps are not safe for concurrent use. Use mutexes or channels for concurrent access.
- **Performance**: Maps provide O(1) average-case complexity for lookups, insertions, and deletions.

If you have a specific question about maps (e.g., sorting, concurrency, or use cases), let me know!