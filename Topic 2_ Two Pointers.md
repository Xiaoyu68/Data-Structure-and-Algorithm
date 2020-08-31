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

### Question 4 Triplet Sum to Zero
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
Time Complexity: O(N^2)
Space Complexity: O(N)

### Question 5 Triplet Sum Close to Target
Given an array of unsorted numbers and a target number, find a triplet in the array whose sum is as close to the target number as possible, return the sum of the triplet. If there are more than one such triplet, return the sum of the triplet with the smallest sum.
 - Example 1:
```sh
Input: [-2, 0, 1, 2], target=2
Output: 1
Explanation: The triplet [-2, 1, 2] has the closest sum to the target.
```
 - Example 2:
```sh
Input: [-3, -1, 1, 2], target=1
Output: 0
Explanation: The triplet [-3, 1, 2] has the closest sum to the target.
```
 - Example 3:
```sh
Input: [1, 0, 1, 1], target=100
Output: 3
Explanation: The triplet [1, 1, 1] has the closest sum to the target.
```
This problem follows the Two Pointers pattern and is quite similar to Triplet Sum to Zero.

We can follow a similar approach to iterate through the array, taking one number at a time. At every step, we will save the difference between the triplet and the target number, so that in the end, we can return the triplet with the closest sum.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static int searchTriplet(vector<int>& arr, int targetSum){
        sort(arr.begin(), arr.end());
        int minDiff = INT_MAX;
        for(int i = 0;i < arr.size() - 2;i++){
            int l = i + 1;
            int r = arr.size() - 1;
            while(l < r){
                int diff = targetSum - arr[i] - arr[l] - arr[r];
                if(diff == 0){
                    return targetSum - diff;
                }
                if(abs(diff) < abs(minDiff)){
                    minDiff = diff;
                }
                if(diff > 0){
                    r--;
                }
                if(diff < 0){
                    l++;
                }
            }
        }
        return targetSum - minDiff;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static int searchTriplet(int[] arr, int targetSum){
        Arrays.sort(arr);
        int minDiff = Integer.MAX_VALUE;
        for(int i = 0;i < arr.length - 2;i++){
            int l = i + 1;
            int r = arr.length - 1;
            while(l < r){
              int diff = targetSum - arr[i] - arr[l] - arr[r];
                if(diff == 0){
                    return targetSum - diff;
                }
                if(Math.abs(diff) < Math.abs(minDiff)){
                    minDiff = diff;
                }
                if(diff > 0){
                    r--;
                }
                if(diff < 0){
                    l++;
                }
            }
        }
        return targetSum - minDiff;
    }
};
```
Time Complexity: O(N^2)
Space Complexity: O(N)

### Question 6 Triplets with Smaller Sum
Given an array arr of unsorted numbers and a target sum, count all triplets in it such that arr[i] + arr[j] + arr[k] < target where i, j, and k are three different indices. Write a function to return the count of such triplets.
 - Example 1:
```sh
Input: [-1, 0, 2, 3], target=3 
Output: 2
Explanation: There are two triplets whose sum is less than the target: [-1, 0, 3], [-1, 0, 2]
```
 - Example 2:
```sh
Input: [-1, 4, 2, 1, 3], target=5 
Output: 4
Explanation: There are four triplets whose sum is less than the target: 
   [-1, 1, 4], [-1, 1, 3], [-1, 1, 2], [-1, 2, 3]
