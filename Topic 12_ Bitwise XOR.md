# Topic 12: Bitwise XOR


[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

XOR is a logical bitwise operator that returns 0 (false) if both bits are the same and returns 1 (true) otherwise. In other words, it only returns 1 if exactly one bit is set to 1 out of the two bits in comparison.
It is surprising to know the approaches that the XOR operator enables us to solve certain problems. For example, let’s take a look at the following problem:

Given an array of n-1 integers in the range from 1 to n, find the one number that is missing from the array.

### Question 1: Find missing value

 - Example 1
 ```sh
Input: 1, 5, 2, 6, 4
Answer: 3
 ```
How can we avoid this? Can XOR help us here?

Remember the important property of XOR that it returns 0 if both the bits in comparison are the same. In other words, XOR of a number with itself will always result in 0. This means that if we XOR all the numbers in the input array with all numbers from the range 11 to nn then each number in the input is going to get zeroed out except the missing number. Following are the set of steps to find the missing number using XOR:

XOR all the numbers from 1 to nn, let’s call it x1.
XOR all the numbers in the input array, let’s call it x2.
The missing number can be found by x1 XOR x2.
Here is what the algorithm will look like:

 
### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static int findMissingNumber(vector<int> &arr){
        int sum = 1;
        for(int i = 2;i <= arr.size() + 1;i++){
            sum^=i;
        }
        for(int i = 0;i < arr.size();i++){
            sum^=arr[i];
        }
        return sum;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    static int findMissingNumber(int[] arr){
        int sum = 1;
        for(int i = 2;i <= arr.length + 1;i++){
            sum^=i;
        }
        for(int i = 0;i < arr.length;i++){
            sum^=arr[i];
        }
        return sum;
    }
};
```
Time complexity #
O(N)
Space complexity #
O(1)

Important properties of XOR to remember #
Following are some important properties of XOR to remember:

Taking XOR of a number with itself returns 0, e.g.,

1 ^ 1 = 0
29 ^ 29 = 0
Taking XOR of a number with 0 returns the same number, e.g.,

1 ^ 0 = 1
31 ^ 0 = 31
XOR is Associative & Commutative, which means:

(a ^ b) ^ c = a ^ (b ^ c)
a ^ b = b ^ a
### Question 2 Single Number

In a non-empty array of integers, every number appears twice except for one, find that single number.

 - Example 1
 ```sh
Input: 1, 4, 2, 1, 3, 2, 3
Output: 4
 ```
  - Example 2
 ```sh
