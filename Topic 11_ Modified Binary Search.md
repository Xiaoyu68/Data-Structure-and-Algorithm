# Topic 11: Modified Binary Search


[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

As we know, whenever we are given a sorted Array or LinkedList or Matrix, and we are asked to find a certain element, the best algorithm we can use is the Binary Search.

This pattern describes an efficient way to handle all problems involving Binary Search. We will go through a set of problems that will help us build an understanding of this pattern so that we can apply this technique to other problems we might come across in the interviews.

### Question 1: Order-agnostic Binary Search

Given a sorted array of numbers, find if a given number ‘key’ is present in the array. Though we know that the array is sorted, we don’t know if it’s sorted in ascending or descending order. You should assume that the array can have duplicates.

Write a function to return the index of the ‘key’ if it is present in the array, otherwise return -1.

 - Example 1
 ```sh
Input: [4, 6, 10], key = 10
Output: 2
 ```
 
  - Example 2
 ```sh
Input: [1, 2, 3, 4, 5, 6, 7], key = 5
Output: 4
 ```
 
   - Example 3
 ```sh
Input: [10, 6, 4], key = 10
Output: 0
 ```
 
   - Example 4
 ```sh
Input: [10, 6, 4], key = 4
Output: 2
 ```
To make things simple, let’s first solve this problem assuming that the input array is sorted in ascending order. Here are the set of steps for Binary Search:

Let’s assume start is pointing to the first index and end is pointing to the last index of the input array (let’s call it arr). This means:
    int start = 0;
    int end = arr.length - 1;
First, we will find the middle of start and end. An easy way to find the middle would be: middle=(start+end)/2middle=(start+end)/2. For Java and C++, this equation will work for most cases, but when start or end is large, this equation will give us the wrong result due to integer overflow. Imagine that end is equal to the maximum range of an integer (e.g. for Java: int end = Integer.MAX_VALUE). Now adding any positive number to end will result in an integer overflow. Since we need to add both the numbers first to evaluate our equation, an overflow might occur. The safest way to find the middle of two numbers without getting an overflow is as follows:
     middle  = start + (end-start)/2
The above discussion is not relevant for Python, as we don’t have the integer overflow problem in pure Python.

Next, we will see if the ‘key’ is equal to the number at index middle. If it is equal we return middle as the required index.
If ‘key’ is not equal to number at index middle, we have to check two things:
If key < arr[middle], then we can conclude that the key will be smaller than all the numbers after index middle as the array is sorted in the ascending order. Hence, we can reduce our search to end = mid - 1.
If key > arr[middle], then we can conclude that the key will be greater than all numbers before index middle as the array is sorted in the ascending order. Hence, we can reduce our search to start = mid + 1.
We will repeat steps 2-4 with new ranges of start to end. If at any time start becomes greater than end, this means that we can’t find the ‘key’ in the input array and we must return ‘-1’.

If the array is sorted in the descending order, we have to update the step 4 above as:

If key > arr[middle], then we can conclude that the key will be greater than all numbers after index middle as the array is sorted in the descending order. Hence, we can reduce our search to end = mid - 1.
If key < arr[middle], then we can conclude that the key will be smaller than all the numbers before index middle as the array is sorted in the descending order. Hence, we can reduce our search to start = mid + 1.
Finally, how can we figure out the sort order of the input array? We can compare the numbers pointed out by start and end index to find the sort order. If arr[start] < arr[end], it means that the numbers are sorted in ascending order otherwise they are sorted in the descending order.

 
### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static int search(const vector<int>& arr, int key){
        bool dir = true;
        if(arr[0] > arr[arr.size() - 1]){
            dir = false;
        }
        int l = 0;
        int r = arr.size() - 1;
        while(l <= r){
            int mid = l + (r - l)/2;
            if(arr[mid] ==  key){
                return mid;
            }
            if(dir){
                if(arr[mid] < key){
                    l = mid + 1;
                }
                else{
                    r = mid - 1;
                }
            }
            else{
                if(arr[mid] < key){
                    r = mid - 1;
                }
                else{
                    l = mid + 1;
                }
            }
            
        }
        return -1;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
  public static int search(int[] arr, int key) {
    int start = 0, end = arr.length - 1;
    boolean isAscending = arr[start] < arr[end];
    while (start <= end) {
      // calculate the middle of the current range
      int mid = start + (end - start) / 2;

      if (key == arr[mid])
        return mid;

      if (isAscending) { // ascending order
        if (key < arr[mid]) {
          end = mid - 1; // the 'key' can be in the first half
        } else { // key > arr[mid]
          start = mid + 1; // the 'key' can be in the second half
        }
      } else { // descending order        
        if (key > arr[mid]) {
          end = mid - 1; // the 'key' can be in the first half
        } else { // key < arr[mid]
          start = mid + 1; // the 'key' can be in the second half
        }
      }
    }
    return -1; // element not found
  }
```
Time complexity #
O(log(N))
Space complexity #
O(1)


### Question 2 Ceiling of a Number

Given an array of numbers sorted in an ascending order, find the ceiling of a given number ‘key’. The ceiling of the ‘key’ will be the smallest element in the given array greater than or equal to the ‘key’.

Write a function to return the index of the ceiling of the ‘key’. If there isn’t any ceiling return -1.

 - Example 1
 ```sh
Input: [4, 6, 10], key = 6
Output: 1
Explanation: The smallest number greater than or equal to '6' is '6' having index '1'.
 ```
  - Example 2
 ```sh
Input: [1, 3, 8, 10, 15], key = 12
Output: 4
Explanation: The smallest number greater than or equal to '12' is '15' having index '4'.
 ```
   - Example 3
 ```sh
Input: [4, 6, 10], key = 17
Output: -1
Explanation: There is no number greater than or equal to '17' in the given array.
 ```
   - Example 4
 ```sh
Input: [4, 6, 10], key = -1
Output: 0
Explanation: The smallest number greater than or equal to '-1' is '4' having index '0'.
 ```
This problem follows the Binary Search pattern. Since Binary Search helps us find a number in a sorted array efficiently, we can use a modified version of the Binary Search to find the ceiling of a number.

We can use a similar approach as discussed in Order-agnostic Binary Search. We will try to search for the ‘key’ in the given array. If we find the ‘key’, we return its index as the ceiling. If we can’t find the ‘key’, the next big number will be pointed out by the index start. Consider Example-2 mentioned above:

Since we are always adjusting our range to find the ‘key’, when we exit the loop, the start of our range will point to the smallest number greater than the ‘key’ as shown in the above picture.

We can add a check in the beginning to see if the ‘key’ is bigger than the biggest number in the input array. If so, we can return ‘-1’.

### Solution:
- C++ Solution:
```sh
Class solution{
public:
  static int searchCeilingOfANumber(const vector<int>& arr, int key) {
    if (key > arr[arr.size() - 1]) {  // if the 'key' is bigger than the biggest element
      return -1;
    }

    int start = 0, end = arr.size() - 1;
    while (start <= end) {
      int mid = start + (end - start) / 2;
      if (key < arr[mid]) {
        end = mid - 1;
      } else if (key > arr[mid]) {
        start = mid + 1;
      } else {  // found the key
        return mid;
      }
    }
    // since the loop is running until 'start <= end', so at the end of the while loop, 'start ==
    // end+1' we are not able to find the element in the given array, so the next big number will be
    // arr[start]
    return start;
  }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
  public static int searchCeilingOfANumber(int[] arr, int key) {
    if (key > arr[arr.length - 1]) // if the 'key' is bigger than the biggest element
      return -1;

    int start = 0, end = arr.length - 1;
    while (start <= end) {
      int mid = start + (end - start) / 2;
      if (key < arr[mid]) {
        end = mid - 1;
      } else if (key > arr[mid]) {
        start = mid + 1;
      } else { // found the key
        return mid;
      }
    }
    // since the loop is running until 'start <= end', so at the end of the while loop, 'start == end+1'
    // we are not able to find the element in the given array, so the next big number will be arr[start]
    return start;
  }
```
Time Complexity: O(logN)
Space Complexity: O(1)

### Question 3 Next Letter

Given an array of lowercase letters sorted in ascending order, find the smallest letter in the given array greater than a given ‘key’.

Assume the given array is a circular list, which means that the last letter is assumed to be connected with the first letter. This also means that the smallest letter in the given array is greater than the last letter of the array and is also the first letter of the array.

Write a function to return the next letter of the given ‘key’.

 - Example 1
 ```sh
Input: ['a', 'c', 'f', 'h'], key = 'f'
Output: 'h'
Explanation: The smallest letter greater than 'f' is 'h' in the given array.
 ```
  - Example 1
 ```sh
Input: ['a', 'c', 'f', 'h'], key = 'f'
Output: 'h'
Explanation: The smallest letter greater than 'f' is 'h' in the given array.
 ```
  - Example 1
 ```sh
Input: ['a', 'c', 'f', 'h'], key = 'f'
Output: 'h'
Explanation: The smallest letter greater than 'f' is 'h' in the given array.
 ```
  - Example 1
 ```sh
Input: ['a', 'c', 'f', 'h'], key = 'f'
Output: 'h'
Explanation: The smallest letter greater than 'f' is 'h' in the given array.
 ```
 The problem follows the Binary Search pattern. Since Binary Search helps us find an element in a sorted array efficiently, we can use a modified version of it to find the next letter.

We can use a similar approach as discussed in Ceiling of a Number. There are a couple of differences though:

The array is considered circular, which means if the ‘key’ is bigger than the last letter of the array or if it is smaller than the first letter of the array, the key’s next letter will be the first letter of the array.
The other difference is that we have to find the next biggest letter which can’t be equal to the ‘key’. This means that we will ignore the case where key == arr[middle]. To handle this case, we can update our start range to start = middle +1.
In the end, instead of returning the element pointed out by start, we have to return the letter pointed out by start % array_length. This is needed because of point 2 discussed above. Imagine that the last letter of the array is equal to the ‘key’. In that case, we have to return the first letter of the input array.
 
### Solution:
- C++ Solution:
```sh
Class solution{
public:
  static char searchNextLetter(const vector<char>& letters, char key) {
    int n = letters.size();
    if (key < letters[0] || key > letters[n - 1]) {
      return letters[0];
    }

    int start = 0, end = n - 1;
    while (start <= end) {
      int mid = start + (end - start) / 2;
      if (key < letters[mid]) {
        end = mid - 1;
      } else {  // if (key >= letters[mid]) {
        start = mid + 1;
      }
    }
    // since the loop is running until 'start <= end', so at the end of the
    // while loop, 'start == end+1'
    return letters[start % n];
  }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
  public static char searchNextLetter(char[] letters, char key) {
    int n = letters.length;
    if (key < letters[0] || key > letters[n - 1])
      return letters[0];

    int start = 0, end = n - 1;
    while (start <= end) {
      int mid = start + (end - start) / 2;
      if (key < letters[mid]) {
        end = mid - 1;
      } else { //if (key >= letters[mid]) {
        start = mid + 1;
      }
    }
    // since the loop is running until 'start <= end', so at the end of the while loop, 'start == end+1'
    return letters[start % n];
  }
```
 - Time Complexity
  O(logN)
 - Space Complexity
  O(1)

### Question 4 Number Range

Given an array of numbers sorted in ascending order, find the range of a given number ‘key’. The range of the ‘key’ will be the first and last position of the ‘key’ in the array.

Write a function to return the range of the ‘key’. If the ‘key’ is not present return [-1, -1].

 - Example 1
 ```sh
Input: [4, 6, 6, 6, 9], key = 6
Output: [1, 3]
 ```
  - Example 2
 ```sh
Input: [1, 3, 8, 10, 15], key = 10
Output: [3, 3]
 ```
   - Example 3
 ```sh
Input: [1, 3, 8, 10, 15], key = 12
Output: [-1, -1]
 ```
The problem follows the Binary Search pattern. Since Binary Search helps us find a number in a sorted array efficiently, we can use a modified version of the Binary Search to find the first and the last position of a number.

We can use a similar approach as discussed in Order-agnostic Binary Search. We will try to search for the ‘key’ in the given array; if the ‘key’ is found (i.e. key == arr[middle) we have two options:

When trying to find the first position of the ‘key’, we can update end = middle - 1 to see if the key is present before middle.
When trying to find the last position of the ‘key’, we can update start = middle + 1 to see if the key is present after middle.
In both cases, we will keep track of the last position where we found the ‘key’. These positions will be the required range.

### Solution:
 - C++ Solution:
```sh
class solution {
 public:
  static pair<int, int> findRange(const vector<int>& arr, int key) {
    pair<int, int> res(-1, -1);
    res.first = search(arr, key, ture);
    res.second = search(arr, key, false);
    return res;
  }
  
  static int search(const vector<int>& arr, int key, bool flag){
      int res = -1;
      int l = 0;
      int r = arr.size();
      while(l <= r){
          int mid = l + (r - l)/2;
          if(arr[mid] > key){
              l = mid + 1;
          }
          else if(arr[mid] < key){
              r = mid - 1;
          }
          else{
              res = mid;
              if(flag){
                  r = mid - 1;
              }
              else{
                  l = mid + 1;
              }
          }
      }
      return res;
  }
};
```
  - Java Solution:
 ```sh
Class solution{
  public static int[] findRange(int[] arr, int key) {
    int[] result = new int[] { -1, -1 };
    result[0] = search(arr, key, false);
    if (result[0] != -1) // no need to search, if 'key' is not present in the input array
      result[1] = search(arr, key, true);
    return result;
  }

  // modified Binary Search
  private static int search(int[] arr, int key, boolean findMaxIndex) {
    int keyIndex = -1;
    int start = 0, end = arr.length - 1;
    while (start <= end) {
      int mid = start + (end - start) / 2;
      if (key < arr[mid]) {
        end = mid - 1;
      } else if (key > arr[mid]) {
        start = mid + 1;
      } else { // key == arr[mid]
        keyIndex = mid;
        if (findMaxIndex)
          start = mid + 1; // search ahead to find the last index of 'key'
        else
          end = mid - 1; // search behind to find the first index of 'key'
      }
    }
    return keyIndex;
  }
```
 - Time Complexity
   O(logN)
 - Space Complexity
O(1)
### Question 5 Search in a Sorted Infinite Array

Given an infinite sorted array (or an array with unknown size), find if a given number ‘key’ is present in the array. Write a function to return the index of the ‘key’ if it is present in the array, otherwise return -1.

Since it is not possible to define an array with infinite (unknown) size, you will be provided with an interface ArrayReader to read elements of the array. ArrayReader.get(index) will return the number at index; if the array’s size is smaller than the index, it will return Integer.MAX_VALUE.

 - Example 1
 ```sh
Input: [4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30], key = 16
Output: 6
Explanation: The key is present at index '6' in the array.
 ```
  - Example 2
 ```sh
Input: [4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30], key = 11
Output: -1
Explanation: The key is not present in the array.
 ```
   - Example 2
 ```sh
Input: [1, 3, 8, 10, 15], key = 15
Output: 4
Explanation: The key is present at index '4' in the array.
 ```
   - Example 2
 ```sh
Input: [1, 3, 8, 10, 15], key = 200
Output: -1
Explanation: The key is not present in the array.
 ```

The problem follows the Binary Search pattern. Since Binary Search helps us find a number in a sorted array efficiently, we can use a modified version of the Binary Search to find the ‘key’ in an infinite sorted array.

The only issue with applying binary search in this problem is that we don’t know the bounds of the array. To handle this situation, we will first find the proper bounds of the array where we can perform a binary search.

An efficient way to find the proper bounds is to start at the beginning of the array with the bound’s size as ‘1’ and exponentially increase the bound’s size (i.e., double it) until we find the bounds that can have the key.

### Solution:
 - C++ Solution:
```sh
class ArrayReader{
    public:
        vector<int> arr;
        ArrayReader(const vector<int> &arr){this->arr = arr;}
        virtual int get(int index){
            if(index >= arr.size()){
                return numeric_limits<int>::max();
            }
            return arr[index];
        }
}
class solution {
 public:
  static int search(ArrayReader *reader, int key) {
    int start = 0;
    int end = 0;
    while(reader.get(end) < key){
        int nstart = end + 1;
        end = nstart + (end - start);
        start = nstart
    }
    nsearch(ArrayReader *reader, key, start, end);
  }
 private:
  static int binarySearch(ArrayReader *reader, int key, int start, int end) {
    while (start <= end) {
      int mid = start + (end - start) / 2;
      if (key < reader->get(mid)) {
        end = mid - 1;
      } else if (key > reader->get(mid)) {
        start = mid + 1;
      } else {  // found the key
        return mid;
      }
    }
    return -1;
  }
};
```
  - Java Solution:
 ```sh
class ArrayReader {
  int[] arr;

  ArrayReader(int[] arr) {
    this.arr = arr;
  }

  public int get(int index) {
    if (index >= arr.length)
      return Integer.MAX_VALUE;
    return arr[index];
  }
}

class SearchInfiniteSortedArray {

  public static int search(ArrayReader reader, int key) {
    // find the proper bounds first
    int start = 0, end = 1;
    while (reader.get(end) < key) {
      int newStart = end + 1;
      end += (end - start + 1) * 2; // increase to double the bounds size
      start = newStart;
    }
    return binarySearch(reader, key, start, end);
  }

  private static int binarySearch(ArrayReader reader, int key, int start, int end) {
    while (start <= end) {
      int mid = start + (end - start) / 2;
      if (key < reader.get(mid)) {
        end = mid - 1;
      } else if (key > reader.get(mid)) {
        start = mid + 1;
      } else { // found the key
        return mid;
      }
    }

    return -1;
  }
```
 - Time Complexity
   O(logN)
 - Space Complexity
O(1)
### Question 6 Minimum Difference Element

Given an array of numbers sorted in ascending order, find the element in the array that has the minimum difference with the given ‘key’.

 - Example 1
  ```sh
Input: [4, 6, 10], key = 4
Output: 4
 ```
  - Example 2
 ```sh
Input: [4, 6, 10], key = 4
Output: 4
 ```
   - Example 3
 ```sh
Input: [1, 3, 8, 10, 15], key = 12
Output: 10
 ```
   - Example 4
 ```sh
Input: [4, 6, 10], key = 17
Output: 10
 ```
The problem follows the Binary Search pattern. Since Binary Search helps us find a number in a sorted array efficiently, we can use a modified version of the Binary Search to find the number that has the minimum difference with the given ‘key’.

We can use a similar approach as discussed in Order-agnostic Binary Search. We will try to search for the ‘key’ in the given array. If we find the ‘key’ we will return it as the minimum difference number. If we can’t find the ‘key’, (at the end of the loop) we can find the differences between the ‘key’ and the numbers pointed out by indices start and end, as these two numbers will be closest to the ‘key’. The number that gives minimum difference will be our required number.

### Solution:
 - C++ Solution:
```sh
class MinimumDifference {
 public:
  static int searchMinDiffElement(const vector<int>& arr, int key) {
    if (key < arr[0]) {
      return arr[0];
    }
    if (key > arr[arr.size() - 1]) {
      return arr[arr.size() - 1];
    }

    int start = 0, end = arr.size() - 1;
    while (start <= end) {
      int mid = start + (end - start) / 2;
      if (key < arr[mid]) {
        end = mid - 1;
      } else if (key > arr[mid]) {
        start = mid + 1;
      } else {
        return arr[mid];
      }
    }

    // at the end of the while loop, 'start == end+1'
    // we are not able to find the element in the given array
    // return the element which is closest to the 'key'
    if ((arr[start] - key) < (key - arr[end])) {
      return arr[start];
    }
    return arr[end];
  }
};
```
  - Java Solution:
 ```sh
Class solution{
  public static int searchMinDiffElement(int[] arr, int key) {
    if (key < arr[0])
      return arr[0];
    if (key > arr[arr.length - 1])
      return arr[arr.length - 1];

    int start = 0, end = arr.length - 1;
    while (start <= end) {
      int mid = start + (end - start) / 2;
      if (key < arr[mid]) {
        end = mid - 1;
      } else if (key > arr[mid]) {
        start = mid + 1;
      } else {
        return arr[mid];
      }
    }

    // at the end of the while loop, 'start == end+1'
    // we are not able to find the element in the given array
    // return the element which is closest to the 'key'
    if ((arr[start] - key) < (key - arr[end]))
      return arr[start];
    return arr[end];
  }
```
 - Time Complexity
   O(logN)
 - Space Complexity
O(1)
### Question 7 Bitonic Array Maximum

Find the maximum value in a given Bitonic array. An array is considered bitonic if it is monotonically increasing and then monotonically decreasing. Monotonically increasing or decreasing means that for any index i in the array arr[i] != arr[i+1].

 - Example 1
 ```sh
Input: [1, 3, 8, 12, 4, 2]
Output: 12
Explanation: The maximum number in the input bitonic array is '12'.
 ```
  - Example 2
 ```sh
Input: [3, 8, 3, 1]
Output: 8
 ```
   - Example 3
 ```sh
Input: [1, 3, 8, 12]
Output: 12
 ```
   - Example 4
 ```sh
Input: [10, 9, 8]
Output: 10
 ```
A bitonic array is a sorted array; the only difference is that its first part is sorted in ascending order and the second part is sorted in descending order. We can use a similar approach as discussed in Order-agnostic Binary Search. Since no two consecutive numbers are same (as the array is monotonically increasing or decreasing), whenever we calculate the middle, we can compare the numbers pointed out by the index middle and middle+1 to find if we are in the ascending or the descending part. So:

If arr[middle] > arr[middle + 1], we are in the second (descending) part of the bitonic array. Therefore, our required number could either be pointed out by middle or will be before middle. This means we will be doing: end = middle.
If arr[middle] < arr[middle + 1], we are in the first (ascending) part of the bitonic array. Therefore, the required number will be after middle. This means we will be doing: start = middle + 1.
We can break when start == end. Due to the two points mentioned above, both start and end will be pointing at the maximum number of the bitonic array.

### Solution:
 - C++ Solution:
```sh
class MaxInBitonicArray {
 public:
  static int findMax(const vector<int>& arr) {
    int start = 0, end = arr.size() - 1;
    while (start < end) {
      int mid = start + (end - start) / 2;
      if (arr[mid] > arr[mid + 1]) {
        end = mid;
      } else {
        start = mid + 1;
      }
    }

    // at the end of the while loop, 'start == end'
    return arr[start];
  }
};
```
  - Java Solution:
 ```sh
class MaxInBitonicArray {

  public static int findMax(int[] arr) {
    int start = 0, end = arr.length - 1;
    while (start < end) {
      int mid = start + (end - start) / 2;
      if (arr[mid] > arr[mid + 1]) {
        end = mid;
      } else {
        start = mid + 1;
      }
    }

    // at the end of the while loop, 'start == end'
    return arr[start];
  }
```
Time complexity #
O(logn)

Space complexity #
The space complexity of this algorithm will also be exponential, estimated at O(2^N)though the actual will be ( O(4^n/\sqrt{n}).
O(1)

### Question 8 Search Bitonic Array

Given an expression containing digits and operations (+, -, *), find all possible ways in which the expression can be evaluated by grouping the numbers and operators using parentheses.

 - Example 1
 ```sh
Input: [1, 3, 8, 4, 3], key=4
Output: 3
 ```
  - Example 2
 ```sh
Input: [3, 8, 3, 1], key=8
Output: 1
 ```
   - Example 3
 ```sh
Input: [1, 3, 8, 12], key=12
Output: 3
 ```
   - Example 4
 ```sh
Input: [10, 9, 8], key=10
Output: 0
 ```
The problem follows the Binary Search pattern. Since Binary Search helps us efficiently find a number in a sorted array we can use a modified version of the Binary Search to find the ‘key’ in the bitonic array.

Here is how we can search in a bitonic array:

First, we can find the index of the maximum value of the bitonic array, similar to Bitonic Array Maximum. Let’s call the index of the maximum number maxIndex.
Now, we can break the array into two sub-arrays:
Array from index ‘0’ to maxIndex, sorted in ascending order.
Array from index maxIndex+1 to array_length-1, sorted in descending order.
We can then call Binary Search separately in these two arrays to search the ‘key’. We can use the same Order-agnostic Binary Search for searching.

### Solution:
 - C++ Solution:
```sh
class SearchBitonicArray {
 public:
  static int search(const vector<int> &arr, int key) {
    int maxIndex = findMax(arr);
    int keyIndex = binarySearch(arr, key, 0, maxIndex);
    if (keyIndex != -1) {
      return keyIndex;
    }
    return binarySearch(arr, key, maxIndex + 1, arr.size() - 1);
  }

  // find index of the maximum value in a bitonic array
  static int findMax(const vector<int> &arr) {
    int start = 0, end = arr.size() - 1;
    while (start < end) {
      int mid = start + (end - start) / 2;
      if (arr[mid] > arr[mid + 1]) {
        end = mid;
      } else {
        start = mid + 1;
      }
    }

    // at the end of the while loop, 'start == end'
    return start;
  }

 private:
  // order-agnostic binary search
  static int binarySearch(const vector<int> &arr, int key, int start, int end) {
    while (start <= end) {
      int mid = start + (end - start) / 2;

      if (key == arr[mid]) {
        return mid;
      }

      if (arr[start] < arr[end]) {  // ascending order
        if (key < arr[mid]) {
          end = mid - 1;
        } else {  // key > arr[mid]
          start = mid + 1;
        }
      } else {  // descending order
        if (key > arr[mid]) {
          end = mid - 1;
        } else {  // key < arr[mid]
          start = mid + 1;
        }
      }
    }
    return -1;  // element is not found
  }
};
```
  - Java Solution:
 ```sh
class SearchBitonicArray {

  public static int search(int[] arr, int key) {
    int maxIndex = findMax(arr);
    int keyIndex = binarySearch(arr, key, 0, maxIndex);
    if (keyIndex != -1)
      return keyIndex;
    return binarySearch(arr, key, maxIndex + 1, arr.length - 1);
  }

  // find index of the maximum value in a bitonic array
  public static int findMax(int[] arr) {
    int start = 0, end = arr.length - 1;
    while (start < end) {
      int mid = start + (end - start) / 2;
      if (arr[mid] > arr[mid + 1]) {
        end = mid;
      } else {
        start = mid + 1;
      }
    }

    // at the end of the while loop, 'start == end'
    return start;
  }

  // order-agnostic binary search
  private static int binarySearch(int[] arr, int key, int start, int end) {
    while (start <= end) {
      int mid = start + (end - start) / 2;

      if (key == arr[mid])
        return mid;

      if (arr[start] < arr[end]) { // ascending order
        if (key < arr[mid]) {
          end = mid - 1;
        } else { // key > arr[mid]
          start = mid + 1;
        }
      } else { // descending order        
        if (key > arr[mid]) {
          end = mid - 1;
        } else { // key < arr[mid]
          start = mid + 1;
        }
      }
    }
    return -1; // element is not found
  }
```
Time complexity #
O(logN)

Space complexity #
O(1)
### Question 9 Search in Rotated Array

Given an array of numbers which is sorted in ascending order and also rotated by some arbitrary number, find if a given ‘key’ is present in it.

Write a function to return the index of the ‘key’ in the rotated array. If the ‘key’ is not present, return -1. You can assume that the given array does not have any duplicates.
 - Example 1
 ```sh
Input: [10, 15, 1, 3, 8], key = 15
Output: 1
Explanation: '15' is present in the array at index '1'.
 ```
  - Example 2
 ```sh
Input: [4, 5, 7, 9, 10, -1, 2], key = 10
Output: 4
Explanation: '10' is present in the array at index '4'.
 ```
The problem follows the Binary Search pattern. We can use a similar approach as discussed in Order-agnostic Binary Search and modify it similar to Search Bitonic Array to search for the ‘key’ in the rotated array.

After calculating the middle, we can compare the numbers at indices start and middle. This will give us two options:

If arr[start] <= arr[middle], the numbers from start to middle are sorted in ascending order.
Else, the numbers from middle+1 to end are sorted in ascending order.
Once we know which part of the array is sorted, it is easy to adjust our ranges. For example, if option-1 is true, we have two choices:

By comparing the ‘key’ with the numbers at index start and middle we can easily find out if the ‘key’ lies between indices start and middle; if it does, we can skip the second part => end = middle -1.
Else, we can skip the first part => start = middle + 1.

### Solution:
 - C++ Solution:
```sh
class TreeNode{
public:
    int val = 0;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x){
        val = x;
    }
}
class solution {
 public:
  static int search(const vector<int>& arr, int key) {
    int start = 0, end = arr.size() - 1;
    while (start <= end) {
      int mid = start + (end - start) / 2;
      if (arr[mid] == key) {
        return mid;
      }

      if (arr[start] <= arr[mid]) {  // left side is sorted in ascending order
        if (key >= arr[start] && key < arr[mid]) {
          end = mid - 1;
        } else {  // key > arr[mid]
          start = mid + 1;
        }
      } else {  // right side is sorted in ascending order
        if (key > arr[mid] && key <= arr[end]) {
          start = mid + 1;
        } else {
          end = mid - 1;
        }
      }
    }

    // we are not able to find the element in the given array
    return -1;
  }
};
```
  - Java Solution:
 ```sh
class TreeNode {
  int val;
  TreeNode left;
  TreeNode right;

  TreeNode(int x) {
    val = x;
  }
};

class UniqueTrees {

  public static int search(int[] arr, int key) {
    int start = 0, end = arr.length - 1;
    while (start <= end) {
      int mid = start + (end - start) / 2;
      if (arr[mid] == key)
        return mid;

      if (arr[start] <= arr[mid]) { // left side is sorted in ascending order
        if (key >= arr[start] && key < arr[mid]) {
          end = mid - 1;
        } else { //key > arr[mid]
          start = mid + 1;
        }
      } else { // right side is sorted in ascending order       
        if (key > arr[mid] && key <= arr[end]) {
          start = mid + 1;
        } else {
          end = mid - 1;
        }
      }
    }

    // we are not able to find the element in the given array
    return -1;
  }
```
Time complexity #
O(logN)

Space complexity #
The space complexity of this algorithm will also be exponential, estimated at O(2^N)though the actual will be ( O(4^n/\sqrt{n}).
O(1)

### __Question 10 Rotation Count__

Given an array of numbers which is sorted in ascending order and is rotated ‘k’ times around a pivot, find ‘k’.

You can assume that the array does not have any duplicates.

 - Example 1
 ```sh
Input: [10, 15, 1, 3, 8]
Output: 2
Explanation: The array has been rotated 2 times.
 ```
  - Example 2
 ```sh
Input: [4, 5, 7, 9, 10, -1, 2]
Output: 5
Explanation: The array has been rotated 5 times.
 ```
   - Example 3
 ```sh
Input: [1, 3, 8, 10]
Output: 0
Explanation: The array has not been rotated.
 ```
This problem follows the Binary Search pattern. We can use a similar strategy as discussed in Search in Rotated Array.

In this problem, actually, we are asked to find the index of the minimum element. The number of times the minimum element is moved to the right will be equal to the number of rotations. An interesting fact about the minimum element is that it is the only element in the given array which is smaller than its previous element. Since the array is sorted in ascending order, all other elements are bigger than their previous element.

After calculating the middle, we can compare the number at index middle with its previous and next number. This will give us two options:

If arr[middle] > arr[middle + 1], then the element at middle + 1 is the smallest.
If arr[middle - 1] > arr[middle], then the element at middle is the smallest.
To adjust the ranges we can follow the same approach as discussed in Search in Rotated Array. Comparing the numbers at indices start and middle will give us two options:

If arr[start] < arr[middle], the numbers from start to middle are sorted.
Else, the numbers from middle + 1 to end are sorted.

### Solution:
 - C++ Solution:
```sh
class solution {
 public:
  static int countRotations(const vector<int>& arr) {
    int start = 0, end = arr.size() - 1;
    while (start < end) {
      int mid = start + (end - start) / 2;

      if (mid < end && arr[mid] > arr[mid + 1]) {  // if mid is greater than the next element
        return mid + 1;
      }
      if (mid > start && arr[mid - 1] > arr[mid]) {  // if mid is smaller than the previous element
        return mid;
      }

      if (arr[start] < arr[mid]) {  // left side is sorted, so the pivot is on right side
        start = mid + 1;
      } else {  // right side is sorted, so the pivot is on the left side
        end = mid - 1;
      }
    }

    return 0;  // the array has not been rotated
  }
};
```
  - Java Solution:
 ```sh
class RotationCountOfRotatedArray {

  public static int countRotations(int[] arr) {
    int start = 0, end = arr.length - 1;
    while (start < end) {
      int mid = start + (end - start) / 2;

      if (mid < end && arr[mid] > arr[mid + 1]) // if mid is greater than the next element
        return mid + 1;
      if (mid > start && arr[mid - 1] > arr[mid]) // if mid is smaller than the previous element
        return mid;

      if (arr[start] < arr[mid]) { // left side is sorted, so the pivot is on right side
        start = mid + 1;
      } else { // right side is sorted, so the pivot is on the left side     
        end = mid - 1;
      }
    }

    return 0; // the array has not been rotated
  }
```
Time complexity #
O(logN)

Space complexity #
The space complexity of this algorithm will also be exponential, estimated at O(2^N)though the actual will be ( O(4^n/\sqrt{n}).
0(1)
### Todos

 - Write MORE Tests
 - Add Night Mode

License
----

MIT
