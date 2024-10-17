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

### 8. **What are Fibonacci Heaps, and how do they differ from Binary Heaps? In what situations would you prefer using a Fibonacci Heap over a Binary Heap?**

**Answer:**
- **Fibonacci Heap**:
  - A **Fibonacci Heap** is a collection of trees (like a binomial heap) that supports more efficient amortized time complexity for some operations compared to binary heaps.
  - **Time Complexity**:
    - **Insert**: O(1) amortized.
    - **Find Min**: O(1).
    - **Decrease Key**: O(1) amortized, making Fibonacci heaps highly suitable for algorithms like Dijkstra’s or Prim’s that frequently perform decrease key operations.
    - **Extract Min**: O(log n) amortized.
  - **Structure**: Unlike binary heaps, Fibonacci heaps allow trees of any structure, and they maintain a looser form of balance using lazy deletion. It only consolidates trees during `extract min`, not during `insert` or `decrease key`.

- **Binary Heap**:
  - **Time Complexity**:
    - **Insert**: O(log n).
    - **Find Min**: O(1).
    - **Decrease Key**: O(log n).
    - **Extract Min**: O(log n).
  - Binary heaps are a simple and efficient structure for priority queues but lack the fast amortized `decrease key` operation that Fibonacci heaps excel at.

- **Use Case**: Use Fibonacci Heaps when `decrease key` operations are frequent (e.g., Dijkstra’s or Prim’s algorithm). For general-purpose use with fewer decrease key operations, a binary heap may be more straightforward and efficient.

---

### 9. **Explain Tries (Prefix Trees) and how they are used in applications like autocomplete or spell-checkers. How do they differ from hash-based structures?**

**Answer:**
- **Trie (Prefix Tree)**:
  - A **Trie** is a tree-like data structure that stores strings (usually words) by breaking them into individual characters. Each node represents a single character, and the path from the root to a node represents a prefix of a word.
  - **Operations**:
    - **Insert**: O(L), where L is the length of the string.
    - **Search**: O(L).
    - **Autocomplete**: Starting from a prefix, traverse the Trie to find all words that share that prefix.
    - **Spell-Check**: Use to find words that are similar or differ by a small number of characters.

- **Difference from Hash-Based Structures**:
  - **Memory Efficiency**: Tries are more memory-efficient for storing large sets of strings with shared prefixes because they reuse common prefixes.
  - **Ordered Data**: Tries inherently store data in lexicographical order, whereas hash-based structures do not maintain any ordering.
  - **Prefix Operations**: Hash-based structures like `unordered_map` or `hash_map` do not efficiently support operations like finding all keys that start with a given prefix, while Tries are optimized for such use cases.

---

### 10. **What is the difference between `std::vector` and `std::list` in terms of time complexity for insertions, deletions, and access?**

**Answer:**
- **std::vector**:
  - **Access**: O(1) for direct access via indices (random access).
  - **Insertion**:
    - O(1) (amortized) when inserting at the end.
    - O(n) when inserting at arbitrary positions, as all subsequent elements must be shifted.
  - **Deletion**: O(n) when deleting from arbitrary positions, due to shifting elements.
  - **Memory**: Contiguous memory allocation, which improves cache locality.

- **std::list** (doubly linked list):
  - **Access**: O(n), as direct access requires traversing the list.
  - **Insertion**: O(1) for inserting at arbitrary positions (if you have an iterator to the position), since only pointers are updated.
  - **Deletion**: O(1) for removing elements (if you have an iterator to the position), since only pointers are updated.
  - **Memory**: Non-contiguous, as each element is stored separately with pointers to the previous and next elements.

- **When to use**:
  - Use `std::vector` when you need fast access to elements by index and mostly append elements at the end.
  - Use `std::list` when frequent insertions and deletions from arbitrary positions are required, but direct element access is not a concern.

---

### 11. **How does a Binary Search Tree (BST) work, and what are the conditions for it to become unbalanced? How can you fix an unbalanced BST?**

**Answer:**
- **Binary Search Tree (BST)**:
  - A **BST** is a binary tree where the left child of any node contains values less than the node, and the right child contains values greater than the node. This property allows efficient searching, insertion, and deletion operations.
  
- **Conditions for Imbalance**:
  - A BST becomes **unbalanced** when nodes are inserted in such a way that one subtree is significantly deeper than the other. For example, inserting elements in ascending order results in a skewed tree (similar to a linked list), leading to O(n) time complexity for search operations.

- **Fixing Unbalanced BST**:
  - To fix an unbalanced BST, you can use **self-balancing trees** like Red-Black Trees or AVL Trees, which perform rotations and ensure that the tree remains balanced, maintaining O(log n) time complexity for operations.

---

### 12. **What is a Bloom Filter? Explain how it can be used to reduce memory usage and false positives in set membership checks.**

