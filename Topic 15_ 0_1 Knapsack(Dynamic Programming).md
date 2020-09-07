# Topic 15: 0/1 Knapsack (Dynamic Programming)
 

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

0/1 Knapsack pattern is based on the famous problem with the same name which is efficiently solved using Dynamic Programming (DP).

In this pattern, we will go through a set of problems to develop an understanding of DP. We will always start with a brute-force recursive solution to see the overlapping subproblems, i.e., realizing that we are solving the same problems repeatedly.

After the recursive solution, we will modify our algorithm to apply advanced techniques of Memoization and Bottom-Up Dynamic Programming to develop a complete understanding of this pattern.


### Question 1: 0/1 Knapsack
Given the weights and profits of ‘N’ items, we are asked to put these items in a knapsack which has a capacity ‘C’. The goal is to get the maximum profit out of the items in the knapsack. Each item can only be selected once, as we don’t have multiple quantities of any item.

Let’s take the example of Merry, who wants to carry some fruits in the knapsack to get maximum profit. Here are the weights and profits of the fruits:

Items: { Apple, Orange, Banana, Melon }
Weights: { 2, 3, 1, 4 }
Profits: { 4, 5, 3, 7 }
Knapsack capacity: 5

Let’s try to put various combinations of fruits in the knapsack, such that their total weight is not more than 5:

Apple + Orange (total weight 5) => 9 profit
Apple + Banana (total weight 3) => 7 profit
Orange + Banana (total weight 4) => 8 profit
Banana + Melon (total weight 5) => 10 profit

This shows that Banana + Melon is the best combination as it gives us the maximum profit and the total weight does not exceed the capacity.

Given two integer arrays to represent weights and profits of ‘N’ items, we need to find a subset of these items which will give us maximum profit such that their cumulative weight is not more than a given number ‘C’. Each item can only be selected once, which means either we put an item in the knapsack or we skip it.

### Top-down Dynamic Programming with Memoization 
Memoization is when we store the results of all the previously solved sub-problems and return the results from memory if we encounter a problem that has already been solved.

Since we have two changing values (capacity and currentIndex) in our recursive function knapsackRecursive(), we can use a two-dimensional array to store the results of all the solved sub-problems. As mentioned above, we need to store results for every sub-array (i.e. for every possible index ‘i’) and for every possible capacity ‘c’.
### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static int solveKnapsack(const vector<int> &profit, vector<int> &weight, int capacity){
        vector<vector<int>>dp(weight.size(),vector<int>(capacity + 1, -1));
        return helper(profit, weight, capacity, dp, 0);
    }
    
    int helper(const vector<int> &profit, vector<int> &weight, int capacity, vector<vector<int>>& dp, int id){
        if(id >= profit.size() || capacity <= 0){
            return 0;
        }
        if(dp[id][capacity] != -1){
            return dp[id][capacity];
        }
        int profit1 = 0;
        if(weight[id] <= capacity){
            profit1 = profit[id] + helper(profit, weight, capacity - weight[id], dp, id + 1);
        }
        int profit2 = helper(profit, weight, capacity, dp, id+1);
        dp[id][capacity] = max(profit1, profit2);
        return dp[id][capacity];
    }
};
```
  - Java Solution:
 ```sh
 class Solution{
  public int solveKnapsack(int[] profits, int[] weights, int capacity) {
    Integer[][] dp = new Integer[profits.length][capacity + 1];
    return this.knapsackRecursive(dp, profits, weights, capacity, 0);
  }

  private int knapsackRecursive(Integer[][] dp, int[] profits, int[] weights, int capacity,
      int currentIndex) {

    // base checks
    if (capacity <= 0 || currentIndex >= profits.length)
      return 0;

    // if we have already solved a similar problem, return the result from memory
    if(dp[currentIndex][capacity] != null)
      return dp[currentIndex][capacity];

    // recursive call after choosing the element at the currentIndex
    // if the weight of the element at currentIndex exceeds the capacity, we shouldn't process this
    int profit1 = 0;
    if( weights[currentIndex] <= capacity )
        profit1 = profits[currentIndex] + knapsackRecursive(dp, profits, weights,
                capacity - weights[currentIndex], currentIndex + 1);

    // recursive call after excluding the element at the currentIndex
    int profit2 = knapsackRecursive(dp, profits, weights, capacity, currentIndex + 1);

    dp[currentIndex][capacity] = Math.max(profit1, profit2);
    return dp[currentIndex][capacity];
  }
 }
