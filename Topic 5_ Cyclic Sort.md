# Topic 5: Cyclic Sort


[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

This pattern describes an interesting approach to deal with problems involving arrays containing numbers in a given range. For example, take the following problem:

You are given an unsorted array containing numbers taken from the range 1 to ‘n’. The array can have duplicates, which means that some numbers will be missing. Find all the missing numbers.

To efficiently solve this problem, we can use the fact that the input array contains numbers in the range of 1 to ‘n’. For example, to efficiently sort the array, we can try placing each number in its correct place, i.e., placing ‘1’ at index ‘0’, placing ‘2’ at index ‘1’, and so on. Once we are done with the sorting, we can iterate the array to find all indices that are missing the correct numbers. These will be our required numbers.

Let’s jump on to our first problem to understand the Cyclic Sort pattern in detail.
### Question 1 Cyclic Sort

We are given an array containing ‘n’ objects. Each object, when created, was assigned a unique number from 1 to ‘n’ based on their creation sequence. This means that the object with sequence number ‘3’ was created just before the object with sequence number ‘4’.

Write a function to sort the objects in-place on their creation sequence number in O(n)O(n) and without any extra space. For simplicity, let’s assume we are passed an integer array containing only the sequence numbers, though each number is actually an object.

Example 1:
```sh
Input: [3, 1, 5, 4, 2]
Output: [1, 2, 3, 4, 5]
```
Example 2:
```sh
Input: [2, 6, 4, 3, 1, 5]
Output: [1, 2, 3, 4, 5, 6]
```
Example 3:
```sh
Input: [1, 5, 6, 4, 3, 2]
Output: [1, 2, 3, 4, 5, 6]
```
As we know, the input array contains numbers in the range of 1 to ‘n’. We can use this fact to devise an efficient way to sort the numbers. Since all numbers are unique, we can try placing each number at its correct place, i.e., placing ‘1’ at index ‘0’, placing ‘2’ at index ‘1’, and so on.

To place a number (or an object in general) at its correct index, we first need to find that number. If we first find a number and then place it at its correct place, it will take us O(N^2), which is not acceptable.

Instead, what if we iterate the array one number at a time, and if the current number we are iterating is not at the correct index, we swap it with the number at its correct index. This way we will go through all numbers and place them in their correct indices, hence, sorting the whole array.


### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  void sort(vector<int>& nums){
        int i = 0;
        while(i < nums.size()){
            int j = nums[i] - 1;
            if(i != j){
                swap(nums[i],nums[j]);
            }
            else{
                i++;
            }
        }
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static void sort(int[] nums){
        int i = 0;
        while(i < nums.length){
            int j = nums[i] - 1;
            if(i != j){
                swap(nums[i],nums[j]);
            }
            else{
                i++;
            }
        }
    }
};
```
Time complexity #
O(N)

Space complexity #
O(1)

### Question 2 Find the Missing Number
We are given an array containing ‘n’ distinct numbers taken from the range 0 to ‘n’. Since the array has only ‘n’ numbers out of the total ‘n+1’ numbers, find the missing number.
 - Example 1:
```sh
Input: [4, 0, 3, 1]
Output: 2
```
 - Example 2:
```sh
Input: [8, 3, 5, 2, 4, 6, 0, 1]
Output: 7
```
This problem follows the Cyclic Sort pattern. Since the input array contains unique numbers from the range 0 to ‘n’, we can use a similar strategy as discussed in Cyclic Sort to place the numbers on their correct index. Once we have every number in its correct place, we can iterate the array to find the index which does not have the correct number, and that index will be our missing number.

However, there are two differences with Cyclic Sort:

In this problem, the numbers are ranged from ‘0’ to ‘n’, compared to ‘1’ to ‘n’ in the Cyclic Sort. This will make two changes in our algorithm:
In this problem, each number should be equal to its index, compared to index + 1 in the Cyclic Sort. Therefore => nums[i] == nums[nums[i]]
Since the array will have ‘n’ numbers, which means array indices will range from 0 to ‘n-1’. Therefore, we will ignore the number ‘n’ as we can’t place it in the array, so => nums[i] < nums.length
Say we are at index i. If we swap the number at index i to place it at the correct index, we can still have the wrong number at index i. This was true in Cyclic Sort too. It didn’t cause any problems in Cyclic Sort as over there, we made sure to place one number at its correct place in each step, but that wouldn’t be enough in this problem as we have one extra number due to the larger range. Therefore, we will not move to the next number after the swap until we have a correct number at the index i.


### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static int findMissingNumber(vector<int>& nums){
        int i = 0;
        while(i < nums.size()){
            if(nums[i] < nums.size() && nums[i] != nums[nums[i]]){
                swap(nums[i], nums[nums[i]]);
            }
            else{
                i++;
            }
        }
        for(int i = 0;i < nums.size();i++){
            if(nums[i] != i){
                return i;
            }
        }
        return nums.size();
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static int sort(int[] nums){
        int i = 0;
        while(i < nums.length){
            if(nums[i] < nums.length && nums[i] != nums[nums[i]]){
                swap(nums[i], nums[nums[i]]);
            }
            else{
                i++;
            }
        }
        for(int i = 0;i < nums.length; i++){
            if(i != nums[i]){
                return i;
            }
        }
        return nums.length;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 3 Find all Missing Numbers
We are given an unsorted array containing numbers taken from the range 1 to ‘n’. The array can have duplicates, which means some numbers will be missing. Find all those missing numbers.
 - Example 1:
```sh
Input: [2, 3, 1, 8, 2, 3, 5, 1]
Output: 4, 6, 7
Explanation: The array should have all numbers from 1 to 8, due to duplicates 4, 6, and 7 are missing.
```
 - Example 2:
```sh
Input: [2, 4, 1, 2]
Output: 3
```
 - Example 3:
```sh
Input: [2, 3, 2, 1]
Output: 4
```
This problem follows the Cyclic Sort pattern and shares similarities with Find the Missing Number with one difference. In this problem, there can be many duplicates whereas in ‘Find the Missing Number’ there were no duplicates and the range was greater than the length of the array.

However, we will follow a similar approach though as discussed in Find the Missing Number to place the numbers on their correct indices. Once we are done with the cyclic sort we will iterate the array to find all indices that are missing the correct numbers.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static vector<int> findNumbers(vector<int>& nums){
        vector<int> res;
        int i = 0;
        while(i < nums.size()){
            if(nums[i] != nums[nums[i] - 1]){
                swap(nums[i], nums[nums[i] - 1]);
            }
            else{
                i++;
            }
        }
        for(int i = 0;i < nums.size(); i++){
            if(i != nums[i] - 1){
                res.push_back(i + 1)
            }
        }
        return res;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
     static List<Integer> finNumbers(int[] nums){
        List<Interval> res = new ArrayList<>();
        int i = 0;
        while(i < nums.length){
            if(nums[i] != nums[nums[i] - 1]){
                swap(nums[i], nums[nums[i] - 1]);
            }
            else{
                i++;
            }
        }
        for(int i = 0;i < nums.length; i++){
            if(i != nums[i] - 1){
                res.add(i + 1)
            }
        }
        return res;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 4 Find the Duplicate Number
We are given an unsorted array containing ‘n+1’ numbers taken from the range 1 to ‘n’. The array has only one duplicate but it can be repeated multiple times. Find that duplicate number without using any extra space. You are, however, allowed to modify the input array.
 - Example 1:
```sh
Input: [1, 4, 4, 3, 2]
Output: 4
```
 - Example 2:
```sh
Input: [2, 1, 3, 3, 5, 4]
Output: 3
```
 - Example 3:
```sh
Input: [2, 4, 1, 4, 4]
Output: 4
```
### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static int findNumber(vector<int>& nums){
        int i = 0;
        while(i < nums.size()){
            if(nums[i] != i + 1){
                if(nums[i] != nums[nums[i] - 1]){
                    swap(nums[i], nums[nums[i] - 1]);
                }
                else{
                    return nums[i];
                }
            }
            else{
                i++;
            }
        }
        return -1;
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static int findNumber(int[] nums){
        int i = 0;
        while(i < nums.length){
            if(nums[i] != i + 1){
                if(nums[i] != nums[nums[i] - 1]){
                    swap(nums[i], nums[nums[i] - 1]);
                }
                else{
                    return nums[i];
                }
            }
            else{
                i++;
            }
        }
        return -1;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 5 Find all Duplicate Numbers
We are given an unsorted array containing ‘n’ numbers taken from the range 1 to ‘n’. The array has some duplicates, find all the duplicate numbers without using any extra space.
 - Example 1:
```sh
Input: [3, 4, 4, 5, 5]
Output: [4, 5]
```
 - Example 2:
```sh
Input: [5, 4, 7, 2, 3, 5, 3]
Output: [3, 5]
```
This problem follows the Cyclic Sort pattern and shares similarities with Find the Duplicate Number. Following a similar approach, we will place each number at its correct index. After that, we will iterate through the array to find all numbers that are not at the correct indices. All these numbers are duplicates.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static vector<int> findNumbers(vector<int>& nums){
        vector<int> res;
        int i = 0;
        while(i < nums.size()){
            if(nums[i] != nums[nums[i] - 1]){
                swap(nums[i], nums[nums[i] - 1]);
            }
            else{
                i++;
            }
        }
        for(int i = 0;i < nums.size(); i++){
            if(i != nums[i] - 1){
                res.push_back(nums[i]);
            }
        }
        return res;
    }
};
```
  - Java Solution:
 ```sh
Class solution{
     static List<Integer> finNumbers(int[] nums){
        List<Interval> res = new ArrayList<>();
        int i = 0;
        while(i < nums.length){
            if(nums[i] != nums[nums[i] - 1]){
                swap(nums[i], nums[nums[i] - 1]);
            }
            else{
                i++;
            }
        }
        for(int i = 0;i < nums.length; i++){
            if(i != nums[i] - 1){
                res.add(nums[i]);
            }
        }
        return res;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 6 Find the Corrupt Pair
We are given an unsorted array containing ‘n’ numbers taken from the range 1 to ‘n’. The array originally contained all the numbers from 1 to ‘n’, but due to a data error, one of the numbers got duplicated which also resulted in one number going missing. Find both these numbers.

 - Example 1:
```sh
Input: [3, 1, 2, 5, 2]
Output: [2, 4]
Explanation: '2' is duplicated and '4' is missing.
```
 - Example 2:
```sh
Input: [3, 1, 2, 3, 6, 4]
Output: [3, 5]
Explanation: '3' is duplicated and '5' is missing.
```
This problem follows the Cyclic Sort pattern and shares similarities with Find all Duplicate Numbers. Following a similar approach, we will place each number at its correct index. Once we are done with the cyclic sort, we will iterate through the array to find the number that is not at the correct index. Since only one number got corrupted, the number at the wrong index is the duplicated number and the index itself represents the missing number.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static vector<int> findNumbers(vector<int>& nums){
        vector<int> res;
        int i = 0;
        while(i < nums.size()){
            if(nums[i] != nums[nums[i] - 1]){
                swap(nums[i], nums[nums[i] - 1]);
            }
            else{
                i++;
            }
        }
        for(int i = 0;i < nums.size(); i++){
            if(i != nums[i] - 1){
                res.push_back(nums[i]);
                res.push_back(i + 1);
            }
        }
        return res;
    }
};
```
  - Java Solution:
 ```sh
Class solution{
     static List<Integer> finNumbers(int[] nums){
        List<Interval> res = new ArrayList<>();
        int i = 0;
        while(i < nums.length){
            if(nums[i] != nums[nums[i] - 1]){
                swap(nums[i], nums[nums[i] - 1]);
            }
            else{
                i++;
            }
        }
        for(int i = 0;i < nums.length; i++){
            if(i != nums[i] - 1){
                res.add(nums[i]);
                res.addd(i + 1);
            }
        }
        return res;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 7 Find the Smallest Missing Positive Number
Given an unsorted array containing numbers, find the smallest missing positive number in it.
 - Example 1:
```sh
Input: [-3, 1, 5, 4, 2]
Output: 3
Explanation: The smallest missing positive number is '3'
```
 - Example 2:
```sh
Input: [3, -2, 0, 1, 2]
Output: 4
```
 - Example 3:
```sh
Input: [3, 2, 5, 1]
Output: 4
```
This problem follows the Cyclic Sort pattern and shares similarities with Find the Missing Number with one big difference. In this problem, the numbers are not bound by any range so we can have any number in the input array.

However, we will follow a similar approach though as discussed in Find the Missing Number to place the numbers on their correct indices and ignore all numbers that are out of the range of the array (i.e., all negative numbers and all numbers greater than or equal to the length of the array). Once we are done with the cyclic sort we will iterate the array and the first index that does not have the correct number will be the smallest missing positive number!

### Solution:
 - C++ Solution:
 ```sh
