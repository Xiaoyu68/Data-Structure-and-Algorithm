# Topic 8: Tree Depth First Search


[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

This pattern is based on the Depth First Search (DFS) technique to traverse a tree.

We will be using recursion (or we can also use a stack for the iterative approach) to keep track of all the previous (parent) nodes while traversing. This also means that the space complexity of the algorithm will be O(H)O(H), where ‘H’ is the maximum height of the tree.


### Question 1: Binary Tree Path Sum

Given a binary tree and a number ‘S’, find if the tree has a path from root-to-leaf such that the sum of all the node values of that path equals ‘S’.

As we are trying to search for a root-to-leaf path, we can use the Depth First Search (DFS) technique to solve this problem.

To recursively traverse a binary tree in a DFS fashion, we can start from the root and at every step, make two recursive calls one for the left and one for the right child.

Here are the steps for our Binary Tree Path Sum problem:

Start DFS with the root of the tree.
If the current node is not a leaf node, do two things:
Subtract the value of the current node from the given number to get a new sum => S = S - node.value
Make two recursive calls for both the children of the current node with the new number calculated in the previous step.
At every step, see if the current node being visited is a leaf node and if its value is equal to the given number ‘S’. If both these conditions are true, we have found the required root-to-leaf path, therefore return true.
If the current node is a leaf but its value is not equal to the given number ‘S’, return false.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  bool hasPath(TreeNode *root, int sum){
        if(root == nullptr){
            return false;
        }
        if(root->val == sum && root->left == nullptr && root->right == nullptr){
            return true;
        }
        return hasPath(root->left, sum - root->val) || hasPath(root->right, sum - root->val);
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static  boolean hasPath(TreeNode root, int sum){
        if(root == null){
            return false;
        }
        if(root.val == sum && root.left == null && root.right == null){
            return true;
        }
        return hasPath(root.left, sum - root.val) || hasPath(root.right, sum - root.val);
    }
};
```
Time complexity #
O(N)
Space complexity #
O(N)


### Question 2 All Paths for a Sum
Given a binary tree and a number ‘S’, find all paths from root-to-leaf such that the sum of all the node values of each path equals ‘S’.

This problem follows the Binary Tree Path Sum pattern. We can follow the same DFS approach. There will be two differences:

Every time we find a root-to-leaf path, we will store it in a list.
We will traverse all paths and will not stop processing after finding the first path.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  vector<vector<int>> findPath(TreeNode *root, int sum){
        vector<vector<int>> res;
        vector<int> cur;
        helper(root, sum, res, cur);
        return res;
    }
    
    void helper(TreeNode *root, int sum, vector<vector<int>>& res, vector<int>& cur){
        if(root == NULL){
            return;
        }
        cur.push_back(root->val);
        if(root->val == sum && root->left == NULL && root->right == NULL){
            res.push_back(vector<int>(cur));
        }
        else{
            helper(root->left, sum - root->val, res, cur);
            helper(root->right, sum - root->val, res, cur);
        }
        cur.pop_back();
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static  List<List<Integer>> findPath(TreeNode root, int sum){
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> cur = new ArrayList<>();
        helper(root, sum, res, cur);
        return res;
    }
    
    public void helper(TreeNode root, int sum, List<List<Integer>> res, List<Integer> cur){
        if(root == null){
            return;
        }
        cur.addk(root.val);
        if(root.val == sum && root.left == null && root.right == null){
            res.add(new ArrayList<Integer>(cur));
        }
        else{
            helper(root.left, sum - root.val, res, cur);
            helper(root.right, sum - root.val, res, cur);
        }
        cur.remove(cur.size() - 1);
    }
};
```
Time Complexity: O(N)
Space Complexity: O(N)

### Question 3 Sum of Path Number

Given a binary tree where each node can only have a digit (0-9) value, each root-to-leaf path will represent a number. Find the total sum of all the numbers represented by all paths.

This problem follows the Binary Tree Path Sum pattern. We can follow the same DFS approach. The additional thing we need to do is to keep track of the number representing the current path.

How do we calculate the path number for a node? Taking the first example mentioned above, say we are at node ‘7’. As we know, the path number for this node is ‘17’, which was calculated by: 1 * 10 + 7 => 17. We will follow the same approach to calculate the path number of each node.
### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  int findSumOfPathNumbers(TreeNode *root){
        return helper(root,0);
    }
    int helper(TreeNode *root, int sum){
        if(root == nullptr){
            return 0;
        }
        sum = sum*10 + root->val;
        if(root->left == nullptr && root->right == nullptr){
            return sum;
        }
        return helper(root->left, sum) + helper(root->right, sum);
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static int findSumOfPathNumbers(TreeNode root){
        return helper(root,0);
    }
    public int helper(TreeNode root, int sum){
        if(root == null){
            return 0;
        }
        sum = sum*10 + root.val;
        if(root.left == null && root.right == null){
            return sum;
        }
        return helper(root.left, sum) + helper(root.right, sum);
    }
};
```
 - Time Complexity
  O(N)
 - Space Complexity
  O(N)

### Question 5 Path With Given Sequence

Given a binary tree and a number sequence, find if the sequence is present as a root-to-leaf path in the given tree.

This problem follows the Binary Tree Path Sum pattern. We can follow the same DFS approach and additionally, track the element of the given sequence that we should match with the current node. Also, we can return false as soon as we find a mismatch between the sequence and the node value.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  bool findPath(TreeNode *root, const vector<int>& sequence){
        if(root == nullptr){
            return sequence.empty();
        }
        return helper(root, sequence, 0);
    }
    
    bool helper(TreeNode *root, const vector<int>& sequence, int id){
        if(root == nullptr){
            return false;
        }
        if(id >= sequence.size() || sequence[id] != root->val){
            return false;
        }
        if(id == sequence.size() - 1 && root->right == nullptr && root->left == nullptr){
            return true;
        }
        return helper(root->left, sequence, id + 1) || helper(root->right, sequence ,id + 1)
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static  boolean findPath(TreeNode root, int[] sequence){
        if(root == null){
            return sequence.isEmpty();
        }
        return helper(root, sequence, 0);
    }
    
    public boolean helper(TreeNode root, int[] sequence, int id){
        if(root == null){
            return false;
        }
        if(id >= sequence.size() || sequence[id] != root.val){
            return false;
        }
        if(id == sequence.size() - 1 && root.right == null && root.left == null){
            return true;
        }
        return helper(root.left, sequence, id + 1) || helper(root.right, sequence ,id + 1)
    }
};
```
Time Complexity: O(N)
Space Complexity: O(N)

