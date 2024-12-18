# Sorting algorithms

## Overview

## Bubble sort

## Selection sort

## Insertion sort

## Merge sort

- Time complexity: $O(n \log n)$
- Space complexity: $O(n)$

```java
public class Mergesort {
    public void mergeSort(int[] array, int left, int right) {
        if (left >= right) {
            return;
        }

        int mid = Math.floorDiv(left + right, 2);
        mergeSort(array, left, mid);
        mergeSort(array, mid + 1, right);
        merge(array, left, mid, right);
    }

    private void merge(int[] array, int left, int mid, int right) {
        int leftSubarrayLen = mid - left + 1;
        int rightSubarrayLen = right - mid;

        int[] leftSubArray = new int[leftSubarrayLen];
        int[] rightSubArray = new int[rightSubarrayLen];

        for (int i = 0; i < leftSubarrayLen; i += 1) {
            leftSubArray[i] = array[left + i];
        }
        for (int j = 0; j < rightSubarrayLen; j += 1) {
            rightSubArray[j] = array[mid + j + 1];
        }

        int i = 0;
        int j = 0;
        int k = left;

        while (i < leftSubarrayLen && j < rightSubarrayLen) {
            if (leftSubArray[i] <= rightSubArray[j]) {
                array[k] = leftSubArray[i];
                i += 1;
            } else {
                array[k] = rightSubArray[j];
                j += 1;
            }
            k += 1;
        }

        while (i < leftSubarrayLen) {
            array[k] = leftSubArray[i];
            i += 1;
            k += 1;
        }
        while (j < rightSubarrayLen) {
            array[k] = rightSubArray[j];
            j += 1;
            k += 1;
        }
    }
}
```

# Quicksort

- Time complexity: $O(n \log n)$
- Space complexity: $O(n)$

```java
public class QuickSort {
    public static void quickSort(int[] array, int low, int high) {
        if (low < high) {
            int pivot = partition(array, low, high);
            quickSort(array, low, pivot - 1);
            quickSort(array, pivot + 1, high);
        }
    }

    public static int partition(int[] array, int low, int high) {
        int pivot = array[high];
        int i = low - 1;

        for (int j = low; j < high; j += 1) {
            if (array[j] < pivot) {
                i += 1;
                swap(array, i, j);
            }
        }

        swap(array, i + 1, high);

        return i + 1;
    }

    private static void swap(int[] array, int i, int j) {
        int temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
}
```

## Heapsort