```
Time and Space complexity #
Time and Space complexity #
Since our memoization array dp[profits.length][capacity+1] stores the results for all subproblems, we can conclude that we will not have more than N*C subproblems (where ‘N’ is the number of items and ‘C’ is the knapsack capacity). This means that our time complexity will be O(N*C).

The above algorithm will use O(N*C) space for the memoization array. Other than that we will use O(N) space for the recursion call-stack. So the total space complexity will be O(N*C + N), which is asymptotically equivalent to O(N*C).O(N∗C).

### Bottom-up Dynamic Programming
Let’s try to populate our dp[][] array from the above solution by working in a bottom-up fashion. Essentially, we want to find the maximum profit for every sub-array and for every possible capacity. This means that dp[i][c] will represent the maximum knapsack profit for capacity ‘c’ calculated from the first ‘i’ items.

So, for each item at index ‘i’ (0 <= i < items.length) and capacity ‘c’ (0 <= c <= capacity), we have two options:

Exclude the item at index ‘i’. In this case, we will take whatever profit we get from the sub-array excluding this item => dp[i-1][c]
Include the item at index ‘i’ if its weight is not more than the capacity. In this case, we include its profit plus whatever profit we get from the remaining capacity and from remaining items => profit[i] + dp[i-1][c-weight[i]]
Finally, our optimal solution will be maximum of the above two values:

    dp[i][c] = max (dp[i-1][c], profit[i] + dp[i-1][c-weight[i]]) 
### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static int solveKnapsack(const vector<int> &profits, vector<int> &weight, int capacity){
        vector<vector<int>>dp(weight.size(),vector<int>(capacity + 1, 0));
        for(int i = 0;i <= capacity;i++){
            if(weight[0] <= i){
                dp[0][i] = profits[0];
            }
        }
        for(int i = 1;i < weight.size();i++){
            for(int j = 1;j <= capacity;j++){
            int profit1 = 0;
            int profit2 = 0;
                if(weight[i] <= j){
                    profit1 = profits[i] + dp[i - 1][c - weights[i]];
                }
                profit2 = dp[i - 1][c];
                dp[i][c] = max(profit1, profit2);
            }
        }
        return dp[weight.size() - 1][capacity];
    }
};
```
  - Java Solution:
 ```sh
 class Solution{
  public int solveKnapsack(int[] profits, int[] weights, int capacity) {
    // basic checks
    if (capacity <= 0 || profits.length == 0 || weights.length != profits.length)
      return 0;

    int n = profits.length;
    int[][] dp = new int[n][capacity + 1];

    // populate the capacity=0 columns, with '0' capacity we have '0' profit
    for(int i=0; i < n; i++)
      dp[i][0] = 0;

    // if we have only one weight, we will take it if it is not more than the capacity
    for(int c=0; c <= capacity; c++) {
      if(weights[0] <= c)
        dp[0][c] = profits[0];
    }

    // process all sub-arrays for all the capacities
    for(int i=1; i < n; i++) {
      for(int c=1; c <= capacity; c++) {
        int profit1= 0, profit2 = 0;
        // include the item, if it is not more than the capacity
        if(weights[i] <= c)
          profit1 = profits[i] + dp[i-1][c-weights[i]];
        // exclude the item
        profit2 = dp[i-1][c];
        // take maximum
        dp[i][c] = Math.max(profit1, profit2);
      }
    }
    // maximum profit will be at the bottom-right corner.
    return dp[n-1][capacity];
  }
 ```
### 1-d dp vector
### Solution:
 - C++ Solution:
```sh
Class solution{
public:
  int solveKnapsack(const vector<int> &profits, vector<int> &weights, int capacity) {
    // basic checks
    if (capacity <= 0 || profits.empty() || weights.size() != profits.size()) {
      return 0;
    }

    int n = profits.size();
    vector<int> dp(capacity + 1);

    // if we have only one weight, we will take it if it is not more than the
    // capacity
    for (int c = 0; c <= capacity; c++) {
      if (weights[0] <= c) {
        dp[c] = profits[0];
      }
    }

    // process all sub-arrays for all the capacities
    for (int i = 1; i < n; i++) {
      for (int c = capacity; c >= 0; c--) {
        int profit1 = 0, profit2 = 0;
        // include the item, if it is not more than the capacity
        if (weights[i] <= c) {
          profit1 = profits[i] + dp[c - weights[i]];
        }
        // exclude the item
        profit2 = dp[c];
        // take maximum
        dp[c] = max(profit1, profit2);
      }
    }

    return dp[capacity];
  }
};
 ```

### Question 2 Equal Subset Sum Partition

Given a set of positive numbers, find if we can partition it into two subsets such that the sum of elements in both subsets is equal.

 - Example 1
 ```sh
Input: {1, 2, 3, 4}
Output: True
Explanation: The given set can be partitioned into two subsets with equal sum: {1, 4} & {2, 3}
 ```
  - Example 2
 ```sh
Input: L1=[5, 8, 9], L2=[1, 7], K=3
Output: 7
Explanation: The 3rd smallest number among all the arrays is 7.
 ```
  - Example 3
 ```sh
Input: {2, 3, 4, 6}
Output: False
Explanation: The given set cannot be partitioned into two subsets with equal sum.
 ```
### Top-down Dynamic Programming with Memoization
We can use memoization to overcome the overlapping sub-problems. As stated in previous lessons, memoization is when we store the results of all the previously solved sub-problems so we can return the results from memory if we encounter a problem that has already been solved.