### Question 6 Count Paths for a Sum

Given a binary tree and a number ‘S’, find all paths in the tree such that the sum of all the node values of each path equals ‘S’. Please note that the paths can start or end at any node but all paths must follow direction from parent to child (top to bottom).

This problem follows the Binary Tree Path Sum pattern. We can follow the same DFS approach. But there will be four differences:

We will keep track of the current path in a list which will be passed to every recursive call.

Whenever we traverse a node we will do two things:

Add the current node to the current path.
As we added a new node to the current path, we should find the sums of all sub-paths ending at the current node. If the sum of any sub-path is equal to ‘S’ we will increment our path count.
We will traverse all paths and will not stop processing after finding the first path.

Remove the current node from the current path before returning from the function. This is needed to Backtrack while we are going up the recursive call stack to process other paths.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  int countPaths(TreeNode *root, int S){
        vector<int> currentPath;
        return helper(root, S, currentPath);
    }
    
    int helper(TreeNode *root, int S, vector<int> &currentPath){
        if(root == nullptr){
            return;
        }
        int count = 0;
        int sum = 0;
        currentPath.push_back(root->val);
        for(int  i = currentPath.size() - 1;i >= 0;i--){
            sum += currentPath[i];
            if(sum == S){
                count++;
            }
        }
        count += helper(root->left, S, currentPath);
        count += helper(root->right, S, currentPath);
        currentPath.pop_back();
        return count;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static  int countPaths(TreeNode *root, int S){
        List<Integer> currentPath = new ArrayList<>();
        return helper(root, S, currentPath);
    }
    
    int helper(TreeNode root, int S, List<Integer> currentPath){
        if(root == null){
            return;
        }
        int count = 0;
        int sum = 0;
        currentPath.add(root.val);
        for(int  i = currentPath.size() - 1;i >= 0;i--){
            sum += currentPath[i];
            if(sum == S){
                count++;
            }
        }
        count += helper(root.left, S, currentPath);
        count += helper(root.right, S, currentPath);
        currentPath.remove(currentPath.size());
        return count;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(N)

### Question 7 Tree Diameter

Given a binary tree, find the length of its diameter. The diameter of a tree is the number of nodes on the longest path between any two leaf nodes. The diameter of a tree may or may not pass through the root.

Note: You can always assume that there are at least two leaf nodes in the given tree.

This problem follows the Binary Tree Path Sum pattern. We can follow the same DFS approach. There will be a few differences:

At every step, we need to find the height of both children of the current node. For this, we will make two recursive calls similar to DFS.
The height of the current node will be equal to the maximum of the heights of its left or right children, plus ‘1’ for the current node.
The tree diameter at the current node will be equal to the height of the left child plus the height of the right child plus ‘1’ for the current node: diameter = leftTreeHeight + rightTreeHeight + 1. To find the overall tree diameter, we will use a class level variable. This variable will store the maximum diameter of all the nodes visited so far, hence, eventually, it will have the final tree diameter.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  int findDiameter(TreeNode *root){
        int res = 0;
        helper(root, res);
        return res;
    }
    
    int helper(TreeNode *root, int& res){
        if(roor == nullptr){
            return 0;
        }
        int l = helper(root->left, res);
        int r = helper(root->right, res);
        int d = l + r + 1;
        res = max(res, d);
        return max(l, r) + 1;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static  int findDiameter(TreeNode root){
        int res = 0;
        helper(root, res);
        return res;
    }
    
    public int helper(TreeNode root, int res){
        if(roor == null){
            return 0;
        }
        int l = helper(root.left, res);
        int r = helper(root.right, res);
        int d = l + r + 1;
        res = Math.max(res, d);
        return Math.max(l, r) + 1;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(N)

### Question 8 Path with Maximum Sum

Given a binary tree, connect each node with its level order successor. The last node of each level should point to the first node of the next level.

This problem follows the Binary Tree Level Order Traversal pattern. We can follow the same BFS approach. The only difference will be that while traversing we will remember (irrespective of the level) the previous node to connect it with the current node.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  int findMaximuPathSum(TreeNode *root){
        int res = 0;
        return helper(root, res);
        return res;
    }
    int helper(TreeNode* root, int& res){
        if(root == nullptr){
            return 0;
        }
        int sum = 0;
        int l = max(helper(root->left),0);
        int r = max(helper(root->right),0);
        int sum = l + r + root->val;
        res = max(res, sum);
        return max(l, r) + root->val;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static int findMaximuPathSum(TreeNode root){
        int res = 0;
        return helper(root, res);
        return res;
    }
    public int helper(TreeNode root, int res){
        if(root == null){
            return 0;
        }
        int sum = 0;
        int l = Math.max(helper(root.left),0);
        int r = Math.max(helper(root.right),0);
        int sum = l + r + root.val;
        res = Math.max(res, sum);
        return Math.max(l, r) + root.val;
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
