# Sort

## Insertion Sort

```
insertionSort(A, N)
    for i = 1 to N - 1
        v = A[i]
        j = i - 1
        while j >= 0 && A[j] > v
            A[j + 1] = v
            j--
        A[j + 1] = v
```

- Insertion sort is stable
- Time complexity is O(N^2)

## Bubble Sort

```
bubbleSort(A, N)
    flag = 1
    while flag
        flag = 0
        for j = N - 1 to 1
            if A[j] < A[j - 1]
                swap(A[j], A[j - 1])
                flag = 1
```

- Bubble sort is stable. But if we replace A[j] < A[j - 1] to A[j] <= A[j - 1], the algorithm is unstable
- Time complexity is O(N^2)

## Selection Sort

```
It is very straightforward. It will select a minimum value at every step.
selectionSort(A, N)
    for i = 0 to N - 1
        minj = i
        for j : i to N - 1
            if A[j] < A[minj]
                minj = j
        A[i]与A【minj]交换
```
- Time Complexity is O(n^2)
- Unstable



## Shell Sort

```
insertionSort(A, n, g)
    for i = g to n - 1
        v = A[i]
        j = i - g
        while j >= 0 && A[j] > v
            A[j + g] = A[j]
            j = j - g
            cnt++
        A[j + g] = v
shellSort(A, n)
    cnt = 0
    m = ?
    G[] = {}
    for i = 0 to m - 1
        insertionSort(A, n, G[i])
```
