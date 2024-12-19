# Priority Queue

## Overview

A priority queue is an abstract data type where elements are assigned priorities, such those with higher priorities are served before those with lower priorities.

| **Operation**  | **Description**                                                                  |
| -------------- | -------------------------------------------------------------------------------- |
| `size()`       | Returns the number of elements in the priority queue.                            |
| `top()`        | Returns the element with the top priority without removing it.                   |
| `add(element)` | Adds an element to the priority queue.                                           |
| `remove_top()` | Removes and returns the element with the highest priority in the priority queue. |

## Binary Heap

### Overview

A binary heap is a complete binary tree with one of the heap ordering property:

- The min-heap property: the value of each parent node is less than or equal to the value of its two child nodes.
- The max-heap property: the value of each parent node is greater than or equal to the value of its two child nodes.\
  Hence, the heap ordering property implies that the minimum-value element is at the root for a min-heap, and the maximum-value element is at the root for a max-heap.

In code, a common layout for heaps are arrays, where the root is always at index $1$, and for some node at index $i$:

- the left child is located at index $2i$
- the right child is located at index $2i + 1$
- its parent node is located at $\\lfloor\\frac{i}{2}\\rfloor$

### Time complexity

| **Operation** | **Time complexity** |
| ------------- | ------------------- |
| lookup        | $O(1)$              |
| insertion     | $O(\\log n)$        |
| deletion      | $O(\\log n)$        |

### Sift up and sift down mechanism

Many heap operations make use of sifting mechanisms, called "sift up" and "sift down" to place nodes in their correct position within the tree.

**Sift up**\
For some node, we repeatly "sift" the node "up" if the node's key is less than its parent's key (min-heap) or if the node's key is greater than its parent's key (max-heap) by swapping positions with its parent.

**Sift down**\
For some node, we repeatly "sift" the node "down" when its key is greater than one of its children's key by swapping it with the smallest children (min-heap) or when its key is less than one of its children's key by swapping it with the largest children (max-heap).

### Build a heap from initial elements

To build a heap from initial elements of a list in $O(n)$ worst case, we build in a bottom-up manner as follows:

1. Copy over all elements in the original list into the backing dynamic array which has the first element at index 0 as `null`.
1. Beginning at the parent of the last element in the list, up to and including index $1$, repeatedly call sift down.

### Lookup

Return the element at index $1$.

### Insertion

1. Add an element as the righmost leaf of the heap (i.e. the last element in the backing array).
1. Sift up the node containing the element to correct its position within the tree.

### Deletion

1. Set the root node's key to be the same key of the last node in the heap.
1. Remove the last node in the heap now that we've placed its key in the root node.
1. Perform sift down on the root node to place the node in its correct position within the tree.

### Code

**Binary max heap**

```java
import java.util.ArrayList;

public class BinaryMaxHeapPriorityQueue<E extends Comparable<E>> {
    private ArrayList<E> data;

    public BinaryMaxHeapPriorityQueue() {
        this.data = new ArrayList<>();
        this.data.add(null);
    }

    public BinaryMaxHeapPriorityQueue(ArrayList<E> initialElements) {
        this.data = new ArrayList<>();
        this.data.add(null);
        this.data.addAll(initialElements);
        buildMaxHeap();
    }

    public int size() {
        return this.data.size() - 1;
    }

    public void add(E element) {
        this.data.add(element);
        siftUp(this.data.size() - 1);
    }

    public E top() {
        if (this.data.size() == 1) {
            return null;
        }
        return this.data.get(1);
    }

    public E removeTop() {
        E top = this.data.get(1);
        this.data.set(1, this.data.getLast());
        this.data.removeLast();
        siftDown(1);
        return top;
    }

    private void buildMaxHeap() {
        for (int i = parent(this.data.size() - 1); i >= 1; i -= 1) {
            siftDown(i);
        }
    }

    private void siftDown(int i) {
        int largest = i;
        int left = left(i);
        int right = right(i);
        if (left <= this.data.size() - 1 && this.data.get(left).compareTo(this.data.get(largest)) > 0) {
            largest = left;
        }
        if (right <= this.data.size() - 1 && this.data.get(right).compareTo(this.data.get(largest)) > 0) {
            largest = right;
        }
        if (largest != i) {
            swap(i, largest);
            siftDown(largest);
        }
    }

    private void siftUp(int i) {
        if (i > 1 && this.data.get(i).compareTo(this.data.get(parent(i))) > 0) {
            swap(i, parent(i));
            siftUp(parent(i));
        }
    }

    private int parent(int i) {
        return i / 2;
    }

    private int left(int i) {
        return 2 * i;
    }

    private int right(int i) {
        return 2 * i + 1;
    }

    private void swap(int i, int j) {
        E temp = this.data.get(i);
        this.data.set(i, this.data.get(j));
        this.data.set(j, temp);
    }
}
```

**Binary min heap**

```java
import java.util.ArrayList;

public class BinaryMinHeapPriorityQueue<E extends Comparable<E>> {
    private ArrayList<E> data;

    public BinaryMinHeapPriorityQueue() {
        this.data = new ArrayList<>();
        this.data.add(null);
    }

    public BinaryMinHeapPriorityQueue(ArrayList<E> initialElements) {
        this.data = new ArrayList<>();
        this.data.add(null);
        this.data.addAll(initialElements);
        buildMinHeap();
    }

    public int size() {
        return this.data.size() - 1;
    }

    public void add(E element) {
        this.data.add(element);
        siftUp(this.data.size() - 1);
    }

    public E top() {
        if (this.data.size() == 1) {
            return null;
        }
        return this.data.get(1);
    }

    public E removeTop() {
        E top = this.data.get(1);
        this.data.set(1, this.data.getLast());
        this.data.removeLast();
        siftDown(1);
        return top;
    }

    private void buildMinHeap() {
        for (int i = parent(this.data.size() - 1); i >= 1; i--) {
            siftDown(i);
        }
    }

    private void siftDown(int i) {
        int smallest = i;
        int left = left(i);
        int right = right(i);
        if (left <= this.data.size() - 1 && this.data.get(left).compareTo(this.data.get(smallest)) < 0) {
            smallest = left;
        }
        if (right <= this.data.size() - 1 && this.data.get(right).compareTo(this.data.get(smallest)) < 0) {
            smallest = right;
        }
        if (smallest != i) {
            swap(i, smallest);
            siftDown(smallest);
        }
    }

    private void siftUp(int i) {
        if (i > 1 && this.data.get(i).compareTo(this.data.get(parent(i))) < 0) {
            swap(i, parent(i));
            siftUp(parent(i));
        }
    }

    private int parent(int i) {
        return i / 2;
    }

    private int left(int i) {
        return 2 * i;
    }

    private int right(int i) {
        return 2 * i + 1;
    }

    private void swap(int i, int j) {
        E temp = this.data.get(i);
        this.data.set(i, this.data.get(j));
        this.data.set(j, temp);
    }
}
```
