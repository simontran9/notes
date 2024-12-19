# List

## Definition

A list is an abstract data type where elements are ordered consecutively.

Each element is assigned a unique index from $0$ to $N-1$, where $N$ is the total number of elements in the list. They can be added or removed anywhere in the list.

Lists are dynamic, meaning they can grow or shrink as elements are added or removed.

## Operations

| **Operation**         | **Description**                                                |
| --------------------- | -------------------------------------------------------------- |
| `size()`              | Returns the number of elements in the list.                    |
| `get(index)`          | Retrieves the element at the specified index in the list.      |
| `first()`             | Retrieves the first element in the list.                       |
| `last()`              | Retrieves the last element in the list.                        |
| `set(index, element)` | Updates the element at the specified index with a new element. |
| `add(index, element)` | Inserts a new element at the specified index.                  |
| `add_first(element)`  | Inserts a new element to the beginning of the list.            |
| `add_last(element)`   | Inserts a new element to the end of the list.                  |
| `remove(index)`       | Deletes the element at the specified index.                    |
| `remove_first()`      | Deletes the first element in the list.                         |
| `remove_last()`       | Deletes the last element in the list.                          |

## Dynamic Array List

### Overview

A dynamic array list uses an underlying array to store elements. It dynamically resizes itself when the capacity is exceeded.

### Time complexity

| **Operation** | **Time complexity**                 |
| ------------- | ----------------------------------- |
| Lookup        | $O(1)$                              |
| Insertion     | $O(n)$                              |
| Deletion      | $O(1)$ at the end, $O(n)$ elsewhere |

### Lookup

Return the data at a specified index with the array syntax.

### Update

Update the data at a specified index with the array syntax.

### Insertion

1. Deny the insertion request if the specified index is less than $0$ or greater than $N$ (current size).
1. When the array is full, increase its capacity by a factor of the growth factor. Allocate a new array and copy elements from the old array into the new one.
1. Shift all elements from the specified index onwards one position to the right, then place the new element at the specified index.

### Deletion

1. Deny the deletion request if the index is not between $0$ and $N - 1$.
1. Shift all elements from the specified index onwards one position to the left.
1. Set the last element in the array to `null`

### Resizing Strategy

A growth factor of 1.5 is preferred over 2 as it minimizes memory fragmentation and increases the chances of reusing freed memory for subsequent allocations.

### Code

```java
public class ArrayList<E> {
    private static int INITIAL_CAPACITY = 10;
    private static double GROWTH_FACTOR = 1.5;

    private E[] data;
    private int size;

    public ArrayList() {
        this.data = (E[]) new Object[INITIAL_CAPACITY];
        this.size = 0;
    }

    public int size() {
        return this.size;
    }

    public E get(int index) {
        if (index >= this.size) {
            throw new IndexOutOfBoundsException();
        }
        return this.data[index];
    }

    public E getFirst() {
        if (this.size == 0) {
            throw new IndexOutOfBoundsException();
        }
        return this.data[0];
    }

    public E getLast() {
        if (this.size == 0) {
            throw new IndexOutOfBoundsException();
        }
        return this.data[this.size - 1];
    }

    public E set(int index, E element) {
        if (index < 0 || index >= this.size) {
            throw new IndexOutOfBoundsException();
        }
        E oldElement = this.data[index];
        this.data[index] = element;
        return oldElement;
    }

    public void add(int index, E element) {
        if (index < 0 || index > this.size) {
            throw new IndexOutOfBoundsException();
        }
        if (this.size == this.data.length) {
            resize((int) Math.round(this.data.length * GROWTH_FACTOR));
        }
        if (index < this.size) {
            System.arraycopy(this.data, index, this.data, index + 1, this.size - index);
        }
        this.data[index] = element;
        this.size += 1;
    }

    public void addFirst(E element) {
        add(0, element);
    }

    public void addLast(E element) {
        add(this.size, element);
    }

    public E remove(int index) {
        if (index >= size) {
            throw new IndexOutOfBoundsException();
        }
        E oldElement = this.data[index];
        System.arraycopy(this.data, index + 1, this.data, index, this.size - index - 1);
        this.data[this.size - 1] = null;
        this.size -= 1;
        return oldElement;
    }

    public E removeFirst() {
        if (this.size == 0) {
            throw new IndexOutOfBoundsException();
        }
        return remove(0);
    }

    public E removeLast() {
        if (this.size == 0) {
            throw new IndexOutOfBoundsException();
        }
        return remove(this.size - 1);
    }

    private void resize(int newCapacity) {
        E[] newData = (E[]) new Object[newCapacity];
        System.arraycopy(this.data, 0, newData, 0, this.size);
        this.data = newData;
    }
}
```

