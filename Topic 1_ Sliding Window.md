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
    static int findLength(const string& str){
        unordered_map<char,int> mp;
        int maxLen = 0;
        int l = 0;
        for(int r = 0;r < str.size(); r++){
            if(mp.find(str[r]) != mp.end()){
                l = max(l, mp[r] + 1);
            }
            mp[str[r]] = r;
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
    public static int findLength(String str){
        Map<Character, Integer> mp = new HashMap<>();
        int maxLen = 0;
        int l = 0;
        for(int r = 0;r < str.length(); r++){
            if(mp.contianKeys(str.charAt(r))){
                l = Math.max(l, mp.get(str.charAt(r)) + 1);
            }
            mp.put(str.charAt(r), r);
            maxLen = Math.max(maxLen, r - l + 1);
        }
        return maxLen;
    }
};
```
Time Complexity #
The time complexity of the above algorithm will be O(N) where ‘N’ is the number of characters in the input string.

Space Complexity #
The space complexity of the algorithm will be O(K) where KK is the number of distinct characters in the input string. This also means K<=N, because in the worst case, the whole string might not have any repeating character so the entire string will be added to the HashMap. Having said that, since we can expect a fixed set of characters in the input string (e.g., 26 for English letters), we can say that the algorithm runs in fixed space O(1); in this case, we can use a fixed-size array instead of the HashMap.

### Question 7 Longest Substring with Same Letters after Replacement
Given a string with lowercase letters only, if you are allowed to replace no more than ‘k’ letters with any letter, find the length of the longest substring having the same letters after replacement.
 - Example 1:
```sh
Input: String="aabccbb", k=2
Output: 5
Explanation: Replace the two 'c' with 'b' to have a longest repeating substring "bbbbb".
```
 - Example 2:
```sh
Input: String="abbcb", k=1
Output: 4
Explanation: Replace the 'c' with 'b' to have a longest repeating substring "bbbb".
```
 - Example 3:
```sh
Input: String="abccde", k=1
Output: 3
Explanation: Replace the 'b' or 'd' with 'c' to have the longest repeating substring "ccc".
```
This problem follows the Sliding Window pattern and we can use a similar dynamic sliding window strategy as discussed in No-repeat Substring. We can use a HashMap to count the frequency of each letter.

We’ll iterate through the string to add one letter at a time in the window. We’ll also keep track of the count of the maximum repeating letter in any window (let’s call it maxRepeatLetterCount). So at any time, we know that we can have a window which has one letter repeating maxRepeatLetterCount times, this means we should try to replace the remaining letters. If we have more than ‘k’ remaining letters, we should shrink the window as we are not allowed to replace more than ‘k’ letters.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static int findLength(const string& str, int k){
        int l = 0;
        int maxLen = 0;
        int windowSize = 0;
        unordered_map<char,int>mp;
        for(int r = 0;r < str.size(); r++){
            mp[str[r]]++;
            windowSize = max(windowSize, mp[str[r]]);
            if(r - l + 1 - windowSize > k){
                mp[str[l]]--;
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
        int l = 0;
        int maxLen = 0;
        int windowSize = 0;
        Map<Character,Integer>mp = new HashMap<>();
        for(int r = 0;r < str.length(); r++){
            mp.put(str.charAt(r),mp.getOrDefault(str.charAt(r),0) + 1);
            windowSize = Math.max(windowSize, mp.get(str.chatAt(r))));
            if(r - l + 1 - windowSize > k){
                mp.put(str.charAt(l),mp.get(str.charAt(l)) - 1);
                l++;
            }
            maxLen = Math.max(maxLen, r - l + 1);
        }
        return maxLen;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 8 Longest Subarray with Ones after Replacement
Given an array containing 0s and 1s, if you are allowed to replace no more than ‘k’ 0s with 1s, find the length of the longest contiguous subarray having all 1s.
 - Example 1:
```sh
Input: Array=[0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 1], k=2
Output: 6
Explanation: Replace the '0' at index 5 and 8 to have the longest contiguous subarray of 1s having length 6.
```
 - Example 2:
```sh
Input: Array=[0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 1], k=3
Output: 9
Explanation: Replace the '0' at index 6, 9, and 10 to have the longest contiguous subarray of 1s having length 9.
```
This problem follows the Sliding Window pattern and is quite similar to Longest Substring with same Letters after Replacement. The only difference is that, in the problem, we only have two characters (1s and 0s) in the input arrays.

Following a similar approach, we’ll iterate through the array to add one number at a time in the window. We’ll also keep track of the maximum number of repeating 1s in the current window (let’s call it maxOnesCount). So at any time, we know that we can have a window which has 1s repeating maxOnesCount time, so we should try to replace the remaining 0s. If we have more than ‘k’ remaining 0s, we should shrink the window as we are not allowed to replace more than ‘k’ 0s.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static int findLength(const vector<int> &arr, int k){
        int l = 0;
        int windowSize = 0;
        int maxLen = 0;
        unordered_map<int,int> mp;
        for(int r = 0;r < arr.size();r++){
            mp[r]++;
            windowSize = max(windowSize, mp[1]);
            if(r - l + 1 - windowSize > k){
                l++;
                mp[l]--;
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
    public static int findLength(int[] arr, int k){
        int l = 0;
        int maxLen = 0;
        int windowSize = 0;
        Map<Integer,Integer>mp = new HashMap<>();
        for(int r = 0;r < str.length(); r++){
            mp.put(str.charAt(r),mp.getOrDefault(str.charAt(r),0) + 1);
            windowSize = Math.max(windowSize, mp.get(1));
            if(r - l + 1 - windowSize > k){
                mp.put(str.charAt(l),mp.get(str.charAt(l)) - 1);
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

### Question 9 Permutation in a String
Given a string and a pattern, find out if the string contains any permutation of the pattern.

Permutation is defined as the re-arranging of the characters of the string. For example, “abc” has the following six permutations:

abc
acb
bac
bca
cab
cba
If a string has ‘n’ distinct characters it will have n!n! permutations.
 - Example 1:
```sh
Input: String="oidbcaf", Pattern="abc"
Output: true
Explanation: The string contains "bca" which is a permutation of the given pattern.
```
 - Example 2:
```sh
Input: String="odicf", Pattern="dc"
Output: false
Explanation: No permutation of the pattern is present in the given string as a substring.
```
 - Example 3:
```sh
Input: String="bcdxabcdy", Pattern="bcdyabcdx"
Output: true
Explanation: Both the string and the pattern are a permutation of each other.
```
 - Example 4:
```sh
Input: String="aaacb", Pattern="abc"
Output: true
Explanation: The string contains "acb" which is a permutation of the given pattern.
```
This problem follows the Sliding Window pattern and we can use a similar sliding window strategy as discussed in Longest Substring with K Distinct Characters. We can use a HashMap to remember the frequencies of all characters in the given pattern. Our goal will be to match all the characters from this HashMap with a sliding window in the given string. Here are the steps of our algorithm:

Create a HashMap to calculate the frequencies of all characters in the pattern.
Iterate through the string, adding one character at a time in the sliding window.
If the character being added matches a character in the HashMap, decrement its frequency in the map. If the character frequency becomes zero, we got a complete match.
If at any time, the number of characters matched is equal to the number of distinct characters in the pattern (i.e., total characters in the HashMap), we have gotten our required permutation.
If the window size is greater than the length of the pattern, shrink the window to make it equal to the size of the pattern. At the same time, if the character going out was part of the pattern, put it back in the frequency HashMap.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static bool findPermutation(const string& str, const string &pattern){
        int l = 0;
        int matched = 0;
        unordered_map<char,int> mp;
        for(int i = 0;i < pattern.size();i++){
            mp[pattern[i]]++;
        }
        for(int r = 0;r < str.size(); r++){
            if(mp.find(str[r]) != mp.end()){
                mp[str[r]]--;
                if(mp[str[r]] == 0){
                    matched++;
                }
            }
            if(matched == mp.size()){
                return true;
            }
            if(r >= pattern.size() - 1){
                if(mp.find(str[l]) != mp.end()){
                    if(mp[str[l]] == 0){
                        matched--;
                    }
                    mp[str[l]]++;
                }
                l++;
            }
        }
        return false;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static boolean findPermutation(String str, String pattern){
        int l = 0;
        int matched = 0;
        Map<Character,Integer>mp = new HashMap<>();
        for(int i = 0;i < pattern.length();i++){
            mp.put(str.charAt(i), mp.getOrDefault(str.charAt(i),0) + 1);
        }
        for(int r = 0;r < str.length(); r++){
            if(mp.containsKey(str.charAt(r))){
                mp.put(str.charAt(r), mp.get(str.charAt(r)) - 1);
                if(mp.get(str.charAt(r)) == 0){
                    matched++;
                }
            }
            if(matched == mp.size()){
                return true;
            }
            if(r >= pattern.length() - 1){
                if(mp.containsKey(str.charAt(l))){
                    if(mp.get(str.charAt(l)) == 0){
                        matched--;
                    }
                    mp.put(str.charAt(l), mp.get(str.charAt(l)) + 1);
                }
                l++;
            }
        }
        return false;
    }
};
```
Time Complexity: O(M + N)
Space Complexity: O(M)

### Question 10 String Anagrams
Given a string and a pattern, find all anagrams of the pattern in the given string.

Anagram is actually a Permutation of a string. For example, “abc” has the following six anagrams:

abc
acb
bac
bca
cab
cba
Write a function to return a list of starting indices of the anagrams of the pattern in the given string.
 - Example 1:
```sh
Input: String="ppqp", Pattern="pq"
Output: [1, 2]
Explanation: The two anagrams of the pattern in the given string are "pq" and "qp".
```
 - Example 2:
```sh
Input: String="abbcabc", Pattern="abc"
Output: [2, 3, 4]
Explanation: The three anagrams of the pattern in the given string are "bca", "cab", and "abc".
```
This problem follows the Sliding Window pattern and is very similar to Permutation in a String. In this problem, we need to find every occurrence of any permutation of the pattern in the string. We will use a list to store the starting indices of the anagrams of the pattern in the string.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static vector<int> findStringAnagrams(const string& str, const string &pattern){
        vector<int> res;
        int l = 0;
        int matched = 0;
        unordered_map<char,int> mp;
        for(int i = 0;i < pattern.size();i++){
            mp[pattern[i]]++;
        }
        for(int r = 0;r < str.size(); r++){
            if(mp.find(str[r]) != mp.end()){
                mp[str[r]]--;
                if(mp[str[r]] == 0){
                    matched++;
                }
            }
            if(matched == mp.size()){
                res.push_back(l);
            }
            if(r >= pattern.size() - 1){
                if(mp.find(str[l]) != mp.end()){
                    if(mp[str[l]] == 0){
                        matched--;
                    }
                    mp[str[l]]++;
                }
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
    public static boolean findPermutation(String str, String pattern){
        int l = 0;
        int matched = 0;
        List<Integer> res = new ArrayList<>();
        Map<Character,Integer>mp = new HashMap<>();
        for(int i = 0;i < pattern.length();i++){
            mp.put(str.charAt(i), mp.getOrDefault(str.charAt(i),0) + 1);
        }
        for(int r = 0;r < str.length(); r++){
            if(mp.containsKey(str.charAt(r))){
                mp.put(str.charAt(r), mp.get(str.charAt(r)) - 1);
                if(mp.get(str.charAt(r)) == 0){
                    matched++;
                }
            }
            if(matched == mp.size()){
                res.add(l);
            }
            if(r >= pattern.length() - 1){
                if(mp.containsKey(str.charAt(l))){
                    if(mp.get(str.charAt(l)) == 0){
                        matched--;
                    }
                    mp.put(str.charAt(l), mp.get(str.charAt(l)) + 1);
                }
                l++
            }
        }
        return res;
    }
};
```
Time Complexity: O(N + M)
Space Complexity: O(M)

### Question 11 Smallest Window containing Substring
Given a string and a pattern, find the smallest substring in the given string which has all the characters of the given pattern.
 - Example 1:
```sh
Input: String="aabdec", Pattern="abc"
Output: "abdec"
Explanation: The smallest substring having all characters of the pattern is "abdec"
```
 - Example 2:
```sh
Input: String="abdabca", Pattern="abc"
Output: "abc"
Explanation: The smallest substring having all characters of the pattern is "abc".
```
 - Example 3:
```sh
Input: String="adcad", Pattern="abc"
Output: ""
Explanation: No substring in the given string has all characters of the pattern.
```
This problem follows the Sliding Window pattern and has a lot of similarities with Permutation in a String with one difference. In this problem, we need to find a substring having all characters of the pattern which means that the required substring can have some additional characters and doesn’t need to be a permutation of the pattern. Here is how we will manage these differences:

We will keep a running count of every matching instance of a character.
Whenever we have matched all the characters, we will try to shrink the window from the beginning, keeping track of the smallest substring that has all the matching characters.
We will stop the shrinking process as soon as we remove a matched character from the sliding window. One thing to note here is that we could have redundant matching characters, e.g., we might have two ‘a’ in the sliding window when we only need one ‘a’. In that case, when we encounter the first ‘a’, we will simply shrink the window without decrementing the matched count. We will decrement the matched count when the second ‘a’ goes out of the window.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static string findSubstring(const string& str, const string &pattern){
        int l = 0;
        int matched = 0;
        int sub = 0;
        int minLen = str.size() + 1;
        unordered_map<char,int> mp;
        for(int i = 0;i < pattern.size();i++){
            mp[pattern[i]]++;
        }
        for(int r = 0;r < str.size(); r++){
            if(mp.find(str[r]) != mp.end()){
                mp[str[r]]--;
                if(mp[str[r]] >= 0){
                    matched++;
                }
            }
            while(matched == pattern.size()){
                if(minLen > r - l + 1){
                    minLen = r - l + 1;
                    sub = l;
                }
                if(mp.find(str[l]) != mp.end()){
                    if(mp[str[l]] == 0){
                        matched--;
                    }
                    mp[str[l]]++;
                }
                l++;
            }
        }
        return minLen == str.size() + 1?"":str.substr(sub,minLen);
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static string findSubstring(String str, String pattern){
        int l = 0;
        int matched = 0;
        int sub = 0;
        int minLen = str.length() + 1;
        Map<Character,Integer>mp = new HashMap<>();
        for(int i = 0;i < pattern.length();i++){
            mp.put(str.charAt(i), mp.getOrDefault(str.charAt(i),0) + 1);
        }
        for(int r = 0;r < str.length(); r++){
            if(mp.containsKey(str.charAt(r))){
                mp.put(str.charAt(r), mp.get(str.charAt(r)) - 1);
                if(mp.get(str.charAt(r)) >= 0){
                    matched++;
                }
            }
            while(matched == mp.size()){
                if(minLen > r - l + 1){
                    minLen = r - l + 1;
                    sub = l;
                }
                if(mp.containsKey(str.charAt(l))){
                    if(mp.get(str.charAt(l)) == 0){
                        matched--;
                    }
                    mp.put(str.charAt(l), mp.get(str.charAt(l)) + 1);
                }
                l++;
            }
        }
        return minLen == str.length() + 1?"":str.substr(sub,sub + minLen);
    }
};
```
Time Complexity: O(N + M)
Space Complexity: O(M)

### Question 12 Words Concatenation

Given a string and a list of words, find all the starting indices of substrings in the given string that are a concatenation of all the given words exactly once without any overlapping of words. It is given that all words are of the same length.

 - Example 1:
```sh
Input: String="catfoxcat", Words=["cat", "fox"]
Output: [0, 3]
Explanation: The two substring containing both the words are "catfox" & "foxcat".
```
 - Example 2:
```sh
Input: String="catcatfoxfox", Words=["cat", "fox"]
Output: [3]
Explanation: The only substring containing both the words is "catfox".
```
This problem follows the Sliding Window pattern and has a lot of similarities with Maximum Sum Subarray of Size K. We will keep track of all the words in a HashMap and try to match them in the given string. Here are the set of steps for our algorithm:

Keep the frequency of every word in a HashMap.
Starting from every index in the string, try to match all the words.
In each iteration, keep track of all the words that we have already seen in another HashMap.
If a word is not found or has a higher frequency than required, we can move on to the next character in the string.
Store the index if we have found all the words.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static vector<int> findWordConcatenation(const string& str, const vector<string> &words){
        vector<int> res;
        unordered_map<string, int> mp;
        for(auto w:words){
            mp[w]++;
        }
        int n = words.size();
        int m = words[0].size();
        for(int i = 0;i <= str.size() - m*n;i++){
            unordered_map<string, int> w;
            for(int j = 0;j < n;j++){
                int id = i + j*m;
                string word = str.substr(id,m);
                if(mp.find(word) == mp.end()){
                    break;
                }
                w[word]++;
                if(w[word] > mp[word]){
                    break;
                }
                if(j + 1 == n){
                    res.push_back(i);
                }
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
    public static List<Integer> findWordConcatenation(String str, String[] words){
        List<Integer> res = new ArrayList<>();
        Map<String, Integer> mp = new HashMap<>();
        for(String w:words){
            mp.put(w,mp.getOrDefault(w,0) + 1);
        }
        int n = words.length();
        int m = words[0].length();
        for(int i = 0;i <= str.length() - m*n;i++){
            Map<String, Integer> w = new HashMap<>();
            for(int j = 0;j < n;j++){
                int id = i + j*m;
                String word = str.substr(id,id + m);
                if(!mp.containsKey(word)){
                    break;
                }
                 w.put(word,w.getOrDefault(word,0) + 1);
                if(w.get(word) > mp.get(word){
                    break;
                }
                if(j + 1 == n){
                    res.add(i);
                }
            }
        }
        return res;
    }
};
```
Time Complexity: O(N * M * len)
Space Complexity: O(M)
### Todos

 - Write MORE Tests
 - Add Night Mode

License
----

MIT
