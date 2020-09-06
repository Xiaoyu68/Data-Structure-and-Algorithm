# Topic 13: Top 'K' Elements
 

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Any problem that asks us to find the top/smallest/frequent ‘K’ elements among a given set falls under this pattern.

The best data structure that comes to mind to keep track of ‘K’ elements is Heap. This pattern will make use of the Heap to solve multiple problems dealing with ‘K’ elements at a time from a set of given elements.

Let’s jump onto our first problem to develop an understanding of this pattern.

### Question 1: Top 'K' Number
Given an unsorted array of numbers, find the ‘K’ largest numbers in it.

Note: For a detailed discussion about different approaches to solve this problem, take a look at Kth Smallest Number.
 - Example 1
 ```sh
Input: [3, 1, 5, 12, 2, 11], K = 3
Output: [5, 12, 11]
 ```
  - Example 2
 ```sh
Input: [5, 12, 11, -1, 12], K = 3
Output: [12, 11, 12]
 ```

A brute force solution could be to sort the array and return the largest K numbers. The time complexity of such an algorithm will be O(N*logN)O(N∗logN) as we need to use a sorting algorithm like Timsort if we use Java’s Collection.sort(). Can we do better than that?

The best data structure that comes to mind to keep track of top ‘K’ elements is Heap. Let’s see if we can use a heap to find a better algorithm.

If we iterate through the array one element at a time and keep ‘K’ largest numbers in a heap such that each time we find a larger number than the smallest number in the heap, we do two things:

Take out the smallest number from the heap, and
Insert the larger number into the heap.
This will ensure that we always have ‘K’ largest numbers in the heap. The most efficient way to repeatedly find the smallest number among a set of numbers will be to use a min-heap. As we know, we can find the smallest number in a min-heap in constant time O(1)O(1), since the smallest number is always at the root of the heap. Extracting the smallest number from a min-heap will take O(logN)O(logN) (if the heap has ‘N’ elements) as the heap needs to readjust after the removal of an element.

Let’s take Example-1 to go through each step of our algorithm:

Given array: [3, 1, 5, 12, 2, 11], and K=3

First, let’s insert ‘K’ elements in the min-heap.
After the insertion, the heap will have three numbers [3, 1, 5] with ‘1’ being the root as it is the smallest element.
We’ll iterate through the remaining numbers and perform the above-mentioned two steps if we find a number larger than the root of the heap.
The 4th number is ‘12’ which is larger than the root (which is ‘1’), so let’s take out ‘1’ and insert ‘12’. Now the heap will have [3, 5, 12] with ‘3’ being the root as it is the smallest element.
The 5th number is ‘2’ which is not bigger than the root of the heap (‘3’), so we can skip this as we already have top three numbers in the heap.
The last number is ‘11’ which is bigger than the root (which is ‘3’), so let’s take out ‘3’ and insert ‘11’. Finally, the heap has the largest three numbers: [5, 12, 11]
As discussed above, it will take us O(logK)O(logK) to extract the minimum number from the min-heap. So the overall time complexity of our algorithm will be O(K*logK+(N-K)*logK)O(K∗logK+(N−K)∗logK) since, first, we insert ‘K’ numbers in the heap and then iterate through the remaining numbers and at every step, in the worst case, we need to extract the minimum number and insert a new number in the heap. This algorithm is better than O(N*logN)O(N∗logN).

 
### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    struct greater{
        bool operator()(const int& a, const int& b){
            return a > b;
        }
    }
    static vector<int> findKLargestNumbers(const vector<int> &nums, int k){
        vector<int> minHeap(nums.begin(), nums.begin() + k);
        make_heap(minHeap.begin(), minHeap.end(), greater());
        for(int i = k;i < nums.size();i++){
            if(nums[i] > minHeap.front()){
                pop_heap(minHeap.begin(), minHeap.end(),greater());
                minHeap.pop_back();
                minHeap.push_back(nums[i]);
                push_heap(minHeap.begin(), minHeap.end(), greater());
            }
        }
        return minHeap;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static List<Integer> findKLargesNumbers(int[] nums, int k){
        PriorityQueue<Integer> minHeap = new PriorityQueue<Integer>((a,b)->a-b);
        for(int i = 0;i < k;i++){
            minHeap.add(nums[i]);
        }
        for(int i = k;i < nums.size();i++){
            if(nums[i] > minHeap.peek()){
                minHeap.poll();
                minHeap.add(nums[i]);
            }
        }
        return new ArrayList<>(minHeap);
    }
};
```
Time complexity #
O(N∗logK)
Space complexity #
O(K)

### Question 2 Kth Smallest Number

In a non-empty array of integers, every number appears twice except for one, find that single number.

 - Example 1
 ```sh
Input: [1, 5, 12, 2, 11, 5], K = 3
Output: 5
Explanation: The 3rd smallest number is '5', as the first two smaller numbers are [1, 2].
 ```
  - Example 2
 ```sh
Input: [1, 5, 12, 2, 11, 5], K = 4
Output: 5
Explanation: The 4th smallest number is '5', as the first three small numbers are [1, 2, 5].
 ```
   - Example 3
 ```sh
Input: [5, 12, 11, -1, 12], K = 3
Output: 11
Explanation: The 3rd smallest number is '11', as the first two small numbers are [5, -1].
 ```
This problem follows the Top ‘K’ Numbers pattern but has two differences:

Here we need to find the Kth smallest number, whereas in Top ‘K’ Numbers we were dealing with ‘K’ largest numbers.
In this problem, we need to find only one number (Kth smallest) compared to finding all ‘K’ largest numbers.
We can follow the same approach as discussed in the ‘Top K Elements’ problem. To handle the first difference mentioned above, we can use a max-heap instead of a min-heap. As we know, the root is the biggest element in the max heap. So, since we want to keep track of the ‘K’ smallest numbers, we can compare every number with the root while iterating through all numbers, and if it is smaller than the root, we’ll take the root out and insert the smaller number.

### Solution:
- C++ Solution:
```sh
Class solution{
public:
  static int findKthSmallestNumber(const vector<int>& nums, int k) {
    priority_queue<int> pq;
    for(int i = 0;i < k;i++){
        pq.push(nums[i]);
    }
    for(int i = k;i < nums.size();i++){
        if(nums[k] < pq.top()){
            pq.pop();
            pq.push(nums[k]);
        }
    }
    return pq.top();
  }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
  public static int findKthSmallestNumber(int[] nums, int k) {
    PriorityQueue<Integer> pq = new PriorityQueue<>((a,b)->b-a);
    for(int i = 0;i < k;i++){
        pq.add(nums[i]);
    }
    for(int i = k;i < nums.length;i++){
        if(nums[k] < pq.poll()){
            pq.add(nums[k]);
        }
    }
    return pq.peek();
  }
};
```
Time Complexity: O(N*log(n))
Space Complexity: O(k)

### Question 3 "K" Closest Points to the Origin

Given an array of points in the a 2D2D plane, find ‘K’ closest points to the origin.

 - Example 1
 ```sh
