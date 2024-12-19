# Sorted map and sorted set

## Overview

A sorted map and sorted set are abstract data types which extends the map and set abstract data type respectively, where the keys are sorted according to their natural ordering, or a specified ordering by passing in a compare function.

On top of supporting the same operations of a map and a set ADT, a sorted map and a sorted set support the following extended operations:
| **extended Operation**    | **Description**                                                   |
|------------------|-------------------------------------------------------------------|
| ...         | Same operations as map an set                 |
| `min()`          | Returns the smallest key in the sorted map.       |
| `max()`          | Returns the largest key in the sorted map.        |
| `successor(key)` | Finds the key that immediately follows the given key.   |
| `predecessor(key)`| Finds the key that immediately precedes the given key. |

## AVL tree

### Overview

### Time complexity

### Rotation mechanisms

### Lookup

### Insertion

### Deletion

### Code

```java
public class AVLTreeMap<K extends Comparable<K>, V> {
    private static class Node<K, V> {
        private K key;
        private V value;
        private Node<K, V> left;
        private Node<K, V> right;
        private int height;

        private Node(K key, V value) {
            this.key = key;
            this.value = value;
            this.left = null;
            this.right = null;
            this.height = 0;
        }
    }

    private Node<K, V> root;
    private int size;

    public AVLTreeMap() {
        this.root = null;
        this.size = 0;
    }

    public int size() {
        return this.size;
    }

    public K min() {
        if (this.root == null) {
            return null;
        }
        return minNode(this.root).key;
    }

    public K max() {
        if (this.root == null) {
            return null;
        }
        return maxNode(this.root).key;
    }

    public K successor(K key) {
        Node<K, V> successor = successorNode(this.root, key);
        if (successor == null) {
            return null;
        }
        return successor.key;
    }

    public K predecessor(K key) {
        Node<K, V> predecessor = predecessorNode(this.root, key);
        if (predecessor == null) {
            return null;
        }
        return predecessor.key;
    }

    public V get(K key) {
        Node<K, V> node = getNode(this.root, key);
        if (node == null) {
            return null;
        }
        return node.value;
    }

    public void add(K key, V value) {
        if (getNode(this.root, key) == null) {
            this.size += 1;
        }
        this.root = addNode(this.root, key, value);
    }

    public void remove(K key) {
        if (getNode(this.root, key) != null) {
            this.size -= 1;
        }
        this.root = removeNode(this.root, key);
    }

    private Node<K, V> minNode(Node<K, V> node) {
        while (node.left != null) {
            node = node.left;
        }
        return node;
    }

    private Node<K, V> maxNode(Node<K, V> node) {
        while (node.right != null) {
            node = node.right;
        }
        return node;
    }

    private Node<K, V> successorNode(Node<K, V> node, K key) {
        Node<K, V> successor = null;
        while (node != null) {
            int cmp = key.compareTo(node.key);
            if (cmp < 0) {
                successor = node;
                node = node.left;
            } else {
                node = node.right;
            }
        }
        return successor;
    }

    private Node<K, V> predecessorNode(Node<K, V> node, K key) {
        Node<K, V> predecessor = null;
        while (node != null) {
            int cmp = key.compareTo(node.key);
            if (cmp > 0) {
                predecessor = node;
                node = node.right;
            } else {
                node = node.left;
            }
        }
        return predecessor;
    }

    private Node<K, V> getNode(Node<K, V> node, K key) {
        while (node != null) {
            int cmp = key.compareTo(node.key);
            if (cmp < 0) {
                node = node.left;
            } else if (cmp > 0) {
                node = node.right;
            } else {
                return node;
            }
        }
        return null;
    }

    private Node<K, V> addNode(Node<K, V> node, K key, V value) {
        if (node == null) {
            return new Node<>(key, value);
        }
        int cmp = key.compareTo(node.key);
        if (cmp < 0) {
            node.left = addNode(node.left, key, value);
        } else if (cmp > 0) {
            node.right = addNode(node.right, key, value);
        } else {
            node.value = value;
        }
        updateHeight(node);
        return balance(node);
    }

    private Node<K, V> removeNode(Node<K, V> node, K key) {
        if (node == null) {
            return null;
        }
        int cmp = key.compareTo(node.key);
        if (cmp < 0) {
            node.left = removeNode(node.left, key);
        } else if (cmp > 0) {
            node.right = removeNode(node.right, key);
        } else {
            if (node.left == null) {
                return node.right;
            }
            if (node.right == null) {
                return node.left;
            }
            Node<K, V> successor = minNode(node.right);
            node.key = successor.key;
            node.value = successor.value;
            node.right = removeNode(node.right, successor.key);
        }
        updateHeight(node);
        return balance(node);
    }

    private void updateHeight(Node<K, V> node) {
        node.height = 1 + Math.max(height(node.left), height(node.right));
    }

    private int height(Node<K, V> node) {
        if (node == null) {
            return -1;
        }
        return node.height;
    }

    private Node<K, V> balance(Node<K, V> node) {
        int balanceFactor = balanceFactor(node);
        if (balanceFactor > 1) { // right heavy
            if (balanceFactor(node.right) < 0) { // right triangle
                node.right = rotateRight(node.right);
            }
            node = rotateLeft(node);
        } else if (balanceFactor < -1) { // left heavy
            if (balanceFactor(node.left) > 0) { // left triangle
                node.left = rotateLeft(node.left);
            }
            node = rotateRight(node);
        }
        return node;
    }

    private int balanceFactor(Node<K, V> node) {
        return height(node.right) - height(node.left);
    }

    private Node<K, V> rotateLeft(Node<K, V> root) {
        Node<K, V> newRoot = root.right;
        root.right = newRoot.left;
        newRoot.left = root;
        updateHeight(root);
        updateHeight(newRoot);
        return newRoot;
    }

    private Node<K, V> rotateRight(Node<K, V> root) {
        Node<K, V> newRoot = root.left;
        root.left = newRoot.right;
        newRoot.right = root;
        updateHeight(root);
        updateHeight(newRoot);
        return newRoot;
    }
}
```