Since we need to store the results for every subset and for every possible sum, therefore we will be using a two-dimensional array to store the results of the solved sub-problems. The first dimension of the array will represent different subsets and the second dimension will represent different ‘sums’ that we can calculate from each subset. These two dimensions of the array can also be inferred from the two changing values (sum and currentIndex) in our recursive function canPartitionRecursive().
### Solution:
- C++ Solution:
```sh
Class solution{
public:
  static bool canPartition(const vector<int>& num) {
    int sum = 0;
    for(int i = 0;i < num.size();i++){
        sum += num[i];
    }
    if(sum % 2 != 0){
        return false;
    }
    vector<vector<int>> dp(num.size(),vector<int>(sum/2+1,-1));
    return helper(num, sum/2, dp, 0);
    }
    
    bool helper(const vector<int>& num, int sum, vector<vector<int>>& dp, int id){
        if(id >= num.size() || sum < 0){
            return false;
        }
        if(sum == 0){
            return true;
        }
        if(dp[id][sum] == -1){
            if(helper(num, sum - num[id], dp, id + 1){
                return true;
            }
            dp[id][sum] = helper(num, sum,dp,id+1);
        }
        return dp[id][sum] == 1?true:false;
    }
};
```
  - Java Solution:
 ```sh
  public boolean canPartition(int[] num) {
    int sum = 0;
    for (int i = 0; i < num.length; i++)
      sum += num[i];

    // if 'sum' is a an odd number, we can't have two subsets with equal sum
    if (sum % 2 != 0)
      return false;

    Boolean[][] dp = new Boolean[num.length][sum / 2 + 1];
    return this.canPartitionRecursive(dp, num, sum / 2, 0);
  }

  private boolean canPartitionRecursive(Boolean[][] dp, int[] num, int sum, int currentIndex) {
    // base check
    if (sum == 0)
      return true;

    if (num.length == 0 || currentIndex >= num.length)
      return false;

    // if we have not already processed a similar problem
    if (dp[currentIndex][sum] == null) {
      // recursive call after choosing the number at the currentIndex
      // if the number at currentIndex exceeds the sum, we shouldn't process this
      if (num[currentIndex] <= sum) {
        if (canPartitionRecursive(dp, num, sum - num[currentIndex], currentIndex + 1)) {
          dp[currentIndex][sum] = true;
          return true;
        }
      }

      // recursive call after excluding the number at the currentIndex
      dp[currentIndex][sum] = canPartitionRecursive(dp, num, sum, currentIndex + 1);
    }

    return dp[currentIndex][sum];
  }
```
The above algorithm has the time and space complexity of O(N*S)O(N∗S), where ‘N’ represents total numbers and ‘S’ is the total sum of all the numbers.
### Bottom-up Dynamic Programming
Let’s try to populate our dp[][] array from the above solution by working in a bottom-up fashion. Essentially, we want to find if we can make all possible sums with every subset. This means, dp[i][s] will be ‘true’ if we can make the sum ‘s’ from the first ‘i’ numbers.

So, for each number at index ‘i’ (0 <= i < num.length) and sum ‘s’ (0 <= s <= S/2), we have two options:

Exclude the number. In this case, we will see if we can get ‘s’ from the subset excluding this number: dp[i-1][s]
Include the number if its value is not more than ‘s’. In this case, we will see if we can find a subset to get the remaining sum: dp[i-1][s-num[i]]
If either of the two above scenarios is true, we can find a subset of numbers with a sum equal to ‘s’.

### Solution:
- C++ Solution:
```sh
Class solution{
public:
  bool canPartition(const vector<int> &num) {
    int n = num.size();
    // find the total sum
    int sum = 0;
    for (int i = 0; i < n; i++) {
      sum += num[i];
    }

    // if 'sum' is a an odd number, we can't have two subsets with same total
    if (sum % 2 != 0) {
      return false;
    }

    // we are trying to find a subset of given numbers that has a total sum of ‘sum/2’.
    sum /= 2;

    vector<vector<bool>> dp(n, vector<bool>(sum + 1));

    // populate the sum=0 columns, as we can always for '0' sum with an empty set
    for (int i = 0; i < n; i++) {
      dp[i][0] = true;
    }

    // with only one number, we can form a subset only when the required sum is
    // equal to its value
    for (int s = 1; s <= sum; s++) {
      dp[0][s] = (num[0] == s ? true : false);
    }

    // process all subsets for all sums
    for (int i = 1; i < n; i++) {
      for (int s = 1; s <= sum; s++) {
        // if we can get the sum 's' without the number at index 'i'
        if (dp[i - 1][s]) {
          dp[i][s] = dp[i - 1][s];
        } else if (s >= num[i]) {  // else if we can find a subset to get the remaining sum
          dp[i][s] = dp[i - 1][s - num[i]];
        }
      }
    }

    // the bottom-right corner will have our answer.
    return dp[n - 1][sum];
  }
};
```
  - Java Solution:
 ```sh
 class Solution
  public boolean canPartition(int[] num) {
    int n = num.length;
    // find the total sum
    int sum = 0;
    for (int i = 0; i < n; i++)
      sum += num[i];

    // if 'sum' is a an odd number, we can't have two subsets with same total
    if(sum % 2 != 0)
      return false;

    // we are trying to find a subset of given numbers that has a total sum of ‘sum/2’.
    sum /= 2;

    boolean[][] dp = new boolean[n][sum + 1];

    // populate the sum=0 columns, as we can always for '0' sum with an empty set
    for(int i=0; i < n; i++)
      dp[i][0] = true;

    // with only one number, we can form a subset only when the required sum is equal to its value
    for(int s=1; s <= sum ; s++) {
      dp[0][s] = (num[0] == s ? true : false);
    }

    // process all subsets for all sums
    for(int i=1; i < n; i++) {
      for(int s=1; s <= sum; s++) {
        // if we can get the sum 's' without the number at index 'i'
        if(dp[i-1][s]) {
          dp[i][s] = dp[i-1][s];
        } else if (s >= num[i]) { // else if we can find a subset to get the remaining sum
          dp[i][s] = dp[i-1][s-num[i]];
        }
      }
    }

    // the bottom-right corner will have our answer.
    return dp[n-1][sum];
  }
```
### Question 3 Subset Sum

