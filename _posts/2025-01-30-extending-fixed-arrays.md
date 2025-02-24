---
date: 2025-01-30
title: "Extending Fixed Arrays: Exploring Dynamic Memory, Vectors, and Ownership in C++"
layout: post
excerpt: >-
  Explores dynamic memory management, emphasizing the use of <code>new</code>
  and <code>delete</code>, and STL <code>vector</code>. It covers 2D
  containers, fixed vs. dynamic arrays, memory security, recursion algorithms,
  and best practices for efficient and secure C++ programming.
---

Alright, so we left off talking about 2D containers. We previously discussed
a 2D container that was done at compile time, where we used C-style arrays with
square brackets and constants for size. This approach doesn't allow runtime
control, but now we're looking at a version that does, which means we manage
the memory ourselves using `new` and `delete`.

When using `new` with square brackets, we allocate an array dynamically. For
example, in line four of our code, we declare `a` as an array of `int*`. This
means `a` is an array of pointers to integers. More specifically, it's a
pointer to a pointer to an integer (`int** a`). When we initialize `a`, each
pointer in the array can point to a separate block of integers.

To properly allocate memory, we first allocate space for the array of pointers.
Then, in a loop, we allocate space for each row individually. Every `new`
operation must be paired with a `delete`. Since we use `new[]` to allocate an
array, we must use `delete[]` to free it. The order of deletion is important:
we delete the inner arrays first and then delete the outer array.

This is essentially what the STL `vector` does for us. When we use
`vector::push_back`, the vector handles memory allocation, resizing, and
cleanup. It ensures proper memory management without requiring us to manually
manage `new` and `delete`. When `delete[]` is used, it calls destructors on
each element before freeing memory. For built-in types like `int`, there is no
destructor, but for objects with destructors, they will be properly called.

## Nested Vectors and Memory Considerations

When discussing nested vectors, it's important to clarify that there is no such
thing as a "2D vector." Instead, we use a vector of vectors. Each vector is
one-dimensional, but we can store other vectors inside, effectively creating a
multi-dimensional structure. When resizing the outer vector, it creates empty
inner vectors that must be populated separately. Some constructor syntaxes
allow initialization in one step, while others require iterating through and
initializing each inner vector individually.

## Pros and Cons of Fixed and Dynamic Memory Allocation

Fixed arrays have several advantages:

- They are automatically deallocated when they go out of scope.
- They provide fast memory access because the compiler knows row sizes in
  advance, enabling efficient indexing.

However, fixed arrays also come with drawbacks:

- They don't work well for large data sizes because they reside on the stack.
- Their sizes are determined at compile time, which limits flexibility.
- Passing them as function arguments requires explicitly specifying dimensions.

Dynamic memory allocation addresses these limitations:

- It allows variable-sized data structures.
- It enables jagged arrays (where rows have different sizes).
- It makes swapping and copying rows more efficient.

However, dynamic allocation has its own set of issues:

- Memory management must be handled manually.
- Improper use can lead to memory leaks or segmentation faults.
- Accessing elements requires two memory operations instead of one, making it
  slightly slower.

For these reasons, the STL `vector` provides a more convenient and reliable
alternative for dynamic memory management.

## Avoiding Fixed-Length Buffers

A historical practice in C programming was to allocate a fixed number of bytes
for data structures like names (e.g., assuming a name would fit within 30
bytes). While functional, this approach introduces security risks because
uninitialized memory retains old values. If a program requests memory and
inadvertently reads past valid data, it may expose sensitive information from
previously running processes.

Instead of using fixed-length buffers, modern best practices involve
dynamically allocating only the required memory, using `std::string` or
dynamically sized containers.

## Addressing Security Issues in Memory Allocation

Memory allocation errors can create security vulnerabilities. For example:

- If a buffer is allocated with excess space and later returned to the user, it
  might expose residual data.
- `strcpy()` copies strings without bounds checking, leading to buffer
  overflows. Using `std::string` mitigates this issue by dynamically managing
  memory and ensuring safe copying.

## Moore's Algorithm and the Misra-Gries Algorithm

A fundamental problem in computing is determining the majority element in an
array-an element appearing more than 50% of the time. This can be solved in
linear time using **Moore's algorithm**. Similarly, the **Misra-Gries
algorithm** extends this idea to finding elements that appear more than `n/3`
times. These algorithms optimize space and runtime complexity, making them
useful for large datasets.

## Container Management and Memory Ownership

In C++, objects stored in containers can be managed in three ways:

1. **By value** - The container owns the object, making it responsible for its
   lifecycle.
2. **By pointer** - The container stores pointers, but ownership is ambiguous.
3. **By reference** - This avoids copying but is generally less flexible than
   pointer-based management.

