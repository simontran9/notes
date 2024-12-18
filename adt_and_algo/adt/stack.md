# Stack

## Overview

A stack of elements, such that the last in is the first out (LIFO).

| Operation             | Description                                                    |
| --------------------- | -------------------------------------------------------------- |
| `size()`              | Returns the number of elements in the stack.                  |
| `push(element)`       | Adds an element to the top of the stack.                      |
| `pop()`               | Removes and returns the top element of the stack.             |
| `top()` | Returns the element at the top of the stack without removing it. |

## Dynamic Array Stack

### Overview

See dynamic array overview in `list.md`.

### Access

Return the last element of the backed array via the array syntax.

### Insertion

1. Increase the backed array if it's full.
3. Add the specified element to the end of the backed array.

### Deletion

If the stack is not empty, remove the last element of the backed array.

### Resizing Strategy

See resizing strategy in `list.md`.

## Singly Linked Structure Stack

### Overview

See singly linked list overview in `list.md`.

### Access

Return the data of the head node.

### Insertion

1. Create a new node containing the specified element.
2. Set the `next` pointer of the new node to the current head node.
3. Update the head pointer to the new node.

### Deletion

1. Update the head pointer to the next node.
2. Unlink the old node from the linked structure.
4. If the queue becomes empty, set the tail pointer to `null`.
