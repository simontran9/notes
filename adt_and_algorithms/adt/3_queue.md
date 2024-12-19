# Queue

## Overview

A queue is an abstract data type where elements are ordered in a first in is the first out (FIFO) fashion.

| **Operation**             | **Description**                                                    |
| --------------------- | -------------------------------------------------------------- |
| `size()`              | Returns the number of elements in the queue.                  |
| `enqueue(element)`    | Adds an element to the end of the queue.                      |
| `dequeue()`           | Removes and returns the front element of the queue.           |
| `front()`             | Returns the front element without removing it.                |

## Circular dynamic array queue

### Overview

A circular array queue uses a dynamic array that wraps around using a front pointer and a back pointer to avoid shifting elements unnecessarily. It efficiently utilizes space by reusing vacated positions as elements are dequeued.

### Time complexity

| **Operation**             | **Time complexity**                                                    |
| --- | --- |
| Lookup | $O(1)$ |
| Insertion | $O(1)$ |
| Deletion |$O(1)$ |

### Lookup

Return the first element of the backing array by accessing the `front` index.

### Insertion

1. If the queue is full, resize the backing array by allocating a new array by the growth factor. Copy existing elements in order from `front` to `back`, then, reset the `front` to 0 and `back` to the current size.
2. Place the new element at the `back` index.
3. Update the `back` index to `(back + 1) % capacity`.

### Deletion

1. Set the `front` index element to `null` to assist garbage collection.
2. Update the `front` index to `(front + 1) % capacity`.

### Resizing Strategy

See resizing strategy in `list.md`.

### Code

```java
import java.util.NoSuchElementException;

public class CircularArrayQueue<E> {
    private static final int INITIAL_CAPACITY = 10;
    private static final double GROWTH_FACTOR = 1.5;

    private E[] data;
    private int front;
    private int back;
    private int size;

    public CircularArrayQueue() {
        this.data = (E[]) new Object[INITIAL_CAPACITY];
        this.front = 0;
        this.back = 0;
        this.size = 0;
    }

    public int size() {
        return this.size;
    }

    public E front() {
        if (this.size == 0) {
            return null;
        }
        return this.data[this.front];
    }

    public void enqueue(E element) {
        if (this.size == this.data.length) {
            this.resize((int) Math.round(this.data.length * GROWTH_FACTOR));
        }
        this.data[this.back] = element;
        this.back = (this.back + 1) % this.data.length;
        this.size += 1;
    }

    public E dequeue() {
        if (this.size == 0) {
            throw new NoSuchElementException("Queue is empty");
        }
        E oldElement = this.data[this.front];
        this.data[this.front] = null;
        this.front = (this.front + 1) % this.data.length;
        this.size -= 1;
        return oldElement;
    }

    private void resize(int newCapacity) {
        E[] biggerArray = (E[]) new Object[newCapacity];
        for (int i = 0; i < this.size; i += 1) {
            biggerArray[i] = this.data[(this.front + i) % this.data.length];
        }
        this.data = biggerArray;
        this.front = 0;
        this.back = this.size;
    }
}
```

## Singly linked structure queue

### Overview

See singly linked list overview in `list.md`.

| **Operation**             | **Time complexity**                                                    |
| --- | --- |
| Lookup | $O(1)$ |
| Insertion | $O(1)$ |
| Deletion |$O(1)$ |

### Lookup

Return the data of the head node.

### Insertion

1. Create a new node containing the specified element.
2. Set the `next` pointer of the current tail node to the new node.
3. Update the tail pointer to the new node.

### Deletion

1. Update the head pointer to the next node.
2. Unlink the old head node from the linked structure.
3. If the queue becomes empty, set the tail pointer to `null`.

### Code

```java
public class SinglyLinkedListQueue<E> {
    private static class Node<T> {
        private T data;
        private Node<T> next;

        public Node(T data) {
            this.data = data;
            this.next = null;
        }
    }

    private Node<E> head;
    private Node<E> tail;
    private int size;

    public SinglyLinkedListQueue() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }

    public int size() {
        return this.size;
    }

    public E front() {
        if (this.head == null) {
            return null;
        }
        return this.head.data;
    }

    public void enqueue(E element) {
        Node<E> newNode = new Node<>(element);
        if (this.size == 0) {
            this.head = newNode;
        } else {
            this.tail.next = newNode;
        }
        this.tail = newNode;
        this.size += 1;
    }

    public E dequeue() {
        if (this.size == 0) {
            throw new NoSuchElementException("Queue is empty");
        }
        Node<E> removeNode = this.head;
        this.head = removeNode.next;
        E removedElement = removeNode.data;
        removeNode.next = null;
        this.size -= 1;
        if (this.size == 0) {
            this.tail = null;
        }
        return removedElement;
    }
}
```