**Answer:**
- A **Bloom Filter** is a probabilistic data structure used to test whether an element is a member of a set. It may yield false positives (indicating an element is in the set when it is not) but never false negatives.

- **Structure**:
  - A Bloom Filter uses a **bit array** and multiple independent hash functions.
  - When inserting an element, the element is hashed using several hash functions, and the bits at the resulting indices in the array are set to 1.
  - To check if an element is in the set, hash it using the same hash functions and check if all the bits at the resulting indices are set to 1.

- **Advantages**:
  - **Memory Efficient**: The bit array uses less memory than storing the actual elements.
  - **Fast Membership Check**: O(k) time complexity, where k is the number of hash functions used.

- **False Positives**: It can falsely indicate that an element is present in the set, but it guarantees no false negatives (if the filter says an element is not present, it is indeed not present).

---

### 13. **Explain the advantages of using a `std::priority_queue` in C++. How does it differ from a regular `std::queue`, and what are its use cases?**

**Answer:**
- **std::priority_queue**:
  - A **priority queue** is a container that provides constant-time access to the largest (or smallest, depending on comparison criteria) element.
  - Elements are **ordered by priority**, meaning that the highest-priority element is always at the front and can be accessed in O(1).
  - **Insertion and Deletion** take O(log n) time since it is typically implemented using a heap.

- **Difference from `std::queue`**:
  - In a regular **queue**, elements are processed in **FIFO** order (first-in-first-out).
  - In a **priority queue**, elements are processed based on their priority (largest/smallest value) instead of the order they were inserted.

- **Use Cases**:
  - **Task Scheduling**: Where tasks with higher priority need to be executed first.
  - **Graph Algorithms**: Like Dijkstra’s algorithm, which needs access to the smallest element (minimum cost node) frequently.

---

### 14. **What are `std::shared_ptr` and `std::unique_ptr` in C++? When would you choose one over the other, and what problems do they solve?**

**Answer:**
- **`std::unique_ptr`**:
  - A **smart pointer** that ensures **exclusive ownership** of a dynamically allocated object. No two `unique_ptr` can point to the same object. It automatically deletes the object when the `unique_ptr` goes out of scope.
  - **Use Case**: Use `unique_ptr` when you need **exclusive ownership** of an object, meaning that no other part of the code should share ownership of the resource.

- **`std::shared_ptr`**:
  - A **smart pointer** that allows **shared ownership** of a dynamically allocated object. Multiple `shared_ptr` instances can point to the same object, and the object is only deleted when the last `shared_ptr` goes out of scope.
  - **Use Case**: Use `shared_ptr` when you need **shared ownership**. For example, if you need to pass the same object across different parts of the program, and you want to ensure the object’s lifetime is managed automatically.

- **Problem Solved**:
  - Both `shared_ptr` and `unique_ptr` solve the problem of **manual memory management**, helping avoid memory leaks and dangling pointers by automatically managing the lifetime of dynamically allocated objects.
 
Here are the answers for questions 15 to 20 related to data structures for a senior C++ developer:

### 15. **What is a Red-Black Tree, and how does it ensure balanced tree properties? Compare it with AVL Trees in terms of balancing and performance.**

**Answer:**
- **Red-Black Tree**: 
  - A **Red-Black Tree** is a self-balancing binary search tree where each node contains an extra bit to store the color (either red or black). 
  - The tree maintains a balance by enforcing these properties:
    1. Every node is either red or black.
    2. The root is always black.
    3. Red nodes cannot have red children (no two consecutive red nodes).
    4. Every path from a node to its descendants must have the same number of black nodes.
  - This ensures that the longest path in the tree is no more than twice the shortest path, resulting in a balanced tree.

- **AVL Tree**: 
  - An **AVL Tree** is also a self-balancing binary search tree where the difference in heights between the left and right subtrees of any node is at most 1.
  
- **Comparison**:
  - **Balancing**: AVL Trees are more strictly balanced than Red-Black Trees, which results in faster lookups (O(log n)) because of the tighter height constraint.
  - **Performance**: Red-Black Trees allow more relaxed balancing, so they perform faster insertions and deletions (O(log n)) than AVL Trees due to fewer rotations needed to maintain balance.

- **Use Case**: Red-Black Trees are preferred in systems (like the Linux kernel) where insertions and deletions are frequent, while AVL Trees are used in scenarios requiring frequent lookups with minimal insertion/deletion.

---

### 16. **What are Skip Lists, and how do they improve search operations compared to a standard linked list? How do they compare to balanced binary trees like AVL or Red-Black Trees?**

**Answer:**
- **Skip List**:
  - A **Skip List** is a data structure that extends a standard linked list with multiple levels of linked lists, where each level skips over several elements. The lowest level contains all the elements, while higher levels contain fewer elements, effectively creating a hierarchy of lists.
  - **Search**: O(log n) on average because elements can be skipped at each level, improving search time from O(n) in a regular linked list.
  
