# Topic 14: K-way merge
 

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

This pattern helps us solve problems that involve a list of sorted arrays.

Whenever we are given ‘K’ sorted arrays, we can use a Heap to efficiently perform a sorted traversal of all the elements of all arrays. We can push the smallest (first) element of each sorted array in a Min Heap to get the overall minimum. While inserting elements to the Min Heap we keep track of which array the element came from. We can, then, remove the top element from the heap to get the smallest element and push the next element from the same array, to which this smallest element belonged, to the heap. We can repeat this process to make a sorted traversal of all elements.

### Question 1: Merge K sorted Lists
Given an array of ‘K’ sorted LinkedLists, merge them into one sorted list.
 - Example 1
 ```sh
Input: L1=[2, 6, 8], L2=[3, 6, 7], L3=[1, 3, 4]
Output: [1, 2, 3, 3, 4, 6, 6, 7, 8]
 ```
  - Example 2
 ```sh
Input: L1=[5, 8, 9], L2=[1, 7]
Output: [1, 5, 7, 8, 9]
 ```

A brute force solution could be to add all elements of the given ‘K’ lists to one list and sort it. If there are a total of ‘N’ elements in all the input lists, then the brute force solution will have a time complexity of O(N*logN)O(N∗logN) as we will need to sort the merged list. Can we do better than this? How can we utilize the fact that the input lists are individually sorted?

If we have to find the smallest element of all the input lists, we have to compare only the smallest (i.e. the first) element of all the lists. Once we have the smallest element, we can put it in the merged list. Following a similar pattern, we can then find the next smallest element of all the lists to add it to the merged list.

The best data structure that comes to mind to find the smallest number among a set of ‘K’ numbers is a Heap. Let’s see how can we use a heap to find a better algorithm.

We can insert the first element of each array in a Min Heap.
After this, we can take out the smallest (top) element from the heap and add it to the merged list.
After removing the smallest element from the heap, we can insert the next element of the same list into the heap.
We can repeat steps 2 and 3 to populate the merged list in sorted order.

 
### Solution:
 - C++ Solution:
```sh
class ListNode{
public:
   int value = 0;
   ListNode* next;
   ListNode(int value){
       this->value = value;
       this->next = nullptr;
   }
};
Class solution{
public:
    struct cmp{
        bool operator()(ListNode* a, ListNode* b){
            return a->value > b->value;
        }
    }
    static ListNode* merge(const vector<ListNode*> &lists){
        priority_queue<ListNode*,vector<ListNode*>,cmp> pq;
        for(auto l:lists){
            if(l != nullptr){
                pq.push(l);
            }
        }
        ListNode* head = nullptr;
        ListNode* cur = nullptr;
        while(!pq.empty()){
            if(head == nullptr){
                head = cur = pq.top();
            }
            else{
                cur->next= pq.top();
                cur = cur->next;
            }
            if(pq.top()->next != nullptr){
                pq.push(pq.top()->next);
            }
            pq.pop();
        }
        return head;
    }
};
```
  - Java Solution:
 ```sh
class ListNode {
  int value;
  ListNode next;

  ListNode(int value) {
    this.value = value;
  }
}

class MergeKSortedLists {

  public static ListNode merge(ListNode[] lists) {
    PriorityQueue<ListNode> minHeap = new PriorityQueue<ListNode>((n1, n2) -> n1.value - n2.value);

    // put the root of each list in the min heap
    for (ListNode root : lists)
      if (root != null)
        minHeap.add(root);

    // take the smallest (top) element form the min-heap and add it to the result; 
    // if the top element has a next element add it to the heap
    ListNode resultHead = null, resultTail = null;
    while (!minHeap.isEmpty()) {
      ListNode node = minHeap.poll();
      if (resultHead == null) {
        resultHead = resultTail = node;
      } else {
        resultTail.next = node;
        resultTail = resultTail.next;
      }
      if (node.next != null)
        minHeap.add(node.next);
    }

    return resultHead;
  }
```
Time complexity #
O(N∗logK)
Space complexity #
O(K)

