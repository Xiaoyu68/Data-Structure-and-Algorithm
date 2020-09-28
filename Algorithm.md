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

# Stack

## Implement stack using array

```
initialize()
    top  = 0

isEmpty()
    return top == 0

isFull()
    return top >= MAX - 1
push(x)
    if isFull()
        错误（上溢）
    top++
    S[top] = x
pop()
    if isEmpty()
        错误（下溢）
    top--
    return S[top + 1]
```
# Queue

## Implement queue using array

```
initialize()
    head = tail = 0

isEmpty()
    return head == tail

isFull()
    return head == (tail + 1)%MAX

enqueue(x)
    if isFull()
        错误（上溢）
    Q[tail] = x
    if tail + 1 == MAX
        tail = 0
    else
        tail++

dequeue()
    if isEmpty()
        错误（下溢）
    x = Q[head]
    if head + 1 == MAX
        head = 0
    else
        head++
    return x;
```
# Search
## Linear Search

向线性搜索中引入“标记”可以将算法效率提高常数倍。所谓标记，就是我们在数组等数据结构中设置的一个拥有特殊值的元素。借助这项编程技巧，我们能达到简化循环控制等诸多目的。在线性搜索中，我们可以把含有目标关键字的数据放在数组末尾，用做标记。

```
Method 1:
linearSearch()
    for i = 0 to n -1
        if A[i] 与key相等
            return i
    return NOT_FOUND

Method2:
linearSearch()
    i = 0
    A[n] = key
    while A[i] 与 key 不同
        i++
    if i == n
        return NOT_FOUND
    return i
```
方法1和方法2区别主要在于主循环中比较运算的次数。方法1需要两个比较运算，一个是for循环的结束条件，另一个是关键字之间的比较。相对地，方法2只需要比较一个。由于标记能确保while不成为死循环，因此可以省去循环结束条件。
- Time Complexity: O(n)
## Binary Search

```
Binary search steps:
    1. The whole array is the searching area
    2. Check the middle element
    3. If mid == key, then end the search
    4. If mid < key, left = mid + 1;
        if mid > key, right = mid - 1;
After finished each step, the time will be halved
```
```
binarySearch(A, key)
    left = 0
    right = n
    while left < right
        mid = (left + right)/2
        if A[mid] == key
            return mid
        else if key < A[mid]
            right = end
        else
            left = mid + 1
    return NOT_FOUND
```
## Dictionary

散列法是一种搜素算法，它可以根据各元素的值来确定存储位置，然后将位置保管在散列表中，从而实现数据的高速搜索。其中散列表是一种数据结构，能对包含关键字的数据集合高效地执行动态插入、搜索、删除操作。虽然链表也能完成同样操作、但搜索和删除的复杂度都高达O(n)

```
insert(data)
    T[h(data.key)] = data

search(data)
    return T[h(data.key)]

h(k) = k mod m
```
为解决不同key对应同一散列值的情况，开放地址法是常用手段之一。在双散列结构中一旦出现冲突，程序会调用第二个散列函数来求散列值。
H(k) = h(k,i) = (h1(k) + i*h2(k)) mod m
散列函数h(k,i)拥有关键字k和整数i两个参数。

```
h1(key)
    return key mod m

h2(key)
    return 1 + (key mod (m - 1))

h(key, i)
    return (h1(key) + i * h2(key)) mod m

insert(T, key)
    i = 0
    while true
        j = h(key, i)
        if T[j] == NIL
            T[j] = key
            return j
        else 
            i = i + 1
search(T, key)
    i = 0
    while true
        j = h(key, i)
        if T[j] == key
            return j
        else if T[j] == NIL or i >= m
            return NIL
        else 
            i = i + 1
```
- Time Complexity
    O(1)

