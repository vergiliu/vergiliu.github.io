---
layout: post
title: "Generic"
date: 2015-12-31 22:22:22
categories: [books, reviews]
tags: [books, reviews, 2015]
---
Algorithms (Part I) - Princeton

CH1 Union Find

CH2 Stacks, Queues, Bags
- loitering - references to objects that are no longer used
- stack implmented as array
    - resizing array at 1/4 and 1/2 full
    - constant amortized time overall
- queues - linked list and array implementation
- generics

CH N
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
- heapsort: N * lg N, in-place in worst case, not stable, poor use of cache memory (worst case 2N log N)
- priority queues
    - complete binary tree: tree balanced except for lowest level
    - binary heap, can be implemented in an array, root is @ A[1], A[0] empty
        - implicit implementation as a tree
- symbol tables
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