## Singly Linked Structure List

### Overview

A singly linked list consists of nodes where each node contains a data element and a reference (or pointer) to the next node. The list has a head pointing to the first node and typically a tail pointing to the last node.

### Time complexity

| **Operation** | **Time complexity**                          |
| ------------- | -------------------------------------------- |
| Lookup        | $O(1)$ at the head or tail, $O(n)$ elsewhere |
| Insertion     | $O(1)$ at the head or tail, $O(n)$ elsewhere |
| Deletion      | $O(1)$ at the head, $O(n)$ elsewhere         |

### Lookup

**Case 1: Index is $0**\
Return the data at the head node.

**Case 2: Index is $N - 1$**\
Return the data at the tail node.

**Case 3: Any other valid index**\
Start at the head and iterate through the list until reaching the specified index. Return the node's data.

### Update

**Case 1: Index is $0$ or $N - 1$**\
Replace the data in the head or tail node as appropriate.

**Case 2: Any other valid index**\
Traverse the list to the specified index and update the data in the corresponding node.

### Insertion

**Case 1: Index is $0$**\
Create a new node and make it the new head.

**Case 2: Index is $N$**\
Create a new node and make it the new tail.

**Case 3: Any other valid index**\
Create a new node, traverse to the node at index $i - 1$, link the new node to the next node, and update the previous node to point to the new node.

### Deletion

**Case 1: Index is $0$**\
Remove the head node and make the next node the new head. If the list becomes empty, set the tail to `null`.

**Case 2: Index is $N - 1$**\
Traverse the list to the second-to-last node, set its `next` pointer to `null`, and update the tail to this node.

**Case 3: Any other valid index**\
Traverse to the node at index $i - 1$, update its `next` pointer to skip the node being removed, and unlink the node being removed.

### Code

