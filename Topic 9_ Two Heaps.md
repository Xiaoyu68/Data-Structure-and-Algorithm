# Topic 9: Two Heaps


[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

In many problems, where we are given a set of elements such that we can divide them into two parts. To solve the problem, we are interested in knowing the smallest element in one part and the biggest element in the other part. This pattern is an efficient approach to solve such problems.

This pattern uses two Heaps to solve these problems; A Min Heap to find the smallest element and a Max Heap to find the biggest element.


### Question 1: Find the Median of a Number Stream

Design a class to calculate the median of a number stream. The class should have the following two methods:

insertNum(int num): stores the number in the class
findMedian(): returns the median of all numbers inserted in the class
If the count of numbers inserted in the class is even, the median will be the average of the middle two numbers.

 - Example 1
 ```sh
 1. insertNum(3)
2. insertNum(1)
3. findMedian() -> output: 2
4. insertNum(5)
5. findMedian() -> output: 3
6. insertNum(4)
7. findMedian() -> output: 3.5
 ```
 As we know, the median is the middle value in an ordered integer list. So a brute force solution could be to maintain a sorted list of all numbers inserted in the class so that we can efficiently return the median whenever required. Inserting a number in a sorted list will take O(N)O(N) time if there are ‘N’ numbers in the list. This insertion will be similar to the Insertion sort. Can we do better than this? Can we utilize the fact that we don’t need the fully sorted list - we are only interested in finding the middle element?

Assume ‘x’ is the median of a list. This means that half of the numbers in the list will be smaller than (or equal to) ‘x’ and half will be greater than (or equal to) ‘x’. This leads us to an approach where we can divide the list into two halves: one half to store all the smaller numbers (let’s call it smallNumList) and one half to store the larger numbers (let’s call it largNumList). The median of all the numbers will either be the largest number in the smallNumList or the smallest number in the largNumList. If the total number of elements is even, the median will be the average of these two numbers.

The best data structure that comes to mind to find the smallest or largest number among a list of numbers is a Heap. Let’s see how we can use a heap to find a better algorithm.

We can store the first half of numbers (i.e., smallNumList) in a Max Heap. We should use a Max Heap as we are interested in knowing the largest number in the first half.
We can store the second half of numbers (i.e., largeNumList) in a Min Heap, as we are interested in knowing the smallest number in the second half.
Inserting a number in a heap will take O(logN)O(logN), which is better than the brute force approach.
At any time, the median of the current list of numbers can be calculated from the top element of the two heaps.
Let’s take the Example-1 mentioned above to go through each step of our algorithm:

insertNum(3): We can insert a number in the Max Heap (i.e. first half) if the number is smaller than the top (largest) number of the heap. After every insertion, we will balance the number of elements in both heaps, so that they have an equal number of elements. If the count of numbers is odd, let’s decide to have more numbers in max-heap than the Min Heap.

insertNum(1): As ‘1’ is smaller than ‘3’, let’s insert it into the Max Heap.

Now, we have two elements in the Max Heap and no elements in Min Heap. Let’s take the largest element from the Max Heap and insert it into the Min Heap, to balance the number of elements in both heaps.

findMedian(): As we have an even number of elements, the median will be the average of the top element of both the heaps -> (1+3)/2 = 2.0(1+3)/2=2.0
insertNum(5): As ‘5’ is greater than the top element of the Max Heap, we can insert it into the Min Heap. After the insertion, the total count of elements will be odd. As we had decided to have more numbers in the Max Heap than the Min Heap, we can take the top (smallest) number from the Min Heap and insert it into the Max Heap.

findMedian(): Since we have an odd number of elements, the median will be the top element of Max Heap -> 3. An odd number of elements also means that the Max Heap will have one extra element than the Min Heap.
insertNum(4): Insert ‘4’ into Min Heap.

findMedian(): As we have an even number of elements, the median will be the average of the top element of both the heaps -> (3+4)/2 = 3.5(3+4)/2=3.5
 
### Solution:
 - C++ Solution:
```sh
Class solution{
    priority_queue<int> q1;
    priority_queue<int, vector<int>, greater<int>> q2;
public:
    void insertNum(int num){
        if(q1.empty() || num <= q1.top()){
            q1.push(num);
        }
        else{
            q2.push(num);
        }
        if(q1.size() > q2.size() + 1){
            q2.push(q1.top());
            q1.pop();
        }
        if(q2.size() > q1.size()){
            q1.push(q2.top());
            q2.pop();
        }
    }
    
    double findMedian(){
        if(q1.size() == q2.size()){
            return (double)(q1.top() + q2.top())/2;
        }
        return q1.top();
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    PriorityQueue<Integer> q1;
    PriorityQueue<Integer> q2;
    public MedianOfAStream(){
        q1 = new PriorityQueue<>((a, b)->b-a);
        q2 = new PriorityQueue<>((a,b) -> a-b);
    }
    public void insertNum(int num){
        if(q1.isEmpty() || num <= q1.peek()){
            q1.add(num);
        }
        else{
            q2.add(num);
        }
        if(q1.size() > q2.size() + 1){
            q2.add(q1.poll());
        }
        if(q2.size() > q1.size()){
            q1.add(q2.poll());
        }
    }
    
    public double findMedian(){
        if(q1.size() == q2.size()){
            return (double)(q1.peek() + q2.peek())/2;
        }
        return q1.peek();
    }
};
```
Time complexity #
O(logN)
Space complexity #
O(N)


### Question 2 Sliding Window Median
Given an array of numbers and a number ‘k’, find the median of all the ‘k’ sized sub-arrays (or windows) of the array.

 - Example 1
 ```sh
Input: nums=[1, 2, -1, 3, 5], k = 2
Output: [1.5, 0.5, 1.0, 4.0]
Explanation: Lets consider all windows of size ‘2’:

[1, 2, -1, 3, 5] -> median is 1.5
[1, 2, -1, 3, 5] -> median is 0.5
[1, 2, -1, 3, 5] -> median is 1.0
[1, 2, -1, 3, 5] -> median is 4.0
 ```
  - Example 2
 ```sh
Input: nums=[1, 2, -1, 3, 5], k = 3
Output: [1.0, 2.0, 3.0]
Explanation: Lets consider all windows of size ‘3’:

[1, 2, -1, 3, 5] -> median is 1.0
[1, 2, -1, 3, 5] -> median is 2.0
[1, 2, -1, 3, 5] -> median is 3.0
 ```
 This problem follows the Two Heaps pattern and share similarities with Find the Median of a Number Stream. We can follow a similar approach of maintaining a max-heap and a min-heap for the list of numbers to find their median.

The only difference is that we need to keep track of a sliding window of ‘k’ numbers. This means, in each iteration, when we insert a new number in the heaps, we need to remove one number from the heaps which is going out of the sliding window. After the removal, we need to rebalance the heaps in the same way that we did while inserting.
### Solution:
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    PriorityQueue<Integer> q1 = new PriorityQueue<>(Collections.reversOrder());
    PriorityQueue<Integer> q2 = new PriorityQueue<>();
    public static  double[] findSlidingWindowMedian(int[] num, int k){
        double[] result = new double[nums.length - k + 1];
        for(int i = 0;i < num.length,i++){
            if(q1.isEmpty() || num[i] <=q1.peek()){
                q1.add(num[i]);
            }
            else{
                q2.add(num[i]);
            }
            if(q1.size() > q2.size() + 1){
                q2.add(q1.poll());
            }
            if(q2.size() > q1.size()){
                q1.add(q2.poll());
            }
            if(i >= k - 1){
                if(q1.size() == q2.size()){
                    res[i - k + 1] = (q1.peek() + q2.peek())/2;
                }else{
                    res[i - k + 1] = q1.peek();
                }
                if(num[i - k +1] <= q1.peek()){
                    q1.remove(num[i - k + 1]);
                }
                if(num[i - k + 1] >= q2.peek()){
                    q2.remove(num[i - k + 1]);
                }
                            if(q1.size() > q2.size() + 1){
                q2.add(q1.poll());
            }
            if(q2.size() > q1.size()){
                q1.add(q2.poll());
            }
            }
        }
        return res;
    }
};
```
Time Complexity: O(N * K)
Space Complexity: O(K)

### Question 3 Maximize Capital

Given a set of investment projects with their respective profits, we need to find the most profitable projects. We are given an initial capital and are allowed to invest only in a fixed number of projects. Our goal is to choose projects that give us the maximum profit. Write a function that returns the maximum total capital after selecting the most profitable projects.

We can start an investment project only when we have the required capital. Once a project is selected, we can assume that its profit has become our capital.

 - Example 1
 ```sh