- **Comparison with Balanced Trees**:
  - **Search Efficiency**: Both Skip Lists and balanced trees (like AVL, Red-Black) offer O(log n) search time. Skip Lists achieve this probabilistically, while balanced trees guarantee it through strict balancing rules.
  - **Simplicity**: Skip Lists are easier to implement than balanced trees because they do not require complex rotations or rebalancing.
  - **Memory**: Skip Lists require extra pointers (logarithmic space overhead) but do not require rebalancing, making them simpler for concurrent data structures.
  
- **Use Case**: Skip Lists are commonly used in applications like in-memory databases or distributed systems due to their simplicity in concurrent environments.

---

### 17. **Explain what a Splay Tree is and how it differs from other self-balancing trees. What are the advantages and disadvantages of using a Splay Tree?**

**Answer:**
- **Splay Tree**:
  - A **Splay Tree** is a type of self-adjusting binary search tree where recently accessed elements are moved to the root using a process called **splaying**.
  - This tree does not maintain strict balancing like AVL or Red-Black Trees but instead moves frequently accessed nodes closer to the root to optimize future accesses.

- **Operations**:
  - **Search, Insert, and Delete**: O(log n) amortized time. However, in the worst case, these operations could take O(n) if the tree becomes highly unbalanced, but splaying helps maintain amortized performance.

- **Advantages**:
  - **Locality of Reference**: Frequently accessed elements are moved to the root, making them faster to access in future operations (good for caching).
  - **No Strict Balancing Overhead**: No rotations are needed, as in AVL or Red-Black Trees.
  
- **Disadvantages**:
  - **Worst-Case Performance**: Splay trees can become unbalanced after certain sequences of operations, leading to O(n) worst-case time complexity.
  
- **Use Case**: Splay Trees are useful in scenarios where certain elements are accessed repeatedly (e.g., caching, data compression algorithms).

---

### 18. **What is a Segment Tree, and how does it solve range queries efficiently? Explain how updates are handled in a Segment Tree.**

**Answer:**
- **Segment Tree**:
  - A **Segment Tree** is a binary tree used to efficiently solve range queries (like sum, minimum, maximum) and range updates on an array. It divides the array into segments and stores aggregate information for each segment in the tree nodes.
  - **Construction**: Takes O(n) time, where each node stores the result (like sum or min) for a range of the array.
  
- **Range Queries**:
  - Range queries (like the sum of elements from index `l` to `r`) can be answered in O(log n) time by combining results from relevant segments of the tree.
  
- **Updates**:
  - **Point Updates** (updating a specific element in the array): O(log n), because we only update the segments affected by the change.
  - **Range Updates** (updating all elements in a range): For this, we use a **lazy propagation** technique to delay updates and make sure that the range updates are also handled in O(log n).

- **Use Case**: Segment Trees are useful in competitive programming and applications that require frequent updates and queries on array segments, such as range sum queries or range minimum queries.

---

### 19. **Explain the difference between a Deque (Double-ended queue) and a Circular Queue. How is a Deque implemented in C++ using `std::deque`?**

**Answer:**
- **Deque (Double-ended queue)**:
  - A **Deque** is a generalization of a queue that allows insertion and deletion of elements from both the front and the rear.
  - **Operations**: O(1) time complexity for insertion and deletion at both ends.
  - In C++, the **`std::deque`** is implemented as a dynamic array of blocks, allowing efficient insertion and deletion at both ends without the overhead of reallocating memory like in `std::vector`.

- **Circular Queue**:
  - A **Circular Queue** is a fixed-size data structure where the last position is connected back to the first position, forming a circle.
  - It allows efficient use of space by reusing the empty positions when elements are dequeued.
  - It has fixed capacity, and both insertions and deletions are allowed only at the rear and front, respectively.
  
- **Use Case**:
  - **Deque**: Used when operations are required at both ends (e.g., sliding window problems, task scheduling).
  - **Circular Queue**: Used in scenarios with fixed capacity (e.g., buffers, scheduling algorithms).

---

### 20. **What is a Treap, and how does it combine properties of both binary search trees and heaps? Explain its use case in randomized algorithms.**

**Answer:**
- **Treap**:
  - A **Treap** is a combination of a **binary search tree (BST)** and a **heap**. It maintains the BST property based on the node keys and the heap property based on priority values assigned randomly.
  - **BST Property**: The left child of any node has a smaller key, and the right child has a larger key.
  - **Heap Property**: Each node has a random priority, and a node's priority is greater than that of its children.

- **Randomized Algorithm**:
  - The random priority ensures that the tree remains balanced with high probability, leading to O(log n) time complexity for insertions, deletions, and search.
  
- **Use Case**: Treaps are used in randomized algorithms where probabilistic balancing (rather than strict balancing) is sufficient, such as randomized search algorithms or dynamic sets in competitive programming.

