# Topic 10: Subsets


[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

A huge number of coding interview problems involve dealing with Permutations and Combinations of a given set of elements. This pattern describes an efficient Breadth First Search (BFS) approach to handle all these problems.


### Question 1: Subsets

Given a set with distinct elements, find all of its distinct subsets.

 - Example 1
 ```sh
Input: [1, 3]
Output: [], [1], [3], [1,3]
 ```
 
  - Example 2
 ```sh
Input: [1, 5, 3]
Output: [], [1], [5], [3], [1,5], [1, 3], [5, 3], [1, 5, 3]
 ```
To generate all subsets of the given set, we can use the Breadth First Search (BFS) approach. We can start with an empty set, iterate through all numbers one-by-one, and add them to existing sets to create new subsets.

Let’s take the example-2 mentioned above to go through each step of our algorithm:

Given set: [1, 5, 3]

Start with an empty set: [[]]
Add the first number (1) to all the existing subsets to create new subsets: [[], [1]];
Add the second number (5) to all the existing subsets: [[], [1], [5], [1,5]];
Add the third number (3) to all the existing subsets: [[], [1], [5], [1,5], [3], [1,3], [5,3], [1,5,3]].
Here is the visual representation of the above steps:
Since the input set has distinct elements, the above steps will ensure that we will not have any duplicate subsets.
 
### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static vector<vector<int>> findSubsets(const vector<int>& nums){
        vector<vector<int>> res;
        res.push_back({});
        for(int i = 0;i < nums.size();i++){
            for(int j = 0;j < res.size();i++){
                vector<int> tmp(res[i]);
                tmp.push_back(nums[i]);
                res.push_back(tmp);
            }
        }
        return res;
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static List<List<Integer>> findSubsets(int[] nums){
        List<List<Integer>> res = new ArrayList<>();
        res.add(new ArrayList<>());
        for(int i = 0;i < nums.length;i++){
            for(int j = 0;j < res.size();i++){
                List<Integer> tmp = new ArrayList(res.get(i));
                tmp.add(nums[i]);
                res.add(tmp);
            }
        }
        return res;
};
```
Time complexity #
O(N * 2^N)
Space complexity #
O(2^N)


### Question 2 Subsets with Duplicates
Given a set of numbers that might contain duplicates, find all of its distinct subsets.

 - Example 1
 ```sh
Input: [1, 3, 3]
Output: [], [1], [3], [1,3], [3,3], [1,3,3]
 ```
  - Example 2
 ```sh
Input: [1, 5, 3, 3]
Output: [], [1], [5], [3], [1,5], [1,3], [5,3], [1,5,3], [3,3], [1,3,3], [3,3,5], [1,5,3,3] 
 ```
This problem follows the Subsets pattern and we can follow a similar Breadth First Search (BFS) approach. The only additional thing we need to do is handle duplicates. Since the given set can have duplicate numbers, if we follow the same approach discussed in Subsets, we will end up with duplicate subsets, which is not acceptable. To handle this, we will do two extra things:

Sort all numbers of the given set. This will ensure that all duplicate numbers are next to each other.
Follow the same BFS approach but whenever we are about to process a duplicate (i.e., when the current and the previous numbers are same), instead of adding the current number (which is a duplicate) to all the existing subsets, only add it to the subsets which were created in the previous step.
Let’s take Example-2 mentioned above to go through each step of our algorithm:

Given set: [1, 5, 3, 3]  
Sorted set: [1, 3, 3, 5]
Start with an empty set: [[]]
Add the first number (1) to all the existing subsets to create new subsets: [[], [1]];
Add the second number (3) to all the existing subsets: [[], [1], [3], [1,3]].
The next number (3) is a duplicate. If we add it to all existing subsets we will get:
    [[], [1], [3], [1,3], [3], [1,3], [3,3], [1,3,3]]
We got two duplicate subsets: [3], [1,3]  
Whereas we only needed the new subsets: [3,3], [1,3,3]  
To handle this instead of adding (3) to all the existing subsets, we only add it to the new subsets which were created in the previous (3rd) step:

    [[], [1], [3], [1,3], [3,3], [1,3,3]]
Finally, add the forth number (5) to all the existing subsets: [[], [1], [3], [1,3], [3,3], [1,3,3], [5], [1,5], [3,5], [1,3,5], [3,3,5], [1,3,3,5]]
### Solution:
- C++ Solution:
```sh
Class solution{
public:
    static vector<vector<int>> findSubsets(const vector<int>& nums){
        vector<vector<int>> res;
        res.push_back({});
        int startIdex = 0;
        int endIdex = 0;
        for(int i = 0;i < nums.size();i++){
            startIdex = 0;
            if(i != 0 && nums[i] == nums[i-1]){
                startIdex = endIdex + 1;
            }
            endIndex = res.size() - 1;
            for(int j = startIdex;j <= endIndex;i++){
                vector<int> tmp(res[i]);
                tmp.push_back(nums[i]);
                res.push_back(tmp);
            }
        }
        return res;
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static List<List<Integer>> findSubsets(int[] nums){
        List<List<Integer>> res = new ArrayList<>();
        res.add(new ArrayList<>());
        int startId = 0;
        int endId = 0;
        for(int i = 0;i < nums.length;i++){
            startId = 0;
            if(i == 0 && nums[i] == nums[i-1]){
                startId = endId + 1;
            }
            endId = res.size() - 1;
            for(int j = startId;j <= endId;i++){
                List<Integer> tmp = new ArrayList(res.get(i));
                tmp.add(nums[i]);
                res.add(tmp);
            }
        }
        return res;
};
```
Time Complexity: O(2^N)
Space Complexity: O(2^N)

### Question 3 Permutation

Given a set of distinct numbers, find all of its permutations.

Permutation is defined as the re-arranging of the elements of the set. For example, {1, 2, 3} has the following six permutations:

{1, 2, 3}
{1, 3, 2}
{2, 1, 3}
{2, 3, 1}
{3, 1, 2}
{3, 2, 1}
If a set has ‘n’ distinct elements it will have n!n! permutations.

 - Example 1
 ```sh
Input: [1,3,5]
Output: [1,3,5], [1,5,3], [3,1,5], [3,5,1], [5,1,3], [5,3,1]
 ```
### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  vector<vector<int>> findPermutations(const vector<int> &nums){
        vector<vector<int>> res;
        if(nums.size() == 0){
            return res;
        }
        vector<int> cur;
        vector<bool> visited(nums.size(),0);
        helper(nums,0,cur,res,visited);
        return res;
    }
    
    void helper(const vector<int> &nums, int id, vector<int> &cur, vector<vector<int>>& res, vector<bool>& visited){
        if(id == nums.size()){
            res.push_back(cur);
            return;
        }
        for(int i = 0;i < nums.size();i++){
            if(visited[i] == true){continue;}
            cur.push_back(nums[i]);
            visited[i] = true;
            helper(nums, id + 1, cur, res, visited);
            cur.pop_back();
            visited[i] = false;
        }
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static  List<List<Integer>> findPermutations(const int[] nums){
        List<List<Integer>> res = new ArrayList<>();
        if(nums.length == 0){
            return res;
        }
        List<Integer> cur;
        List<Boolean> visited = new ArrayList(nums.length);
        helper(nums,0,cur,res,visited);
        return res;
    }
    
    void helper(const int[] nums, int id, List<Integer> cur, List<List<Integer>> res, List<Boolean> visited){
        if(id == nums.length){
            res.add(cur);
            return;
        }
        for(int i = 0;i < nums.length;i++){
            if(visited.get(i) == true){continue;}
            cur.add(nums[i]);
            visited.get(i) = true;
            helper(nums, id + 1, cur, res, visited);
            cur.remove(cur.size() - 1);
            visited.get(i) = false;
        }
    }
};
```
 - Time Complexity
  O(N∗N!)
 - Space Complexity
  O(N∗N!)

### Question 4 String Permutations by changing case

Given a string, find all of its permutations preserving the character sequence but changing case.

 - Example 1
 ```sh
Input: "ad52"
Output: "ad52", "Ad52", "aD52", "AD52" 
 ```
  - Example 2
 ```sh
Input: "ab7c"
Output: "ab7c", "Ab7c", "aB7c", "AB7c", "ab7C", "Ab7C", "aB7C", "AB7C"
 ```
 
This problem follows the Subsets pattern and can be mapped to Permutations.

Let’s take Example-2 mentioned above to generate all the permutations. Following a BFS approach, we will consider one character at a time. Since we need to preserve the character sequence, we can start with the actual string and process each character (i.e., make it upper-case or lower-case) one by one:

Starting with the actual string: "ab7c"
Processing the first character (‘a’), we will get two permutations: "ab7c", "Ab7c"
Processing the second character (‘b’), we will get four permutations: "ab7c", "Ab7c", "aB7c", "AB7c"
Since the third character is a digit, we can skip it.
Processing the fourth character (‘c’), we will get a total of eight permutations: "ab7c", "Ab7c", "aB7c", "AB7c", "ab7C", "Ab7C", "aB7C", "AB7C"
Let’s analyze the permutations in the 3rd and the 5th step. How can we generate the permutations in the 5th step from the permutations in the 3rd step?

If we look closely, we will realize that in the 5th step, when we processed the new character (‘c’), we took all the permutations of the previous step (3rd) and changed the case of the letter (‘c’) in them to create four new permutations.

### Solution:
 - C++ Solution:
```sh
class solution {
 public:
  static vector<string> findLetterCaseStringPermutations(const string& str) {
    vector<string> permutations;
    if (str == "") {
      return permutations;
    }

    permutations.push_back(str);
    // process every character of the string one by one
    for (int i = 0; i < str.length(); i++) {
      if (isalpha(str[i])) {  // only process characters, skip digits
        // we will take all existing permutations and change the letter case appropriately
        int n = permutations.size();
        for (int j = 0; j < n; j++) {
          vector<char> chs(permutations[j].begin(), permutations[j].end());
          // if the current character is in upper case change it to lower case or vice versa
          if (isupper(chs[i])) {
            chs[i] = tolower(chs[i]);
          } else {
            chs[i] = toupper(chs[i]);
          }
          permutations.push_back(string(chs.begin(), chs.end()));
        }
      }
    }
    return permutations;
  }
};
```
  - Java Solution:
 ```sh
Class solution{
  public static List<String> findLetterCaseStringPermutations(String str) {
    List<String> permutations = new ArrayList<>();
    if (str == null)
      return permutations;

    permutations.add(str);
    // process every character of the string one by one
    for (int i = 0; i < str.length(); i++) {
      if (Character.isLetter(str.charAt(i))) { // only process characters, skip digits
        // we will take all existing permutations and change the letter case appropriately
        int n = permutations.size();
        for (int j = 0; j < n; j++) {
          char[] chs = permutations.get(j).toCharArray();
          // if the current character is in upper case change it to lower case or vice versa
          if (Character.isUpperCase(chs[i]))
            chs[i] = Character.toLowerCase(chs[i]);
          else
            chs[i] = Character.toUpperCase(chs[i]);
          permutations.add(String.valueOf(chs));
        }
      }
    }
    return permutations;
  }
```
 - Time Complexity
   O(N*2^N)
 - Space Complexity
O(N*2^N)
### Question 5 Balanced Parenthese

For a given number ‘N’, write a function to generate all combination of ‘N’ pairs of balanced parentheses.

 - Example 1
 ```sh
Input: N=2
Output: (()), ()()
 ```
  - Example 2
 ```sh
Input: N=3
Output: ((())), (()()), (())(), ()(()), ()()()
 ```
This problem follows the Subsets pattern and can be mapped to Permutations. We can follow a similar BFS approach.

Let’s take Example-2 mentioned above to generate all the combinations of balanced parentheses. Following a BFS approach, we will keep adding open parentheses ( or close parentheses ). At each step we need to keep two things in mind:

We can’t add more than ‘N’ open parenthesis.
To keep the parentheses balanced, we can add a close parenthesis ) only when we have already added enough open parenthesis (. For this, we can keep a count of open and close parenthesis with every combination.
Following this guideline, let’s generate parentheses for N=3:

Start with an empty combination: “”
At every step, let’s take all combinations of the previous step and add ( or ) keeping the above-mentioned two rules in mind.
For the empty combination, we can add ( since the count of open parenthesis will be less than ‘N’. We can’t add ) as we don’t have an equivalent open parenthesis, so our list of combinations will now be: “(”
For the next iteration, let’s take all combinations of the previous set. For “(” we can add another ( to it since the count of open parenthesis will be less than ‘N’. We can also add ) as we do have an equivalent open parenthesis, so our list of combinations will be: “((”, “()”
In the next iteration, for the first combination “((”, we can add another ( to it as the count of open parenthesis will be less than ‘N’, we can also add ) as we do have an equivalent open parenthesis. This gives us two new combinations: “(((” and “(()”. For the second combination “()”, we can add another ( to it since the count of open parenthesis will be less than ‘N’. We can’t add ) as we don’t have an equivalent open parenthesis, so our list of combinations will be: “(((”, “(()”, ()("
Following the same approach, next we will get the following list of combinations: “((()”, “(()(”, “(())”, “()((”, “()()”
Next we will get: “((())”, “(()()”, “(())(”, “()(()”, “()()(”
Finally, we will have the following combinations of balanced parentheses: “((()))”, “(()())”, “(())()”, “()(())”, “()()()”
We can’t add more parentheses to any of the combinations, so we stop here.

### Solution:
 - C++ Solution:
```sh
class solution {
 public:
  static vector<string> generateValidParentheses(int num) {
    vector<string> result;
    vector<char> parenthesesString(2 * num);
    generateValidParenthesesRecursive(num, 0, 0, parenthesesString, 0, result);
    return result;
  }

 private:
  static void generateValidParenthesesRecursive(int num, int openCount, int closeCount,
                                                vector<char> &parenthesesString, int index,
                                                vector<string> &result) {
    // if we've reached the maximum number of open and close parentheses, add to the result
    if (openCount == num && closeCount == num) {
      result.push_back(string(parenthesesString.begin(), parenthesesString.end()));
    } else {
      if (openCount < num) {  // if we can add an open parentheses, add it
        parenthesesString[index] = '(';
        generateValidParenthesesRecursive(num, openCount + 1, closeCount, parenthesesString,
                                          index + 1, result);
      }

      if (openCount > closeCount) {  // if we can add a close parentheses, add it
        parenthesesString[index] = ')';
        generateValidParenthesesRecursive(num, openCount, closeCount + 1, parenthesesString,
                                          index + 1, result);
      }
    }
  }
};
```
  - Java Solution:
 ```sh
Class solution{
  public static List<String> generateValidParentheses(int num) {
    List<String> result = new ArrayList<String>();
    char[] parenthesesString = new char[2 * num];
    generateValidParenthesesRecursive(num, 0, 0, parenthesesString, 0, result);
    return result;
  }

  private static void generateValidParenthesesRecursive(int num, int openCount, int closeCount,
      char[] parenthesesString, int index, List<String> result) {

    // if we've reached the maximum number of open and close parentheses, add to the result
    if (openCount == num && closeCount == num) {
      result.add(new String(parenthesesString));
    } else {
      if (openCount < num) { // if we can add an open parentheses, add it
        parenthesesString[index] = '(';
        generateValidParenthesesRecursive(num, openCount + 1, closeCount, parenthesesString, index + 1, result);
      }

      if (openCount > closeCount) { // if we can add a close parentheses, add it
        parenthesesString[index] = ')';
        generateValidParenthesesRecursive(num, openCount, closeCount + 1, parenthesesString, index + 1, result);
      }
    }
  }
```
 - Time Complexity
   O(N*2^N)
 - Space Complexity
O(N*2^N)
### Question 6 Unique Generalized Abbreviations

Given a word, write a function to generate all of its unique generalized abbreviations.

Generalized abbreviation of a word can be generated by replacing each substring of the word by the count of characters in the substring. Take the example of “ab” which has four substrings: “”, “a”, “b”, and “ab”. After replacing these substrings in the actual word by the count of characters we get all the generalized abbreviations: “ab”, “1b”, “a1”, and “2”.

 - Example 1
 ```sh
Input: "BAT"
Output: "BAT", "BA1", "B1T", "B2", "1AT", "1A1", "2T", "3"
 ```
  - Example 2
 ```sh
Input: "code"
Output: "code", "cod1", "co1e", "co2", "c1de", "c1d1", "c2e", "c3", "1ode", "1od1", "1o1e", "1o2", 
"2de", "2d1", "3e", "4"
 ```
 
This problem follows the Subsets pattern and can be mapped to Permutations.

Let’s take Example-2 mentioned above to generate all the permutations. Following a BFS approach, we will consider one character at a time. Since we need to preserve the character sequence, we can start with the actual string and process each character (i.e., make it upper-case or lower-case) one by one:

Starting with the actual string: "ab7c"
Processing the first character (‘a’), we will get two permutations: "ab7c", "Ab7c"
Processing the second character (‘b’), we will get four permutations: "ab7c", "Ab7c", "aB7c", "AB7c"
Since the third character is a digit, we can skip it.
Processing the fourth character (‘c’), we will get a total of eight permutations: "ab7c", "Ab7c", "aB7c", "AB7c", "ab7C", "Ab7C", "aB7C", "AB7C"
Let’s analyze the permutations in the 3rd and the 5th step. How can we generate the permutations in the 5th step from the permutations in the 3rd step?

If we look closely, we will realize that in the 5th step, when we processed the new character (‘c’), we took all the permutations of the previous step (3rd) and changed the case of the letter (‘c’) in them to create four new permutations.

### Solution:
 - C++ Solution:
```sh
class solution {
 public:
  static vector<string> generateGeneralizedAbbreviation(const string &word) {
    vector<string> result;
    string abWord = "";
    generateAbbreviationRecursive(word, abWord, 0, 0, result);
    return result;
  }

 private:
  static void generateAbbreviationRecursive(const string &word, string &abWord, int start,
                                            int count, vector<string> &result) {
    if (start == word.length()) {
      if (count != 0) {
        abWord += to_string(count);
      }
      result.push_back(abWord);
    } else {
      // continue abbreviating by incrementing the current abbreviation count
      string newWord(abWord);
      generateAbbreviationRecursive(word, newWord, start + 1, count + 1, result);

      // restart abbreviating, append the count and the current character to the string
      if (count != 0) {
        abWord += to_string(count);
      }
      abWord += word[start];
      generateAbbreviationRecursive(word, abWord, start + 1, 0, result);
    }
  }
};
```
  - Java Solution:
 ```sh
Class solution{
  public static List<String> generateGeneralizedAbbreviation(String word) {
    List<String> result = new ArrayList<String>();
    generateAbbreviationRecursive(word, new StringBuilder(), 0, 0, result);
    return result;
  }

  private static void generateAbbreviationRecursive(String word, StringBuilder abWord, int start, int count,
      List<String> result) {

    if (start == word.length()) {
      if (count != 0)
        abWord.append(count);
      result.add(abWord.toString());
    } else {
      // continue abbreviating by incrementing the current abbreviation count
      generateAbbreviationRecursive(word, new StringBuilder(abWord), start + 1, count + 1, result);

      // restart abbreviating, append the count and the current character to the string
      if (count != 0)
        abWord.append(count);
      generateAbbreviationRecursive(word, new StringBuilder(abWord).append(word.charAt(start)), start + 1, 0, result);
    }
  }
```
 - Time Complexity
   O(N*2^N)
 - Space Complexity
O(N*2^N)
### Question 7 Evaluate Expression

Given an expression containing digits and operations (+, -, *), find all possible ways in which the expression can be evaluated by grouping the numbers and operators using parentheses.

 - Example 1
 ```sh
Input: "1+2*3"
Output: 7, 9
Explanation: 1+(2*3) => 7 and (1+2)*3 => 9
 ```
  - Example 2
 ```sh
Input: "2*3-4-5"
Output: 8, -12, 7, -7, -3 
Explanation: 2*(3-(4-5)) => 8, 2*(3-4-5) => -12, 2*3-(4-5) => 7, 2*(3-4)-5 => -7, (2*3)-4-5 => -3
 ```
 
This problem follows the Subsets pattern and can be mapped to Balanced Parentheses. We can follow a similar BFS approach.

Let’s take Example-1 mentioned above to generate different ways to evaluate the expression.

We can iterate through the expression character-by-character.
we can break the expression into two halves whenever we get an operator (+, -, *).
The two parts can be calculated by recursively calling the function.
Once we have the evaluation results from the left and right halves, we can combine them to produce all results.

### Solution:
 - C++ Solution:
```sh
class solution {
 public:
  static vector<int> diffWaysToEvaluateExpression(const string& input) {
    vector<int> res;
    if(input.find('+') == string::npos && input.find('-') == string::npos && input.find('*') == string::npos){
        res.push_back(stoi(input));
    }
    for(int i = 0; i < input.size;i++){
        if(!isdigit(input[i])){
            vector<int> l = diffWaysToEvaluateExpression(input.substr(0,i));
            vector<int> r = diffWaysToEvaluateExpress(input.substr(i+1));
            for(auto a:l){
                for(auto b:r){
                    if(input[i] == '+'){
                        res.push_back(a + b);
                    }
                    else if(input[i] == '-'){
                        res.push_back(a - b);
                    }
                    else{
                        res.push_back(a * b);
                    }
                }
            }
        }
    }
    returnr res;
    }
};
```
  - Java Solution:
 ```sh
Class solution{
    public static List<Integer> diffWaysToEvaluateExpression(String input) {
    List<Integer> result = new ArrayList<>();
    // base case: if the input string is a number, parse and add it to output.
    if (!input.contains("+") && !input.contains("-") && !input.contains("*")) {
      result.add(Integer.parseInt(input));
    } else {
      for (int i = 0; i < input.length(); i++) {
        char chr = input.charAt(i);
        if (!Character.isDigit(chr)) {
          // break the equation here into two parts and make recursively calls
          List<Integer> leftParts = diffWaysToEvaluateExpression(input.substring(0, i));
          List<Integer> rightParts = diffWaysToEvaluateExpression(input.substring(i + 1));
          for (int part1 : leftParts) {
            for (int part2 : rightParts) {
              if (chr == '+')
                result.add(part1 + part2);
              else if (chr == '-')
                result.add(part1 - part2);
              else if (chr == '*')
                result.add(part1 * part2);
            }
          }
        }
      }
    }
    return result;
  }
```
Time complexity #
The time complexity of this algorithm will be exponential and will be similar to Balanced Parentheses. Estimated time complexity will be O(N*2^N)but the actual time complexity ( O(4^n/\sqrt{n})is bounded by the Catalan number and is beyond the scope of a coding interview. See more details here.

Space complexity #
The space complexity of this algorithm will also be exponential, estimated at O(2^N)though the actual will be ( O(4^n/\sqrt{n}).
O(N*2^N)

### Question 7 Evaluate Expression

Given an expression containing digits and operations (+, -, *), find all possible ways in which the expression can be evaluated by grouping the numbers and operators using parentheses.

 - Example 1
 ```sh
Input: "1+2*3"
Output: 7, 9
Explanation: 1+(2*3) => 7 and (1+2)*3 => 9
 ```
  - Example 2
 ```sh
Input: "2*3-4-5"
Output: 8, -12, 7, -7, -3 
Explanation: 2*(3-(4-5)) => 8, 2*(3-4-5) => -12, 2*3-(4-5) => 7, 2*(3-4)-5 => -7, (2*3)-4-5 => -3
 ```
 
This problem follows the Subsets pattern and can be mapped to Balanced Parentheses. We can follow a similar BFS approach.

Let’s take Example-1 mentioned above to generate different ways to evaluate the expression.

We can iterate through the expression character-by-character.
we can break the expression into two halves whenever we get an operator (+, -, *).
The two parts can be calculated by recursively calling the function.
Once we have the evaluation results from the left and right halves, we can combine them to produce all results.

### Solution:
 - C++ Solution:
```sh
class solution {
 public:
  static vector<int> diffWaysToEvaluateExpression(const string& input) {
    vector<int> res;
    if(input.find('+') == string::npos && input.find('-') == string::npos && input.find('*') == string::npos){
        res.push_back(stoi(input));
    }
    for(int i = 0; i < input.size;i++){
        if(!isdigit(input[i])){
            vector<int> l = diffWaysToEvaluateExpression(input.substr(0,i));
            vector<int> r = diffWaysToEvaluateExpress(input.substr(i+1));
            for(auto a:l){
                for(auto b:r){
                    if(input[i] == '+'){
                        res.push_back(a + b);
                    }
                    else if(input[i] == '-'){
                        res.push_back(a - b);
                    }
                    else{
                        res.push_back(a * b);
                    }
                }
            }
        }
    }
    returnr res;
    }
};
```
  - Java Solution:
 ```sh
Class solution{
    public static List<Integer> diffWaysToEvaluateExpression(String input) {
    List<Integer> result = new ArrayList<>();
    // base case: if the input string is a number, parse and add it to output.
    if (!input.contains("+") && !input.contains("-") && !input.contains("*")) {
      result.add(Integer.parseInt(input));
    } else {
      for (int i = 0; i < input.length(); i++) {
        char chr = input.charAt(i);
        if (!Character.isDigit(chr)) {
          // break the equation here into two parts and make recursively calls
          List<Integer> leftParts = diffWaysToEvaluateExpression(input.substring(0, i));
          List<Integer> rightParts = diffWaysToEvaluateExpression(input.substring(i + 1));
          for (int part1 : leftParts) {
            for (int part2 : rightParts) {
              if (chr == '+')
                result.add(part1 + part2);
              else if (chr == '-')
                result.add(part1 - part2);
              else if (chr == '*')
                result.add(part1 * part2);
            }
          }
        }
      }
    }
    return result;
  }
```
Time complexity #
The time complexity of this algorithm will be exponential and will be similar to Balanced Parentheses. Estimated time complexity will be O(N*2^N)but the actual time complexity ( O(4^n/\sqrt{n})is bounded by the Catalan number and is beyond the scope of a coding interview. See more details here.

Space complexity #
The space complexity of this algorithm will also be exponential, estimated at O(2^N)though the actual will be ( O(4^n/\sqrt{n}).
O(N*2^N)

### Question 8 Structurally Unique Binary Search Tree

Given a number ‘n’, write a function to return all structurally unique Binary Search Trees (BST) that can store values 1 to ‘n’?

 - Example 1
 ```sh
Input: 2
Output: List containing root nodes of all structurally unique BSTs.
Explanation: Here are the 2 structurally unique BSTs storing all numbers from 1 to 2:
 ```
  - Example 2
 ```sh
Input: 3
Output: List containing root nodes of all structurally unique BSTs.
Explanation: Here are the 5 structurally unique BSTs storing all numbers from 1 to 3:
 ```
 
This problem follows the Subsets pattern and is quite similar to Evaluate Expression. Following a similar approach, we can iterate from 1 to ‘n’ and consider each number as the root of a tree. All smaller numbers will make up the left sub-tree and bigger numbers will make up the right sub-tree. We will make recursive calls for the left and right sub-trees

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
  static vector<TreeNode*> findUniqueTrees(int n) {
        return helper(1, n);
    }
    static vector<TreeNode*> helper(int start, int end){
        vector<TreeNode*> res;
        if(start > end){
            res.push_back(nullptr);
            return res;
        }
        for(int i = start, i <= end;i++){
            vector<TreeNode*> l = helper(start, i - 1);
            vector<TreeNode*> r = helper(i + 1, end);
            for(auto a:l){
                for(auto b:r){
                    TreeNode *root = new TreeNode(i);
                    root->left = a;
                    root->right = b;
                    res.push_back(root);
                }
            }
        }
        return res;
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
  public static List<TreeNode> findUniqueTrees(int n) {
    if (n <= 0)
      return new ArrayList<TreeNode>();
    return findUniqueTreesRecursive(1, n);
  }

  public static List<TreeNode> findUniqueTreesRecursive(int start, int end) {
    List<TreeNode> result = new ArrayList<>();
    // base condition, return 'null' for an empty sub-tree
    // consider n=1, in this case we will have start=end=1, this means we should have only one tree
    // we will have two recursive calls, findUniqueTreesRecursive(1, 0) & (2, 1)
    // both of these should return 'null' for the left and the right child
    if (start > end) {
      result.add(null);
      return result;
    }

    for (int i = start; i <= end; i++) {
      // making 'i' root of the tree
      List<TreeNode> leftSubtrees = findUniqueTreesRecursive(start, i - 1);
      List<TreeNode> rightSubtrees = findUniqueTreesRecursive(i + 1, end);
      for (TreeNode leftTree : leftSubtrees) {
        for (TreeNode rightTree : rightSubtrees) {
          TreeNode root = new TreeNode(i);
          root.left = leftTree;
          root.right = rightTree;
          result.add(root);
        }
      }
    }
    return result;
  }
```
Time complexity #
The time complexity of this algorithm will be exponential and will be similar to Balanced Parentheses. Estimated time complexity will be O(N*2^N)but the actual time complexity ( O(4^n/\sqrt{n})is bounded by the Catalan number and is beyond the scope of a coding interview. See more details here.

Space complexity #
The space complexity of this algorithm will also be exponential, estimated at O(2^N)though the actual will be ( O(4^n/\sqrt{n}).
O(N*2^N)

### Question 9 Count of Structually Unique Binary Search Trees

Given a number ‘n’, write a function to return the count of structurally unique Binary Search Trees (BST) that can store values 1 to ‘n’.

 - Example 1
 ```sh
Input: 2
Output: 2
Explanation: As we saw in the previous problem, there are 2 unique BSTs storing numbers from 1-2.
 ```
  - Example 2
 ```sh
Input: 3
Output: 5
Explanation: There will be 5 unique BSTs that can store numbers from 1 to 5.
 ```
This problem is similar to Structurally Unique Binary Search Trees. Following a similar approach, we can iterate from 1 to ‘n’ and consider each number as the root of a tree and make two recursive calls to count the number of left and right sub-trees.

### Solution:
 - C++ Solution:
```sh
class TreeNode {
 public:
  int val = 0;
  TreeNode *left;
  TreeNode *right;

  TreeNode(int x) { val = x; }
};

class CountUniqueTrees {
 public:
  int countTrees(int n) {
    if (n <= 1) return 1;
    int count = 0;
    for (int i = 1; i <= n; i++) {
      // making 'i' root of the tree
      int countOfLeftSubtrees = countTrees(i - 1);
      int countOfRightSubtrees = countTrees(n - i);
      count += (countOfLeftSubtrees * countOfRightSubtrees);
    }
    return count;
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

class CountUniqueTrees {
  public int countTrees(int n) {
    if (n <= 1)
      return 1;
    int count = 0;
    for (int i = 1; i <= n; i++) {
      // making 'i' root of the tree
      int countOfLeftSubtrees = countTrees(i - 1);
      int countOfRightSubtrees = countTrees(n - i);
      count += (countOfLeftSubtrees * countOfRightSubtrees);
    }
    return count;
  }
```
Time complexity #
The time complexity of this algorithm will be exponential and will be similar to Balanced Parentheses. Estimated time complexity will be O(N*2^N)but the actual time complexity ( O(4^n/\sqrt{n})is bounded by the Catalan number and is beyond the scope of a coding interview. See more details here.

Space complexity #
The space complexity of this algorithm will also be exponential, estimated at O(2^N)though the actual will be ( O(4^n/\sqrt{n}).
O(N*2^N)
### Todos

 - Write MORE Tests
 - Add Night Mode

License
----

MIT