Given a set of positive numbers, determine if a subset exists whose sum is equal to a given number ‘S’.

 - Example 1
 ```sh
Input: {1, 2, 3, 7}, S=6
Output: True
The given set has a subset whose sum is '6': {1, 2, 3}
 ```
  - Example 2
 ```sh
Input: {1, 2, 7, 1, 5}, S=10
Output: True
The given set has a subset whose sum is '10': {1, 2, 7}
 ```
  - Example 3
 ```sh
Input: {1, 3, 4, 8}, S=6
Output: False
The given set does not have any subset whose sum is equal to '6'.
 ```
We’ll try to find if we can make all possible sums with every subset to populate the array dp[TotalNumbers][S+1].

For every possible sum ‘s’ (where 0 <= s <= S), we have two options:

Exclude the number. In this case, we will see if we can get the sum ‘s’ from the subset excluding this number => dp[index-1][s]
Include the number if its value is not more than ‘s’. In this case, we will see if we can find a subset to get the remaining sum => dp[index-1][s-num[index]]
If either of the above two scenarios returns true, we can find a subset with a sum equal to ‘s’.


### Solution:
- C++ Solution:
```sh
 public:
  virtual bool canPartition(const vector<int> &num, int sum) {
    int n = num.size();
    vector<vector<bool>> dp(n, vector<bool>(sum + 1));

    // populate the sum=0 columns, as we can always form '0' sum with an empty set
    for (int i = 0; i < n; i++) {
      dp[i][0] = true;
    }

    // with only one number, we can form a subset only when the required sum is equal to its value
    for (int s = 1; s <= sum; s++) {
      dp[0][s] = (num[0] == s ? true : false);
    }

    // process all subsets for all sums
    for (int i = 1; i < num.size(); i++) {
      for (int s = 1; s <= sum; s++) {
        // if we can get the sum 's' without the number at index 'i'
        if (dp[i - 1][s]) {
          dp[i][s] = dp[i - 1][s];
        } else if (s >= num[i]) {
          // else include the number and see if we can find a subset to get the remaining sum
          dp[i][s] = dp[i - 1][s - num[i]];
        }
      }
    }

    // the bottom-right corner will have our answer.
    return dp[num.size() - 1][sum];
  }
};
```
  - Java Solution:
 ```sh

class Solution {
  public boolean canPartition(int[] num, int sum) {
    int n = num.length;
    boolean[][] dp = new boolean[n][sum + 1];

    // populate the sum=0 columns, as we can always form '0' sum with an empty set
    for (int i = 0; i < n; i++)
      dp[i][0] = true;

    // with only one number, we can form a subset only when the required sum is
    // equal to its value
    for (int s = 1; s <= sum; s++) {
      dp[0][s] = (num[0] == s ? true : false);
    }

    // process all subsets for all sums
    for (int i = 1; i < num.length; i++) {
      for (int s = 1; s <= sum; s++) {
        // if we can get the sum 's' without the number at index 'i'
        if (dp[i - 1][s]) {
          dp[i][s] = dp[i - 1][s];
        } else if (s >= num[i]) {
          // else include the number and see if we can find a subset to get the remaining
          // sum
          dp[i][s] = dp[i - 1][s - num[i]];
        }
      }
    }

    // the bottom-right corner will have our answer.
    return dp[num.length - 1][sum];
  }
```
 - Time Complexity
  O(N*s)
 - Space Complexity
  O(n*s)
### another solution
class SubsetSum {