### Question 2 Kth Smallest Number in M sorted Lists

Given ‘M’ sorted arrays, find the K’th smallest number among all the arrays.

 - Example 1
 ```sh
Input: L1=[2, 6, 8], L2=[3, 6, 7], L3=[1, 3, 4], K=5
Output: 4
Explanation: The 5th smallest number among all the arrays is 4, this can be verified from the merged 
list of all the arrays: [1, 2, 3, 3, 4, 6, 6, 7, 8]
 ```
  - Example 2
 ```sh
Input: L1=[5, 8, 9], L2=[1, 7], K=3
Output: 7
Explanation: The 3rd smallest number among all the arrays is 7.
 ```

This problem follows the K-way merge pattern and we can follow a similar approach as discussed in Merge K Sorted Lists.

We can start merging all the arrays, but instead of inserting numbers into a merged list, we will keep count to see how many elements have been inserted in the merged list. Once that count is equal to ‘K’, we have found our required number.

A big difference from Merge K Sorted Lists is that in this problem, the input is a list of arrays compared to LinkedLists. This means that when we want to push the next number in the heap we need to know what the index of the current number in the current array was. To handle this, we will need to keep track of the array and the element indices.

### Solution:
- C++ Solution:
```sh
Class solution{
public:
  struct cmp{
      bool operator()(pair<int,pair<int,int>>a, pair<int,pair<int,int>> b){
          a.first > b.first;
      }
  }
  static int findKthSmallest(const vector<vector<int>>& lists, int k) {
    priority_queue<pair<int,pair<int,int>>> pq;
    for(int i = 0;i < lists.size();i++){
        pq.push_back(make_pair(lists[i][0],make_pair(i,0)));
    }
    
    int result = 0;
    int count = 0;
    while(!pq.empty()){
        count++;
        auto tmp = pq.top();
        pq.pop();
        result = tmp.first;
        if(count == k){
            break;
        }
        if(tmp.second.second < lists[tmp.second.first].size() - 1){
            pq.push(make_pair(list[tmp.second.first][++tmp.second.second],make_pair(tmp.second.first,tmp.second.second)));
        }
        return result;
    }
};
```
  - Java Solution:
 ```sh
class Node {
  int elementIndex;
  int arrayIndex;

  Node(int elementIndex, int arrayIndex) {
    this.elementIndex = elementIndex;
    this.arrayIndex = arrayIndex;
  }
}

class KthSmallestInMSortedArrays {

  public static int findKthSmallest(List<Integer[]> lists, int k) {
    PriorityQueue<Node> minHeap = new PriorityQueue<Node>(
        (n1, n2) -> lists.get(n1.arrayIndex)[n1.elementIndex] - lists.get(n2.arrayIndex)[n2.elementIndex]);

    // put the 1st element of each array in the min heap
    for (int i = 0; i < lists.size(); i++)
      if (lists.get(i) != null)
        minHeap.add(new Node(0, i));

    // take the smallest (top) element form the min heap, if the running count is equal to k return the number
    // if the array of the top element has more elements, add the next element to the heap
    int numberCount = 0, result = 0;
    while (!minHeap.isEmpty()) {
      Node node = minHeap.poll();
      result = lists.get(node.arrayIndex)[node.elementIndex];
      if (++numberCount == k)
        break;
      node.elementIndex++;
      if (lists.get(node.arrayIndex).length > node.elementIndex)
        minHeap.add(node);
    }
    return result;
  }
```
Time Complexity: O(N*log(n))
Space Complexity: O(k)

### Question 3 Kth Smallest Number in a Sorted Matrix

Given an N * NN∗N matrix where each row and column is sorted in ascending order, find the Kth smallest element in the matrix.

 - Example 1
 ```sh
Input: Matrix=[
    [2, 6, 8], 
    [3, 7, 10],
    [5, 8, 11]
  ], 
  K=5
Output: 7
Explanation: The 5th smallest number in the matrix is 7.
 ```