Input: Project Capitals=[0,1,2], Project Profits=[1,2,3], Initial Capital=1, Number of Projects=2
Output: 6
Explanation:

With initial capital of ‘1’, we will start the second project which will give us profit of ‘2’. Once we selected our first project, our total capital will become 3 (profit + initial capital).
With ‘3’ capital, we will select the third project, which will give us ‘3’ profit.
After the completion of the two projects, our total capital will be 6 (1+2+3).
 ```
  - Example 2
 ```sh
Input: Project Capitals=[0,1,2,3], Project Profits=[1,2,3,5], Initial Capital=0, Number of Projects=3
Output: 8
Explanation:

With ‘0’ capital, we can only select the first project, bringing out capital to 1.
Next, we will select the second project, which will bring our capital to 3.
Next, we will select the fourth project, giving us a profit of 5.
After selecting the three projects, our total capital will be 8 (1+2+5).
 ```
### Solution:
 - C++ Solution:
```sh
Class solution{
    priority_queue<int> q1;
    priority_queue<int, vector<int>, greater<int>> q2;
public:
    static  int findMaximumCapital(const vector<int> &capital, const vector<int> &profits, int numberOfProjects, int initialCapital){
        int n = profit.size();
        for(int i = 0;i < n;i++){
            q2.push(i);
        }
        int sum = initialCapital;
        for(int i = 0;i < numberOfProjects;i++){
            while(!q2.empty() && q2.top() <= sum){
                q1.push(q2.top());
                q2.pop();
            }
            if(q2.empty()){
                break;
            }
            sum += profits[q2.top()];
            q2.pop();
        }
        return sum;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    PriorityQueue<Integer> q1 = new PriorityQueue<Integer>(Collections.reverseOrder());
    PriorityQueue<Integer> q1 = new PriorityQueue<Integer>();
    static  int findMaximumCapital(const vector<int> &capital, const vector<int> &profits, int numberOfProjects, int initialCapital){
        int n = profit.size();
        for(int i = 0;i < n;i++){
            q2.add(i);
        }
        int sum = initialCapital;
        for(int i = 0;i < numberOfProjects;i++){
            while(!q2.isEmpty() && q2.peek() <= sum){
                q1.add(q2.poll());
            }
            if(q2.isEmpty()){
                break;
            }
            sum += profits[q2.poll()];
        }
        return sum;
    }
};
```
 - Time Complexity
  O(NlogN + NlogN)
 - Space Complexity
  O(N)

### Question 4 Next Interval

Given an array of intervals, find the next interval of each interval. In a list of intervals, for an interval ‘i’ its next interval ‘j’ will have the smallest ‘start’ greater than or equal to the ‘end’ of ‘i’.

Write a function to return an array containing indices of the next interval of each input interval. If there is no next interval of a given interval, return -1. It is given that none of the intervals have the same start point.

 - Example 1
 ```sh
Input: Intervals [[2,3], [3,4], [5,6]]
Output: [1, 2, -1]
Explanation: The next interval of [2,3] is [3,4] having index ‘1’. Similarly, the next interval of [3,4] is [5,6] having index ‘2’. There is no next interval for [5,6] hence we have ‘-1’.
 ```
  - Example 2
 ```sh
Input: Intervals [[3,4], [1,5], [4,6]]
Output: [2, -1, -1]
Explanation: The next interval of [3,4] is [4,6] which has index ‘2’. There is no next interval for [1,5] and [4,6].
 ```
 
 A brute force solution could be to take one interval at a time and go through all the other intervals to find the next interval. This algorithm will take O(N^2) where ‘N’ is the total number of intervals. Can we do better than that?

We can utilize the Two Heaps approach. We can push all intervals into two heaps: one heap to sort the intervals on maximum start time (let’s call it maxStartHeap) and the other on maximum end time (let’s call it maxEndHeap). We can then iterate through all intervals of the `maxEndHeap’ to find their next interval. Our algorithm will have the following steps:

Take out the top (having highest end) interval from the maxEndHeap to find its next interval. Let’s call this interval topEnd.
Find an interval in the maxStartHeap with the closest start greater than or equal to the start of topEnd. Since maxStartHeap is sorted by ‘start’ of intervals, it is easy to find the interval with the highest ‘start’. Let’s call this interval topStart.
Add the index of topStart in the result array as the next interval of topEnd. If we can’t find the next interval, add ‘-1’ in the result array.
Put the topStart back in the maxStartHeap, as it could be the next interval of other intervals.
Repeat the steps 1-4 until we have no intervals left in maxEndHeap.

### Solution:
 - C++ Solution:
```sh
class Interval {
 public:
  int start = 0;
  int end = 0;

  Interval(int start, int end) {
    this->start = start;
    this->end = end;
  }
};

class NextInterval {
 public:
  struct startCompare {
    bool operator()(const pair<Interval, int> &x, const pair<Interval, int> &y) {
      return y.first.start > x.first.start;
    }
  };

  struct endCompare {
    bool operator()(const pair<Interval, int> &x, const pair<Interval, int> &y) {
      return y.first.end > x.first.end;
    }
  };

  static vector<int> findNextInterval(const vector<Interval> &intervals) {
    int n = intervals.size();
    // heap for finding the maximum start
    priority_queue<pair<Interval, int>, vector<pair<Interval, int>>, startCompare> maxStartHeap;
    // heap for finding the minimum end
    priority_queue<pair<Interval, int>, vector<pair<Interval, int>>, endCompare> maxEndHeap;

    vector<int> result(n);
    for (int i = 0; i < intervals.size(); i++) {
      maxStartHeap.push(make_pair(intervals[i], i));
      maxEndHeap.push(make_pair(intervals[i], i));
    }

    // go through all the intervals to find each interval's next interval
    for (int i = 0; i < n; i++) {
      // let's find the next interval of the interval which has the highest 'end'
      auto topEnd = maxEndHeap.top();
      maxEndHeap.pop();

      result[topEnd.second] = -1;  // defaults to -1
      if (maxStartHeap.top().first.start >= topEnd.first.end) {
        auto topStart = maxStartHeap.top();
        maxStartHeap.pop();
        // find the the interval that has the closest 'start'
        while (!maxStartHeap.empty() && maxStartHeap.top().first.start >= topEnd.first.end) {
          topStart = maxStartHeap.top();
          maxStartHeap.pop();
        }
        result[topEnd.second] = topStart.second;
        // put the interval back as it could be the next interval of other intervals
        maxStartHeap.push(topStart);
      }
    }

    return result;
  }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
 Class Interval{
     int start = 0;
     int end = 0;
     Interval(int start, int end){
         this.start = start;
         this.end = end;
     }
 }
Class solution{
public static  int[] findNextInterval(Interval[] intervals){
    int n = intervals.length;
    // heap for finding the maximum start
    PriorityQueue<Integer> maxStartHeap = new PriorityQueue<>(n, (i1, i2) -> intervals[i2].start - intervals[i1].start);
    // heap for finding the minimum end
    PriorityQueue<Integer> maxEndHeap = new PriorityQueue<>(n, (i1, i2) -> intervals[i2].end - intervals[i1].end);
    int[] result = new int[n];
    for (int i = 0; i < intervals.length; i++) {
      maxStartHeap.offer(i);
      maxEndHeap.offer(i);
    }

    // go through all the intervals to find each interval's next interval
    for (int i = 0; i < n; i++) {
      int topEnd = maxEndHeap.poll(); // let's find the next interval of the interval which has the highest 'end' 
      result[topEnd] = -1; // defaults to -1
      if (intervals[maxStartHeap.peek()].start >= intervals[topEnd].end) {
        int topStart = maxStartHeap.poll();
        // find the the interval that has the closest 'start'
        while (!maxStartHeap.isEmpty() && intervals[maxStartHeap.peek()].start >= intervals[topEnd].end) {
          topStart = maxStartHeap.poll();
        }
        result[topEnd] = topStart;
        maxStartHeap.add(topStart); // put the interval back as it could be the next interval of other intervals
      }
    }
    return result;
  }
};
```
 - Time Complexity
  O(NlogN + NlogN)
 - Space Complexity
  O(N)



### Todos

 - Write MORE Tests
 - Add Night Mode

License
----

MIT
