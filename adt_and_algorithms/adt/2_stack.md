# Stack

## Overview

A queue is an abstract data type where elements are ordered in a last in is the first out (LIFO) fashion.

| **Operation**   | **Description**                                                  |
| --------------- | ---------------------------------------------------------------- |
| `size()`        | Returns the number of elements in the stack.                     |
| `push(element)` | Adds an element to the top of the stack.                         |
| `pop()`         | Removes and returns the top element of the stack.                |
| `top()`         | Returns the element at the top of the stack without removing it. |

## Dynamic array stack

### Overview

See dynamic array overview in `list.md`.

| **Operation** | **Time complexity** |
| ------------- | ------------------- |
| Lookup        | $O(1)$              |
| Insertion     | $O(1)$              |
| Deletion      | $O(1)$              |

### Lookup

Return the last element of the backed array via the array syntax.

### Insertion

1. Increase the backed array if it's full.
1. Add the specified element to the end of the backed array.

### Deletion

If the stack is not empty, remove the last element of the backed array.

### Resizing strategy

See resizing strategy in `list.md`.

### Code

```java
import java.util.NoSuchElementException;

public class ArrayStack<E> {
    private static final int INITIAL_CAPACITY = 10;
    private static final double GROWTH_FACTOR = 1.5;

    private E[] data;
    private int size;

    public ArrayStack() {
        this.data = (E[]) new Object[INITIAL_CAPACITY];
        this.size = 0;
    }

    public int size() {
        return this.size;
    }

    public E top() {
        if (this.size == 0) return null;
        return this.data[this.size - 1];
    }

    public void push(E element) {
        if (this.size == this.data.length) {
            this.resize((int) Math.round(this.data.length * GROWTH_FACTOR));
        }
        this.data[this.size] = element;
        this.size += 1;
    }

    public E pop() {
        if (this.size == 0) {
            throw new NoSuchElementException("Stack is empty");
        }
        E oldElement = this.data[this.size - 1];
        this.data[this.size - 1] = null;
        this.size -= 1;
        return oldElement;
    }

    private void resize(int newCapacity) {
        E[] biggerArray = (E[]) new Object[newCapacity];
        System.arraycopy(this.data, 0, biggerArray, 0, this.size);
        this.data = biggerArray;
    }
}
```

## Singly linked structure stack

### Overview

See singly linked list overview in `list.md`.

| **Operation** | **Time complexity** |
| ------------- | ------------------- |
| Lookup        | $O(1)$              |
| Insertion     | $O(1)$              |
| Deletion      | $O(1)$              |

### Lookup

Return the data of the head node.

### Insertion

1. Create a new node containing the specified element.
1. Set the `next` pointer of the new node to the current head node.
1. Update the head pointer to the new node.

### Deletion

1. Update the head pointer to the next node.
1. Unlink the old node from the linked structure.
1. If the queue becomes empty, set the tail pointer to `null`.

### Code

```java
public class SinglyLinkedListStack<E> {
    private static class Node<E> {
        private E data;
        private Node<E> next;

        public Node(E data) {
            this.data = data;
            this.next = null;
        }
    }

    private Node<E> head;
    private int size;

    public SinglyLinkedListStack() {
        this.head = null;
        this.size = 0;
    }

    public int size() {
        return this.size;
    }

    public E top() {
        if (this.head == null) {
            return null;
        }
        return this.head.data;
    }

    public void push(E element) {
        Node<E> newNode = new Node<>(element);
        newNode.next = this.head;
        this.head = newNode;
        this.size += 1;
    }

    public E pop() {
        if (this.size == 0) {
             throw new NoSuchElementException("Stack is empty");
        }
        Node<E> removeNode = this.head;
        this.head = removeNode.next;
        E removedElement = removeNode.data;
        removeNode.next = null;
        this.size -= 1;
        return removedElement;
    }
}
```