Input: 7, 9, 7
Output: 9
 ```
One straight forward solution can be to use a HashMap kind of data structure and iterate through the input:

If number is already present in HashMap, remove it.
If number is not present in HashMap, add it.
In the end, only number left in the HashMap is our required single number.
Time and space complexity Time Complexity of the above solution will be O(n)O(n) and space complexity will also be O(n)O(n).

Can we do better than this using the XOR Pattern?

Solution with XOR #
Recall the following two properties of XOR:

It returns zero if we take XOR of two same numbers.
It returns the same number if we XOR with zero.
So we can XOR all the numbers in the input; duplicate numbers will zero out each other and we will be left with the single number.



### Solution:
- C++ Solution:
```sh
Class solution{
public:
  static int findSingleNumber(const vector<int>& arr) {
    int sum = arr[0];
    for(int i = 1;i < arr.size();i++){
        sum^=arr[i];
    }
    return sum;
  }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
  public static int findSingleNumber(int[] arr) {
    int sum = arr[0];
    for(int i = 1;i < arr.length;i++){
        sum^=arr[i];
    }
    return sum;
  }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 3 Two Single Number

In a non-empty array of numbers, every number appears exactly twice except two numbers that appear only once. Find the two numbers that appear only once.

 - Example 1
 ```sh
Input: [1, 4, 2, 1, 3, 5, 6, 2, 3, 5]
Output: [4, 6]
 ```
  - Example 2
 ```sh
Input: [2, 1, 3, 2]
Output: [1, 3]
 ```
This problem is quite similar to Single Number, the only difference is that, in this problem, we have two single numbers instead of one. Can we still use XOR to solve this problem?

Let’s assume num1 and num2 are the two single numbers. If we do XOR of all elements of the given array, we will be left with XOR of num1 and num2 as all other numbers will cancel each other because all of them appeared twice. Let’s call this XOR n1xn2. Now that we have XOR of num1 and num2, how can we find these two single numbers?

As we know that num1 and num2 are two different numbers, therefore, they should have at least one bit different between them. If a bit in n1xn2 is ‘1’, this means that num1 and num2 have different bits in that place, as we know that we can get ‘1’ only when we do XOR of two different bits, i.e.,

1 XOR 0 = 0 XOR 1 = 1
We can take any bit which is ‘1’ in n1xn2 and partition all numbers in the given array into two groups based on that bit. One group will have all those numbers with that bit set to ‘0’ and the other with the bit set to ‘1’. This will ensure that num1 will be in one group and num2 will be in the other. We can take XOR of all numbers in each group separately to get num1 and num2, as all other numbers in each group will cancel each other. Here are the steps of our algorithm:

Taking XOR of all numbers in the given array will give us XOR of num1 and num2, calling this XOR as n1xn2.
Find any bit which is set in n1xn2. We can take the rightmost bit which is ‘1’. Let’s call this rightmostSetBit.
Iterate through all numbers of the input array to partition them into two groups based on rightmostSetBit. Take XOR of all numbers in both the groups separately. Both these XORs are our required numbers.
 
### Solution:
- C++ Solution:
```sh
Class solution{
public:
  static vector<int> findSingleNumbers(vector<int> &nums) {
    // get the XOR of the all the numbers
    int n1xn2 = 0;
    for (int num : nums) {
      n1xn2 ^= num;
    }

    // get the rightmost bit that is '1'
    int rightmostSetBit = 1;
    while ((rightmostSetBit & n1xn2) == 0) {
      rightmostSetBit = rightmostSetBit << 1;
    }
    int num1 = 0, num2 = 0;
    for (int num : nums) {
      if ((num & rightmostSetBit) != 0) // the bit is set
        num1 ^= num;
      else // the bit is not set
        num2 ^= num;
    }
    return vector<int>{num1, num2};
  }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
  public static int[] findSingleNumbers(int[] nums) {
    // get the XOR of the all the numbers
    int n1xn2 = 0;
    for (int num : nums) {
      n1xn2 ^= num;
    }

    // get the rightmost bit that is '1'
    int rightmostSetBit = 1;
    while ((rightmostSetBit & n1xn2) == 0) {
      rightmostSetBit = rightmostSetBit << 1;
    }
    int num1 = 0, num2 = 0;
    for (int num : nums) {
      if ((num & rightmostSetBit) != 0) // the bit is set
        num1 ^= num;
      else // the bit is not set
        num2 ^= num;
    }
    return new int[] { num1, num2 };
  }
```
 - Time Complexity
  O(logN)
 - Space Complexity
  O(1)

### Question 4 Complement of Base 10 number

Every non-negative integer N has a binary representation, for example, 8 can be represented as “1000” in binary and 7 as “0111” in binary.

The complement of a binary representation is the number in binary that we get when we change every 1 to a 0 and every 0 to a 1. For example, the binary complement of “1010” is “0101”.

For a given positive number N in base-10, return the complement of its binary representation as a base-10 integer.

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
Recall the following properties of XOR:

It will return 1 if we take XOR of two different bits i.e. 1^0 = 0^1 = 1.

It will return 0 if we take XOR of two same bits i.e. 0^0 = 1^1 = 0. In other words, XOR of two same numbers is 0.

It returns the same number if we XOR with 0.

From the above-mentioned first property, we can conclude that XOR of a number with its complement will result in a number that has all of its bits set to 1. For example, the binary complement of “101” is “010”; and if we take XOR of these two numbers, we will get a number with all bits set to 1, i.e., 101 ^ 010 = 111

We can write this fact in the following equation:

number ^ complement = all_bits_set
Let’s add ‘number’ on both sides:

number ^ number ^ complement = number ^ all_bits_set
From the above-mentioned second property:

0 ^ complement = number ^ all_bits_set
From the above-mentioned third property:

complement = number ^ all_bits_set
We can use the above fact to find the complement of any number.

How do we calculate ‘all_bits_set’? One way to calculate all_bits_set will be to first count the bits required to store the given number. We can then use the fact that for a number which is a complete power of ‘2’ i.e., it can be written as pow(2, n), if we subtract ‘1’ from such a number, we get a number which has ‘n’ least significant bits set to ‘1’. For example, ‘4’ which is a complete power of ‘2’, and ‘3’ (which is one less than 4) has a binary representation of ‘11’ i.e., it has ‘2’ least significant bits set to ‘1’.

### Solution:
 - C++ Solution:
```sh
class solution {
 public:
  static int bitwiseComplement(int num) {
    int c = 0;
    while(num > 0){
        num >>1;
        c++;
    }
    int n  = pow(2,c) - 1;
    return num^n
  }
};
```
  - Java Solution:
 ```sh
Class solution{
   public static int bitwiseComplement(int num) {
    int c = 0;
    while(num > 0){
        num >>1;
        c++;
    }
    int n  = Math.pow(2,c) - 1;
    return num^n
  }
};
```
 - Time Complexity
   O(b)
 - Space Complexity
O(1)
### Question 5 

Given a binary matrix representing an image, we want to flip the image horizontally, then invert it.

To flip an image horizontally means that each row of the image is reversed. For example, flipping [0, 1, 1] horizontally results in [1, 1, 0].

To invert an image means that each 0 is replaced by 1, and each 1 is replaced by 0. For example, inverting [1, 1, 0] results in [0, 0, 1].

 - Example 1
 ```sh
Input: [
  [1,0,1],
  [1,1,1],
  [0,1,1]
]
Output: [
  [0,1,0],
  [0,0,0],
  [0,0,1]
]
Explanation: First reverse each row: [[1,0,1],[1,1,1],[1,1,0]]. Then, invert the image: [[0,1,0],[0,0,0],[0,0,1]]
 ```
  - Example 2
 ```sh
Input: [
  [1,1,0,0],
  [1,0,0,1],
  [0,1,1,1], 
  [1,0,1,0]
]
Output: [
  [1,1,0,0],
  [0,1,1,0],
  [0,0,0,1],
  [1,0,1,0]
]
Explanation: First reverse each row: [[0,0,1,1],[1,0,0,1],[1,1,1,0],[0,1,0,1]]. Then invert the image: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
 ```

Flip: We can flip the image in place by replacing ith element from left with the ith element from the right.

Invert: We can take XOR of each element with 1. If it is 1 then it will become 0 and if it is 0 then it will become 1.

### Solution:
 - C++ Solution:
```sh
class ArrayReader{
    public:
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
  - C++ Solution:
 ```sh

class solution {
    public:
        static vector<vector<int>> flipAndInvertImage(vector<vector<int>> arr){
            int s = arr[0].size();
            for(int row = 0;row < arr.size();row++){
                for(int col = 0;col < s;col++){
                    int temp = arr[row][col]^1;
                    arr[row][col] = row[row][s - 1- col]^1;
                    arr[row][s - 1 - col] = temp;
                }
            }
            return arr;
        }

```
  - Java Solution:
 ```sh

class solution {
    public:
        static int[][] flipAndInvertImage(int[][] arr){
            int s = arr[0].length;
            for(int row = 0;row < arr.length;row++){
                for(int col = 0;col < s;col++){
                    int temp = arr[row][col]^1;
                    arr[row][col] = row[row][s - 1- col]^1;
                    arr[row][s - 1 - col] = temp;
                }
            }
            return arr;
        }

```
 - Time Complexity
   O(logN)
 - Space Complexity
O(1)
### Todos

 - Write MORE Tests
 - Add Night Mode

License
----

MIT