  static boolean canPartition(int[] num, int sum) {
    int n = num.length;
    boolean[] dp = new boolean[sum + 1];

    // handle sum=0, as we can always have '0' sum with an empty set
    dp[0] = true;

    // with only one number, we can have a subset only when the required sum is equal to its value
    for (int s = 1; s <= sum; s++) {
      dp[s] = (num[0] == s ? true : false);
    }

    // process all subsets for all sums
    for (int i = 1; i < n; i++) {
      for (int s = sum; s >= 0; s--) {
        // if dp[s]==true, this means we can get the sum 's' without num[i], hence we can move on to
        // the next number else we can include num[i] and see if we can find a subset to get the
        // remaining sum
        if (!dp[s] && s >= num[i]) {
          dp[s] = dp[s - num[i]];
        }
      }
    }

    return dp[sum];
  }
}

### Question 4 Minimum Subset Sum Difference

Given a set of positive numbers, partition the set into two subsets with minimum difference between their subset sums.

 - Example 1
 ```sh
Input: {1, 2, 3, 9}
Output: 3
Explanation: We can partition the given set into two subsets where minimum absolute difference 
between the sum of numbers is '3'. Following are the two subsets: {1, 2, 3} & {9}.
 ```
  - Example 2
 ```sh
Input: {1, 2, 7, 1, 5}
Output: 0
Explanation: We can partition the given set into two subsets where minimum absolute difference 
between the sum of number is '0'. Following are the two subsets: {1, 2, 5} & {7, 1}.
 ```
   - Example 3
 ```sh
Input: {1, 3, 100, 4}
Output: 92
Explanation: We can partition the given set into two subsets where minimum absolute difference 
between the sum of numbers is '92'. Here are the two subsets: {1, 3, 4} & {100}.
 ```
### Top-down Dynamic Programming with Memorization
We can use memoization to overcome the overlapping sub-problems.

We will be using a two-dimensional array to store the results of the solved sub-problems. We can uniquely identify a sub-problem from ‘currentIndex’ and ‘Sum1’ as ‘Sum2’ will always be the sum of the remaining numbers.
### Solution:
 - C++ Solution:
```sh
class Solution{
  int canPartition(const vector<int> &num) {
    int sum = 0;
    for (int i = 0; i < num.size(); i++) {
      sum += num[i];
    }

    vector<vector<int>> dp(num.size(), vector<int>(sum + 1, -1));
    return this->canPartitionRecursive(dp, num, 0, 0, 0);
  }

 private:
  int canPartitionRecursive(vector<vector<int>> &dp, const vector<int> &num, int currentIndex,
                            int sum1, int sum2) {
    // base check
    if (currentIndex == num.size()) {
      return abs(sum1 - sum2);
    }

    // check if we have not already processed similar problem
    if (dp[currentIndex][sum1] == -1) {
      // recursive call after including the number at the currentIndex in the first set
      int diff1 = canPartitionRecursive(dp, num, currentIndex + 1, sum1 + num[currentIndex], sum2);

      // recursive call after including the number at the currentIndex in the second set
      int diff2 = canPartitionRecursive(dp, num, currentIndex + 1, sum1, sum2 + num[currentIndex]);

      dp[currentIndex][sum1] = min(diff1, diff2);
    }

    return dp[currentIndex][sum1];
  }
};
```
  - Java Solution:
 ```sh

class SmallestRange {
  public int canPartition(int[] num) {
    int sum = 0;
    for (int i = 0; i < num.length; i++)
      sum += num[i];

    Integer[][] dp = new Integer[num.length][sum + 1];
    return this.canPartitionRecursive(dp, num, 0, 0, 0);
  }

  private int canPartitionRecursive(Integer[][] dp, int[] num, int currentIndex, int sum1, int sum2) {
    // base check
    if(currentIndex == num.length)
      return Math.abs(sum1 - sum2);

    // check if we have not already processed similar problem
    if(dp[currentIndex][sum1] == null) {
      // recursive call after including the number at the currentIndex in the first set
      int diff1 = canPartitionRecursive(dp, num, currentIndex + 1, sum1 + num[currentIndex], sum2);

      // recursive call after including the number at the currentIndex in the second set
      int diff2 = canPartitionRecursive(dp, num, currentIndex + 1, sum1, sum2 + num[currentIndex]);

      dp[currentIndex][sum1] = Math.min(diff1, diff2);
    }

    return dp[currentIndex][sum1];
  }
```
 - Time Complexity
   O(n*logn)
 - Space Complexity
O(n)
### Bottom-up Dynamic Programming
- C++ Solution
 ```sh
class PartitionSet {
 public:
  virtual int canPartition(const vector<int> &num) {
    int sum = 0;
    for (int i = 0; i < num.size(); i++) {
      sum += num[i];
    }

    int n = num.size();
    vector<vector<bool>> dp(n, vector<bool>(sum / 2 + 1, false));

    // populate the sum=0 columns, as we can always form '0' sum with an empty set
    for (int i = 0; i < n; i++) {
      dp[i][0] = true;
    }

    // with only one number, we can form a subset only when the sum is equal to that number
    for (int s = 0; s <= sum / 2; s++) {
      dp[0][s] = (num[0] == s ? true : false);
    }

    // process all subsets for all sums
    for (int i = 1; i < num.size(); i++) {
      for (int s = 1; s <= sum / 2; s++) {
        // if we can get the sum 's' without the number at index 'i'
        if (dp[i - 1][s]) {
          dp[i][s] = dp[i - 1][s];
        } else if (s >= num[i]) {
          // else include the number and see if we can find a subset to get the remaining sum
          dp[i][s] = dp[i - 1][s - num[i]];
        }
      }
    }

    int sum1 = 0;
    // Find the largest index in the last row which is true
    for (int i = sum / 2; i >= 0; i--) {
      if (dp[n - 1][i] == true) {
        sum1 = i;
        break;
      }
    }

    int sum2 = sum - sum1;
    return abs(sum2 - sum1);
  }
};
 ```