```java
public class AVLTreeSet<K extends Comparable<K>> {
    private static class Node<K> {
        private K key;
        private Node<K> left;
        private Node<K> right;
        private int height;

        private Node(K key) {
            this.key = key;
            this.left = null;
            this.right = null;
            this.height = 0;
        }
    }

    private Node<K> root;
    private int size;

    public AVLTreeSet() {
        this.root = null;
        this.size = 0;
    }

    public int size() {
        return this.size;
    }

    public K min() {
        if (this.root == null) {
            return null;
        }
        return minNode(this.root).key;
    }

    public K max() {
        if (this.root == null) {
            return null;
        }
        return maxNode(this.root).key;
    }

    public K successor(K key) {
        Node<K> successor = successorNode(this.root, key);
        if (successor == null) {
            return null;
        }
        return successor.key;
    }

    public K predecessor(K key) {
        Node<K> predecessor = predecessorNode(this.root, key);
        if (predecessor == null) {
            return null;
        }
        return predecessor.key;
    }

    public boolean contains(K key) {
        return getNode(this.root, key) != null;
    }

    public void add(K key) {
        if (getNode(this.root, key) == null) {
            this.size += 1;
        }
        this.root = addNode(this.root, key);
    }

    public void remove(K key) {
        if (getNode(this.root, key) != null) {
            this.size -= 1;
        }
        this.root = removeNode(this.root, key);
    }

    private Node<K> minNode(Node<K> node) {
        while (node.left != null) {
            node = node.left;
        }
        return node;
    }

    private Node<K> maxNode(Node<K> node) {
        while (node.right != null) {
            node = node.right;
        }
        return node;
    }

    private Node<K> successorNode(Node<K> node, K key) {
        Node<K> successor = null;
        while (node != null) {
            int cmp = key.compareTo(node.key);
            if (cmp < 0) {
                successor = node;
                node = node.left;
            } else {
                node = node.right;
            }
        }
        return successor;
    }

    private Node<K> predecessorNode(Node<K> node, K key) {
        Node<K> predecessor = null;
        while (node != null) {
            int cmp = key.compareTo(node.key);
            if (cmp > 0) {
                predecessor = node;
                node = node.right;
            } else {
                node = node.left;
            }
        }
        return predecessor;
    }

    private Node<K> getNode(Node<K> node, K key) {
        while (node != null) {
            int cmp = key.compareTo(node.key);
            if (cmp < 0) {
                node = node.left;
            } else if (cmp > 0) {
                node = node.right;
            } else {
                return node;
            }
        }
        return null;
    }

    private Node<K> addNode(Node<K> node, K key) {
        if (node == null) {
            return new Node<>(key);
        }
        int cmp = key.compareTo(node.key);
        if (cmp < 0) {
            node.left = addNode(node.left, key);
        } else if (cmp > 0) {
            node.right = addNode(node.right, key);
        } else {
            return node;
        }
        updateHeight(node);
        return balance(node);
    }

    private Node<K> removeNode(Node<K> node, K key) {
        if (node == null) {
            return null;
        }
        int cmp = key.compareTo(node.key);
        if (cmp < 0) {
            node.left = removeNode(node.left, key);
        } else if (cmp > 0) {
            node.right = removeNode(node.right, key);
        } else {
            if (node.left == null) {
                return node.right;
            }
            if (node.right == null) {
                return node.left;
            }
            Node<K> successor = minNode(node.right);
            node.key = successor.key;
            node.right = removeNode(node.right, successor.key);
        }
        updateHeight(node);
        return balance(node);
    }

    private void updateHeight(Node<K> node) {
        node.height = 1 + Math.max(height(node.left), height(node.right));
    }

    private int height(Node<K> node) {
        if (node == null) {
            return -1;
        }
        return node.height;
    }

    private Node<K> balance(Node<K> node) {
        int balanceFactor = balanceFactor(node);
        if (balanceFactor > 1) { // right heavy
            if (balanceFactor(node.right) < 0) { // right triangle
                node.right = rotateRight(node.right);
            }
            node = rotateLeft(node);
        } else if (balanceFactor < -1) { // left heavy
            if (balanceFactor(node.left) > 0) { // left triangle
                node.left = rotateLeft(node.left);
            }
            node = rotateRight(node);
        }
        return node;
    }

    private int balanceFactor(Node<K> node) {
        return height(node.right) - height(node.left);
    }

    private Node<K> rotateLeft(Node<K> root) {
        Node<K> newRoot = root.right;
        root.right = newRoot.left;
        newRoot.left = root;
        updateHeight(root);
        updateHeight(newRoot);
        return newRoot;
    }

    private Node<K> rotateRight(Node<K> root) {
        Node<K> newRoot = root.left;
        root.left = newRoot.right;
        newRoot.right = root;
        updateHeight(root);
        updateHeight(newRoot);
        return newRoot;
    }
}
```

