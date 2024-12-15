# List

## Overview

- Elements are organized in a sequential, linear order.
- Each element in the list is assigned a unique index ranging from $0$ to $N-1$, where $N$ is the total number of elements in the list.
- Lists are dynamic, meaning they can grow or shrink as elements are added or removed.

## Operations

| Operation             | Description                                                    |
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

### Retrieval

Return the data at a specified index with the array syntax.

### Update

Update the data at a specified index with the array syntax.

### Insertion

1. Deny the insertion request if the specified index is less than $0$ or greater than $N$ (current size).
2. When the array is full, increase its capacity by a factor of the growth factor. Allocate a new array and copy elements from the old array into the new one.
3. Shift all elements from the specified index onwards one position to the right, then place the new element at the specified index.

### Deletion

1. Deny the deletion request if the index is not between $0$ and $N - 1$.
2. Shift all elements from the specified index onwards one position to the left.
3. Set the last element in the array to `null`  

### Resizing Strategy

A growth factor of 1.5 is preferred over 2 as it minimizes memory fragmentation and increases the chances of reusing freed memory for subsequent allocations.

## Singly Linked Structure List

### Overview

A singly linked list consists of nodes where each node contains a data element and a reference (or pointer) to the next node. Additionally, it has a head and usually a tail pointer pointing to the first and last node, respectively.

### Retrieval

- **Case 1: Index is $0$**
  Return the data at the head node.
- **Case 2: Index is $N - 1$**
  Return the data at the tail node.
- **Case 3: Anywhere else**
  Start at the head and iterate through the list until reaching the specified index. Then, return the node's data.

### Update

- **Case 1: Index is $0$ or $N - 1$**
  Replace the data in the head or tail node as appropriate.
- **Case 2: Anywhere else**
  Traverse the list to the specified index, then replace the data in the corresponding node.

### Insertion

1. If the index is $0$, make the new node the new head.
2. If the index is $N$, make the new node the new tail.
3. For any other index, iterate to the node at index $i - 1$, link the new node to the next node, and update the previous node to point to the new node.

### Deletion

1. If the index is $0$, remove the head node and make the next node the new head. If the list becomes empty, set the tail to `null`.
2. If the index is $N - 1$, traverse the list to the second-to-last node, set its `next` pointer to `null`, and update the tail to this node.
3. For any other index, traverse to the node at index $i - 1$, update its `next` pointer to skip the node being removed, and unlink the node being removed from the list

## Doubly Linked Structure List

### Overview

- Consists of nodes where each node contains a data element, a reference to the previous node, and a reference to the next node.
- Typically has a sentinel head and sentinel tail to alleviate certain edge cases.

### Retrieval

- **Case 1: Index is $0$**
  Return the data at the head node.
- **Case 2: Index is $N - 1$**
  Return the data at the tail node.
- **Case 3: Anywhere else**
  If the index is less than $N / 2$, traverse from the head; otherwise, traverse from the tail. Return the data from the node at the specified index.

### Update

- **Case 1: Index is $0$ or $N - 1$**
  Replace the data in the head or tail node as appropriate.
- **Case 2: Anywhere else**
  Traverse from the closest end (head or tail) to the specified index, then replace the data in the corresponding node.

### Insertion

1. If the index is $0$, create a new node, link it as the new head, and update the dummy head's next reference.
2. If the index is $N$, create a new node, link it as the new tail, and update the dummy tail's previous reference.
3. For any other index, traverse to the node at index $i - 1$, insert the new node between this node and the next, updating the references of all involved nodes.

### Deletion

1. If the index is $0$, remove the head node by updating the dummy head's next reference to the second node. Adjust the second node's `prev` pointer.
2. If the index is $N - 1$, remove the tail node by updating the dummy tail's previous reference to the second-to-last node. Adjust this node's `next` pointer.
3. For any other index, traverse to the node at index $i - 1, update its `next` pointer to skip the node being removed, and adjust the `prev` pointer of the subsequent node.