- Java Solution
 ```sh
class PartitionSet {
 public:
  virtual int canPartition(const vector<int> &num) {
    int sum = 0;
    for (int i = 0; i < num.size(); i++) {
      sum += num[i];
    }

    int n = num.size();
    vector<vector<bool>> dp(n, vector<bool>(sum / 2 + 1, false));

    // populate the sum=0 columns, as we can always form '0' sum with an empty set
    for (int i = 0; i < n; i++) {
      dp[i][0] = true;
    }

    // with only one number, we can form a subset only when the sum is equal to that number
    for (int s = 0; s <= sum / 2; s++) {
      dp[0][s] = (num[0] == s ? true : false);
    }

    // process all subsets for all sums
    for (int i = 1; i < num.size(); i++) {
      for (int s = 1; s <= sum / 2; s++) {
        // if we can get the sum 's' without the number at index 'i'
        if (dp[i - 1][s]) {
          dp[i][s] = dp[i - 1][s];
        } else if (s >= num[i]) {
          // else include the number and see if we can find a subset to get the remaining sum
          dp[i][s] = dp[i - 1][s - num[i]];
        }
      }
    }

    int sum1 = 0;
    // Find the largest index in the last row which is true
    for (int i = sum / 2; i >= 0; i--) {
      if (dp[n - 1][i] == true) {
        sum1 = i;
        break;
      }
    }

    int sum2 = sum - sum1;
    return abs(sum2 - sum1);
  }
};
 ```
 O(N∗S)
### Question 5 Count of Subset Sum
Given a set of positive numbers, find the total number of subsets whose sum is equal to a given number ‘S’.
 - Example 1
 ```sh
Input: {1, 1, 2, 3}, S=4
Output: 3
The given set has '3' subsets whose sum is '4': {1, 1, 2}, {1, 3}, {1, 3}
Note that we have two similar sets {1, 3}, because we have two '1' in our input.
 ```
  - Example 2
 ```sh
Input: {1, 2, 7, 1, 5}, S=9
Output: 3
The given set has '3' subsets whose sum is '9': {2, 7}, {1, 7, 1}, {1, 2, 1, 5}
 ```
### Top-down Dynamic Programming with Memorization
We can use memoization to overcome the overlapping sub-problems. We will be using a two-dimensional array to store the results of solved sub-problems. As mentioned above, we need to store results for every subset and for every possible sum.
### Solution:
 - C++ Solution:
```sh
 public:
  virtual int countSubsets(const vector<int> &num, int sum) {
    vector<vector<int>> dp(num.size(), vector<int>(sum + 1, -1));
    return this->countSubsetsRecursive(dp, num, sum, 0);
  }

 private:
  int countSubsetsRecursive(vector<vector<int>> &dp, const vector<int> &num, int sum,
                            int currentIndex) {
    // base checks
    if (sum == 0) {
      return 1;
    }

    if (num.empty() || currentIndex >= num.size()) {
      return 0;
    }

    // check if we have not already processed a similar problem
    if (dp[currentIndex][sum] == -1) {
      // recursive call after choosing the number at the currentIndex
      // if the number at currentIndex exceeds the sum, we shouldn't process this
      int sum1 = 0;
      if (num[currentIndex] <= sum) {
        sum1 = countSubsetsRecursive(dp, num, sum - num[currentIndex], currentIndex + 1);
      }

      // recursive call after excluding the number at the currentIndex
      int sum2 = countSubsetsRecursive(dp, num, sum, currentIndex + 1);

      dp[currentIndex][sum] = sum1 + sum2;
    }

    return dp[currentIndex][sum];
  }
};
```
  - Java Solution:
 ```sh

class Solution {

  public int countSubsets(int[] num, int sum) {
    Integer[][] dp = new Integer[num.length][sum + 1];
    return this.countSubsetsRecursive(dp, num, sum, 0);
  }

  private int countSubsetsRecursive(Integer[][] dp, int[] num, int sum, int currentIndex) {
    // base checks
    if (sum == 0)
      return 1;

    if(num.length == 0 || currentIndex >= num.length)
      return 0;

    // check if we have not already processed a similar problem
    if(dp[currentIndex][sum] == null) {
      // recursive call after choosing the number at the currentIndex
      // if the number at currentIndex exceeds the sum, we shouldn't process this
      int sum1 = 0;
      if( num[currentIndex] <= sum )
        sum1 = countSubsetsRecursive(dp, num, sum - num[currentIndex], currentIndex + 1);

      // recursive call after excluding the number at the currentIndex
      int sum2 = countSubsetsRecursive(dp, num, sum, currentIndex + 1);

      dp[currentIndex][sum] = sum1 + sum2;
    }

    return dp[currentIndex][sum];
  }
```
 - Time Complexity
   O(N * S)
 - Space Complexity
