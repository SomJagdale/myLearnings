### 1. **Explain the differences between a HashMap and a Balanced Tree in terms of time complexity, use cases, and memory consumption. When would you prefer one over the other?**

**Answer:**
- **HashMap (C++: `std::unordered_map`)**:
  - **Time Complexity**: Average O(1) for insertions, deletions, and lookups. In the worst case, due to hash collisions, the time complexity could degrade to O(n) if all elements hash to the same bucket (rare with a good hash function).
  - **Use Cases**: Ideal when you need fast lookups and don't care about the ordering of elements. Common use cases are cache implementations, frequency counting, or any scenario where key-value association is essential.
  - **Memory Consumption**: Higher memory overhead due to the storage of hash buckets and potential hash collisions. However, it is scalable, and the memory footprint grows with the load factor (typically around 0.75).

- **Balanced Tree (C++: `std::map`)**:
  - **Time Complexity**: O(log n) for insertions, deletions, and lookups because it is usually implemented as a Red-Black Tree (self-balancing binary search tree).
  - **Use Cases**: Use when the ordering of elements is important, like maintaining sorted keys, or when you require range queries (e.g., finding all elements within a specific range).
  - **Memory Consumption**: More memory efficient than a HashMap in some cases, but it requires extra memory to store pointers for parent, left, and right children. Memory usage increases logarithmically with the number of elements.
  
- **When to prefer one over the other**:
  - Use `HashMap` when you need fast lookups without caring about the order of keys.
  - Use `Balanced Tree` when you need sorted data or need to perform operations like finding the closest element in a range.

---

### 2. **How would you implement an LRU Cache from scratch using C++? Which data structures would you use and why?**

**Answer:**
- To implement an **LRU Cache** (Least Recently Used), the goal is to have O(1) access and O(1) updates when inserting or deleting cache entries.

- **Data Structures**:
  - **HashMap (`std::unordered_map`)**: To store key-value pairs with constant-time lookup.
  - **Doubly Linked List**: To keep track of the order in which elements are accessed. The most recently accessed element should be moved to the front, and the least recently used element should be at the back. The doubly linked list allows for O(1) removal and insertion.

- **Approach**:
  - Use the **HashMap** where the key is the cache key, and the value is a pointer to the corresponding node in the doubly linked list.
  - The **doubly linked list** maintains the access order. The head of the list will always contain the most recently accessed element, and the tail will contain the least recently accessed element.
  - On cache access or insertion:
    1. If the key exists in the map, move the corresponding node to the front of the list.
    2. If the key does not exist, insert it at the front of the list. If the cache is full, remove the node at the back of the list (the least recently used element).

---

### 3. **What is the difference between `std::vector` and `std::deque` in C++? Which scenarios would lead you to choose one over the other?**

**Answer:**
- **std::vector**:
  - **Structure**: A dynamic array that provides contiguous memory.
  - **Time Complexity**: O(1) for insertion at the end (amortized), O(n) for insertions at the beginning or in the middle.
  - **Use Case**: Use `std::vector` when you need fast access via indices and primarily work with appending data. It's cache-friendly because of the contiguous memory.

- **std::deque**:
  - **Structure**: A double-ended queue, where elements are stored in non-contiguous blocks.
  - **Time Complexity**: O(1) for insertions and deletions at both the front and the back.
  - **Use Case**: Use `std::deque` when you need efficient insertions and deletions at both ends of the container. `std::deque` provides the flexibility of efficient insertion/removal from both ends, which `std::vector` lacks.

---

### 4. **Explain the Red-Black Tree and how it guarantees O(log n) operations for insertion, deletion, and search.**

**Answer:**
- A **Red-Black Tree** is a balanced binary search tree that ensures O(log n) time complexity for insertion, deletion, and lookup by maintaining specific balancing properties.

- **Properties of Red-Black Tree**:
  1. Each node is either red or black.
  2. The root of the tree is always black.
  3. All leaves (NIL nodes) are black.
  4. If a node is red, both of its children must be black (no two red nodes can be adjacent).
  5. Every path from a node to its descendant leaves must have the same number of black nodes.

- **Balancing**:
  - When inserting or deleting nodes, the tree may temporarily violate its properties. The tree then uses **rotations** and **recoloring** to restore balance, ensuring that the height of the tree remains logarithmic in terms of the number of nodes.
  - The balancing operations (rotations) involve a fixed number of pointer changes, which is why the time complexity remains O(log n).

---

### 5. **What are B-Trees, and why are they frequently used in databases or file systems? How does their structure differ from binary trees?**

**Answer:**
- **B-Trees** are self-balancing multi-level trees optimized for systems that read and write large blocks of data, such as databases and file systems. 

- **Structure**:
  - Unlike binary trees, each node in a B-Tree can contain multiple keys and have multiple children. This reduces the height of the tree, minimizing the number of disk accesses required to find a specific key.
  - Each node can store **a variable number of keys**, and children are ordered such that all keys in a child node are within a certain range.

- **Why used in databases**:
  - B-Trees minimize **disk I/O** operations. Since disk access is slow, B-Trees aim to keep tree height low and store many keys in a single node, reducing the number of nodes that need to be loaded from disk.
  - **Balanced**: They maintain balance by splitting or merging nodes when they become too full or too empty, ensuring that operations (insertion, deletion, search) remain O(log n).

---

### 6. **How does the `std::unordered_map` achieve O(1) lookup in most cases? What are the underlying data structures, and how are hash collisions handled?**

**Answer:**
- **Underlying Data Structure**: `std::unordered_map` is implemented using a **hash table**.
  
- **O(1) Lookup**:
  - **Hash Function**: When inserting or searching for a key, the hash function calculates the hash value of the key, which determines the index (bucket) where the value is stored.
  - **Bucket Array**: The elements are stored in an array of buckets. The hash value determines the bucket index.

- **Handling Collisions**:
  - **Chaining**: The most common method for resolving collisions in C++'s `std::unordered_map`. Each bucket contains a linked list (or another structure) to hold multiple elements with the same hash value.
  - **Open Addressing** (less common): In this method, collisions are resolved by probing for an empty spot in the array using various probing strategies like linear probing or quadratic probing.

---

### 7. **What is the time complexity of inserting an element into a `std::set`? What data structure is `std::set` based on, and why?**

**Answer:**
- **Time Complexity**: The time complexity for inserting an element into `std::set` is **O(log n)**.
  
- **Underlying Data Structure**: `std::set` is usually implemented as a **Red-Black Tree** (self-balancing binary search tree).
  
- **Why Red-Black Tree**:
  - `std::set` requires ordered storage and uniqueness of elements. A Red-Black tree maintains these properties while ensuring that operations such as insertions, deletions, and lookups occur in logarithmic time.

---

### 8. **Can you explain skip lists and how they can be used to perform fast lookups, inserts, and deletes? How do they compare to balanced binary search trees?**

**Answer:**
- **Skip List**:
  - A **Skip List** is a data structure that allows fast search within an ordered sequence of elements, much like a balanced binary search tree. It achieves this by having multiple layers of linked lists.
  - The bottom-most layer is a simple linked list of all elements. Each higher layer contains fewer elements (like every second or fourth element), allowing you to "skip" parts of the list during search operations.

- **Operations**:
  - **Search**: O(log n) on average by skipping over large sections of the list.
  - **Insertion and Deletion**: O(log n) on average. When inserting or deleting, the structure is adjusted by updating pointers at various levels.

- **Comparison with Balanced Trees**:
  -