## Red-Black tree

### Overview

### Time complexity

### Rotation mechanisms

### Lookup

### Insertion

### Deletion

### Code

```java
public class RedBlackTreeMap<K extends Comparable<K>, V> {
    private static final boolean RED = false;
    private static final boolean BLACK = true;

    private static class Node<K, V> {
        private K key;
        private V value;
        private Node<K, V> left;
        private Node<K, V> right;
        private Node<K, V> parent;
        private boolean color;

        private Node(K key, V value, Node<K, V> left, Node<K, V> right, Node<K, V> parent, boolean color) {
            this.key = key;
            this.value = value;
            this.left = left;
            this.right = right;
            this.parent = parent;
            this.color = color;
        }
    }

    private Node<K, V> root;
    private Node<K, V> NIL; // red-black trees have NIL as their leaves
    private int size;

    public RedBlackTreeMap() {
        this.NIL = new Node<>(null, null, null, null, null, BLACK);
        this.root = NIL;
        this.size = 0;
    }

    public int size() {
        return this.size;
    }

    public K min() {
        if (this.root == NIL) {
            return null;
        }
        return minNode(this.root).key;
    }

    public K max() {
        if (this.root == NIL) {
            return null;
        }
        return maxNode(this.root).key;
    }

    public K successor(K key) {
        Node<K, V> successor = successorNode(this.root, key);
        if (successor == NIL) {
            return null;
        }
        return successor.key;
    }

    public K predecessor(K key) {
        Node<K, V> predecessor = predecessorNode(this.root, key);
        if (predecessor == NIL) {
            return null;
        }
        return predecessor.key;
    }

    public V get(K key) {
        Node<K, V> node = getNode(this.root, key);
        if (node == NIL) {
            return null;
        }
        return node.value;
    }

    public void add(K key, V value) {
        Node<K, V> current = this.root;
        Node<K, V> parent = NIL;
        Node<K, V> node = new Node<>(key, value, NIL, NIL, NIL, RED);
        while (current != NIL) {
            parent = current;
            int cmp = node.key.compareTo(current.key);
            if (cmp < 0) {
                current = current.left;
            } else if (cmp > 0) {
                current = current.right;
            } else {
                current.value = value;
                return;
            }
        }
        node.parent = parent;
        if (parent == NIL) {
            this.root = node;
        } else if (node.key.compareTo(parent.key) < 0) {
            parent.left = node;
        } else {
            parent.right = node;
        }
        this.size += 1;
        insertFixup(node);
    }

    public void remove(K key) {
        Node<K, V> delete = getNode(this.root, key);
        if (delete == NIL) {
            return;
        }
        this.size -= 1;
        Node<K, V> child;
        boolean originalColor = delete.color;
        if (delete.left == NIL) {
            child = delete.right;
            transplant(delete, child);
        } else if (delete.right == NIL) {
            child = delete.left;
            transplant(delete, child);
        } else {
            Node<K, V> successor = minNode(delete.right);
            originalColor = successor.color;
            child = successor.right;
            if (successor != delete.right) {
                transplant(successor, child);
                successor.right = delete.right;
                successor.right.parent = successor;
            } else {
                child.parent = successor;
            }
            transplant(delete, successor);
            successor.left = delete.left;
            successor.left.parent = successor;
            successor.color = delete.color;
        }
        if (originalColor == BLACK) {
            deleteFixup(child);
        }
    }

    private Node<K, V> minNode(Node<K, V> node) {
        while (node.left != NIL) {
            node = node.left;
        }
        return node;
    }

    private Node<K, V> maxNode(Node<K, V> node) {
        while (node.right != NIL) {
            node = node.right;
        }
        return node;
    }

    private Node<K, V> successorNode(Node<K, V> node, K key) {
        Node<K, V> successor = NIL;
        while (node != NIL) {
            int cmp = key.compareTo(node.key);
            if (cmp < 0) {
                successor = node;
                node = node.left;
            } else {
                node = node.right;
            }
        }
        return successor;
    }

    private Node<K, V> predecessorNode(Node<K, V> node, K key) {
        Node<K, V> predecessor = NIL;
        while (node != NIL) {
            int cmp = key.compareTo(node.key);
            if (cmp > 0) {
                predecessor = node;
                node = node.right;
            } else {
                node = node.left;
            }
        }
        return predecessor;
    }

    private Node<K, V> getNode(Node<K, V> node, K key) {
        while (node != NIL) {
            int cmp = key.compareTo(node.key);
            if (cmp < 0) {
                node = node.left;
            } else if (cmp > 0) {
                node = node.right;
            } else {
                return node;
            }
        }
        return NIL;
    }

    private void insertFixup(Node<K, V> node) {
        while (node.parent.color == RED) {
            if (node.parent == node.parent.parent.left) { // node's parent is a left child
                Node<K, V> uncle = node.parent.parent.right;
                if (uncle.color == RED) { // case 1: uncle is red
                    node.parent.color = BLACK;
                    uncle.color = BLACK;
                    node.parent.parent.color = RED;
                    node = node.parent.parent;
                } else {
                    if (node == node.parent.right) { // case 2: uncle is black, and node is a right child
                        node = node.parent;
                        rotateLeft(node);
                    }
                    node.parent.color = BLACK; // case 3: uncle is black, and node is a left child
                    node.parent.parent.color = RED;
                    rotateRight(node.parent.parent);
                }
            } else { // node's parent is a right child
                Node<K, V> uncle = node.parent.parent.left;
                if (uncle.color == RED) {
                    node.parent.color = BLACK; // case 1: uncle is red
                    uncle.color = BLACK;
                    node.parent.parent.color = RED;
                    node = node.parent.parent;
                } else {
                    if (node == node.parent.left) { // case 2: uncle is black, and node is a left child
                        node = node.parent;
                        rotateRight(node);
                    }
                    node.parent.color = BLACK;  // case 3: uncle is black, and node is a right child
                    node.parent.parent.color = RED;
                    rotateLeft(node.parent.parent);
                }
            }
        }
        this.root.color = BLACK;
    }

    private void deleteFixup(Node<K, V> node) {
        while (node != this.root && node.color == BLACK) {
            if (node == node.parent.left) { // node is a left child
                Node<K, V> sibling = node.parent.right;
                if (sibling.color == RED) { // case 1: sibling is red
                    sibling.color = BLACK;
                    node.parent.color = RED;
                    rotateLeft(node.parent);
                    sibling = node.parent.right;
                }
                if (sibling.left.color == BLACK && sibling.right.color == BLACK) { // case 2: sibling is black, and its children are black
                    sibling.color = RED;
                    node = node.parent;
                } else {
                    if (sibling.right.color == BLACK) { // case 3: sibling is black, and its left child is red and its right child is black
                        sibling.left.color = BLACK;
                        sibling.color = RED;
                        rotateRight(sibling);
                        sibling = node.parent.right;
                    }
                    sibling.color = node.parent.color; // case 4: sibling is black, and its right child is red
                    node.parent.color = BLACK;
                    sibling.right.color = BLACK;
                    rotateLeft(node.parent);
                    node = this.root;
                }
            } else { // node is a right child
                Node<K, V> sibling = node.parent.left;
                if (sibling.color == RED) { // case 1: sibling is red
                    sibling.color = BLACK;
                    node.parent.color = RED;
                    rotateRight(node.parent);
                    sibling = node.parent.left;
                }
                if (sibling.right.color == BLACK && sibling.left.color == BLACK) { // case 2: sibling is black, and its children are black
                    sibling.color = RED;
                    node = node.parent;
                } else {
                    if (sibling.left.color == BLACK) { // case 3: sibling is black, and its right child is red and its left child is black
                        sibling.right.color = BLACK;
                        sibling.color = RED;
                        rotateLeft(sibling);
                        sibling = node.parent.left;
                    }
                    sibling.color = node.parent.color; // case 4: sibling is black, and its left child is red
                    node.parent.color = BLACK;
                    sibling.left.color = BLACK;
                    rotateRight(node.parent);
                    node = this.root;
                }
            }
        }
        node.color = BLACK;
    }

    private void transplant(Node<K, V> delete, Node<K, V> replace) {
        if (delete.parent == NIL) {
            this.root = replace;
        } else if (delete == delete.parent.left) {
            delete.parent.left = replace;
        } else {
            delete.parent.right = replace;
        }
        replace.parent = delete.parent;
    }

    private void rotateLeft(Node<K, V> root) {
        Node<K, V> newRoot = root.right;
        root.right = newRoot.left;
        if (newRoot.left != NIL) {
            newRoot.left.parent = root;
        }
        newRoot.parent = root.parent;
        if (root.parent == NIL) {
            this.root = newRoot;
        } else if (root == root.parent.left) {
            root.parent.left = newRoot;
        } else {
            root.parent.right = newRoot;
        }
        newRoot.left = root;
        root.parent = newRoot;
    }

    private void rotateRight(Node<K, V> root) {
        Node<K, V> newRoot = root.left;
        root.left = newRoot.right;
        if (newRoot.right != NIL) {
            newRoot.right.parent = root;
        }
        newRoot.parent = root.parent;
        if (root.parent == NIL) {
            this.root = newRoot;
        } else if (root == root.parent.right) {
            root.parent.right = newRoot;
        } else {
            root.parent.left = newRoot;
        }
        newRoot.right = root;
        root.parent = newRoot;
    }
}
```

