# Queue

## Overview

A queue of elements, such that the first in is the first out (FIFO).

| Operation             | Description                                                    |
| --------------------- | -------------------------------------------------------------- |
| `size()`              | Returns the number of elements in the queue.                  |
| `enqueue(element)`    | Adds an element to the end of the queue.                      |
| `dequeue()`           | Removes and returns the front element of the queue.           |
| `front()`             | Returns the front element without removing it.                |

## Circular Dynamic Array Queue

### Overview

A circular array queue uses a dynamic array that wraps around using a front pointer and a back pointer to avoid shifting elements unnecessarily. It efficiently utilizes space by reusing vacated positions as elements are dequeued.

### Access

Return the first element of the backing array by accessing the `front` index.

### Insertion

1. If the queue is full, resize the backing array to increase its current capacity.
2. Place the new element at the `back` index.
3. Update the `back` index to `(back + 1) % capacity`.

### Deletion

1. Set the `front` index element to `null` to assist garbage collection.
2. Update the `front` index to `(front + 1) % capacity`.

### Resizing Strategy

When the backing array is full, allocate a new array by the growth factor. Copy existing elements in order from `front` to `back`. Reset the `front` to 0 and `back` to the current size.

## Singly Linked Structure Queue

### Overview

See singly linked list overview in `list.md`.

### Access

Return the data of the head node.

### Insertion

1. Create a new node containing the specified element.
2. Set the `next` pointer of the current tail node to the new node.
3. Update the tail pointer to the new node.

### Deletion

1. Update the head pointer to the next node.
2. Unlink the old head node from the linked structure.
3. If the queue becomes empty, set the tail pointer to `null`.
