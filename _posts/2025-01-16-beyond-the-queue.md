---
date: 2025-01-16
title: "Beyond the Queue: Priority Structures, Custom Comparators, and Related Complexities"
layout: post
excerpt: >-
  Covers the fundamentals of priority queues, including their implementation,
  customization using comparators, and performance analysis through Big-O
  notation. Investigates step counting, complexity classes, and practical
  applications for efficient algorithm design in data structures.
---

Today, let's start with a simple exercise-smiling. I walked here from Beyster,
smiling the whole way, and I think there's something to that. Maybe there's a
connection between body motion and mood. Try it-smile right now. See? Easy.
Now, the next thing you need to say is, "I am smart enough to do well in this
course." Say it, and tell someone else the same thing. Confidence is key.

## Exam Expectations and Preparation

Now, let's talk about the exam. It's going to be tough. Not impossible, but
definitely challenging. When we write the exams, we start with difficult
questions, then adjust them to be more reasonable. Eventually, we reach a point
where making them any easier would be like giving away points. Despite these
adjustments, we still end up with a median around 60. This course is difficult
by nature.

After every midterm, students come to me, saying, "I was here, I paid
attention, I studied hard, but I still didn't do well. How can I study better?"
The best way to study isn't cramming at the end-it's working consistently
throughout the course. The multiple-choice section consists of 24 questions,
written by IAs and GSIs-not faculty. They review, refine, and test them to
ensure quality, but the key point is that these questions are written by
students like you. That means if you engage with the material the same way they
do, you can prepare effectively.

## Writing Your Own Test Questions

One of the best ways to study is to write your own test questions. If you and a
study partner create and exchange questions, you're actively thinking about the
material. Even writing a bad question is useful because analyzing why it's bad
improves your understanding. That's why I've set up a Google Form for
submitting test questions. If half of you write one, we'll have 40 test
questions from this lecture alone. I'll compile them into a practice exam and
post it on Ed.

## Priority Queues: A Customizable Container

We've covered basic containers, but today, let's discuss a more advanced
one-the priority queue. Unlike standard queues, which always remove elements in
the same order they were added, priority queues allow you to customize the
order of removal based on priority.

A priority queue consists of data where each item has an associated priority,
usually numerical. Some priorities are explicit (e.g., an emergency call with
priority levels 0, 1, and 2). Others are implicit (e.g., stock prices, where
lower prices might be prioritized over higher ones).

Priority queues operate independently of time. Unlike a standard queue where
arrival order determines processing order, priority queues always remove the
most important item first. If multiple items have the same priority, additional
rules (such as FIFO or tie-breakers) can be applied.

## Implementing and Customizing Priority Queues

A standard priority queue doesn't support modifying an element's priority once
it's inserted. You can have explicit tie-breakers to break priority ties, but a
basic priority queue only allows a single priority value per item.

Implementation-wise, a priority queue can be structured using different
underlying data structures:

- **Unordered sequence**: Fast insertion (O(1)), but slow retrieval (O(n)).
- **Sorted sequence**: Fast retrieval (O(1)), but slow insertion (O(n)).
- **Heap**: Balanced approach with O(log n) for both insertion and retrieval.
- **Array of linked lists**: If the number of priority levels is small and
  fixed, insertion and retrieval can both be O(1).

We'll implement priority queues in Project 2B using multiple approaches. The
standard STL priority queue uses a binary heap. You'll also implement
variations with a sorted sequence, an unordered sequence, and a pairing heap.

## Understanding Comparators

By default, the STL priority queue uses `std::less`, which makes the
highest-priority item the largest value. If you replace `std::less` with
`std::greater`, the smallest item will have the highest priority. Custom
comparators allow finer control, enabling tie-breaking based on multiple
criteria. For instance, in stock trading, when two prices match, the earlier
order takes precedence.

## Complexity Analysis: Measuring Algorithm Efficiency

Complexity analysis is crucial for understanding how algorithms scale. We
express this in Big-O notation:

- **O(1)** - Constant time.
- **O(\log{n})** - Logarithmic time (e.g., binary search).
- **O(n)** - Linear time.
- **O(n \log{n})** - Log-linear time (e.g., sorting algorithms).
- **O(n^2)** - Quadratic time.
- **O(2^n)** - Exponential time.

### Step Counting and Recognizing Patterns