```

This problem follows the Two Pointers pattern and shares similarities with Triplet Sum to Zero. The only difference is that, in this problem, we need to find the triplets whose sum is less than the given target. To meet the condition i != j != k we need to make sure that each number is not used more than once.

Following a similar approach, first, we can sort the array and then iterate through it, taking one number at a time. Let’s say during our iteration we are at number ‘X’, so we need to find ‘Y’ and ‘Z’ such that X + Y + Z < targetX+Y+Z<target. At this stage, our problem translates into finding a pair whose sum is less than “target - Xtarget−X” (as from the above equation Y + Z == target - XY+Z==target−X). We can use a similar approach as discussed in Triplet Sum to Zero.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static int searchTriplets(vector<int>& arr, int target){
        sort(arr.begin(), arr.end());
        int count = 0;
        for(int i = 0;i < arr.size() - 2;i++){
            count += searchPair(arr, target - arr[i], i);
        }
        return count;
    }
    static int searchPair(vector<int>& arr, int targetSum, int id){
            int count = 0;
            int l = id + 1;
            int r = arr.size() - 1;
            while(l < r){
                if(arr[l] + arr[r] < targetSum){
                    count += r - l;
                    l++;
                }
                else{
                    r--;
                }
            }
            return count;
        }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static int searchTriplets(int[] arr, int target){
        Arrays.sort(arr);
        int count = 0;
        for(int i = 0;i < arr.length - 2;i++){
            count += searchPair(arr, target - arr[i], i);
        }
        return count;
    }
    public static int searchPair(int[] arr, int targetSum, int id){
            int count = 0;
            int l = id + 1;
            int r = arr.length - 1;
            while(l < r){
                if(arr[l] + arr[r] < targetSum){
                    count += r - l;
                    l++;
                }
                else{
                    r--;
                }
            }
            return count;
        }
};
```
Time Complexity: O(N^2)
Space Complexity: O(N)

### Question 7 Subarrays with Product Less than a Target
Given an array with positive numbers and a target number, find all of its contiguous subarrays whose product is less than the target number.
 - Example 1:
```sh
Input: [2, 5, 3, 10], target=30 
Output: [2], [5], [2, 5], [3], [5, 3], [10]
Explanation: There are six contiguous subarrays whose product is less than the target.
```
 - Example 2:
```sh
Input: [8, 2, 6, 5], target=50 
Output: [8], [2], [8, 2], [6], [2, 6], [5], [6, 5] 
Explanation: There are seven contiguous subarrays whose product is less than the target.
```
This problem follows the Sliding Window and the Two Pointers pattern and shares similarities with Triplets with Smaller Sum with two differences:

In this problem, the input array is not sorted.
Instead of finding triplets with sum less than a target, we need to find all subarrays having a product less than the target.
The implementation will be quite similar to Triplets with Smaller Sum.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static vector<vector<int>> findSubarrays(const vector<int>& arr, int target){
        vector<vector<int>> res;
        int pro = 1;
        int l = 0;
        for(int i = 0;i < arr.size();i++){
            pro *= arr[i];
            while(pro >= target && l < arr.size()){
                pro /= arr[l++];
            }
            deque<int> temp;
            for(int j = i;j >= l;j--){
                temp.push_front(arr[j]);
                vector<int> rest;
                move(begin(temp),end(temp),back_inserter(rest));
                res.push_back(rest);
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
    public static List<List<Integer>> findSubarrays(int[] arr, int target){
        List<List<Integer>> res = new ArrayList<>();
        int pro = 1;
        int l = 0;
        for(int i = 0;i < arr.length;i++){
            pro *= arr[i];
            while(pro >= target && l < arr.length()){
                pro /= arr[l++];
            }
            List<Integer> temp = new LinkedList<>();
            for(int j = i;j >= l;j--){
                temp.add(0,arr[j]);
                res.add(new ArrayList<>(temp);
            }
        }
        return res;
    }
};
```
Time Complexity: O(N^2)
Space Complexity: O(N)

### Question 9 Dutch National Flag Problem

Given an array containing 0s, 1s and 2s, sort the array in-place. You should treat numbers of the array as objects, hence, we can’t count 0s, 1s, and 2s to recreate the array.

The flag of the Netherlands consists of three colors: red, white and blue; and since our input array also consists of three different numbers that is why it is called Dutch National Flag problem.
 - Example 1:
```sh
Input: [1, 0, 2, 1, 0]
Output: [0 0 1 1 2]
```
 - Example 2:
```sh
Input: [2, 2, 0, 1, 2, 0]
Output: [0 0 1 2 2 2 ]
```

The brute force solution will be to use an in-place sorting algorithm like Heapsort which will take O(N*logN)O(N∗logN). Can we do better than this? Is it possible to sort the array in one iteration?

We can use a Two Pointers approach while iterating through the array. Let’s say the two pointers are called low and high which are pointing to the first and the last element of the array respectively. So while iterating, we will move all 0s before low and all 2s after high so that in the end, all 1s will be between low and high.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static void sort(vector<int>& arr){
        int l = 0;
        int h = arr.size() - 1;
        for(int i = 0;i <= h;){
            if(arr[i] == 0){
                swap(arr[i], arr[l]);
                l++;
                i++;
            }
            else if(arr[i] == 1){
                i++;
            }
            else{
                swap(arr[i], arr[h]);
                h--;
            }
        }
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static void sort(int[] arr){
        int l = 0;
        int h = arr.length - 1;
        for(int i = 0;i <= h;){
            if(arr[i] == 0){
                swap(arr[i], arr[l]);
                l++;
                i++;
            }
            else if(arr[i] == 1){
                i++;
            }
            else{
                swap(arr[i], arr[h]);
                h--;
            }
        }
    }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 9 Quadruple Sum to Target
Given an array of unsorted numbers and a target number, find all unique quadruplets in it, whose sum is equal to the target number.
 - Example 1:
```sh
Input: [4, 1, 2, -1, 1, -3], target=1
Output: [-3, -1, 1, 4], [-3, 1, 1, 2]
Explanation: Both the quadruplets add up to the target.
```
 - Example 2:
```sh
Input: [2, 0, -1, 1, -2, 2], target=2
Output: [-2, 0, 2, 2], [-1, 0, 1, 2]
Explanation: Both the quadruplets add up to the target.
```
This problem follows the Two Pointers pattern and shares similarities with Triplet Sum to Zero.

We can follow a similar approach to iterate through the array, taking one number at a time. At every step during the iteration, we will search for the quadruplets similar to Triplet Sum to Zero whose sum is equal to the given target.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static vector<vector<int>> searchQuadruplets(vector<int>& arr, int target){
        sort(arr.begin(), arr.end());
        vector<vector<int>> res;
        for(int i = 0;i < arr.size() - 3;i++){
            if(i != 0 && arr[i] == arr[i-1]){
                continue;
            }
            for(int j = i + 1;j < arr.size() - 2;j++){
                if(j != i + 1 && arr[j] == arr[j - 1]){
                    continue;
                }
                searchPair(arr, target, i, j, res);
            }
        }
        return res;
    }
    static void searchPair(vector<int>& arr, int target, int f, int s, vector<vector<int>>& res){
            int l = j + 1;
            int r = arr.size() - 1;
            while(l < r){
                int sum = arr[f] + arr[s] + arr[l] + arr[r];
                if(sum == target){
                    res.push_back({arr[f], arr[s], arr[l], arr[r]});
                    r--;
                    l++;
                    while(l < r && arr[l] == arr[l - 1]){
                        l++;
                    }
                    while(l < r && arr[r] == arr[r + 1]){
                        r++;
                    }
                }
                else if(sum > target){
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
        for(int i = 0;i < arr.length - 3;i++){
            if(i != 0 && arr[i] == arr[i-1]){
                continue;
            }
            for(int j = i + 1;j < arr.length - 2;j++){
                if(j != i + 1 && arr[j] == arr[j - 1]){
                    continue;
                }
                searchPair(arr, target, i, j, res);
            }
        }
        return res;
    }
    static void searchPair(int[] arr, int targetSum, int l, int r, List<List<Integer>> res){
            int l = j + 1;
            int r = arr.size() - 1;
            while(l < r){
                int sum = arr[f] + arr[s] + arr[l] + arr[r];
                if(sum == target){
                    res.push_back({arr[f], arr[s], arr[l], arr[r]});
                    r--;
                    l++;
                    while(l < r && arr[l] == arr[l - 1]){
                        l++;
                    }
                    while(l < r && arr[r] == arr[r + 1]){
                        r++;
                    }
                }
                else if(sum > target){
                    r--;
                }
                else{
                    l++;
                }
            }
        }
};
```
Time Complexity: O(N * log(N))
Space Complexity: O(N)

### Question 10 Comparing Strings containing Backspaces
Given two strings containing backspaces (identified by the character ‘#’), check if the two strings are equal.
 - Example 1:
```sh
Input: str1="xy#z", str2="xzz#"
Output: true
Explanation: After applying backspaces the strings become "xz" and "xz" respectively.
```
 - Example 2:
```sh
Input: str1="xy#z", str2="xyz#"
Output: false
Explanation: After applying backspaces the strings become "xz" and "xy" respectively.
```
 - Example 3:
```sh
Input: str1="xp#", str2="xyz##"
Output: true
Explanation: After applying backspaces the strings become "x" and "x" respectively.
In "xyz##", the first '#' removes the character 'z' and the second '#' removes the character 'y'.
```
 - Example 4:
```sh
Input: str1="xywrrmp", str2="xywrrmu#p"
Output: true
Explanation: After applying backspaces the strings become "xywrrmp" and "xywrrmp" respectively.
```
To compare the given strings, first, we need to apply the backspaces. An efficient way to do this would be from the end of both the strings. We can have separate pointers, pointing to the last element of the given strings. We can start comparing the characters pointed out by both the pointers to see if the strings are equal. If, at any stage, the character pointed out by any of the pointers is a backspace (’#’), we will skip and apply the backspace until we have a valid character available for comparison.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static bool compare(const string& str1, const string &str2){
        int index1 = str1.size() - 1;
        int index2 = str2.size() - 1;
        while(index1 > 0 || index2 > 0){
            int i1 = helper(str1, index1);
            int i2 = helper(str2, index2);
            if(i1 < 0 && i2 < 0){
                return true;
            }
            if(i1 < 0 || i2 < 0){
                return false;
            }
            if(str1[i1] != str2[i2]){
                return false;
            }
            index1 = i1 - 1;
            index2 = i2 - 1;
        }
        return true;
    }
    static int helper(string str, int id){
        int c = 0;
        while(id > 0){
            if(str[id] == '#'){
                c++;
            }
            else if(c > 0){
                c--;
            }
            else{
                break;
            }
            id--;
        }
        return id;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static boolean compare(int[] arr){
        int index1 = str1.length - 1;
        int index2 = str2.length - 1;
        while(index1 > 0 || index2 > 0){
            int i1 = helper(str1, index1);
            int i2 = helper(str2, index2);
            if(i1 < 0 && i2 < 0){
                return true;
            }
            if(i1 < 0 || i2 < 0){
                return false;
            }
            if(str1[i1] != str2[i2]){
                return false;
            }
            index1 = i1 - 1;
            index2 = i2 - 1;
        }
        return true;
    }
    static int helper(String str, int id){
        int c = 0;
        while(id > 0){
            if(str.charAt(id) == '#'){
                c++;
            }
            else if(c > 0){
                c--;
            }
            else{
                break;
            }
            id--;
        }
        return id;
};
```
Time Complexity: O(N^2)
Space Complexity: O(N)

### Question 11 Mininum Window Sort
Given an array, find the length of the smallest subarray in it which when sorted will sort the whole array.
 - Example 1:
```sh
Input: [1, 2, 5, 3, 7, 10, 9, 12]
Output: 5
Explanation: We need to sort only the subarray [5, 3, 7, 10, 9] to make the whole array sorted
```
 - Example 2:
```sh
Input: [1, 3, 2, 0, -1, 7, 10]
Output: 5
Explanation: We need to sort only the subarray [1, 3, 2, 0, -1] to make the whole array sorted
```
 - Example 3:
```sh
Input: [1, 2, 3]
Output: 0
Explanation: The array is already sorted
```
 - Example 4:
```sh
Input: [3, 2, 1]
Output: 3
Explanation: The whole array needs to be sorted.
```
As we know, once an array is sorted (in ascending order), the smallest number is at the beginning and the largest number is at the end of the array. So if we start from the beginning of the array to find the first element which is out of sorting order i.e., which is smaller than its previous element, and similarly from the end of array to find the first element which is bigger than its previous element, will sorting the subarray between these two numbers result in the whole array being sorted?

Let’s try to understand this with Example-2 mentioned above. In the following array, what are the first numbers out of sorting order from the beginning and the end of the array:

    [1, 3, 2, 0, -1, 7, 10]
Starting from the beginning of the array the first number out of the sorting order is ‘2’ as it is smaller than its previous element which is ‘3’.
Starting from the end of the array the first number out of the sorting order is ‘0’ as it is bigger than its previous element which is ‘-1’
As you can see, sorting the numbers between ‘3’ and ‘-1’ will not sort the whole array. To see this, the following will be our original array after the sorted subarray:

    [1, -1, 0, 2, 3, 7, 10]
The problem here is that the smallest number of our subarray is ‘-1’ which dictates that we need to include more numbers from the beginning of the array to make the whole array sorted. We will have a similar problem if the maximum of the subarray is bigger than some elements at the end of the array. To sort the whole array we need to include all such elements that are smaller than the biggest element of the subarray. So our final algorithm will look like:

From the beginning and end of the array, find the first elements that are out of the sorting order. The two elements will be our candidate subarray.
Find the maximum and minimum of this subarray.
Extend the subarray from beginning to include any number which is bigger than the minimum of the subarray.
Similarly, extend the subarray from the end to include any number which is smaller than the maximum of the subarray.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
  static int sort(const vector<int>& arr) {
    int low = 0, high = arr.size() - 1;
    // find the first number out of sorting order from the beginning
    while (low < arr.size() - 1 && arr[low] <= arr[low + 1]) {
      low++;
    }

    if (low == arr.size() - 1) {  // if the array is sorted
      return 0;
    }

    // find the first number out of sorting order from the end
    while (high > 0 && arr[high] >= arr[high - 1]) {
      high--;
    }

    // find the maximum and minimum of the subarray
    int subarrayMax = numeric_limits<int>::min(), subarrayMin = numeric_limits<int>::max();
    for (int k = low; k <= high; k++) {
      subarrayMax = max(subarrayMax, arr[k]);
      subarrayMin = min(subarrayMin, arr[k]);
    }

    // extend the subarray to include any number which is bigger than the minimum of the subarray
    while (low > 0 && arr[low - 1] > subarrayMin) {
      low--;
    }
    // extend the subarray to include any number which is smaller than the maximum of the subarray
    while (high < arr.size() - 1 && arr[high + 1] < subarrayMax) {
      high++;
    }

    return high - low + 1;
  }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static int sort(int[] arr) {
    int low = 0, high = arr.length - 1;
    // find the first number out of sorting order from the beginning
    while (low < arr.length - 1 && arr[low] <= arr[low + 1])
      low++;

    if (low == arr.length - 1) // if the array is sorted
      return 0;

    // find the first number out of sorting order from the end
    while (high > 0 && arr[high] >= arr[high - 1])
      high--;

    // find the maximum and minimum of the subarray
    int subarrayMax = Integer.MIN_VALUE, subarrayMin = Integer.MAX_VALUE;
    for (int k = low; k <= high; k++) {
      subarrayMax = Math.max(subarrayMax, arr[k]);
      subarrayMin = Math.min(subarrayMin, arr[k]);
    }

    // extend the subarray to include any number which is bigger than the minimum of the subarray 
    while (low > 0 && arr[low - 1] > subarrayMin)
      low--;
    // extend the subarray to include any number which is smaller than the maximum of the subarray
    while (high < arr.length - 1 && arr[high + 1] < subarrayMax)
      high++;

    return high - low + 1;
  }
```
Time Complexity: O(N^2)
Space Complexity: O(N)
### Todos

 - Write MORE Tests
 - Add Night Mode

License
----

MIT