Input: points = [[1,2],[1,3]], K = 1
Output: [[1,2]]
Explanation: The Euclidean distance between (1, 2) and the origin is sqrt(5).
The Euclidean distance between (1, 3) and the origin is sqrt(10).
Since sqrt(5) < sqrt(10), therefore (1, 2) is closer to the origin.
 ```
  - Example 2
 ```sh
Input: point = [[1, 3], [3, 4], [2, -1]], K = 2
Output: [[1, 3], [2, -1]]
 ```
The Euclidean distance of a point P(x,y) from the origin can be calculated through the following formula:
sqrt((x^x + y^y)
This problem follows the Top ‘K’ Numbers pattern. The only difference in this problem is that we need to find the closest point (to the origin) as compared to finding the largest numbers.

Following a similar approach, we can use a Max Heap to find ‘K’ points closest to the origin. While iterating through all points, if a point (say ‘P’) is closer to the origin than the top point of the max-heap, we will remove that top point from the heap and add ‘P’ to always keep the closest points in the heap
### Solution:
- C++ Solution:
```sh
class Point {
 public:
  int x = 0;
  int y = 0;

  Point(int x, int y) {
    this->x = x;
    this->y = y;
  }

  int distFromOrigin() const {
    // ignoring sqrt
    return (x * x) + (y * y);
  }

  const bool operator<(const Point& p) { return p.distFromOrigin() > this->distFromOrigin(); }
};

class KClosestPointsToOrigin {
 public:
  static vector<Point> findClosestPoints(const vector<Point>& points, int k) {
    // put first 'k' points in the vector
    vector<Point> maxHeap(points.begin(), points.begin() + k);
    make_heap(maxHeap.begin(), maxHeap.end());

    // go through the remaining points of the input array, if a point is closer to the origin than
    // the top point of the max-heap, remove the top point from heap and add the point from the
    // input array
    for (int i = k; i < points.size(); i++) {
      if (points[i].distFromOrigin() < maxHeap.front().distFromOrigin()) {
        pop_heap(maxHeap.begin(), maxHeap.end());
        maxHeap.pop_back();
        maxHeap.push_back(points[i]);
        push_heap(maxHeap.begin(), maxHeap.end());
      }
    }

    // the heap has 'k' points closest to the origin
    return maxHeap;
  }
};
```
  - Java Solution:
 ```sh
class Point {
  int x;
  int y;

  public Point(int x, int y) {
    this.x = x;
    this.y = y;
  }

  public int distFromOrigin() {
    // ignoring sqrt
    return (x * x) + (y * y);
  }
}

class KClosestPointsToOrigin {

