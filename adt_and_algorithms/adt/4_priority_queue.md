# Priority Queue ADT

## Overview

A queue of elements where elements are ordered by their priority, which tends to be in terms of their size.

## Binary Heap

```java
import java.util.ArrayList;

public class BinaryMaxHeapPriorityQueue<K extends Comparable<K>> {
    private ArrayList<K> data;

    public BinaryMaxHeapPriorityQueue() {
        this.data = new ArrayList<>();
        this.data.add(null);
    }

    public BinaryMaxHeapPriorityQueue(ArrayList<K> initialKeys) {
        this.data = new ArrayList<>();
        this.data.add(null);
        this.data.addAll(initialKeys);
        buildMaxHeap();
    }

    public int size() {
        return this.data.size() - 1;
    }

    public void add(K key) {
        this.data.add(key);
        siftUp(this.data.size() - 1);
    }

    public K top() {
        if (this.data.size() == 1) {
            return null;
        }
        return this.data.get(1);
    }

    public K removeTop() {
        K top = this.data.get(1);
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
        K temp = this.data.get(i);
        this.data.set(i, this.data.get(j));
        this.data.set(j, temp);
    }
}
```

```java
import java.util.ArrayList;

public class BinaryMinHeapPriorityQueue<K extends Comparable<K>> {
    private ArrayList<K> data;

    public BinaryMinHeapPriorityQueue() {
        this.data = new ArrayList<>();
        this.data.add(null);
    }

    public BinaryMinHeapPriorityQueue(ArrayList<K> initialKeys) {
        this.data = new ArrayList<>();
        this.data.add(null);
        this.data.addAll(initialKeys);
        buildMinHeap();
    }

    public int size() {
        return this.data.size() - 1;
    }

    public void add(K key) {
        this.data.add(key);
        siftUp(this.data.size() - 1);
    }

    public K top() {
        if (this.data.size() == 1) {
            return null;
        }
        return this.data.get(1);
    }

    public K removeTop() {
        K top = this.data.get(1);
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
        K temp = this.data.get(i);
        this.data.set(i, this.data.get(j));
        this.data.set(j, temp);
    }
}
```