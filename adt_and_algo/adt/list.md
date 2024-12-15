# List

## Overview

- elements are ordered linearly and labelled by indices from $0$ to $N - 1$ if we have $N$ elements
- can grow or shrink freely
- insertion can occur anywhere, but access, update, and deletion operations must occur within the bounds of valid indices

| Operation       | Description                                                                |
|----------------------|---------------------------------------------------------------------------------|
| `size()`            | Returns the number of elements in the list.                                    |
| `get(index)`        | Retrieves the element at the specified index in the list.                      |
| `first()`           | Retrieves the first element in the list.                                       |
| `last()`            | Retrieves the last element in the list.                                        |
| `set(index, element)` | Replaces the element at the specified index with a new element in the list.    |
| `add(index, element)` | Inserts a new element at the specified index, shifting subsequent elements in the list. |
| `add_first(element)`  | Adds a new element to the beginning of the list.                             |
| `add_last(element)`   | Adds a new element to the end of the list.                                   |
| `remove(index)`       | Removes the element at the specified index, shifting subsequent elements in the list. |
| `remove_first()`      | Removes the first element in the list.                                       |
| `remove_last()`       | Removes the last element in the list.                                        |

## Dynamic array list

### overview

- begin with an initial array
- whenever we try to insert an element at an index outside of the current index bounds, the dynamic array "dynamically" increases in size by allocating a new array of a certain size determined by a growth factor (ideally 1.5 since it reduces memory fragmentation and increases the chance of reusing freed memory for subsequent allocations), and moving over the data in the current array to the new array

**access**
access happens just like an array — we specify an index

### update

### insertion

### deletion

## singly linked structure list

## doubly linked structure list

### overview

### access

**case 1: index is 0 or $N - 1$**
return the data if the specified index was $0$, or the data at the tail if it was $N - 1$

**case 2: anywhere else**
If the specified index is less than $N / 2$, begin at the head and iterate until we reach the node with the specified index, and then return its data. Otherwise, begin at the tail, and iterate until we reach the node with the specified index, and then return its data.

### insertion

**case 1: head**

**case 2: anywhere else**

### update

**case 1: head or tail**

**case 2: anywhere else**

### deletion

**case 1: head or tail**

**case 2: anywhere else**