A common issue arises when a container stores pointers to dynamically allocated
objects. If the container is destroyed but the objects remain allocated, memory
leaks occur. Conversely, if an object is deleted while still referenced by a
container, accessing it results in undefined behavior.

To prevent such issues, **smart pointers** (e.g., `std::shared_ptr` and
`std::unique_ptr`) manage ownership explicitly. Reference counting (used in
Java and C++ smart pointers) helps track the number of references to an object
and automatically deallocates memory when the reference count reaches zero.

## Copy Constructors and Assignment Operators

When dealing with dynamic memory, a **copy constructor** is necessary to ensure
deep copies. Without it, copying an object results in two instances pointing to
the same memory, leading to double deletes. The copy constructor should:

- Allocate new memory.
- Copy elements from the source object.
- Ensure each instance has its own copy of the data.

The **copy-swap idiom** is a preferred method for implementing assignment
operators. It involves:

1. Creating a temporary copy of the right-hand object.
2. Swapping the contents with the current object.
3. Allowing the temporary copy to go out of scope, triggering its destructor
   and freeing the old memory.

This approach eliminates the need for self-assignment checks and guarantees
exception safety.

## Final Thoughts

Understanding memory management is crucial in C++. Proper use of `new` and
`delete`, choosing appropriate containers, and writing correct copy
constructors and assignment operators all contribute to robust and efficient
programs. The techniques discussed today-from managing 2D containers to
preventing memory leaks-are fundamental skills that will be essential in your
projects and future software development.

That wraps up today's lecture. Be sure to review Moore's and Misra-Gries
algorithms, and practice implementing copy constructors, destructors, and
assignment operators.

---

## Study Questions

### Short Answer Questions

1. What are the primary advantages of using `std::vector` over raw arrays?
2. Explain how `delete[]` works and why it must be paired with `new[]`.
3. How does Moore's algorithm help find the majority element in an array?
4. What role does reference counting play in memory management?
5. What is the difference between deep copying and shallow copying in C++?
6. What are the key differences between a fixed-length array and a dynamically
   allocated array in C++?
7. How does the STL `vector` simplify dynamic memory management compared to
   manual `new` and `delete` operations?
8. What is the significance of the copy-swap idiom in implementing assignment
   operators?
9. Why is it important to avoid using fixed-length buffers for variable-length
   data?
10. How do smart pointers like `std::unique_ptr` and `std::shared_ptr` help
    manage memory ownership in C++?

### Multiple-Choice Questions

1. What is a key difference between a `std::vector` and a C-style array?\
   A) `std::vector` requires explicit memory allocation\
   B) `std::vector` can dynamically resize itself\
   C) C-style arrays are more memory efficient than `std::vector`\
   D) `std::vector` cannot store objects

2. When using `new[]` to allocate memory, which function must be used to
   properly deallocate it?\
   A) `free()`\
   B) `delete`\
   C) `delete[]`\
   D) `dealloc()`

3. What is the main advantage of using smart pointers like `std::shared_ptr`?\
   A) They prevent segmentation faults\
   B) They automatically manage memory and prevent leaks\
   C) They replace all traditional pointers in C++\
   D) They use more memory than raw pointers

4. What does the copy-swap idiom help prevent?\
   A) Memory leaks\
   B) Dangling pointers\
   C) Self-assignment issues\
   D) Syntax errors

5. What happens if you attempt to delete an already deleted pointer?\
   A) The program continues without issue\
   B) It results in undefined behavior and possible crashes\
   C) The pointer is automatically reset to `nullptr`\
   D) The memory is freed twice without issue

6. Which of the following is true about `std::vector`?\
   A) It always allocates memory on the stack\
   B) It provides bounds checking with `operator[]`\
   C) It always maintains a fixed size\
   D) It automatically manages dynamic memory

7. What is one risk of using raw pointers in containers?\
   A) Containers automatically delete raw pointers\
   B) Memory leaks can occur if not handled properly\
   C) Raw pointers improve performance significantly\
   D) Raw pointers cannot be stored in containers

8. Why should `std::string` be preferred over `char[]` for string storage?\
   A) `std::string` dynamically resizes and prevents buffer overflow\
   B) `char[]` is faster for string operations\
   C) `std::string` cannot be modified after creation\
   D) `std::string` does not allocate memory dynamically

9. What does the `std::move` function do in C++?\
   A) Moves an object to another location in memory\
   B) Transfers ownership of resources without copying\
   C) Deletes an object and creates a new instance\
   D) Prevents modifications to an object

10. In C++, what is the purpose of `std::unique_ptr`?\
    A) It allows multiple pointers to share ownership of an object\
    B) It ensures that a pointer is deleted when it goes out of scope\
    C) It prevents any object from being deleted\
    D) It automatically copies objects when needed

### Answer Key

1. B
2. C
3. B
4. C
5. B
6. D
7. B
8. A
9. B
10. B