O(N * S)
### Bottom-up Dynamic Programming
We will try to find if we can make all possible sums with every subset to populate the array db[TotalNumbers][S+1].

So, at every step we have two options:

Exclude the number. Count all the subsets without the given number up to the given sum => dp[index-1][sum]
Include the number if its value is not more than the ‘sum’. In this case, we will count all the subsets to get the remaining sum => dp[index-1][sum-num[index]]
To find the total sets, we will add both of the above two values:

    dp[index][sum] = dp[index-1][sum] + dp[index-1][sum-num[index]])
### Solution:
 - C++ Solution:
```sh
 public:
    virtual int countSubsets(const vector<int> &num, int sum) {
    int n = num.size();
    vector<vector<int>> dp(n, vector<int>(sum + 1, 0));

    // populate the sum=0 columns, as we will always have an empty set for zero sum
    for (int i = 0; i < n; i++) {
      dp[i][0] = 1;
    }

    // with only one number, we can form a subset only when the required sum is
    // equal to its value
    for (int s = 1; s <= sum; s++) {
      dp[0][s] = (num[0] == s ? 1 : 0);
    }

    // process all subsets for all sums
    for (int i = 1; i < num.size(); i++) {
      for (int s = 1; s <= sum; s++) {
        // exclude the number
        dp[i][s] = dp[i - 1][s];
        // include the number, if it does not exceed the sum
        if (s >= num[i]) {
          dp[i][s] += dp[i - 1][s - num[i]];
        }
      }
    }

    // the bottom-right corner will have our answer.
    return dp[num.size() - 1][sum];
  }
};
```
  - Java Solution:
 ```sh

class Solution {
  public int countSubsets(int[] num, int sum) {
    int n = num.length;
    int[][] dp = new int[n][sum + 1];

    // populate the sum=0 columns, as we will always have an empty set for zero sum
    for(int i=0; i < n; i++)
      dp[i][0] = 1;

    // with only one number, we can form a subset only when the required sum is equal to its value
    for(int s=1; s <= sum ; s++) {
      dp[0][s] = (num[0] == s ? 1 : 0);
    }

    // process all subsets for all sums
    for(int i=1; i < num.length; i++) {
      for(int s=1; s <= sum; s++) {
        // exclude the number
        dp[i][s] = dp[i-1][s];
        // include the number, if it does not exceed the sum
        if(s >= num[i])
          dp[i][s] += dp[i-1][s-num[i]];
      }
    }

    // the bottom-right corner will have our answer.
    return dp[num.length-1][sum];
  }
```
 - Time Complexity
   O(N * S)
 - Space Complexity
O(N * S)
### O(S) Space
### Solution:
 - C++ Solution:
```sh
 public:
  static int countSubsets(const vector<int> &num, int sum) {
    int n = num.size();
    vector<int> dp(sum + 1);
    dp[0] = 1;

    // with only one number, we can form a subset only when the required sum is
    // equal to its value
    for (int s = 1; s <= sum; s++) {
      dp[s] = (num[0] == s ? 1 : 0);
    }

    // process all subsets for all sums
    for (int i = 1; i < num.size(); i++) {
      for (int s = sum; s >= 0; s--) {
        if (s >= num[i]) {
          dp[s] += dp[s - num[i]];
        }
      }
    }

    return dp[sum];
  }
};
```
### Todos
### Question 5 Target Sum

You are given a set of positive numbers and a target sum ‘S’. Each number should be assigned either a ‘+’ or ‘-’ sign. We need to find the total ways to assign symbols to make the sum of the numbers equal to the target ‘S’.

 - Example 1
 ```sh
Input: {1, 1, 2, 3}, S=1
Output: 3
Explanation: The given set has '3' ways to make a sum of '1': {+1-1-2+3} & {-1+1-2+3} & {+1+1+2-3}
 ```
  - Example 2
 ```sh
Input: {1, 2, 7, 1}, S=9
Output: 2
Explanation: The given set has '2' ways to make a sum of '9': {+1+2+7-1} & {-1+2+7+1}
 ```
This problem follows the 0/1 Knapsack pattern and can be converted into Count of Subset Sum. Let’s dig into this.

We are asked to find two subsets of the given numbers whose difference is equal to the given target ‘S’. Take the first example above. As we saw, one solution is {+1-1-2+3}. So, the two subsets we are asked to find are {1, 3} & {1, 2} because,

    (1 + 3) - (1 + 2 ) = 1
