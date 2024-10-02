A pq pops the element of the queue with the highest priority according to some ordering function.
Main Operations: Insert (key, data), DeleteMin/ DeleteMax, GetMinimum/ GetMaximum
Auxiliary PQ Operations: Kth smallest/ Kth largest, Size, HeapSort (Sort pq based on priority (key)).

# PQ Applications:
1. Data Compression: Huffman coding
2. Shortest Path Algorithm: Djikstra's algorithm
3. Minimum Spanning Tree: Prim's Algorithm
4. Job scheduling
5. Event driven simulation: customers in a line
6. Selection problem: finding  Kth smallest/ Kth largest

# Comparing Implementations
| Implementation            | Insertion           | Deletion (DeleteMax)  | Find Min      |
| --------------------------|:-------------------:| -------------------:  | ------------: |
|Unordered Array            |1 (insert at one end)|     n                 |    n          |
|Unordered List             |1 (insert at one end)|     n                 |    n          |
|Ordered array              |    n                |1 (delete from one end)|    1          |
|Ordered Lst                |    n                |1 (delete from one end)|  1            |
|Binary Search Tree         |log n(average)       |     log n(average)    |log n(average) |
|Balanced Binary Search Tree|    log n            |     log n             |  log n        |
|Binary Heap                |    log n            |     log n             |    1          |

Heaps are complete binary trees i.e. all leaf nodes are present at levels h and h - 1.

**Property of a heap**: Value of a node must be less than or equal (<=) in case of Min Heap or greater than or equal (>=) to its child nodes value in case of Max Heap.

**Types of Heap**: Min Heap, Max Heap

**Representation of a Heap**: Heap is represented using an array since a heap forms a complete binary tree so there will be no wastage of memory.

**Difference between a binary heap and a BST**: 
1. A binary heap has better locality of reference than a binary search tree (BST) because it's implemented using arrays. Locality of reference (a.k.a principle of locality) refers to a program's tendency to access the same memory locations over a short period of time (Read more here: https://www.geeksforgeeks.org/locality-of-reference-and-cache-operation-in-cache-memory/). Programs that exhibit strong locality of reference can benefit from caching, prefetching, and advanced branch predictors.
2. Building a binary heap takes O(N) time, which is faster than the O(N log N) time for a BST.
3. Binary heaps use less memory than BSTs because they don't require extra space for pointers.
4. BST is an ordered data structure that does not allow duplicates. Duplication in binary heaps is allowed because the nodes have values lesser than or equal to the root.

**Heapifying an element**: Process of adjusting the locations of a heap to make it a heap again once a new element inserted into the heap does satisfy the heap property.

```
#include <iostream>

// Declaring a heap
struct Heap {
    int *array; 
    int count;    // Number of elements in the heap
    int capacity; // Size of the heap
    int heap_type;// Min Heap or Max Heap
}

// Creating a heap: TC -> O(1)
struct Heap* CreateHeap(int capacity, int heap_type) {
    struct Heap* h = (struct Heap*) malloc(sizeof(struct Heap));
    if (h == NULL) {
        print("Memory Error");
        return;
    }
    h->heap_type = heap_type;
    h->count = 0;
    h->capacity = capacity;
    h->array = (int*) malloc(sizeof(int)* h->capacity);
    if(h->array == NULL) {
        printf("Memory Error");
        return;
    }
    return h;
}
// Parent of a Node: TC -> O(1)
int Parent(struct Heap* h, int i) {
    if(i <= 0 || i >= h->count) {
        // return -1 on array out of bound exception scenario.
        return -1;
    }
    return (i-1)/2;
}
// Left Child of a Node: TC -> O(1)
int LeftChild(struct Heap* h, int i) {
    int left = (2 * i) + 1;
    if (left >= h->count) {
        return -1;
    }
    return left;
}
// Right Child of a Node: TC -> O(1)
int RightChild(struct Heap* h, int i) {
    int right = (2 * i) + 2;
    if (right >= h->count) {
        return -1;
    }
    return right;
}
// Getting the MAximum element: TC -> O(1)
ibt GetMaximum(struct Heap* h) {
    if(h->counut == 0) {
        return -1;
    }
    return h->array[0];
}
// Heapifying an element at location i: TC -> O(log n), SC -> O(1)
// Here the heap_type is max heap.
// Find the max elements among the node and its children. Swap the max element with the current node, 
// continue until heap property is satisfied at every node. 
void PercolateDown(struct Heap* h, int i) {
    int l, r, max, temp;
    l = LeftChild(h, i);
    r = RightChild(h, i);

    // If both children are invalid, we are at a leaf node
    if (l == -1 && r == -1) {
        return; // Base case: no children, terminate recursion
    }

    // Determine which child (if any) is larger
    max = i; // Start by assuming the current index is the largest

    if (l != -1 && h->array[l] > h->array[max]) {
        max = l;
    }
    
    if (r != -1 && h->array[r] > h->array[max]) {
        max = r;
    }
    
    // If the largest is not the current node, swap and continue
    if (max != i) {
        temp = h->array[i];
        h->array[i] = h->array[max];
        h->array[max] = temp;
        // Recur on the new position of the original node 
        // to check if the heap property is satisfied.
        PercolateDown(h, max);
    }
}

// Deleting an element: TC -> O(log n), SC -> O(1)
// We can delete an element from the root only.
// Copy the last element to the root.
// Delete the last element by reducing the heap size.
// Call PercoloateDown() when deleting an element.
void DeleteMaxElement(struct Heap* h) {
    int data;
    
    // Edge case: if heap is empty.
    if (h->count == 0) {
        return -1;
    }
    data = h->array[0];
    h->array[0] = h->array[h->count - 1];
    h->count = h->count - 1;
    // **NOTE: Call PercoloateDown() when deleting an element.**
    PercolateDown(h, 0); 

    return data;
}
// Inserting an element. TC -> O(log n), SC -> O(1)
int Insert(struct Heap* h, int data) {
    int i;
    if(h->count == h->capacity) {
        ResizeHeap(h)
    }
    h->count++; // increase the heap size to hold the new element
    i = h->count - 1;
    while(i >= 0 && data > h->array[(i - 1)/2]) {
        h->array[i] = h->array[(i - 1)/2];
        i = (i - 1)/2;
    }
    h->array[i] = data;
}
void ResizeHeap(struct Heap* h) {
    int* old_array = h->array;
    h->array = (int*) malloc(sizeof(int)* h->capacity * 2);
    if(h->array == NULL) {
        printf("Memory Error");
        return;
    }
    for(int i = 0; i < h->capacity; i++) {
        h->array = old_array[i];
    }
    h->capacity = h->capacity * 2;
    free(old_array); 
}
void DestroyHeap(struct Heap* h) {
    if(h == NULL) {
        return;
    }
    free(h->array);
    free(h);
    h = NULL;
}
// When building a heap, you heapifying the array n times,
// so TC: O(nlogn) for building a heap i.e. when heapifying an array.
void BuildHeap(struct Heap* h, int A[], int n) {
    // To be completed...
}
int main()
{
    return 0;
}
```