Step counting helps determine complexity. For example, a simple `for` loop with
`n` iterations results in $$O(n)$$. Nested loops indicate $$O(n^2)$$. If an
operation repeatedly halves its input, it's $$O(\log{n}), like binary search.

Consider these two functions:

```cpp
void func1(int n) {
    int sum = 0;
    for (int i = 0; i < n; ++i) {
        sum += i;
    }  // for ..i
    return sum;
}  // func1()
```

This runs in $$O(n)$$ because it has a single loop executing n times.

```cpp
void func2(int n) {
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            sum += i * j;
        }  // for ..j
    }  // for ..i
}  // func2()
```

This runs in $$O(n^2)$$ because of nested loops.

A binary search function, which halves the search space each iteration, runs in
$$O(\log{n})$$.

## Conclusion and Next Steps

We covered priority queues, custom comparators, and complexity analysis. Your
next steps:

- Practice writing test questions.
- Work through complexity problems.
- Prepare for implementing a priority queue in Project 2B.

Next time, we'll discuss bugs in an alternative multiplication algorithm. Walk
out of here smiling!

## Final Thoughts

Priority queues and complexity analysis are crucial topics in algorithm design,
impacting real-world applications from scheduling systems to data processing.
Understanding how to structure data efficiently and analyze algorithmic
performance allows for better decision-making when implementing solutions. As
you continue your studies, keep these concepts in mind and practice recognizing
patterns in complexity analysis. Developing a solid intuition for algorithm
behavior will help you approach programming problems more effectively. Keep
challenging yourself, and don't hesitate to engage in collaborative learning to
reinforce your understanding.

---

## Study Questions

### Short-Answer Questions

1. What distinguishes a priority queue from a standard queue?
2. How does a binary heap improve the efficiency of a priority queue?
3. Why do priority queues use comparators?
4. What is the time complexity of inserting into an unordered sequence priority
   queue?
5. How does a standard priority queue handle duplicate priority values?
6. What is the primary advantage of a sorted sequence priority queue?
7. How can priority queues be optimized when the number of priority levels is
   fixed?
8. What is the purpose of Big-O notation in complexity analysis?
9. What complexity class does binary search belong to, and why?
10. Why is step counting useful for determining algorithm efficiency?

### Multiple-Choice Questions

1. What data structure does the STL priority queue use?\
   A) Unordered sequence\
   B) Sorted sequence\
   C) Heap\
   D) Linked list

2. What is the worst-case time complexity for inserting an element in a sorted
   sequence priority queue?\
   A) $$O(1)$$\
   B) $$O(n)$$\
   C) $$O(\log{n})$$\
   D) $$O(n \log{n})$$

3. What default comparison function does the STL priority queue use?\
   A) `std::greater`\
   B) `std::less`\
   C) `std::equal_to`\
   D) `std::not_equal_to`

4. Which of the following is true about a binary heap?\
   A) It is always a sorted structure.\
   B) It supports $$O(1)$$ insertions.\
   C) It allows $$O(\log{n})$$ insertions and deletions.\
   D) It requires a linked list implementation.

5. What is the time complexity of accessing the top element of a heap-based priority queue?\
   A) $$O(1)$$\
   B) $$O(n)$$\
   C) $$O(\log{n})$$\
   D) $$O(n \log{n})$$

6. How can a priority queue be implemented to achieve O(1) insertion and retrieval?\
   A) Using a heap\
   B) Using a sorted array\
   C) Using an unordered sequence\
   D) Using an array of linked lists with fixed priority levels

7. What is the main drawback of using an unordered sequence for a priority queue?\
   A) Slow insertion\
   B) Slow deletion\
   C) Increased memory usage\
   D) Complexity of implementation

8. Which of the following operations is fastest in a sorted sequence priority queue?\
   A) Insert\
   B) Remove\
   C) Peek\
   D) Search

9. In Big-O notation, what complexity class does binary search belong to?\
   A) $$O(1)$$\
   B) $$O(\log{n})$$\
   C) $$O(n)$$\
   D) $$O(n \log{n})$$

10. What property of a priority queue allows for custom sorting orders?\
   A) The use of linked lists\
   B) The use of comparators\
   C) The use of hashing\
   D) The use of predefined priority levels

### Answer Key

1. C
2. B
3. B
4. C
5. A
6. D
7. B
8. C
9. B
10. B