```java
public class RedBlackTreeSet<K extends Comparable<K>> {
    private static final boolean RED = false;
    private static final boolean BLACK = true;

    private static class Node<K> {
        private K key;
        private Node<K> left;
        private Node<K> right;
        private Node<K> parent;
        private boolean color;

        private Node(K key, Node<K> left, Node<K> right, Node<K> parent, boolean color) {
            this.key = key;
            this.left = left;
            this.right = right;
            this.parent = parent;
            this.color = color;
        }
    }

    private Node<K> root;
    private Node<K> NIL; // red-black trees have NIL as their leaves
    private int size;

    public RedBlackTreeSet() {
        this.NIL = new Node<>(null, null, null, null, BLACK);
        this.root = NIL;
        this.size = 0;
    }

    public int size() {
        return this.size;
    }

    public K min() {
        if (this.root == NIL) {
            return null;
        }
        return minNode(this.root).key;
    }

    public K max() {
        if (this.root == NIL) {
            return null;
        }
        return maxNode(this.root).key;
    }

    public K successor(K key) {
        Node<K> successor = successorNode(this.root, key);
        if (successor == NIL) {
            return null;
        }
        return successor.key;
    }

    public K predecessor(K key) {
        Node<K> predecessor = predecessorNode(this.root, key);
        if (predecessor == NIL) {
            return null;
        }
        return predecessor.key;
    }

    public boolean contains(K key) {
        return getNode(this.root, key) != NIL;
    }

    public void add(K key) {
        Node<K> current = this.root;
        Node<K> parent = NIL;
        Node<K> node = new Node<>(key, NIL, NIL, NIL, RED);
        while (current != NIL) {
            parent = current;
            int cmp = node.key.compareTo(current.key);
            if (cmp < 0) {
                current = current.left;
            } else if (cmp > 0) {
                current = current.right;
            } else {
                return;
            }
        }
        node.parent = parent;
        if (parent == NIL) {
            this.root = node;
        } else if (node.key.compareTo(parent.key) < 0) {
            parent.left = node;
        } else {
            parent.right = node;
        }
        this.size += 1;
        insertFixup(node);
    }

    public void remove(K key) {
        Node<K> delete = getNode(this.root, key);
        if (delete == NIL) {
            return;
        }
        this.size -= 1;
        Node<K> child;
        boolean originalColor = delete.color;
        if (delete.left == NIL) {
            child = delete.right;
            transplant(delete, child);
        } else if (delete.right == NIL) {
            child = delete.left;
            transplant(delete, child);
        } else {
            Node<K> successor = minNode(delete.right);
            originalColor = successor.color;
            child = successor.right;
            if (successor != delete.right) {
                transplant(successor, child);
                successor.right = delete.right;
                successor.right.parent = successor;
            } else {
                child.parent = successor;
            }
            transplant(delete, successor);
            successor.left = delete.left;
            successor.left.parent = successor;
            successor.color = delete.color;
        }
        if (originalColor == BLACK) {
            deleteFixup(child);
        }
    }

    private Node<K> minNode(Node<K> node) {
        while (node.left != NIL) {
            node = node.left;
        }
        return node;
    }

    private Node<K> maxNode(Node<K> node) {
        while (node.right != NIL) {
            node = node.right;
        }
        return node;
    }

    private Node<K> successorNode(Node<K> node, K key) {
        Node<K> successor = NIL;
        while (node != NIL) {
            int cmp = key.compareTo(node.key);
            if (cmp < 0) {
                successor = node;
                node = node.left;
            } else {
                node = node.right;
            }
        }
        return successor;
    }

    private Node<K> predecessorNode(Node<K> node, K key) {
        Node<K> predecessor = NIL;
        while (node != NIL) {
            int cmp = key.compareTo(node.key);
            if (cmp > 0) {
                predecessor = node;
                node = node.right;
            } else {
                node = node.left;
            }
        }
        return predecessor;
    }

    private Node<K> getNode(Node<K> node, K key) {
        while (node != NIL) {
            int cmp = key.compareTo(node.key);
            if (cmp < 0) {
                node = node.left;
            } else if (cmp > 0) {
                node = node.right;
            } else {
                return node;
            }
        }
        return NIL;
    }

    private void insertFixup(Node<K> node) {
        while (node.parent.color == RED) {
            if (node.parent == node.parent.parent.left) { // node's parent is a left child
                Node<K> uncle = node.parent.parent.right;
                if (uncle.color == RED) { // case 1: uncle is red
                    node.parent.color = BLACK;
                    uncle.color = BLACK;
                    node.parent.parent.color = RED;
                    node = node.parent.parent;
                } else {
                    if (node == node.parent.right) { // case 2: uncle is black, and node is a right child
                        node = node.parent;
                        rotateLeft(node);
                    }
                    node.parent.color = BLACK; // case 3: uncle is black, and node is a left child
                    node.parent.parent.color = RED;
                    rotateRight(node.parent.parent);
                }
            } else { // node's parent is a right child
                Node<K> uncle = node.parent.parent.left;
                if (uncle.color == RED) {
                    node.parent.color = BLACK; // case 1: uncle is red
                    uncle.color = BLACK;
                    node.parent.parent.color = RED;
                    node = node.parent.parent;
                } else {
                    if (node == node.parent.left) { // case 2: uncle is black, and node is a left child
                        node = node.parent;
                        rotateRight(node);
                    }
                    node.parent.color = BLACK;  // case 3: uncle is black, and node is a right child
                    node.parent.parent.color = RED;
                    rotateLeft(node.parent.parent);
                }
            }
        }
        this.root.color = BLACK;
    }

    private void deleteFixup(Node<K> node) {
        while (node != this.root && node.color == BLACK) {
            if (node == node.parent.left) { // node is a left child
                Node<K> sibling = node.parent.right;
                if (sibling.color == RED) { // case 1: sibling is red
                    sibling.color = BLACK;
                    node.parent.color = RED;
                    rotateLeft(node.parent);
                    sibling = node.parent.right;
                }
                if (sibling.left.color == BLACK && sibling.right.color == BLACK) { // case 2: sibling is black, and its children are black
                    sibling.color = RED;
                    node = node.parent;
                } else {
                    if (sibling.right.color == BLACK) { // case 3: sibling is black, and its left child is red and its right child is black
                        sibling.left.color = BLACK;
                        sibling.color = RED;
                        rotateRight(sibling);
                        sibling = node.parent.right;
                    }
                    sibling.color = node.parent.color; // case 4: sibling is black, and its right child is red
                    node.parent.color = BLACK;
                    sibling.right.color = BLACK;
                    rotateLeft(node.parent);
                    node = this.root;
                }
            } else { // node is a right child
                Node<K> sibling = node.parent.left;
                if (sibling.color == RED) { // case 1: sibling is red
                    sibling.color = BLACK;
                    node.parent.color = RED;
                    rotateRight(node.parent);
                    sibling = node.parent.left;
                }
                if (sibling.right.color == BLACK && sibling.left.color == BLACK) { // case 2: sibling is black, and its children are black
                    sibling.color = RED;
                    node = node.parent;
                } else {
                    if (sibling.left.color == BLACK) { // case 3: sibling is black, and its right child is red and its left child is black
                        sibling.right.color = BLACK;
                        sibling.color = RED;
                        rotateLeft(sibling);
                        sibling = node.parent.left;
                    }
                    sibling.color = node.parent.color; // case 4: sibling is black, and its left child is red
                    node.parent.color = BLACK;
                    sibling.left.color = BLACK;
                    rotateRight(node.parent);
                    node = this.root;
                }
            }
        }
        node.color = BLACK;
    }

    private void transplant(Node<K> delete, Node<K> replace) {
        if (delete.parent == NIL) {
            this.root = replace;
        } else if (delete == delete.parent.left) {
            delete.parent.left = replace;
        } else {
            delete.parent.right = replace;
        }
        replace.parent = delete.parent;
    }

    private void rotateLeft(Node<K> root) {
        Node<K> newRoot = root.right;
        root.right = newRoot.left;
        if (newRoot.left != NIL) {
            newRoot.left.parent = root;
        }
        newRoot.parent = root.parent;
        if (root.parent == NIL) {
            this.root = newRoot;
        } else if (root == root.parent.left) {
            root.parent.left = newRoot;
        } else {
            root.parent.right = newRoot;
        }
        newRoot.left = root;
        root.parent = newRoot;
    }

    private void rotateRight(Node<K> root) {
        Node<K> newRoot = root.left;
        root.left = newRoot.right;
        if (newRoot.right != NIL) {
            newRoot.right.parent = root;
        }
        newRoot.parent = root.parent;
        if (root.parent == NIL) {
            this.root = newRoot;
        } else if (root == root.parent.right) {
            root.parent.right = newRoot;
        } else {
            root.parent.left = newRoot;
        }
        newRoot.right = root;
        root.parent = newRoot;
    }
}
```

