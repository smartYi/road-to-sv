# 算法题解答技巧

+ 掌握基本的数据结构，算法及概念
+ Most interviewers won't ask about specific algorithms for binary tree balancing or other complex algorithms(他们可能自己都不太记得了，因为不常用)

## Core Data Sturctures, Algorithms, and Concepts

+ Data Structures
	+ Linked Lists
	+ Trees, Tries, & Graphs
	+ Stacks & Queues
	+ Heaps
	+ Vectors / ArrayLists
	+ **(!)Hash Tables**
+ Algorithms
	+ Breadth-First Search
	+ Depth-First Search
	+ Binary Search
	+ Merge Sort
	+ Quick Sort
+ Concepts
	+ Bit Manipulation
	+ Memory (Stack vs. Heap)
	+ Recursion
	+ Dynamic Programming
	+ Big O Time & Space

# A Problem-Solving Flow

**BUD Optimization**: **B**ottlenecks, **U**nnecessary Work, **D**uplicated Work

A bottleneck is a part of your algorithm that slows down the overall runtime. There are two common ways this occurs:

+ You have one-time work that slows down your algorithm.
+ You have a chunk of work that's done repeatedly, like searching.

## 1.Listen

**Pay very close attention** to any information in the problem description. You probably need it all for an optimal algorithm.

+ record **unique** information
+ use all the information in the problem
+ write the pertinent information on the whiteboard

## 2.Example

Most examples are too small or are special cases. **Debug your example**. Is there any way it's a special case? Is it big enough?

Go to the whiteboard and draw an example:

+ Specific: use real numbers or strings
+ Sufficiently large
+ Not a special case

Try to make the best example you can.

## 3.Brute Force

Get a brute-force solution as soon as possible. Don't worry about developing an efficient algorithm yet. State a naive algorithm and its runtime, then optimize from there. Don't code yet though!

## 4.Optimize

Walk through your brute force with **BUD optimization** and try some of these ideas:

+ Look for any unused info. You usually need all the information in a problem.
+ Solve it manually on an example, then reverse engineer your thought process. How did you solve it?
+ Solve it "incorrectly" and then think about why the algorithm fails. Can you fix those issues?
+ Make a time vs. space tradeoff. Hash tables are especially useful!
+ Precompute information. Is there a way that you can reorganize the data(sorting, etc.) or compute some values upfront that will help save time in the long run?
+ Use a hash table. Hash tables are widely used in interview questions and should be at the top of your mind.
+ Think about the best conceivable runtime.

## 5.Walk Through

Now that you have an optimal solution, **walk through your approach in detail**. Make sure you understand each detail before you start coding.

Whiteboard coding is slow, very slow. You need to make sure that you get it as close to "perfect" int he beginning as possible.

## 6.Implement

Your goal is to **write beautiful code**. Modularize your code from the beginning and refactor to clean up anything that isn't beautiful.

**Keep talking!** Your interviewer wants to hear how you approach the problem.

Beautiful code means:

+ **Modularized code**: good coding style & make things easier for you.
+ **Error checks**: add a `todo` and then just explain out loud what you'd like to test.
+ **Use other classes/structs where appropriate**: e.g. points in 2 or 3 dimension
+ **Good variable names**

## 7.Test

Test in this order:

1. Conceptual test. Walk through your code like you would for a detailed code review.
2. Unusual or non-standard code.
3. Hot spots, like arithmetic and null nodes.
4. Small test cases. It's much faster than a big test case and just as effective.
5. Special cases and edge cases.

And when you find bugs, **fix them carefully!**