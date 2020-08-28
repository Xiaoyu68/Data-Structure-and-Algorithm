# Topic 2: Two Pointers


[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

In problems where we deal with sorted arrays (or LinkedLists) and need to find a set of elements that fulfill certain constraints, the Two Pointers approach becomes quite useful. The set of elements could be a pair, a triplet or even a subarray. For example, take a look at the following problem:

  - Given an array of sorted numbers and a target sum, find a pair in the array whose sum is equal to the given target.
 
To solve this problem, we can consider each element one by one (pointed out by the first pointer) and iterate through the remaining elements (pointed out by the second pointer) to find a pair with the given sum. The time complexity of this algorithm will be O(N^2) where ‘N’ is the number of elements in the input array.

Given that the input array is sorted, an efficient way would be to start with one pointer in the beginning and another pointer at the end. At every step, we will see if the numbers pointed by the two pointers add up to the target sum. If they do not, we will do one of two things:

If the sum of the two numbers pointed by the two pointers is greater than the target sum, this means that we need a pair with a smaller sum. So, to try more pairs, we can decrement the end-pointer.
If the sum of the two numbers pointed by the two pointers is smaller than the target sum, this means that we need a pair with a larger sum. So, to try more pairs, we can increment the start-pointer.

### Question 1 Pair with Target Sum
Given an array of sorted numbers and a target sum, find a pair in the array whose sum is equal to the given target.
Write a function to return the indices of the two numbers (i.e. the pair) such that they add up to the given target.

Example 1:
```sh
Input: [1, 2, 3, 4, 6], target=6
Output: [1, 3]
Explanation: The numbers at index 1 and 3 add up to 6: 2+4=6
```
Example 2:
```sh
Input: [2, 5, 9, 11], target=11
Output: [0, 2]
Explanation: The numbers at index 0 and 2 add up to 11: 2+9=11
```
Since the given array is sorted, a brute-force solution could be to iterate through the array, taking one number at a time and searching for the second number through Binary Search. The time complexity of this algorithm will be O(N*logN)O(N∗logN). Can we do better than this?

We can follow the Two Pointers approach. We will start with one pointer pointing to the beginning of the array and another pointing at the end. At every step, we will see if the numbers pointed by the two pointers add up to the target sum. If they do, we have found our pair; otherwise, we will do one of two things:

If the sum of the two numbers pointed by the two pointers is greater than the target sum, this means that we need a pair with a smaller sum. So, to try more pairs, we can decrement the end-pointer.
If the sum of the two numbers pointed by the two pointers is smaller than the target sum, this means that we need a pair with a larger sum. So, to try more pairs, we can increment the start-pointer.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static pair<int, int> search(const vector<int>& arr, int targetSum){
        int l = 0;
        int r = arr.size() - 1;
        while(l < r){
            if(arr[l] + arr[r] > targetSum){
                r--;
            }
            else if(arr[l] + arr[r] < targetSum){
                l++;
            }
            else{
                return make_pair(l,r);
            }
        }
        return make_pair(-1, -1);
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static int[] search(int[] arr, int targetSum){
        int l = 0;
        int r = arr.length - 1;
        while(l < r){
            if(arr[l] + arr[r] > targetSum){
                r--;
            }
            else if(arr[l] + arr[r] < targetSum){
                l++;
            }
            else{
                return new int[]{l, r};
            }
        }
        return new int[]{-1, -1};
    }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 2 Remove Duplicates
Given an array of sorted numbers, remove all duplicates from it. You should not use any extra space; after removing the duplicates in-place return the new length of the array.
 - Example 1:
```sh
Input: [2, 3, 3, 3, 6, 9, 9]
Output: 4
Explanation: The first four elements after removing the duplicates will be [2, 3, 6, 9].
```
 - Example 2:
```sh
Input: [2, 2, 2, 11]
Output: 2
Explanation: The first two elements after removing the duplicates will be [2, 11].
```

In this problem, we need to remove the duplicates in-place such that the resultant length of the array remains sorted. As the input array is sorted, therefore, one way to do this is to shift the elements left whenever we encounter duplicates. In other words, we will keep one pointer for iterating the array and one pointer for placing the next non-duplicate number. So our algorithm will be to iterate the array and whenever we see a non-duplicate number we move it next to the last non-duplicate number we’ve seen.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static int remove(vector<int>& arr){
        int l = 1;
        for(int i = 1;i < arr.size();i++){
            if(arr[l - 1] != arr[i]){
                arr[l] = arr[i];
                l++;
            }
        }
        return l;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static int Remove(int[] arr){
        int l = 1;
        for(int i = 1;i < arr.length;i++){
            if(arr[l - 1] != arr[i]){
                arr[l] = arr[i];
                l++;
            }
        }
        return l;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 3 Squaring a Sorted Array
Given a sorted array, create a new array containing squares of all the number of the input array in the sorted order.
 - Example 1:
```sh
Input: [-2, -1, 0, 2, 3]
Output: [0, 1, 4, 4, 9]
```
 - Example 2:
```sh
Input: [-3, -1, 0, 1, 2]
Output: [0 1 1 4 9]
```

This is a straightforward question. The only trick is that we can have negative numbers in the input array, which will make it a bit difficult to generate the output array with squares in sorted order.

An easier approach could be to first find the index of the first non-negative number in the array. After that, we can use Two Pointers to iterate the array. One pointer will move forward to iterate the non-negative numbers and the other pointer will move backward to iterate the negative numbers. At any step, whichever number gives us a bigger square will be added to the output array. 

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static vector<int> makeSquares(const vector<int>& arr){
        int l = 0;
        int r = arr.size() - 1;
        vector<int> res(arr.size());
        for(int i = arr.size() - 1;i >= 0;i--){
            if(arr[l]*arr[l] >= arr[r]*arr[r]){
                res[i] = arr[l]*arr[l];
                l++;
            }
            else{
                res[i] = arr[r]*arr[r];
                r--;
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
    public static int makeSquares(int[] arr){
        int l = 0;
        int r = arr.length - 1;
        int[] res = new int[arr.length];
        for(int i = 0;i < arr.length;i++){
            if(arr[l]*arr[l] >= arr[r]*arr[r]){
                res[i] = arr[l]*arr[l];
                l++;
            }
            else{
                res[i] = arr[r]*arr[r];
                r--;
            }
        }
        return res;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(N)

### Question4 Triplet Sum to Zero
Given an array of unsorted numbers, find all unique triplets in it that add up to zero.
 - Example 1:
```sh
Input: [-3, 0, 1, 2, -1, 1, -2]
Output: [-3, 1, 2], [-2, 0, 2], [-2, 1, 1], [-1, 0, 1]
Explanation: There are four unique triplets whose sum is equal to zero.
```
 - Example 2:
```sh
Input: [-5, 2, -1, -2, 3]
Output: [[-5, 2, 3], [-2, -1, 3]]
Explanation: There are two unique triplets whose sum is equal to zero.
```

This problem follows the Two Pointers pattern and shares similarities with Pair with Target Sum. A couple of differences are that the input array is not sorted and instead of a pair we need to find triplets with a target sum of zero.

To follow a similar approach, first, we will sort the array and then iterate through it taking one number at a time. Let’s say during our iteration we are at number ‘X’, so we need to find ‘Y’ and ‘Z’ such that X + Y + Z == 0X+Y+Z==0. At this stage, our problem translates into finding a pair whose sum is equal to “-X−X” (as from the above equation Y + Z == -XY+Z==−X).

Another difference from Pair with Target Sum is that we need to find all the unique triplets. To handle this, we have to skip any duplicate number. Since we will be sorting the array, so all the duplicate numbers will be next to each other and are easier to skip.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static vector<vector<int>> searchTriplets(vector<int>& arr){
        sort(arr.begin(), arr.end());
        vector<vector<int>> res;
        for(int i = 0;i < arr.size() - 2;i++){
            if(arr[i] != 0 && arr[i] == arr[i - 1]){
                continue;
            }
            searchPair(arr, -arr[i], i+1, arr.size() - 1, res);
        }
        return res;
    }
    static void searchPair(vector<int>& arr, int targetSum, int l, int r, vector<vector<int>>& res){
            while(l < r){
                if(arr[l] + arr[r] == targetSum){
                    res.push_back({-targetSum, arr[l], arr[r]});
                    r--;
                    l++;
                    while(l < r && arr[l] == arr[l - 1]){
                        l++;
                    }
                    while(l < r && arr[r] == arr[r + 1]){
                        r++;
                    }
                }
                else if(arr[l] + arr[r] > targetSum){
                    r--;
                }
                else{
                    l++;
                }
            }
        }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static List<List<Integer>> searchTriplets(int[] arr){
        Arrays.sort(arr);
        List<List<Integer>> res = new ArrayList<>();
        for(int i = 0;i < arr.length - 2;i++){
            if(arr[i] != 0 && arr[i] == arr[i - 1]){
                continue;
            }
            searchPair(arr, -arr[i], i+1, arr.size() - 1, res);
        }
        return res;
    }
    static void searchPair(int[] arr, int targetSum, int l, int r, List<List<Integer>> res){
            while(l < r){
                if(arr[l] + arr[r] == targetSum){
                    res.add(Arrays.asList(-targetSum, arr[l], arr[r]));
                    r--;
                    l++;
                    while(l < r && arr[l] == arr[l - 1]){
                        l++;
                    }
                    while(l < r && arr[r] == arr[r + 1]){
                        r++;
                    }
                }
                else if(arr[l] + arr[r] > targetSum){
                    r--;
                }
                else{
                    l++;
                }
            }
        }
};
```
Time Complexity: O(N)
Space Complexity: O(N)

### Todos

 - Write MORE Tests
 - Add Night Mode

License
----

MIT