## in-memory B-tree

### Overview

A B-Tree is a rooted tree with the following properties:

**Node**  
For some node `x`, it has the following attributes:
- `n`, the number of keys currently stored in node `x`
- `[keys]`, an array of keys sorted in increasing order.
- `leaf`, a boolean value that is `true` if `x` is a leaf, and `false` if `x` is an internal node.
- `[child]`, pointers to `x`'s child nodes

**Node and children count**  
For some node with $|keys|$, then $|children| = |keys| + 1$.

**Keys as seperators**  
The keys in the node separate the ranges of keys stored in its subtrees.

**Leaves and depth**  
All leaves have the same depth.

**Key lower and upper bound**  
Every node has a lower and upper bound on the number of keys they can contain, which depends on a number $t \ge 2$ called the minimum degree.
```math
t - 1 \le |keys| \le 2t - 1
```
and so from the node and children count relationship defined above, then
```math
t \le |children| \le 2t
```
However, the root node has no lower bound for the key count or children count, but rather that if the tree is nonempty, it must have at least one key.  

**Remark** We consider a node full when it has exactly $2t - 1$ keys.

**Remark** We can call a specific B-tree with some minimum degree $t$ as "$t$-$2t$" tree, (e.g. a B-tree with minimum degree of $2$ is called a $2$-$3$-$4$ tree)

### Time complexity

| **Operation**             | **Time complexity**                                                    |
| --- | --- |
| lookup | $O(n)$ |
| insertion | $O(\log n)$ |
| deletion | $O(\log n)$ |

### Lookup



### Insertion

Insertion occurs only at the leaf nodes. When the leaf node is full, it splits into two, where the median is pushed up to the parent node.

### Deletion

Unlike insertion, deletion can occur at any node — not just at the leaves. 

- case 1: delete a key where the node has the min key count, then take highest from right sibling, or lowest from left sibling and set it as new seperator,then use the old seperator to fill

- case 2: merge with sibling

- case 3: delete tree that's not in a leaf node


### Code
