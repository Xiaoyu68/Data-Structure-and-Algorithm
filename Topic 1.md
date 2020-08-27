# Topic 1: Sliding Window


[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

In many problems dealing with an array (or a LinkedList), we are asked to find or calculate something among all the contiguous subarrays (or sublists) of a given size. For example, take a look at this problem:

  - Given an array, find the average of all contiguous subarrays of size 'k' in it.
 
### Question 1 Average of Subarray of Size K
Let's understand this problem with a real input:
```sh
Array: [1, 3, 2, 6, -1, 4, 1, 8, 2], k = 5
```
```sh
Output: [2.2, 2.8, 2.4, 3.6, 2.8]
```

If we use the brute-force solution to solve this problem, we should use 2 for-loops to go through the whole array list. Then the time complexity will be O(N*K). For this problem, we can find a better solution, which is using sliding window to prevent the overlapping part being calculated many times.
We should set a window covering k-length array, and calculate the average of all the elements in the window. Then we are supposed to move the window and do the same operations until we get the result array. We can reduce the time complexity to O(N) by using this method.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static vector<double> findAverages(int K, const vector<int>& arr){
        if(arr.size() == 0){return {};} //corner case check
        vector<double> res;
        int l = 0;
        double sum = 0;
        for(int r = 0;r < arr.size(); r++){
            sum += arr[r];
            if(r - l == K - 1){
                res.push_back(sum / K);
                sum -= arr[l];
                l++;
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
    public static double[] findAverages(int K, int[] arr){
        double[] res = new double [arr.length - K + 1];
        int l = 0;
        double sum = 0;
        for(int r = 0;r < arr.length; r++){
            sum += arr[r];
            if(r - l == K - 1){
                res[l] = sum / K;
                sum -= arr[l];
                l++;
            }
        }
        return res;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 2 Maximum Sum Subarray of Size K
Given an array of positive number 'k', find the maxium sum of any contiguous subarray of size 'k'.
 - Example 1:
```sh
Input: [2, 1, 5, 1, 3, 2], k = 3
Output: 9
Explanation: Subarray with maxium sum is [5, 1, 3].
```
 - Example 2:
```sh
Input: [2, 3, 4, 1, 5], k = 2
Output: 7
Explanation: Subarray with maxium sum is [3, 4].
```

A basic brute force solution will be to calculate the sum of all ‘k’ sized subarrays of the given array, to find the subarray with the highest sum. We can start from every index of the given array and add the next ‘k’ elements to find the sum of the subarray. The time complexity will be O(N*K).
A better approach is using sliding window. If you observe closely, you will realize that to calculate the sum of a contiguous subarray we can utilize the sum of the previous subarray. For this, consider each subarray as a Sliding Window of size ‘k’. To calculate the sum of the next subarray, we need to slide the window ahead by one element. So to slide the window forward and calculate the sum of the new position of the sliding window, we need to do two things:

Subtract the element going out of the sliding window i.e., subtract the first element of the window.
Add the new element getting included in the sliding window i.e., the element coming right after the end of the window.
This approach will save us from re-calculating the sum of the overlapping part of the sliding window.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static int findMaxSumSubArray(int K, const vector<int>& arr){
        if(arr.size() == 0){return 0;} //corner case check
        int maxNum = 0;
        int l = 0;
        int sum = 0;
        for(int r = 0;r < arr.size(); r++){
            sum += arr[r];
            if(r - l == K - 1){
                maxNum = max(maxNum, sum);
                sum -= arr[l];
                l++;
            }
        }
        return maxNum;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static int findMaxSumSubArray(int K, int[] arr){
        int maxNum = 0;
        int l = 0;
        int sum = 0;
        for(int r = 0;r < arr.length; r++){
            sum += arr[r];
            if(r - l == K - 1){
                maxNum = Math.max(maxNum, sum);
                sum -= arr[l];
                l++;
            }
        }
        return maxNum;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 3 Smallest Subarray with a given sum
Given an array of positive numbers and a positive number ‘S’, find the length of the smallest contiguous subarray whose sum is greater than or equal to ‘S’. Return 0, if no such subarray exists.
 - Example 1:
```sh
Input: [2, 1, 5, 2, 3, 2], k = 7
Output: 2
Explanation: The smallest subarray with a sum great than or equal to '7' is [5, 2].
```
 - Example 2:
```sh
Input: [2, 1, 5, 2, 8], S=7 
Output: 1
Explanation: The smallest subarray with a sum greater than or equal to '7' is [8].
```
 - Example 3:
```sh
Input: [3, 4, 1, 1, 6], S=8 
Output: 3
Explanation: Smallest subarrays with a sum greater than or equal to '8' are [3, 4, 1] or [1, 1, 6].
```
This problem follows the Sliding Window pattern and we can use a similar strategy as discussed in Maximum Sum Subarray of Size K. There is one difference though: in this problem, the size of the sliding window is not fixed. Here is how we will solve this problem:

First, we will add-up elements from the beginning of the array until their sum becomes greater than or equal to ‘S’.
These elements will constitute our sliding window. We are asked to find the smallest such window having a sum greater than or equal to ‘S’. We will remember the length of this window as the smallest window so far.
After this, we will keep adding one element in the sliding window (i.e. slide the window ahead), in a stepwise fashion.
In each step, we will also try to shrink the window from the beginning. We will shrink the window until the window’s sum is smaller than ‘S’ again. This is needed as we intend to find the smallest window. This shrinking will also happen in multiple steps; in each step we will do two things:
Check if the current window length is the smallest so far, and if so, remember its length.
Subtract the first element of the window from the running sum to shrink the sliding window.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static int findMinSubArray(int S, const vector<int>& arr){
        if(arr.size() == 0){return 0;} //corner case check
        int minLen = arr.size();
        int l = 0;
        int sum = 0;
        for(int r = 0;r < arr.size(); r++){
            sum += arr[r];
            while(sum >= S){
                minLen = min(minLen, r - l + 1);
                sum -= arr[l];
                l++;
            }
        }
        return minLen;
    }
};
```
__Attention: we should use "while" here, rather than "if".__
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static int findMinSubArray(int S, int[] arr){
        int minLen = arr.length;
        int l = 0;
        int sum = 0;
        for(int r = 0;r < arr.length; r++){
            sum += arr[r];
            while(sum >= S){
                minLen = Math.min(minLen, r - l + 1);
                sum -= arr[l];
                l++;
            }
        }
        return minLen;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 4 Longest Substring with K Distinct Characters
Given a string, find the length of the longest substring in it with no more than K distinct characters.
 - Example 1:
```sh
Input: String="araaci", K=2
Output: 4
Explanation: The longest substring with no more than '2' distinct characters is "araa".
```
 - Example 2:
```sh
Input: String="araaci", K=1
Output: 2
Explanation: The longest substring with no more than '1' distinct characters is "aa".
```
 - Example 3:
```sh
Input: String="cbbebi", K=3
Output: 5
Explanation: The longest substrings with no more than '3' distinct characters are "cbbeb" & "bbebi".
```
This problem follows the Sliding Window pattern and we can use a similar dynamic sliding window strategy as discussed in Smallest Subarray with a given sum. We can use a HashMap to remember the frequency of each character we have processed. Here is how we will solve this problem:

First, we will insert characters from the beginning of the string until we have ‘K’ distinct characters in the HashMap.
These characters will constitute our sliding window. We are asked to find the longest such window having no more than ‘K’ distinct characters. We will remember the length of this window as the longest window so far.
After this, we will keep adding one character in the sliding window (i.e. slide the window ahead), in a stepwise fashion.
In each step, we will try to shrink the window from the beginning if the count of distinct characters in the HashMap is larger than ‘K’. We will shrink the window until we have no more than ‘K’ distinct characters in the HashMap. This is needed as we intend to find the longest window.
While shrinking, we’ll decrement the frequency of the character going out of the window and remove it from the HashMap if its frequency becomes zero.
At the end of each step, we’ll check if the current window length is the longest so far, and if so, remember its length.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static int findLength(const string& str, int k){
        unordered_map<char,int> mp;
        int maxLen = 0;
        int l = 0;
        for(int r = 0;r < str.size(); r++){
            mp[str[r]]++;
            while(mp.size() > k){
                if(mp[str[l]] > 1){
                    mp[str[l]]--;
                }
                else{
                    mp.erase(str[l]);
                }
                l++;
            }
            maxLen = max(maxLen, r - l + 1);
        }
        return maxLen;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static int findLength(String str, int k){
        if(str == null || str.length() == 0 || str.length() < k){
            return 0;
        }
        int l = 0;
        Map<Character, Integer> mp = new HashMap<>();
        int maxLen = 0;
        for(int r = 0;r < str.length(); r++){
            mp.put(str.charAt(r), mp.getOrDefault(str.charAt(r),0) + 1);
            while(mp.size() > k){
                if(mp.get(str.charAt(l)) > 1){
                    mp.put(str.charAt(l),mp.get(str.charAt(l)) - 1);
                }
                else{
                    mp.remove(str.charAt(l));
                }
                l++;
            }
            maxLen = Math.max(maxLen, r - l + 1);
        }
        return maxLen;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(K)

### Question 5 Fruits into Baskets
Given an array of characters where each character represents a fruit tree, you are given two baskets and your goal is to put maximum number of fruits in each basket. The only restriction is that each basket can have only one type of fruit.

You can start with any tree, but once you have started you can’t skip a tree. You will pick one fruit from each tree until you cannot, i.e., you will stop when you have to pick from a third fruit type.

Write a function to return the maximum number of fruits in both the baskets.
 - Example 1:
```sh
Input: Fruit=['A', 'B', 'C', 'A', 'C']
Output: 3
Explanation: We can put 2 'C' in one basket and one 'A' in the other from the subarray ['C', 'A', 'C']
```
 - Example 2:
```sh
Input: Fruit=['A', 'B', 'C', 'B', 'B', 'C']
Output: 5
Explanation: We can put 3 'B' in one basket and two 'C' in the other basket. 
This can be done if we start with the second letter: ['B', 'C', 'B', 'B', 'C']
```
This problem follows the Sliding Window pattern and is quite similar to Longest Substring with K Distinct Characters. In this problem, we need to find the length of the longest subarray with no more than two distinct characters (or fruit types!). This transforms the current problem into Longest Substring with K Distinct Characters where K=2.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static int findLength(const vector<char>& arr){
        unordered_map<char,int> mp;
        int maxLen = 0;
        int l = 0;
        for(int r = 0;r < str.size(); r++){
            mp[str[r]]++;
            while(mp.size() > 2){
                if(mp[str[l]] > 1){
                    mp[str[l]]--;
                }
                else{
                    mp.erase(str[l]);
                }
                l++;
            }
            maxLen = max(maxLen, r - l + 1);
        }
        return maxLen;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static int findLength(char[] arr){
        if(str == null || str.length() == 0 || str.length() < k){
            return 0;
        }
        int l = 0;
        Map<Character, Integer> mp = new HashMap<>();
        int maxLen = 0;
        for(int r = 0;r < str.length(); r++){
            mp.put(str.charAt(r), mp.getOrDefault(str.charAt(r),0) + 1);
            while(mp.size() > 2){
                if(mp.get(str.charAt(l)) > 1){
                    mp.put(str.charAt(l),mp.get(str.charAt(l)) - 1);
                }
                else{
                    mp.remove(str.charAt(l));
                }
                l++;
            }
            maxLen = Math.max(maxLen, r - l + 1);
        }
        return maxLen;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(K)

### Question 6 No-repeat Substring
Given a string, find the length of the longest substring which has no repeating characters.
 - Example 1:
```sh
Input: String="aabccbb"
Output: 3
Explanation: The longest substring without any repeating characters is "abc".
```
 - Example 2:
```sh
Input: String="abbbb"
Output: 2
Explanation: The longest substring without any repeating characters is "ab".
```
 - Example 3:
```sh
Input: String="abccde"
Output: 3
Explanation: Longest substrings without any repeating characters are "abc" & "cde".
```
This problem follows the Sliding Window pattern and we can use a similar dynamic sliding window strategy as discussed in Longest Substring with K Distinct Characters. We can use a HashMap to remember the last index of each character we have processed. Whenever we get a repeating character we will shrink our sliding window to ensure that we always have distinct characters in the sliding window.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static int findLength(const vector<char>& arr){
        unordered_map<char,int> mp;
        int maxLen = 0;
        int l = 0;
        for(int r = 0;r < str.size(); r++){
            mp[str[r]]++;
            while(mp.size() > 2){
                if(mp[str[l]] > 1){
                    mp[str[l]]--;
                }
                else{
                    mp.erase(str[l]);
                }
                l++;
            }
            maxLen = max(maxLen, r - l + 1);
        }
        return maxLen;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static int findLength(char[] arr){
        if(str == null || str.length() == 0 || str.length() < k){
            return 0;
        }
        int l = 0;
        Map<Character, Integer> mp = new HashMap<>();
        int maxLen = 0;
        for(int r = 0;r < str.length(); r++){
            mp.put(str.charAt(r), mp.getOrDefault(str.charAt(r),0) + 1);
            while(mp.size() > 2){
                if(mp.get(str.charAt(l)) > 1){
                    mp.put(str.charAt(l),mp.get(str.charAt(l)) - 1);
                }
                else{
                    mp.remove(str.charAt(l));
                }
                l++;
            }
            maxLen = Math.max(maxLen, r - l + 1);
        }
        return maxLen;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(K)
### Todos

 - Write MORE Tests
 - Add Night Mode

License
----

MIT