Class solution{
 public:
  static int findNumber(vector<int> &nums) {
    int i = 0;
    while (i < nums.size()) {
      if (nums[i] > 0 && nums[i] <= nums.size() && nums[i] != nums[nums[i] - 1]) {
        swap(nums, i, nums[i] - 1);
      } else {
        i++;
      }
    }

    for (i = 0; i < nums.size(); i++) {
      if (nums[i] != i + 1) {
        return i + 1;
      }
    }

    return nums.size() + 1;
  }
};
```
  - Java Solution:
 ```sh
Class solution{
  public static int findNumber(int[] nums) {
    int i = 0;
    while (i < nums.length) {
      if (nums[i] > 0 && nums[i] <= nums.length && nums[i] != nums[nums[i] - 1])
        swap(nums, i, nums[i] - 1);
      else
        i++;
    }
    
    for (i = 0; i < nums.length; i++)
      if (nums[i] != i + 1)
        return i + 1;

    return nums.length + 1;
  }
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 8 Find the First K Missing Positive Numbers
Given an unsorted array containing numbers and a number ‘k’, find the first ‘k’ missing positive numbers in the array.
 - Example 1:
```sh
Input: [3, -1, 4, 5, 5], k=3
Output: [1, 2, 6]
Explanation: The smallest missing positive numbers are 1, 2 and 6.
```
 - Example 2:
```sh
Input: [2, 3, 4], k=3
Output: [1, 5, 6]
Explanation: The smallest missing positive numbers are 1, 5 and 6.
```
 - Example 3:
```sh
Input: [-2, -3, 4], k=2
Output: [1, 2]
Explanation: The smallest missing positive numbers are 1 and 2.
```
This problem follows the Cyclic Sort pattern and shares similarities with Find the Smallest Missing Positive Number. The only difference is that, in this problem, we need to find the first ‘k’ missing numbers compared to only the first missing number.

We will follow a similar approach as discussed in Find the Smallest Missing Positive Number to place the numbers on their correct indices and ignore all numbers that are out of the range of the array. Once we are done with the cyclic sort we will iterate through the array to find indices that do not have the correct numbers.

If we are not able to find ‘k’ missing numbers from the array, we need to add additional numbers to the output array. To find these additional numbers we will use the length of the array. For example, if the length of the array is 4, the next missing numbers will be 4, 5, 6 and so on. One tricky aspect is that any of these additional numbers could be part of the array. Remember, while sorting, we ignored all numbers that are greater than or equal to the length of the array. So all indices that have the missing numbers could possibly have these additional numbers. To handle this, we must keep track of all numbers from those indices that have missing numbers. Let’s understand this with an example:

    nums: [2, 1, 3, 6, 5], k =2
After the cyclic sort our array will look like:

    nums: [1, 2, 3, 6, 5]
From the sorted array we can see that the first missing number is ‘4’ (as we have ‘6’ on the fourth index) but to find the second missing number we need to remember that the array does contain ‘6’. Hence, the next missing number is ‘7’.

### Solution:
 - C++ Solution:
 ```sh
Class solution{
 public:
  static vector<int> findNumbers(vector<int> &nums, int k) {
    int i = 0;
    while (i < nums.size()) {
      if (nums[i] > 0 && nums[i] <= nums.size() && nums[i] != nums[nums[i] - 1]) {
        swap(nums, i, nums[i] - 1);
      } else {
        i++;
      }
    }

    vector<int> missingNumbers;
    unordered_set<int> extraNumbers;
    for (i = 0; i < nums.size() && missingNumbers.size() < k; i++) {
      if (nums[i] != i + 1) {
        missingNumbers.push_back(i + 1);
        extraNumbers.insert(nums[i]);
      }
    }

    // add the remaining missing numbers
    for (i = 1; missingNumbers.size() < k; i++) {
      int candidateNumber = i + nums.size();
      // ignore if the array contains the candidate number
      if (extraNumbers.find(candidateNumber) == extraNumbers.end()) {
        missingNumbers.push_back(candidateNumber);
      }
    }

    return missingNumbers;
  }
```
  - Java Solution:
 ```sh
Class solution{
  public static List<Integer> findNumbers(int[] nums, int k) {
    int i = 0;
    while (i < nums.length) {
      if (nums[i] > 0 && nums[i] <= nums.length && nums[i] != nums[nums[i] - 1])
        swap(nums, i, nums[i] - 1);
      else
        i++;
    }

    List<Integer> missingNumbers = new ArrayList<>();
    Set<Integer> extraNumbers = new HashSet<>();
    for (i = 0; i < nums.length && missingNumbers.size() < k; i++)
      if (nums[i] != i + 1) {
        missingNumbers.add(i + 1);
        extraNumbers.add(nums[i]);
      }

    // add the remaining missing numbers
    for (i = 1; missingNumbers.size() < k; i++) {
      int candidateNumber = i + nums.length;
      // ignore if the array contains the candidate number
      if (!extraNumbers.contains(candidateNumber))
        missingNumbers.add(candidateNumber);
    }

    return missingNumbers;
  }

  private static void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
  }
```
Time Complexity: O(N + k)
Space Complexity: O(k)
### Todos

 - Write MORE Tests
 - Add Night Mode

License
----

MIT