This problem follows the K-way merge pattern and can be easily converted to Kth Smallest Number in M Sorted Lists. As each row (or column) of the given matrix can be seen as a sorted list, we essentially need to find the Kth smallest number in ‘N’ sorted lists.
### Solution:
- C++ Solution:
```sh
 public:
  struct numCompare {
    bool operator()(const pair<int, pair<int, int>> &x, const pair<int, pair<int, int>> &y) {
      return x.first > y.first;
    }
  };

  static int findKthSmallest(vector<vector<int>> &matrix, int k) {
    int n = matrix.size();
    priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, numCompare>
        minHeap;

    // put the 1st element of each row in the min heap
    // we don't need to push more than 'k' elements in the heap
    for (int i = 0; i < n && i < k; i++) {
      minHeap.push(make_pair(matrix[i][0], make_pair(i, 0)));
    }

    // take the smallest element form the min heap, if the running count is equal to k return the
    // number if the row of the top element has more elements, add the next element to the heap
    int numberCount = 0, result = 0;
    while (!minHeap.empty()) {
      auto heapTop = minHeap.top();
      minHeap.pop();
      result = heapTop.first;
      if (++numberCount == k) {
        break;
      }

      heapTop.second.second++;
      if (n > heapTop.second.second) {
        heapTop.first = matrix[heapTop.second.first][heapTop.second.second];
        minHeap.push(heapTop);
      }
    }
    return result;
  }
};
```
  - Java Solution:
 ```sh

class Node {
  int row;
  int col;

  Node(int row, int col) {
    this.row = row;
    this.col = col;
  }
}

class KthSmallestInSortedMatrix {

  public static int findKthSmallest(int[][] matrix, int k) {
    PriorityQueue<Node> minHeap = new PriorityQueue<Node>((n1, n2) -> matrix[n1.row][n1.col] - matrix[n2.row][n2.col]);

    // put the 1st element of each row in the min heap
    // we don't need to push more than 'k' elements in the heap
    for (int i = 0; i < matrix.length && i < k; i++)
      minHeap.add(new Node(i, 0));

    // take the smallest (top) element form the min heap, if the running count is equal to k return the number
    // if the row of the top element has more elements, add the next element to the heap
    int numberCount = 0, result = 0;
    while (!minHeap.isEmpty()) {
      Node node = minHeap.poll();
      result = matrix[node.row][node.col];
      if (++numberCount == k)
        break;
      node.col++;
      if (matrix[0].length > node.col)
        minHeap.add(node);
    }
    return result;
  }
```
 - Time Complexity
  O(Nlogk)
 - Space Complexity
  O(k)

### Question 4 Smallest Number Range?

Given ‘M’ sorted arrays, find the smallest range that includes at least one number from each of the ‘M’ lists.

 - Example 1
 ```sh
Input: L1=[1, 5, 8], L2=[4, 12], L3=[7, 8, 10]
Output: [4, 7]
Explanation: The range [4, 7] includes 5 from L1, 4 from L2 and 7 from L3.
 ```
  - Example 2
 ```sh
Input: L1=[1, 9], L2=[4, 12], L3=[7, 10, 16]
Output: [9, 12]
Explanation: The range [9, 12] includes 9 from L1, 12 from L2 and 10 from L3.
 ```
This problem follows the K-way merge pattern and we can follow a similar approach as discussed in Merge K Sorted Lists.

We can start by inserting the first number from all the arrays in a min-heap. We will keep track of the largest number that we have inserted in the heap (let’s call it currentMaxNumber).

In a loop, we’ll take the smallest (top) element from the min-heap andcurrentMaxNumber has the largest element that we inserted in the heap. If these two numbers give us a smaller range, we’ll update our range. Finally, if the array of the top element has more elements, we’ll insert the next element to the heap.