Now, let’s say ‘Sum(s1)’ denotes the total sum of set ‘s1’, and ‘Sum(s2)’ denotes the total sum of set ‘s2’. So the required equation is:

    Sum(s1) - Sum(s2) = S
This equation can be reduced to the subset sum problem. Let’s assume that ‘Sum(num)’ denotes the total sum of all the numbers, therefore:

    Sum(s1) + Sum(s2) = Sum(num)
Let’s add the above two equations:

    => Sum(s1) - Sum(s2) + Sum(s1) + Sum(s2) = S + Sum(num)
    => 2 * Sum(s1) =  S + Sum(num)
    => Sum(s1) = (S + Sum(num)) / 2
Which means that one of the set ‘s1’ has a sum equal to (S + Sum(num)) / 2. This essentially converts our problem to: "Find the count of subsets of the given numbers whose sum is equal to (S + Sum(num)) / 2"

### Solution:
- C++ Solution:
```sh
class TargetSum {
 public:
  int findTargetSubsets(const vector<int> &num, int s) {
    int totalSum = 0;
    for (auto n : num) {
      totalSum += n;
    }

    // if 's + totalSum' is odd, we can't find a subset with sum equal to '(s + totalSum) / 2'
    if (totalSum < s || (s + totalSum) % 2 == 1) {
      return 0;
    }

    return countSubsets(num, (s + totalSum) / 2);
  }

  // this function is exactly similar to what we have in 'Count of Subset Sum' problem.
 private:
  int countSubsets(const vector<int> &num, int sum) {
    int n = num.size();
    vector<vector<int>> dp(n, vector<int>(sum + 1, 0));

    // populate the sum=0 columns, as we will always have an empty set for zero sum
    for (int i = 0; i < n; i++) {
      dp[i][0] = 1;
    }

    // with only one number, we can form a subset only when the required sum is
    // equal to the number
    for (int s = 1; s <= sum; s++) {
      dp[0][s] = (num[0] == s ? 1 : 0);
    }

    // process all subsets for all sums
    for (int i = 1; i < num.size(); i++) {
      for (int s = 1; s <= sum; s++) {
        dp[i][s] = dp[i - 1][s];
        if (s >= num[i]) {
          dp[i][s] += dp[i - 1][s - num[i]];
        }
      }
    }

    // the bottom-right corner will have our answer.
    return dp[num.size() - 1][sum];
  }
};
```
  - Java Solution:
 ```sh

class Solution {
  public int findTargetSubsets(int[] num, int s) {
    int totalSum = 0;
    for (int n : num)
        totalSum += n;

    // if 's + totalSum' is odd, we can't find a subset with sum equal to '(s + totalSum) / 2'
    if(totalSum < s || (s + totalSum) % 2 == 1)
        return 0;

    return countSubsets(num, (s + totalSum) / 2);
  }

  // this function is exactly similar to what we have in 'Count of Subset Sum' problem.
  private int countSubsets(int[] num, int sum) {
    int n = num.length;
    int[][] dp = new int[n][sum + 1];

    // populate the sum=0 columns, as we will always have an empty set for zero sum
    for(int i=0; i < n; i++)
      dp[i][0] = 1;

    // with only one number, we can form a subset only when the required sum is equal to the number
    for(int s=1; s <= sum ; s++) {
      dp[0][s] = (num[0] == s ? 1 : 0);
    }

    // process all subsets for all sums
    for(int i=1; i < num.length; i++) {
      for(int s=1; s <= sum; s++) {
          dp[i][s] = dp[i-1][s];
          if(s >= num[i])
            dp[i][s] += dp[i-1][s-num[i]];
      }
    }

    // the bottom-right corner will have our answer.
    return dp[num.length-1][sum];
  }
```
 - Time Complexity
  O(N*s)
 - Space Complexity
  O(n*s)
### another solution
```
class TargetSum {
 public:
  int findTargetSubsets(const vector<int> &num, int s) {
    int totalSum = 0;
    for (auto n : num) {
      totalSum += n;
    }

    // if 's + totalSum' is odd, we can't find a subset with sum equal to '(s + totalSum) / 2'
    if (totalSum < s || (s + totalSum) % 2 == 1) {
      return 0;
    }

    return countSubsets(num, (s + totalSum) / 2);
  }

  // this function is exactly similar to what we have in 'Count of Subset Sum' problem.
 private:
  int countSubsets(const vector<int> &num, int sum) {
    int n = num.size();
    vector<int> dp(sum + 1);
    dp[0] = 1;

    // with only one number, we can form a subset only when the required sum is
    // equal to the number
    for (int s = 1; s <= sum; s++) {
      dp[s] = (num[0] == s ? 1 : 0);
    }

    // process all subsets for all sums
    for (int i = 1; i < num.size(); i++) {
      for (int s = sum; s >= 0; s--) {
        if (s >= num[i]) {
          dp[s] += dp[s - num[i]];
        }
      }
    }

    return dp[sum];
  }
};
```
 - Write MORE Tests
 - Add Night Mode

License
----

MIT
