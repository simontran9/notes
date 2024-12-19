# Map and set

## Overview

## Trie

### Retrieval

### Insertion

### Deletion

### Prefix match

```java
import java.util.AbstractMap;
import java.util.Map;
import java.util.HashMap;
import java.util.ArrayList;

public class TrieMap<V> {
    private static class Node<V> {
        private V value;
        private HashMap<Character, Node<V>> children;

        private Node() {
            this.value = null;
            this.children = new HashMap<>();
        }
    }

    private Node<V> root;
    private int size;

    public TrieMap() {
        this.root = new Node<>();
        this.size = 0;
    }

    public int size() {
        return this.size;
    }

    public V get(String word) {
        Node<V> current = this.root;
        for (int i = 0; i < word.length(); i += 1) {
            char ch = word.charAt(i);
            Node<V> node = current.children.get(ch);
            if (node == null) {
                return null;
            }
            current = node;
        }
        return current.value;
    }

    public void add(String word, V value) {
        Node<V> current = this.root;
        for (int i = 0; i < word.length(); i += 1) {
            char ch = word.charAt(i);
            Node<V> node = current.children.get(ch);
            if (node == null) {
                node = new Node<>();
                current.children.put(ch, node);
            }
            current = node;
        }
        if (current.value == null) {
            this.size += 1;
        }
        current.value = value;
    }

    public void remove(String word) {
        if (remove(this.root, word, 0)) {
            this.size -= 1;
        }
    }

    public ArrayList<Map.Entry<String, V>> prefixMatch(String prefix) {
        ArrayList<Map.Entry<String, V>> results = new ArrayList<>();
        Node<V> current = this.root;
        for (int i = 0; i < prefix.length(); i += 1) {
            char ch = prefix.charAt(i);
            Node<V> node = current.children.get(ch);
            if (node == null) {
                return results;
            }
            current = node;
        }
        collectEntries(new StringBuilder(prefix), results, current);
        return results;
    }

    private boolean remove(Node<V> current, String word, int index) {
        if (index == word.length()) {
            if (current.value == null) {
                return false;
            }
            current.value = null;
            return current.children.isEmpty();
        }
        char ch = word.charAt(index);
        Node<V> node = current.children.get(ch);
        if (node == null) {
            return false;
        }
        boolean shouldDeleteCurrentNode = remove(node, word, index + 1);
        if (shouldDeleteCurrentNode) {
            current.children.remove(ch);
            return current.children.isEmpty() && current.value == null;
        }
        return false;
    }

    private void collectEntries(StringBuilder prefix, ArrayList<Map.Entry<String, V>> results, Node<V> current) {
        if (current.value != null) {
            results.add(new AbstractMap.SimpleEntry<>(prefix.toString(), current.value));
        }
        for (Character ch : current.children.keySet()) {
            prefix.append(ch);
            collectEntries(prefix, results, current.children.get(ch));
            prefix.deleteCharAt(prefix.length() - 1);
        }
    }
}
```

```java
import java.util.HashMap;
import java.util.ArrayList;

public class TrieSet {
    private static class Node {
        private boolean isEndOfWord;
        private HashMap<Character, Node> children;

        private Node() {
            this.isEndOfWord = false;
            this.children = new HashMap<>();
        }
    }

    private Node root;
    private int size;

    public TrieSet() {
        this.root = new Node();
        this.size = 0;
    }

    public int size() {
        return this.size;
    }

    public boolean contains(String word) {
        Node current = this.root;
        for (int i = 0; i < word.length(); i += 1) {
            char ch = word.charAt(i);
            Node node = current.children.get(ch);
            if (node == null) {
                return false;
            }
            current = node;
        }
        return current.isEndOfWord;
    }

    public void add(String word) {
        Node current = this.root;
        for (int i = 0; i < word.length(); i += 1) {
            char ch = word.charAt(i);
            Node node = current.children.get(ch);
            if (node == null) {
                node = new Node();
                current.children.put(ch, node);
            }
            current = node;
        }
        if (!current.isEndOfWord) {
            this.size += 1;
        }
        current.isEndOfWord = true;
    }

    public void remove(String word) {
        if (remove(this.root, word, 0)) {
            this.size -= 1;
        }
    }

    public ArrayList<String> prefixMatch(String prefix) {
        ArrayList<String> results = new ArrayList<>();
        Node current = this.root;
        for (int i = 0; i < prefix.length(); i += 1) {
            char ch = prefix.charAt(i);
            Node node = current.children.get(ch);
            if (node == null) {
                return results;
            }
            current = node;
        }
        collectWords(new StringBuilder(prefix), results, current);
        return results;
    }

    private boolean remove(Node current, String word, int index) {
        if (index == word.length()) {
            if (!current.isEndOfWord) {
                return false;
            }
            current.isEndOfWord = false;
            return current.children.isEmpty();
        }
        char ch = word.charAt(index);
        Node node = current.children.get(ch);
        if (node == null) {
            return false;
        }
        boolean shouldDeleteCurrentNode = remove(node, word, index + 1);
        if (shouldDeleteCurrentNode) {
            current.children.remove(ch);
            return current.children.isEmpty() && !current.isEndOfWord;
        }
        return false;
    }

    private void collectWords(StringBuilder prefix, ArrayList<String> results, Node current) {
        if (current.isEndOfWord) {
            results.add(prefix.toString());
        }
        for (Character ch : current.children.keySet()) {
            prefix.append(ch);
            collectWords(prefix, results, current.children.get(ch));
            prefix.deleteCharAt(prefix.length() - 1);
        }
    }
}
```