We can finish searching the minimum range as soon as an array is completed or, in other terms, the heap has less than ‘M’ elements.

### Solution:
 - C++ Solution:
```sh
class SmallestRange {
 public:
  struct valueCompare {
    bool operator()(const pair<int, pair<int, int>> &x, const pair<int, pair<int, int>> &y) {
      return x.first > y.first;
    }
  };

  static pair<int, int> findSmallestRange(const vector<vector<int>> &lists) {
    // we will store the actual number, list index and element index in the heap
    priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, valueCompare>
        minHeap;

    int rangeStart = 0, rangeEnd = numeric_limits<int>::max(),
        currentMaxNumber = numeric_limits<int>::min();
    // put the 1st element of each array in the min heap
    for (int i = 0; i < lists.size(); i++) {
      if (!lists[i].empty()) {
        minHeap.push(make_pair(lists[i][0], make_pair(i, 0)));
        currentMaxNumber = max(currentMaxNumber, lists[i][0]);
      }
    }

    // take the smallest (top) element form the min heap, if it gives us smaller range, update the
    // ranges if the array of the top element has more elements, insert the next element in the heap
    while (minHeap.size() == lists.size()) {
      auto node = minHeap.top();
      minHeap.pop();
      if (rangeEnd - rangeStart > currentMaxNumber - node.first) {
        rangeStart = node.first;
        rangeEnd = currentMaxNumber;
      }
      node.second.second++;
      if (lists[node.second.first].size() > node.second.second) {
        node.first = lists[node.second.first][node.second.second];
        minHeap.push(node);  // insert the next element in the heap
        currentMaxNumber = max(currentMaxNumber, node.first);
      }
    }

    return make_pair(rangeStart, rangeEnd);
  }
};
```
  - Java Solution:
 ```sh

class SmallestRange {

  public static int[] findSmallestRange(List<Integer[]> lists) {
    PriorityQueue<Node> minHeap = new PriorityQueue<Node>(
        (n1, n2) -> lists.get(n1.arrayIndex)[n1.elementIndex] - lists.get(n2.arrayIndex)[n2.elementIndex]);

    int rangeStart = 0, rangeEnd = Integer.MAX_VALUE, currentMaxNumber = Integer.MIN_VALUE;
    // put the 1st element of each array in the min heap
    for (int i = 0; i < lists.size(); i++)
      if (lists.get(i) != null) {
        minHeap.add(new Node(0, i));
        currentMaxNumber = Math.max(currentMaxNumber, lists.get(i)[0]);
      }

    // take the smallest (top) element form the min heap, if it gives us smaller range, update the ranges
    // if the array of the top element has more elements, insert the next element in the heap
    while (minHeap.size() == lists.size()) {
      Node node = minHeap.poll();
      if (rangeEnd - rangeStart > currentMaxNumber - lists.get(node.arrayIndex)[node.elementIndex]) {
        rangeStart = lists.get(node.arrayIndex)[node.elementIndex];
        rangeEnd = currentMaxNumber;
      }
      node.elementIndex++;
      if (lists.get(node.arrayIndex).length > node.elementIndex) {
        minHeap.add(node); // insert the next element in the heap
        currentMaxNumber = Math.max(currentMaxNumber, lists.get(node.arrayIndex)[node.elementIndex]);
      }
    }
    return new int[] { rangeStart, rangeEnd };
  }
```
 - Time Complexity
   O(n*logn)
 - Space Complexity
O(n)
### Question 5 Top 'K' Pairs with Largest Sums
Given two sorted arrays in descending order, find ‘K’ pairs with the largest sum where each pair consists of numbers from both the arrays.
 - Example 1
 ```sh
Input: L1=[9, 8, 2], L2=[6, 3, 1], K=3
Output: [9, 3], [9, 6], [8, 6] 
Explanation: These 3 pairs have the largest sum. No other pair has a sum larger than any of these.
 ```
  - Example 2
 ```sh
Input: L1=[5, 2, 1], L2=[2, -1], K=3
Output: [5, 2], [5, -1], [2, 2] 
 ```
