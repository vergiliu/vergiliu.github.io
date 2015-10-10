---
layout: post
title: "Generic"
date: 2015-12-31 22:22:22
categories: [books, reviews]
tags: [books, reviews, 2015]
---
Algorithms (Part I) - Princeton

Week1 Union Find

Week2 Stacks, Queues, Bags
- loitering - references to objects that are no longer used
- stack implmented as array
    - resizing array at 1/4 and 1/2 full
    - constant amortized time overall
- queues - linked list and array implementation
- generics - used for linked list - Stack<Integer> s = new Stack<Integer>();
- make stack implementation Iterable
- sorting
    - Java using a callback by implementing a Comparable interface
    - selection sort
        - find smallest, swap w/ I, increment I => N*N/2 compares
    - insertion sort
        - compare 2 elements
        - everything to the left of the current element is swapped until smaller than left element => N*N/4 compares
        - linear time for partially sorted arrays
    - shell sort
        - h-interleaved sorting, good approach for 3x + 1 increments => N ^ 3/2
        - last sort is basically 1-interleaved which is in fact insertion sort
    - shuffle swap
        - knuth shuffle swap I and random number < I
    - convex hull = smallest area that encloses the points
        - Graham scan

Week 3 Sorting methods
- Merge sort
    - copy initial array (can be optimizeds), then each compare each half array and put smallest element in initial array, by incrementing either i (index in 1st half) or j (index in the 2nd half)
    - N log_2 N
    - uses 2 x N memory
    - too complicated for small arrays
    - Bottom up merge: 2 for loops which merge subarrays from 2 elements to N/2
    - is stable
    - computational complexity
        - for mergesort, upper bound == lower bound => optimal regarding complexity, not optimal regarding space
        - Comparators -> implements Comparable; Comparator -> alternate sorting
- Quick sort
    - in place sorting
    - is not stable
    - 1.39 N log N (avg); N * N /2 worst case
    - insertion sort still preferred for small arrays
- shuffle: swap i and r, where r is a random between 0 and length(array)
- stable sort: insertion sort and merge sort
- not-stable sort: quicksort, selection sort and shell sort - long distance exchanges of elements (keys) with equal values
- mergesort
    - makes N/2 * lg N to N * lg N compares
- quicksort
    - average case is 1.39 * N * lg N => 39% more comparisons than mergesort
    - does less data movements, thus is faster than mergesort
    - in -place, no extra space as compared to mergesort
    - not stable
    - extra: 3way partitioning, all items equal in a partition
- quickselect: linear time
- java Arrays.sort() implemented using quicksort for primitive types or mergesort for objects
- 3way quicksort + randomized => linear sorting for a broad class of apps (from linearithmic)
    - not quadratic when more equals keys
- heapsort: N * lg N, in-place in worst case, not stable, poor use of cache memory (worst case 2N log N)
- priority queues
    - complete binary tree: tree balanced except for lowest level
    - binary heap, can be implemented in an array, root is @ A[1], A[0] empty
        - implicit implementation as a tree
- sorting used in variety of situations

Week 4 - Priority Queues
- priority queues used in A* search, data compression huffman, bayesian spam filter
- min or max PQ
- binary heap
    - using complete binary tree (all levels but last are full) height is log N
        - largest key is root of tree (for max heap variant)
    - heap sort
        - N log N worst case sorting time, in place sorting
        - bad for caching, not stable
- symbol tables
    - in simple implementation (ibnary search, array) linear operations for insert
    - we can create an ordered Symbol Table by adding Comparable (Key Comparable<Key>)

Week 5
    - Binary Search Tree  (BST) - explicit implementation as tree: key, value, reference to left / right sub-tree - smaller/greater
        - search and insert are 1.39 lg N for BST in average case, and N in worst case
        - deletion takes sqrt(N)
    - Balanced Search Tree
        - (2-3 tree): tree can have either 2 or 3 leaves and node can have 1 or 2 keys, with middle leaf key value being between the the 2 keys
            - all operations are c * lg N, constant time - c, depends on implementation
    - LLRB - Left Leaning Red-Black BST
        - search works the same as in the BST
        - rotate left operations to move a right-leaning RED link and make it lean left
        - rotate right and flip colors (when 2 RED links are coming out of the same node)
        - search/insert/delete 2* lg N, max height 2 * lg N
    - B-tree
        - generalized version of 2-3 tree
        - Java: TreeMap and TreeSet are implemented using RedBlack-tree
- Hash Tables
    - each type of data depends on type of data
    - hashCode returns 32bit int, equal() and hashCode() should be equal
        - for String - each char * 31 (or a small prime number) + prev_value
        - if array, use for each element to get the hashCode
        - make it positive ( & 0xffff...) and do % M (where M is the size of the array)