```java
public class SinglyLinkedList<E> {
    private static class Node<E> {
        private E data;
        private Node<E> next;

        public Node(E data) {
            this.data = data;
            this.next = null;
        }
    }

    private Node<E> head;
    private Node<E> tail;
    private int size;

    public SinglyLinkedList() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }

    public int size() {
        return this.size;
    }

    public E get(int index) {
        if (index < 0 || index >= this.size) {
            throw new IndexOutOfBoundsException();
        }
        if (index == 0) {
            return this.head.data;
        }
        if (index == this.size - 1) {
            return this.tail.data;
        }
        return getNode(index).data;
    }

    public E getFirst() {
        if (this.head == null) {
            throw new IndexOutOfBoundsException();
        }
        return this.head.data;
    }

    public E getLast() {
        if (this.tail == null) {
            throw new IndexOutOfBoundsException();
        }
        return this.tail.data;
    }

    public E set(int index, E element) {
        if (index >= this.size) {
            throw new IndexOutOfBoundsException();
        }
        if (index == 0) {
            E oldElement = this.head.data;
            this.head.data = element;
            return oldElement;
        }
        if (index == size - 1) {
            E oldElement = this.tail.data;
            this.tail.data = element;
            return oldElement;
        }
        Node<E> node = getNode(index);
        E oldElement = node.data;
        node.data = element;
        return oldElement;
    }

    public void add(int index, E element) {
        if (index > this.size) {
            throw new IndexOutOfBoundsException();
        }
        Node<E> newNode = new Node<>(element);
        if (this.size == 0) {
            this.head = newNode;
            this.tail = newNode;
        } else if (index == 0) {
            newNode.next = this.head;
            this.head = newNode;
        } else if (index == this.size) {
            this.tail.next = newNode;
            this.tail = newNode;
        } else {
            Node<E> prevNode = getNode(index - 1);
            newNode.next = prevNode.next;
            prevNode.next = newNode;
        }
        this.size += 1;
    }

    public void addFirst(E element) {
        add(0, element);
    }

    public void addLast(E element) {
        add(this.size, element);
    }

    public E remove(int index) {
        if (index >= this.size) {
            throw new IndexOutOfBoundsException();
        }
        Node<E> removeNode;
        if (index == 0) {
            removeNode = this.head;
            this.head = removeNode.next;
            if (this.size == 1) {
                this.tail = null;
            }
        } else {
            Node<E> prevNode = getNode(index - 1);
            removeNode = prevNode.next;
            prevNode.next = removeNode.next;
            if (index == this.size - 1) {
                this.tail = prevNode;
            }
        }
        E removedElement = removeNode.data;
        this.size -= 1;
        return removedElement;
    }

    public E removeFirst() {
        if (this.size == 0) {
            throw new IndexOutOfBoundsException();
        }
        return remove(0);
    }

    public E removeLast() {
        if (this.size == 0) {
            throw new IndexOutOfBoundsException();
        }
        return remove(this.size - 1);
    }

    private Node<E> getNode(int index) {
        Node<E> current = this.head;
        for (int i = 0; i < index; i += 1) {
            current = current.next;
        }
        return current;
    }
}
```

## Doubly Linked Structure List

### Overview

A doubly linked list consists of nodes where each node contains a data element, a reference to the previous node, and a reference to the next node. The list typically has a sentinel head and tail to simplify operations at the boundaries.

### Time complexity

| **Operation** | **Time complexity**                          |
| ------------- | -------------------------------------------- |
| Lookup        | $O(1)$ at the head or tail, $O(n)$ elsewhere |
| Insertion     | $O(1)$ at the head or tail, $O(n)$ elsewhere |
| Deletion      | $O(1)$ at the head or tail, $O(n)$ elsewhere |

### Lookup

**Case 1: Index is $0$**\
Return the data at the head node.

**Case 2: Index is $N - 1$**\
Return the data at the tail node.

**Case 3: Any other valid index**\
If the index is less than $N / 2$, traverse from the head; otherwise, traverse from the tail. Return the node's data.

### Update

**Case 1: Index is $0$ or $N - 1$**\
Replace the data in the head or tail node as appropriate.

**Case 2: Any other valid index**\
Traverse from the closest end (head or tail) to the specified index, then update the data in the corresponding node.

### Insertion

**Case 1: Index is $0$**\
Create a new node and update the dummy head's `next` pointer and the old head's `prev` pointer to point to it.

**Case 2: Index is $N$**\
Create a new node and update the dummy tail's `prev` pointer and the old tail's `next` pointer to point to it.

**Case 3: Any other valid index**\
Traverse to the node at index $i - 1$, insert the new node between this node and the next, and update their `next` and `prev` pointers accordingly.

### Deletion

**Case 1: Index is $0$**\
Update the dummy head's `next` pointer to the second node and the second node's `prev` pointer to the dummy head.

**Case 2: Index is $N - 1$**\
Update the dummy tail's `prev` pointer to the second-to-last node and the second-to-last node's `next` pointer to the dummy tail.

**Case 3: Any other valid index**\
Traverse to the node at index $i - 1$, update its `next` pointer to skip the node being removed, and update the `prev` pointer of the node following the removed node to point to the node at index $i - 1$.