  public static List<Point> findClosestPoints(Point[] points, int k) {
    PriorityQueue<Point> maxHeap = new PriorityQueue<>((p1, p2) -> p2.distFromOrigin() - p1.distFromOrigin());
    // put first 'k' points in the max heap
    for (int i = 0; i < k; i++)
      maxHeap.add(points[i]);

    // go through the remaining points of the input array, if a point is closer to the origin than the top point 
    // of the max-heap, remove the top point from heap and add the point from the input array
    for (int i = k; i < points.length; i++) {
      if (points[i].distFromOrigin() < maxHeap.peek().distFromOrigin()) {
        maxHeap.poll();
        maxHeap.add(points[i]);
      }
    }

    // the heap has 'k' points closest to the origin, return them in a list
    return new ArrayList<>(maxHeap);
  }
```
 - Time Complexity
  O(Nlogk)
 - Space Complexity
  O(k)

### Question 4 Connect Ropes

Given ‘N’ ropes with different lengths, we need to connect these ropes into one big rope with minimum cost. The cost of connecting two ropes is equal to the sum of their lengths.

 - Example 1
 ```sh
Input: [1, 3, 11, 5]
Output: 33
Explanation: First connect 1+3(=4), then 4+5(=9), and then 9+11(=20). So the total cost is 33 (4+9+20)
 ```
  - Example 2
 ```sh
Input: [3, 4, 5, 6]
Output: 36
Explanation: First connect 3+4(=7), then 5+6(=11), 7+11(=18). Total cost is 36 (7+11+18)
 ```
   - Example 3
 ```sh
Input: [1, 3, 11, 5, 2]
Output: 42
Explanation: First connect 1+2(=3), then 3+3(=6), 6+5(=11), 11+11(=22). Total cost is 42 (3+6+11+22)
 ```
In this problem, following a greedy approach to connect the smallest ropes first will ensure the lowest cost. We can use a Min Heap to find the smallest ropes following a similar approach as discussed in Kth Smallest Number. Once we connect two ropes, we need to insert the resultant rope back in the Min Heap so that we can connect it with the remaining ropes.

### Solution:
 - C++ Solution:
```sh
class solution {
 public:
  static int minimumCostToConnectRopes(const vector<int> &ropeLengths) {
    priority_queue<int, vector<int>, greater<int>> minHeap;
    // add all ropes to the min heap
    for (int i = 0; i < ropeLengths.size(); i++) {
      minHeap.push(ropeLengths[i]);
    }

    // go through the values of the heap, in each step take top (lowest) rope lengths from the min
    // heap connect them and push the result back to the min heap. keep doing this until the heap is
    // left with only one rope
    int result = 0, temp = 0;
    while (minHeap.size() > 1) {
      temp = minHeap.top();
      minHeap.pop();
      temp += minHeap.top();
      minHeap.pop();
      result += temp;
      minHeap.push(temp);
    }

    return result;
  }
};
```
  - Java Solution:
 ```sh
Class solution{
  public static int minimumCostToConnectRopes(int[] ropeLengths) {
    PriorityQueue<Integer> minHeap = new PriorityQueue<Integer>((n1, n2) -> n1 - n2);
    // add all ropes to the min heap
    for (int i = 0; i < ropeLengths.length; i++)
      minHeap.add(ropeLengths[i]);

    // go through the values of the heap, in each step take top (lowest) rope lengths from the min heap
    // connect them and push the result back to the min heap. 
    // keep doing this until the heap is left with only one rope
    int result = 0, temp = 0;
    while (minHeap.size() > 1) {
      temp = minHeap.poll() + minHeap.poll();
      result += temp;
      minHeap.add(temp);
    }

    return result;
  }
```
 - Time Complexity
   O(n*logn)
 - Space Complexity
O(n)
### Question 5 Top 'K' Frequent Numbers
Given an unsorted array of numbers, find the top ‘K’ frequently occurring numbers in it.

 - Example 1
 ```sh
Input: [1, 3, 5, 12, 11, 12, 11], K = 2
Output: [12, 11]
Explanation: Both '11' and '12' apeared twice.
 ```
  - Example 2
 ```sh
Input: [5, 12, 11, 3, 11], K = 2
Output: [11, 5] or [11, 12] or [11, 3]
Explanation: Only '11' appeared twice, all other numbers appeared once.
 ```
This problem follows Top ‘K’ Numbers. The only difference is that in this problem, we need to find the most frequently occurring number compared to finding the largest numbers.

We can follow the same approach as discussed in the Top K Elements problem. However, in this problem, we first need to know the frequency of each number, for which we can use a HashMap. Once we have the frequency map, we can use a Min Heap to find the ‘K’ most frequently occurring number. In the Min Heap, instead of comparing numbers we will compare their frequencies in order to get frequently occurring numbers

### Solution:
 - C++ Solution:
```sh
class solution {
 public:
 struct cmp{
     bool operator()(pair<int,int>a,pair<int,int>b){
         a.second > b.second;
     }
 }
  static vector<int> findTopKFrenquentNumber(const vector<int>& nums, int k){
      unordered_map<int, int>mp;
      priority_queue<pair<int,int>,vector<pair<int,int>>,cmp> pq;
      for(int i = 0;i < nums.size();i++){
          mp[nums[i]]++;
      }
      for(auto m:mp){
          pq.push(make_pair(m.first,m.second));
          if(pq.size() > k){
              if(m.second > pq.top()){
                  pq.pop();
              }
          }
      }
      while(!pq.empty()){
          res.push_back(pq.top().first);
      }
      return res;
  }
};
```

  - Java Solution:
 ```sh

class solution {
    public:
  public static List<Integer> findTopKFrequentNumbers(int[] nums, int k) {
    // find the frequency of each number
    Map<Integer, Integer> numFrequencyMap = new HashMap<>();
    for (int n : nums)
      numFrequencyMap.put(n, numFrequencyMap.getOrDefault(n, 0) + 1);

    PriorityQueue<Map.Entry<Integer, Integer>> minHeap = new PriorityQueue<Map.Entry<Integer, Integer>>(
        (e1, e2) -> e1.getValue() - e2.getValue());

    // go through all numbers of the numFrequencyMap and push them in the minHeap, which will have 
    // top k frequent numbers. If the heap size is more than k, we remove the smallest (top) number 
    for (Map.Entry<Integer, Integer> entry : numFrequencyMap.entrySet()) {
      minHeap.add(entry);
      if (minHeap.size() > k) {
        minHeap.poll();
      }
    }

    // create a list of top k numbers
    List<Integer> topNumbers = new ArrayList<>(k);
    while (!minHeap.isEmpty()) {
      topNumbers.add(minHeap.poll().getKey());
    }
    return topNumbers;
  }

```
 - Time Complexity
   O(N + NlogK)
 - Space Complexity
O(N)
### Question 6 Frequency Sort

Given a string, sort it based on the decreasing frequency of its characters.

 - Example 1
 ```sh
Input: "Programming"
Output: "rrggmmPiano"
Explanation: 'r', 'g', and 'm' appeared twice, so they need to appear before any other character.
 ```
  - Example 2
 ```sh
Input: "abcbab"
Output: "bbbaac"
Explanation: 'b' appeared three times, 'a' appeared twice, and 'c' appeared only once.
 ```
This problem follows the Top ‘K’ Elements pattern, and shares similarities with Top ‘K’ Frequent Numbers.

We can follow the same approach as discussed in the Top ‘K’ Frequent Numbers problem. First, we will find the frequencies of all characters, then use a max-heap to find the most occurring characters.

### Solution:
 - C++ Solution:
```sh
class solution {
 public:
  struct cmp{
      bool operator()(pair<char,int> a, pair<char,int> b){
          return a.second < b.second;
      }
  }
  static string sortCharacterByFrequency(const string &str) {
    string res = "";
    priority_queue<pair<char,int>,vector<char, int>,cmp> pq;
    unordered_map<char,int>mp;
    for(int i = 0;i < str.length();i++){
        mp[str[i]]++;
    }
    for(auto m:mp){
        pq.push(make_pair(m.first,m.second));
    }
    while(!pq.empty()){
        auto tmp = pq.top();
        pq.pop();
        for(int i = 0;i < tmp.second;i++){
            res.push_back(tmp.first);
        }
    }
    return res;
  }
};
```
  - Java Solution:
 ```sh
Class solution{
    public static String sortCharacterByFrequency(String str) {
    // find the frequency of each character
    Map<Character, Integer> characterFrequencyMap = new HashMap<>();
    for (char chr : str.toCharArray()) {
      characterFrequencyMap.put(chr, characterFrequencyMap.getOrDefault(chr, 0) + 1);
    }

    PriorityQueue<Map.Entry<Character, Integer>> maxHeap = new PriorityQueue<Map.Entry<Character, Integer>>(
        (e1, e2) -> e2.getValue() - e1.getValue());

    // add all characters to the max heap
    maxHeap.addAll(characterFrequencyMap.entrySet());

    // build a string, appending the most occurring characters first
    StringBuilder sortedString = new StringBuilder(str.length());
    while (!maxHeap.isEmpty()) {
      Map.Entry<Character, Integer> entry = maxHeap.poll();
      for (int i = 0; i < entry.getValue(); i++)
        sortedString.append(entry.getKey());
    }
    return sortedString.toString();
  }
```
 - Time Complexity
   O(n*logn)
 - Space Complexity
O(n)
### Question 7 Kth Largest Number in a Stream

Design a class to efficiently find the Kth largest element in a stream of numbers.

The class should have the following two things:

The constructor of the class should accept an integer array containing initial numbers from the stream and an integer ‘K’.
The class should expose a function add(int num) which will store the given number and return the Kth largest number.

 - Example 1
 ```sh
Input: [3, 1, 5, 12, 2, 11], K = 4
1. Calling add(6) should return '5'.
2. Calling add(13) should return '6'.
2. Calling add(4) should still return '6'.
 ```
This problem follows Top ‘K’ Numbers. The only difference is that in this problem, we need to find the most frequently occurring number compared to finding the largest numbers.

We can follow the same approach as discussed in the Top K Elements problem. However, in this problem, we first need to know the frequency of each number, for which we can use a HashMap. Once we have the frequency map, we can use a Min Heap to find the ‘K’ most frequently occurring number. In the Min Heap, instead of comparing numbers we will compare their frequencies in order to get frequently occurring numbers

### Solution:
 - C++ Solution:
```sh
class solution {
        priority_queue<int,vector<int>,greater<int>> pq;
 public:
        KthLargestNumberInStream(const vector<int>& nums, int k){
            for(int i = 0;i < k;i++){
                pq.push(nums[i]);
            }
            for(int i = k;i < nums.size();i++){
                if(nums[i] > pq.top()){
                    pq.pop();
                    pq.push(nums[i]);
                }
            }
        }
        virtual int add(int num){
            if(num > pq.top()){
                pq.pop();
                pq.push(num);
            }
            return pq.top();
        }
};
```
  - Java Solution:
 ```sh

class KthLargestNumberInStream {
  PriorityQueue<Integer> minHeap = new PriorityQueue<Integer>((n1, n2) -> n1 - n2);
  final int k;

  public KthLargestNumberInStream(int[] nums, int k) {
    this.k = k;
    // add the numbers in the min heap
    for (int i = 0; i < nums.length; i++)
      add(nums[i]);
  }

  public int add(int num) {
    // add the new number in the min heap
    minHeap.add(num);

    // if heap has more than 'k' numbers, remove one number
    if (minHeap.size() > this.k)
      minHeap.poll();

    // return the 'Kth largest number
    return minHeap.peek();
  }

```
 - Time Complexity
   O(logK)
 - Space Complexity
O(k)
### Question 8 'K' Closest Numbers

Given a sorted number array and two integers ‘K’ and ‘X’, find ‘K’ closest numbers to ‘X’ in the array. Return the numbers in the sorted order. ‘X’ is not necessarily present in the array.

 - Example 1
 ```sh
Input: [5, 6, 7, 8, 9], K = 3, X = 7
Output: [6, 7, 8]
 ```
  - Example 2
 ```sh
Input: [2, 4, 5, 6, 9], K = 3, X = 6
Output: [4, 5, 6]
 ```
   - Example 3
 ```sh
Input: [2, 4, 5, 6, 9], K = 3, X = 10
Output: [5, 6, 9]
 ```
This problem follows the Top ‘K’ Numbers pattern. The biggest difference in this problem is that we need to find the closest (to ‘X’) numbers compared to finding the overall largest numbers. Another difference is that the given array is sorted.

Utilizing a similar approach, we can find the numbers closest to ‘X’ through the following algorithm:

Since the array is sorted, we can first find the number closest to ‘X’ through Binary Search. Let’s say that number is ‘Y’.
The ‘K’ closest numbers to ‘Y’ will be adjacent to ‘Y’ in the array. We can search in both directions of ‘Y’ to find the closest numbers.
We can use a heap to efficiently search for the closest numbers. We will take ‘K’ numbers in both directions of ‘Y’ and push them in a Min Heap sorted by their absolute difference from ‘X’. This will ensure that the numbers with the smallest difference from ‘X’ (i.e., closest to ‘X’) can be extracted easily from the Min Heap.
Finally, we will extract the top ‘K’ numbers from the Min Heap to find the required numbers.

### Solution:
 - C++ Solution:
```sh
class KClosestElements {
 public:
  struct numCompare {
    bool operator()(const pair<int, int> &x, const pair<int, int> &y) { return x.first > y.first; }
  };

  static vector<int> findClosestElements(const vector<int> &arr, int K, int X) {
    int index = binarySearch(arr, X);
    int low = index - K, high = index + K;
    low = max(low, 0);                      // 'low' should not be less than zero
    high = min(high, (int)arr.size() - 1);  // 'high' should not be greater the size of the array

    priority_queue<pair<int, int>, vector<pair<int, int>>, numCompare> minHeap;
    // add all candidate elements to the min heap, sorted by their absolute difference from 'X'
    for (int i = low; i <= high; i++) {
      minHeap.push(make_pair(abs(arr[i] - X), i));
    }

    // we need the top 'K' elements having smallest difference from 'X'
    vector<int> result;
    for (int i = 0; i < K; i++) {
      result.push_back(arr[minHeap.top().second]);
      minHeap.pop();
    }

    sort(result.begin(), result.end());
    return result;
  }

 private:
  static int binarySearch(const vector<int> &arr, int target) {
    int low = 0;
    int high = (int)arr.size() - 1;
    while (low <= high) {
      int mid = low + (high - low) / 2;
      if (arr[mid] == target) {
        return mid;
      }
      if (arr[mid] < target) {
        low = mid + 1;
      } else {
        high = mid - 1;
      }
    }
    if (low > 0) {
      return low - 1;
    }
    return low;
  }
};
```
  - Java Solution:
 ```sh
class Entry {
  int key;
  int value;

  public Entry(int key, int value) {
    this.key = key;
    this.value = value;
  }
}

class KClosestElements {

  public static List<Integer> findClosestElements(int[] arr, int K, Integer X) {
    int index = binarySearch(arr, X);
    int low = index - K, high = index + K;
    low = Math.max(low, 0); // 'low' should not be less than zero
    high = Math.min(high, arr.length - 1); // 'high' should not be greater the size of the array

    PriorityQueue<Entry> minHeap = new PriorityQueue<>((n1, n2) -> n1.key - n2.key);
    // add all candidate elements to the min heap, sorted by their absolute difference from 'X'
    for (int i = low; i <= high; i++)
      minHeap.add(new Entry(Math.abs(arr[i] - X), i));

    // we need the top 'K' elements having smallest difference from 'X'
    List<Integer> result = new ArrayList<>();
    for (int i = 0; i < K; i++)
      result.add(arr[minHeap.poll().value]);

    Collections.sort(result);
    return result;
  }

  private static int binarySearch(int[] arr, int target) {
    int low = 0;
    int high = arr.length - 1;
    while (low <= high) {
      int mid = low + (high - low) / 2;
      if (arr[mid] == target)
        return mid;
      if (arr[mid] < target) {
        low = mid + 1;
      } else {
        high = mid - 1;
      }
    }
    if (low > 0) {
      return low - 1;
    }
    return low;
  }
```
 - Time Complexity
   O(logN + K*logK)
 - Space Complexity
OK
### Question 9 Maximum Distinct Elements

Given an array of numbers and a number ‘K’, we need to remove ‘K’ numbers from the array such that we are left with maximum distinct numbers.

 - Example 1
 ```sh
Input: [7, 3, 5, 8, 5, 3, 3], and K=2
Output: 3
Explanation: We can remove two occurrences of 3 to be left with 3 distinct numbers [7, 3, 8], we have 
to skip 5 because it is not distinct and occurred twice. 
Another solution could be to remove one instance of '5' and '3' each to be left with three 
distinct numbers [7, 5, 8], in this case, we have to skip 3 because it occurred twice.
 ```
  - Example 2
 ```sh
Input: [3, 5, 12, 11, 12], and K=3
Output: 2
Explanation: We can remove one occurrence of 12, after which all numbers will become distinct. Then 
we can delete any two numbers which will leave us 2 distinct numbers in the result.
 ```
   - Example 3
 ```sh
Input: [1, 2, 3, 3, 3, 3, 4, 4, 5, 5, 5], and K=2
Output: 3
Explanation: We can remove one occurrence of '4' to get three distinct numbers.
 ```
This problem follows the Top ‘K’ Numbers pattern, and shares similarities with Top ‘K’ Frequent Numbers.

We can following a similar approach as discussed in Top ‘K’ Frequent Numbers problem:

First, we will find the frequencies of all the numbers.
Then, push all numbers that are not distinct (i.e., have a frequency higher than one) in a Min Heap based on their frequencies. At the same time, we will keep a running count of all the distinct numbers.
Following a greedy approach, in a stepwise fashion, we will remove the least frequent number from the heap (i.e., the top element of the min-heap), and try to make it distinct. We will see if we can remove all occurrences of a number except one. If we can, we will increment our running count of distinct numbers. We have to also keep a count of how many removals we have done.
If after removing elements from the heap, we are still left with some deletions, we have to remove some distinct elements.

### Solution:
 - C++ Solution:
```sh
class solution {
 public:
struct cmp{
    bool operator()(pair<int,int> a,pair<int,int> b){
        return a.second > b.second;
    }
};
  static int findMaximumDistinctElements(const vector<int>& nums, int k){
      unordered_map<int, int>mp;
      priority_queue<pair<int,int>,vector<pair<int,int>>,cmp> pq;
      for(auto n:nums){
          mp[n]++;
      }
      int count = 0;
      for(auto m:mp){
          if(m->second == 1){
              count++;
          }
          else{
              pq.push(make_pair(m->first, m->second);
          }
      }
      while(k && !pq.empty()){
          k =  k - pq.top().second + 1;
          if(k > 0){
              count++;
          }
      }
      if(k > 0){
          count -= k;
      }
      return count;
  }
};
```
  - Java Solution:
 ```sh

class solution {
    public:
  public static int findMaximumDistinctElements(int[] nums, int k) {
    int distinctElementsCount = 0;
    if (nums.length <= k)
      return distinctElementsCount;

    // find the frequency of each number
    Map<Integer, Integer> numFrequencyMap = new HashMap<>();
    for (int i : nums)
      numFrequencyMap.put(i, numFrequencyMap.getOrDefault(i, 0) + 1);

    PriorityQueue<Map.Entry<Integer, Integer>> minHeap = new PriorityQueue<Map.Entry<Integer, Integer>>(
        (e1, e2) -> e1.getValue() - e2.getValue());

    // insert all numbers with frequency greater than '1' into the min-heap
    for (Map.Entry<Integer, Integer> entry : numFrequencyMap.entrySet()) {
      if (entry.getValue() == 1)
        distinctElementsCount++;
      else
        minHeap.add(entry);
    }

    // following a greedy approach, try removing the least frequent numbers first from the min-heap
    while (k > 0 && !minHeap.isEmpty()) {
      Map.Entry<Integer, Integer> entry = minHeap.poll();
      // to make an element distinct, we need to remove all of its occurrences except one
      k -= entry.getValue() - 1;
      if (k >= 0)
        distinctElementsCount++;
    }

    // if k > 0, this means we have to remove some distinct numbers
    if (k > 0)
      distinctElementsCount -= k;

    return distinctElementsCount;
  }

```
 - Time Complexity
   O(N + NlogK)
 - Space Complexity
O(N)
### Question 10 Sum of Elements

Given an array, find the sum of all numbers between the K1’th and K2’th smallest elements of that array.

 - Example 1
 ```sh
Input: [1, 3, 12, 5, 15, 11], and K1=3, K2=6
Output: 23
Explanation: The 3rd smallest number is 5 and 6th smallest number 15. The sum of numbers coming
between 5 and 15 is 23 (11+12).
 ```
  - Example 2
 ```sh
Input: [3, 5, 8, 7], and K1=1, K2=4
Output: 12
Explanation: The sum of the numbers between the 1st smallest number (3) and the 4th smallest 
number (8) is 12 (5+7).
 ```
This problem follows the Top ‘K’ Numbers pattern, and shares similarities with Kth Smallest Number.

We can find the sum of all numbers coming between the K1’th and K2’th smallest numbers in the following steps:

First, insert all numbers in a min-heap.
Remove the first K1 smallest numbers from the min-heap.
Now take the next K2-K1-1 numbers out of the heap and add them. This sum will be our required output.

### Solution:
 - C++ Solution:
```sh
class SumOfElements {
 public:
  struct numCompare {
    bool operator()(const int &x, const int &y) { return x > y; }
  };

  static int findSumOfElements(const vector<int> &nums, int k1, int k2) {
    priority_queue<int, vector<int>, numCompare> minHeap;

    // insert all numbers to the min heap
    for (int i = 0; i < nums.size(); i++) {
      minHeap.push(nums[i]);
    }

    // remove k1 small numbers from the min heap
    for (int i = 0; i < k1; i++) {
      minHeap.pop();
    }

    int elementSum = 0;
    // sum next k2-k1-1 numbers
    for (int i = 0; i < k2 - k1 - 1; i++) {
      elementSum += minHeap.top();
      minHeap.pop();
    }

    return elementSum;
  }
};
```
  - Java Solution:
 ```sh
class SumOfElements {

  public static int findSumOfElements(int[] nums, int k1, int k2) {
    PriorityQueue<Integer> minHeap = new PriorityQueue<Integer>((n1, n2) -> n1 - n2);
    // insert all numbers to the min heap
    for (int i = 0; i < nums.length; i++)
      minHeap.add(nums[i]);

    // remove k1 small numbers from the min heap
    for (int i = 0; i < k1; i++)
      minHeap.poll();

    int elementSum = 0;
    // sum next k2-k1-1 numbers
    for (int i = 0; i < k2 - k1 - 1; i++)
      elementSum += minHeap.poll();

    return elementSum;
  }
```
 - Time Complexity
   O(n*logn)
 - Space Complexity
O(n)
### Question 11 Rearrange String

Given a string, find if its letters can be rearranged in such a way that no two same characters come next to each other.

 - Example 1
 ```sh
Input: "aappp"
Output: "papap"
Explanation: In "papap", none of the repeating characters come next to each other.
 ```
  - Example 2
 ```sh
Input: "Programming"
Output: "rgmrgmPiano" or "gmringmrPoa" or "gmrPagimnor", etc.
Explanation: None of the repeating characters come next to each other.
 ```
   - Example 3
 ```sh
Input: "aapa"
Output: ""
Explanation: In all arrangements of "aapa", atleast two 'a' will come together e.g., "apaa", "paaa".
 ```
This problem follows the Top ‘K’ Numbers pattern. We can follow a greedy approach to find an arrangement of the given string where no two same characters come next to each other.

We can work in a stepwise fashion to create a string with all characters from the input string. Following a greedy approach, we should first append the most frequent characters to the output strings, for which we can use a Max Heap. By appending the most frequent character first, we have the best chance to find a string where no two same characters come next to each other.

So in each step, we should append one occurrence of the highest frequency character to the output string. We will not put this character back in the heap to ensure that no two same characters are adjacent to each other. In the next step, we should process the next most frequent character from the heap in the same way and then, at the end of this step, insert the character from the previous step back to the heap after decrementing its frequency.

Following this algorithm, if we can append all the characters from the input string to the output string, we would have successfully found an arrangement of the given string where no two same characters appeared adjacent to each other.

### Solution:
 - C++ Solution:
```sh
class solution {
 public:
 struct cmp{
     bool operator()(pair<int,int>a,pair<int,int>b){
         a.second < b.second;
     }
 }
  static string rearrangeString(const string& str){
      unordered_map<char, int>mp;
      priority_queue<pair<char,int>,vector<pair<char,int>>,cmp> pq;
      string res = 0;
      for(auto s:str){
          mp[s]++;
      }
      for(auto m:mp){
          pq.push(make_pair(m.first,m.second));
      }
      pair<char,int> tmp(-1,-1);
      while(!pq.empty()){
          pair<char,int> cur = pq.top();
          pq.pop();
          res+=cur.first;
          if(tmp.second > 0){
              pq.push(tmp);
          }
          cur.second--;
          tmp = cur;
      }
      return res.length() == str.length()? res:"";
  }
};
```

  - Java Solution:
 ```sh
  public static String rearrangeString(String str) {
    Map<Character, Integer> charFrequencyMap = new HashMap<>();
    for (char chr : str.toCharArray())
      charFrequencyMap.put(chr, charFrequencyMap.getOrDefault(chr, 0) + 1);

    PriorityQueue<Map.Entry<Character, Integer>> maxHeap = new PriorityQueue<Map.Entry<Character, Integer>>(
        (e1, e2) -> e2.getValue() - e1.getValue());

    // add all characters to the max heap
    maxHeap.addAll(charFrequencyMap.entrySet());

    Map.Entry<Character, Integer> previousEntry = null;
    StringBuilder resultString = new StringBuilder(str.length());
    while (!maxHeap.isEmpty()) {
      Map.Entry<Character, Integer> currentEntry = maxHeap.poll();
      // add the previous entry back in the heap if its frequency is greater than zero
      if (previousEntry != null && previousEntry.getValue() > 0)
        maxHeap.offer(previousEntry);
      // append the current character to the result string and decrement its count
      resultString.append(currentEntry.getKey());
      currentEntry.setValue(currentEntry.getValue() - 1);
      previousEntry = currentEntry;
    }

    // if we were successful in appending all the characters to the result string, return it
    return resultString.length() == str.length() ? resultString.toString() : "";
  }

```
 - Time Complexity
   O(N + NlogK)
 - Space Complexity
O(N)
### Question 12 Rearrange String K Distance Apart

Given a string and a number ‘K’, find if the string can be rearranged such that the same characters are at least ‘K’ distance apart from each other.
 - Example 1
 ```sh
Input: "mmpp", K=2
Output: "mpmp" or "pmpm"
Explanation: All same characters are 2 distance apart.
 ```
  - Example 2
 ```sh
Input: "Programming", K=3
Output: "rgmPrgmiano" or "gmringmrPoa" or "gmrPagimnor" and a few more
Explanation: All same characters are 3 distance apart.
 ```
   - Example 3
 ```sh
Input: "aab", K=2
Output: "aba"
Explanation: All same characters are 2 distance apart.
 ```
    - Example 4
 ```sh
Input: "aappa", K=3
Output: ""
Explanation: We cannot find an arrangement of the string where any two 'a' are 3 distance apart.
 ```
This problem follows the Top ‘K’ Numbers pattern and is quite similar to Rearrange String. The only difference is that in the ‘Rearrange String’ the same characters need not be adjacent i.e., they should be at least ‘2’ distance apart (in other words, there should be at least one character between two same characters), while in the current problem, the same characters should be ‘K’ distance apart.

Following a similar approach, since we were inserting a character back in the heap in the next iteration, in this problem, we will re-insert the character after ‘K’ iterations. We can keep track of previous characters in a queue to insert them back in the heap after ‘K’ iterations.

### Solution:
 - C++ Solution:
```sh
class solution {
 public:
 struct cmp{
     bool operator()(pair<int,int>a,pair<int,int>b){
         a.second < b.second;
     }
 }
  static string reorganizeString(const string& str, int k){
      unordered_map<char, int>mp;
      priority_queue<pair<char,int>,vector<pair<char,int>>,cmp> pq;
      string res = "";
      for(auto s:str){
          mp[s]++;
      }
      for(auto m:mp){
          pq.push(make_pair(m.first,m.second));
      }
      queue<pair<char,int>> q;
      while(!pq.empty()){
          pair<char,int> cur = pq.top();
          pq.pop();
          res+=cur.first;
          cur.second--;
          q.push(cur);
          if(q.size() == k){
              if(q.top.second > 0){
                  pq.push(q.top());
              }
              q.pop();
          }
      }
      return res.length() == str.length()? res:"";
  }
};
```
  - Java Solution:
 ```sh
Class solution{
  public static String reorganizeString(String str, int k) {
    if (k <= 1)
      return str;

    Map<Character, Integer> charFrequencyMap = new HashMap<>();
    for (char chr : str.toCharArray())
      charFrequencyMap.put(chr, charFrequencyMap.getOrDefault(chr, 0) + 1);

    PriorityQueue<Map.Entry<Character, Integer>> maxHeap = new PriorityQueue<Map.Entry<Character, Integer>>(
        (e1, e2) -> e2.getValue() - e1.getValue());

    // add all characters to the max heap
    maxHeap.addAll(charFrequencyMap.entrySet());

    Queue<Map.Entry<Character, Integer>> queue = new LinkedList<>();
    StringBuilder resultString = new StringBuilder(str.length());
    while (!maxHeap.isEmpty()) {
      Map.Entry<Character, Integer> currentEntry = maxHeap.poll();
      // append the current character to the result string and decrement its count
      resultString.append(currentEntry.getKey());
      currentEntry.setValue(currentEntry.getValue() - 1);
      queue.offer(currentEntry);
      if (queue.size() == k) {
        Map.Entry<Character, Integer> entry = queue.poll();
        if (entry.getValue() > 0)
          maxHeap.add(entry);
      }
    }

    // if we were successful in appending all the characters to the result string, return it
    return resultString.length() == str.length() ? resultString.toString() : "";
  }
```
 - Time Complexity
   O(n*logn)
 - Space Complexity
O(n)
### Question 13 Scheduling Tasks

You are given a list of tasks that need to be run, in any order, on a server. Each task will take one CPU interval to execute but once a task has finished, it has a cooling period during which it can’t be run again. If the cooling period for all tasks is ‘K’ intervals, find the minimum number of CPU intervals that the server needs to finish all tasks.

If at any time the server can’t execute any task then it must stay idle.

 - Example 1
 ```sh
Input: [1, 3, 5, 12, 11, 12, 11], K = 2
Output: [12, 11]
Explanation: Both '11' and '12' apeared twice.
 ```
  - Example 2
 ```sh
Input: [5, 12, 11, 3, 11], K = 2
Output: [11, 5] or [11, 12] or [11, 3]
Explanation: Only '11' appeared twice, all other numbers appeared once.
 ```
This problem follows the Top ‘K’ Elements pattern and is quite similar to Rearrange String K Distance Apart. We need to rearrange tasks such that same tasks are ‘K’ distance apart.

Following a similar approach, we will use a Max Heap to execute the highest frequency task first. After executing a task we decrease its frequency and put it in a waiting list. In each iteration, we will try to execute as many as k+1 tasks. For the next iteration, we will put all the waiting tasks back in the Max Heap. If, for any iteration, we are not able to execute k+1 tasks, the CPU has to remain idle for the remaining time in the next iteration.

### Solution:
 - C++ Solution:
```sh
class solution {
 public:
 struct cmp{
     bool operator()(pair<int,int>a,pair<int,int>b){
         a.second < b.second;
     }
 }
  static int scheduleTasks(const vector<char>& tasks, int k){
      int count = 0;
      unordered_map<int, int>mp;
      priority_queue<pair<int,int>,vector<pair<int,int>>,cmp> pq;
      for(int i = 0;i < nums.size();i++){
          mp[nums[i]]++;
      }
      for(auto m:mp){
          pq.push(make_pair(m.first,m.second));
        }
      while(!pq.empty()){
          vector<pair<char,int>> tmp1;
          int n = 1 + k;
          for(;n > 0 && !pq.empty();n--){
              count++;
              pair<char,int> tmp = make_pair(pq.top().first,pq.top().second--);
              pq.pop();
              tmp1.push_back(tmp);
          }
          for(auto t:tmp1){
              if(t.second > 0){
                  pq.push(t);
              }
          }
          if(!pq.empty()){
              count += n;
          }
      }
      return count;
  }
};
```
  - Java Solution:
 ```sh

class solution {
   public static int scheduleTasks(char[] tasks, int k) {
    int intervalCount = 0;
    Map<Character, Integer> taskFrequencyMap = new HashMap<>();
    for (char chr : tasks)
      taskFrequencyMap.put(chr, taskFrequencyMap.getOrDefault(chr, 0) + 1);

    PriorityQueue<Map.Entry<Character, Integer>> maxHeap = new PriorityQueue<Map.Entry<Character, Integer>>(
        (e1, e2) -> e2.getValue() - e1.getValue());

    // add all tasks to the max heap
    maxHeap.addAll(taskFrequencyMap.entrySet());

    while (!maxHeap.isEmpty()) {
      List<Map.Entry<Character, Integer>> waitList = new ArrayList<>();
      int n = k + 1; // try to execute as many as 'k+1' tasks from the max-heap
      for (; n > 0 && !maxHeap.isEmpty(); n--) {
        intervalCount++;
        Map.Entry<Character, Integer> currentEntry = maxHeap.poll();
        if (currentEntry.getValue() > 1) {
          currentEntry.setValue(currentEntry.getValue() - 1);
          waitList.add(currentEntry);
        }
      }
      maxHeap.addAll(waitList); // put all the waiting list back on the heap
      if (!maxHeap.isEmpty())
        intervalCount += n; // we'll be having 'n' idle intervals for the next iteration
    }

    return intervalCount;
  }

```
 - Time Complexity
   O(N + NlogK)
 - Space Complexity
O(N)
### Question 14 Frequency Stack

Design a class that simulates a Stack data structure, implementing the following two operations:

push(int num): Pushes the number ‘num’ on the stack.
pop(): Returns the most frequent number in the stack. If there is a tie, return the number which was pushed later.

 - Example 1
 ```sh
After following push operations: push(1), push(2), push(3), push(2), push(1), push(2), push(5)
 
1. pop() should return 2, as it is the most frequent number
2. Next pop() should return 1
3. Next pop() should return 2
 ```

This problem follows the Top ‘K’ Elements pattern, and shares similarities with Top ‘K’ Frequent Numbers.

We can use a Max Heap to store the numbers. Instead of comparing the numbers we will compare their frequencies so that the root of the heap is always the most frequently occurring number. There are two issues that need to be resolved though:

How can we keep track of the frequencies of numbers in the heap? When we are pushing a new number to the Max Heap, we don’t know how many times the number has already appeared in the Max Heap. To resolve this, we will maintain a HashMap to store the current frequency of each number. Thus whenever we push a new number in the heap, we will increment its frequency in the HashMap and when we pop, we will decrement its frequency.
If two numbers have the same frequency, we will need to return the number which was pushed later while popping. To resolve this, we need to attach a sequence number to every number to know which number came first.
In short, we will keep three things with every number that we push to the heap:

    1. number // value of the number
    2. frequency // current frequency of the number when it was pushed to the heap
    3. sequenceNumber // a sequence number, to know what number came first
### Solution:
 - C++ Solution:
```sh
class Element {
 public:
  int number = 0;
  int frequency = 0;
  int sequenceNumber = 0;

  Element(int number, int frequency, int sequenceNumber) {
    this->number = number;
    this->frequency = frequency;
    this->sequenceNumber = sequenceNumber;
  }
};

class FrequencyStack {
 public:
  struct frequencyCompare {
    bool operator()(const Element &e1, const Element &e2) {
      if (e1.frequency != e2.frequency) {
        return e2.frequency > e1.frequency;
      }
      // if both elements have same frequency, return the one that was pushed later
      return e2.sequenceNumber > e1.sequenceNumber;
    }
  };

  int sequenceNumber = 0;
  priority_queue<Element, vector<Element>, frequencyCompare> maxHeap;
  unordered_map<int, int> frequencyMap;

  virtual void push(int num) {
    frequencyMap[num] += 1;
    maxHeap.push({num, frequencyMap[num], sequenceNumber++});
  }

  virtual int pop() {
    int num = maxHeap.top().number;
    maxHeap.pop();

    // decrement the frequency or remove if this is the last number
    if (frequencyMap[num] > 1) {
      frequencyMap[num] -= 1;
    } else {
      frequencyMap.erase(num);
    }

    return num;
  }
};
```
  - Java Solution:
 ```sh
class Element {
  int number;
  int frequency;
  int sequenceNumber;

  public Element(int number, int frequency, int sequenceNumber) {
    this.number = number;
    this.frequency = frequency;
    this.sequenceNumber = sequenceNumber;
  }
}

class ElementComparator implements Comparator<Element> {
  public int compare(Element e1, Element e2) {
    if (e1.frequency != e2.frequency)
      return e2.frequency - e1.frequency;
    // if both elements have same frequency, return the one that was pushed later 
    return e2.sequenceNumber - e1.sequenceNumber;
  }
}

class FrequencyStack {
  int sequenceNumber = 0;
  PriorityQueue<Element> maxHeap = new PriorityQueue<Element>(new ElementComparator());
  Map<Integer, Integer> frequencyMap = new HashMap<>();

  public void push(int num) {
    frequencyMap.put(num, frequencyMap.getOrDefault(num, 0) + 1);
    maxHeap.offer(new Element(num, frequencyMap.get(num), sequenceNumber++));
  }

  public int pop() {
    int num = maxHeap.poll().number;

    // decrement the frequency or remove if this is the last number
    if (frequencyMap.get(num) > 1)
      frequencyMap.put(num, frequencyMap.get(num) - 1);
    else
      frequencyMap.remove(num);

    return num;
  }
```
 - Time Complexity
   O(n*logn)
 - Space Complexity
O(n)

### Todos

 - Write MORE Tests
 - Add Night Mode

License
----

MIT
