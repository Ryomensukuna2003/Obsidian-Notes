1. **Size of the dataset:**
    
    - For small datasets (e.g., less than 10 elements) --> Insertion Sort or Selection Sort (due to their lower overhead)
    - For medium-sized datasets --> Merge Sort or Quick Sort
    - For very large datasets --> Heap sort or Merge sort
2. **Presorted or partially sorted data:**
    
    - If the data is partially or nearly sorted, Insertion Sort or Bubble Sort may perform well, as they have adaptive characteristics.
    - If the data is mostly random, Quick Sort or Merge Sort might be more suitable.
3. **Memory constraints:**
    
    - If memory is limited then In-place sorting algorithms like Quick Sort or Heap Sort can be preferable.
4. **Data characteristics:**
    
    - For data with a large number of duplicate values, algorithms like Radix Sort or Counting Sort can be efficient.
5. **Worst-case time complexity:**
    
    - Quick Sort has a worst-case time complexity of O(n2) but is often faster for random data due to its good average-case performance. Merge Sort has a consistent O(n log n) time complexity but may have higher constant factors.

## **Heap sort**

Time Complexity --> O(n log n)
Space Complexity --> O(1)

## Merge sort

Time Complexity --> O(n log n)
Space Complexity --> O(n)

## **Quick sort (Not for large Dataset)**

Time Complexity --> O(n^2) [Average --> O(n log n)]
Space Complexity --> O(1)

## **Selection** sort

Time Complexity --> O(n^2)
Space Complexity --> O(1)

## Insertion Sort

Time Complexity --> O(n^2)
Space Complexity --> O(1)


**Stable Sorting Algorithms** 
1. Insertion Sort
2. Bubble Sort
3. Merge Sort
4. Counting Sort
5. Bucket Sort (with a stable sorting algorithm used within each bucket)
6. Radix Sort


**Unstable Sorting Algorithms**
1. Selection Sort
3. Quick Sort (standard implementation)
4. Heap Sort
5. Bucket Sort (with an unstable sorting algorithm used within each bucket)