### Code

```java
public class DoublyLinkedList<E> {
    private static class Node<E> {
        private Node<E> prev;
        private E data;
        private Node<E> next;

        public Node(E data) {
            this.prev = null;
            this.data = data;
            this.next = null;
        }
    }

    private Node<E> dummyHead;
    private Node<E> dummyTail;
    private int size;

    public DoublyLinkedList() {
        this.dummyHead = new Node<>(null);
        this.dummyTail = new Node<>(null);
        this.dummyHead.next = this.dummyTail;
        this.dummyTail.prev = this.dummyHead;
        this.size = 0;
    }

    public int size() {
        return this.size;
    }

    public E get(int index) {
        if (index >= this.size) {
            throw new IndexOutOfBoundsException();
        }
        return this.getNode(index).data;
    }

    public E getFirst() {
        if (this.size == 0) {
            throw new IndexOutOfBoundsException();
        }
        return this.dummyHead.next.data;
    }

    public E getLast() {
        if (this.size == 0) {
            throw new IndexOutOfBoundsException();
        }
        return this.dummyTail.prev.data;
    }

    public E set(int index, E element) {
        if (index >= this.size) {
            throw new IndexOutOfBoundsException();
        }
        Node<E> node = this.getNode(index);
        E oldElement = node.data;
        node.data = element;
        return oldElement;
    }

    public void add(int index, E element) {
        if (index > this.size) {
            throw new IndexOutOfBoundsException();
        }
        Node<E> newNode = new Node<>(element);
        Node<E> prevNode;
        Node<E> nextNode;
        if (this.size == 0) {
            prevNode = this.dummyHead;
            nextNode = this.dummyTail;
        } else if (index == 0) {
            prevNode = this.dummyHead;
            nextNode = this.dummyHead.next;
        } else if (index == this.size) {
            prevNode = this.dummyTail.prev;
            nextNode = this.dummyTail;
        } else {
            prevNode = this.getNode(index - 1);
            nextNode = prevNode.next;
        }
        prevNode.next = newNode;
        nextNode.prev = newNode;
        newNode.next = nextNode;
        newNode.prev = prevNode;
        this.size += 1;
    }

    public void addFirst(E element) {
        this.add(0, element);
    }

    public void addLast(E element) {
        this.add(this.size, element);
    }

    public E remove(int index) {
        if (index >= this.size) {
            throw new IndexOutOfBoundsException();
        }
        Node<E> removeNode;
        Node<E> prevNode;
        Node<E> nextNode;
        if (index == 0) {
            removeNode = this.dummyHead.next;
            prevNode = this.dummyHead;
            nextNode = removeNode.next;
        } else if (index == this.size - 1) {
            removeNode = this.dummyTail.prev;
            prevNode = removeNode.prev;
            nextNode = this.dummyTail;
        } else {
            prevNode = this.getNode(index - 1);
            removeNode = prevNode.next;
            nextNode = removeNode.next;
        }
        prevNode.next = nextNode;
        nextNode.prev = prevNode;
        E removedElement = removeNode.data;
        this.size -= 1;
        return removedElement;
    }

    public E removeFirst() {
        if (this.size == 0) {
            throw new IndexOutOfBoundsException();
        }
        return this.remove(0);
    }

    public E removeLast() {
        if (this.size == 0) {
            throw new IndexOutOfBoundsException();
        }
        return this.remove(this.size - 1);
    }

    private Node<E> getNode(int index) {
        Node<E> current;
        if (index < this.size / 2) {
            current = this.dummyHead.next;
            for (int i = 0; i < index; i += 1) {
                current = current.next;
            }
        } else {
            current = this.dummyTail.prev;
            for (int i = this.size - 1; i > index; i -= 1) {
                current = current.prev;
            }
        }
        return current;
    }
}
```