This problem follows the K-way merge pattern and we can follow a similar approach as discussed in Merge K Sorted Lists.

We can go through all the numbers of the two input arrays to create pairs and initially insert them all in the heap until we have ‘K’ pairs in Min Heap. After that, if a pair is bigger than the top (smallest) pair in the heap, we can remove the smallest pair and insert this pair in the heap.

We can optimize our algorithms in two ways:

Instead of iterating over all the numbers of both arrays, we can iterate only the first ‘K’ numbers from both arrays. Since the arrays are sorted in descending order, the pairs with the maximum sum will be constituted by the first ‘K’ numbers from both the arrays.
As soon as we encounter a pair with a sum that is smaller than the smallest (top) element of the heap, we don’t need to process the next elements of the array. Since the arrays are sorted in descending order, we won’t be able to find a pair with a higher sum moving forward.
### Solution:
 - C++ Solution:
```sh
 public:
  struct sumCompare {
    bool operator()(const pair<int, int> &x, const pair<int, int> &y) {
      return x.first + x.second > y.first + y.second;
    }
  };

  static vector<pair<int, int>> findKLargestPairs(const vector<int> &nums1,
                                                  const vector<int> &nums2, int k) {
    vector<pair<int, int>> minHeap;
    for (int i = 0; i < nums1.size() && i < k; i++) {
      for (int j = 0; j < nums2.size() && j < k; j++) {
        if (minHeap.size() < k) {
          minHeap.push_back(make_pair(nums1[i], nums2[j]));
          push_heap(minHeap.begin(), minHeap.end(), sumCompare());
        } else {
          // if the sum of the two numbers from the two arrays is smaller than the smallest (top)
          // element of the heap, we can 'break' here. Since the arrays are sorted in the descending
          // order, we'll not be able to find a pair with a higher sum moving forward.
          if (nums1[i] + nums2[j] < minHeap.front().first + minHeap.front().second) {
            break;
          } else {  // else: we have a pair with a larger sum, remove top and insert this pair in
                    // the heap
            pop_heap(minHeap.begin(), minHeap.end(), sumCompare());
            minHeap.pop_back();
            minHeap.push_back(make_pair(nums1[i], nums2[j]));
            push_heap(minHeap.begin(), minHeap.end(), sumCompare());
          }
        }
      }
    }
    return minHeap;
  }
};
```

  - Java Solution:
 ```sh

class LargestPairs {

  public static List<int[]> findKLargestPairs(int[] nums1, int[] nums2, int k) {
    PriorityQueue<int[]> minHeap = new PriorityQueue<int[]>((p1, p2) -> (p1[0] + p1[1]) - (p2[0] + p2[1]));
    for (int i = 0; i < nums1.length && i < k; i++) {
      for (int j = 0; j < nums2.length && j < k; j++) {
        if (minHeap.size() < k) {
          minHeap.add(new int[] { nums1[i], nums2[j] });
        } else {
          // if the sum of the two numbers from the two arrays is smaller than the smallest (top) element of
          // the heap, we can 'break' here. Since the arrays are sorted in the descending order, we'll not be
          // able to find a pair with a higher sum moving forward.
          if (nums1[i] + nums2[j] < minHeap.peek()[0] + minHeap.peek()[1]) {
            break;
          } else { // else: we have a pair with a larger sum, remove top and insert this pair in the heap
            minHeap.poll();
            minHeap.add(new int[] { nums1[i], nums2[j] });
          }
        }
      }
    }
    return new ArrayList<>(minHeap);
  }
```
 - Time Complexity
   O(N + NlogK)
 - Space Complexity
O(N)
### Todos

 - Write MORE Tests
 - Add Night Mode

License
----

MIT
