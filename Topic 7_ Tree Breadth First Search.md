# Topic 7: Tree Breadth First Search


[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

This pattern is based on the Breadth First Search (BFS) technique to traverse a tree.

Any problem involving the traversal of a tree in a level-by-level order can be efficiently solved using this approach. We will use a Queue to keep track of all the nodes of a level before we jump onto the next level. This also means that the space complexity of the algorithm will be O(W)O(W), where ‘W’ is the maximum number of nodes on any level.
### Question 1: Binary Tree Level Order Traversal

 - Example 1
```sh
2->4->6->8->10->null
null<-2<-4<-6<-8<-10
```
- Example 2
```sh
1->2->3->4->5->null
3->4->5->1->2->null
```
Since we need to traverse all nodes of each level before moving onto the next level, we can use the Breadth First Search (BFS) technique to solve this problem.

We can use a Queue to efficiently traverse in BFS fashion. Here are the steps of our algorithm:

Start by pushing the root node to the queue.
Keep iterating until the queue is empty.
In each iteration, first count the elements in the queue (let’s call it levelSize). We will have these many nodes in the current level.
Next, remove levelSize nodes from the queue and push their value in an array to represent the current level.
After removing each node from the queue, insert both of its children into the queue.
If the queue is not empty, repeat from step 3 for the next level.



### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  vector<vector<int>> traverse(TreeNode *root){
        vector<vector<int>> res;
        if(root == nullptr){
            return res;
        }
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            int n = q.size();
            res.push_back({});
            for(int i = 0;i < n;i++){
                TreeNode *t = q.front();
                q.pop();
                res.back().push_back(t->val);
                if(t->left != nullptr){
                    q.push(t->left);
                }
                if(t->right != nullptr){
                    q.push(t->right);
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
    public static  List<List<Integer> traverse(TreeNode root){
        List<List<Integer>> res = new ArrayList<>();
        if(root == null){
            return res;
        }
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            int n = q.size();
            List<Integer> cur = new ArrayList<>();
            for(int i = 0;i < n;i++){
                TreeNode t = q.poll();
                cur.add(t.val);
                if(t.left != null){
                    q.offer(t.left);
                }
                if(t.right != null){
                    q.offer(t.right);
                }
            }
            res.add(cur);
        }
        return res;
};
```
Time complexity #
The time complexity of the above algorithm is O(N)O(N), where ‘N’ is the total number of nodes in the tree. This is due to the fact that we traverse each node once.

Space complexity #
The space complexity of the above algorithm will be O(N)O(N) as we need to return a list containing the level order traversal. We will also need O(N)O(N) space for the queue. Since we can have a maximum of N/2N/2 nodes at any level (this could happen only at the lowest level), therefore we will need O(N)O(N) space to store them in the queue.



### Question 2 Reverse Level Order Traversal
Given a binary tree, populate an array to represent its level-by-level traversal in reverse order, i.e., the lowest level comes first. You should populate the values of all nodes in each level from left to right in separate sub-arrays.

This problem follows the Binary Tree Level Order Traversal pattern. We can follow the same BFS approach. The only difference will be that instead of appending the current level at the end, we will append the current level at the beginning of the result list.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  vector<vector<int>> traverse(TreeNode *root){
        vector<vector<int>> res;
        if(root == nullptr){
            return res;
        }
        queue<TreeNode*> q;
        q.push(root);
        bool flag = true;
        while(!q.empty()){
            int n = q.size();
            vector<int> v(n);
            for(int i = 0;i < n;i++){
                TreeNode *t = q.front();
                q.pop();
                if(flag){
                    v[i] = t->val;
                }
                else{
                    v[n - 1 - i] = t->val;
                }
                if(t->left != nullptr){
                    q.push(t->left);
                }
                if(t->right != nullptr){
                    q.push(t->right);
                }
            }
            res.push_back(v);
            flag = !flag;
        }

        return res;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static  List<List<Integer> traverse(TreeNode root){
        List<List<Integer>> res = new ArrayList<>();
        if(root == null){
            return res;
        }
        boolean flag = true;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            int n = q.size();
            List<Integer> cur = new LinkedList<>();
            for(int i = 0;i < n;i++){
                TreeNode t = q.poll();
                if(flag){
                    cur.add(t.val);
                }
                else{
                    cur.add(0,t.val);
                }
                if(t.left != null){
                    q.offer(t.left);
                }
                if(t.right != null){
                    q.offer(t.right);
                }
            }
            res.add(cur);
            flag = !flag;
        }
        return res;
};
```
Time Complexity: O(N)
Space Complexity: O(N)

### Question 4 Level Averages in a Binary Tree

Given a binary tree, populate an array to represent the averages of all of its levels.

This problem follows the Binary Tree Level Order Traversal pattern. We can follow the same BFS approach. The only difference will be that instead of keeping track of all nodes of a level, we will only track the running sum of the values of all nodes in each level. In the end, we will append the average of the current level to the result array.


### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  vector<double> traverse(TreeNode *root){
        vector<double> res;
        if(root == nullptr){
            return res;
        }
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            int n = q.size();
            double sum = 0;
            for(int i = 0;i < n;i++){
                TreeNode *t = q.front();
                q.pop();
                sum += t.val;
                if(t->left != nullptr){
                    q.push(t->left);
                }
                if(t->right != nullptr){
                    q.push(t->right);
                }
            }
            res.push_back(sum/(double)n);
        }
        return res;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static  List<List<Integer> traverse(TreeNode root){
        List<Double> res = new ArrayList<>();
        if(root == null){
            return res;
        }
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            int n = q.size();
            double sum = 0;
            for(int i = 0;i < n;i++){
                TreeNode t = q.poll();
                sum += t.val;
                if(t.left != null){
                    q.offer(t.left);
                }
                if(t.right != null){
                    q.offer(t.right);
                }
            }
            res.add(sum / n);
        }
        return res;
};
```
 - Time Complexity
  O(N)
 - Space Complexity
  O(N)

### Question 5 Minimum Depth of a Binary Tree

Find the minimum depth of a binary tree. The minimum depth is the number of nodes along the shortest path from the root node to the nearest leaf node.


This problem follows the Binary Tree Level Order Traversal pattern. We can follow the same BFS approach. The only difference will be, instead of keeping track of all the nodes in a level, we will only track the depth of the tree. As soon as we find our first leaf node, that level will represent the minimum depth of the tree.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  int findDepth(TreeNode *root){
        if(root == nullptr){
            return 0;
        }
        queue<TreeNode*> q;
        q.push(root);
        int depth = 0;
        while(!q.empty()){
            int n = q.size();
            depth++;
            for(int i = 0;i < n;i++){
                TreeNode *t = q.front();
                q.pop();
                if(t->left == nullptr && t->right == nullptr){
                    return depth;
                }
                if(t->left != nullptr){
                    q.push(t->left);
                }
                if(t->right != nullptr){
                    q.push(t->right);
                }
            }        
        }
        return depth;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static  List<List<Integer> traverse(TreeNode root){
        List<Double> res = new ArrayList<>();
        if(root == null){
            return 0;
        }
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        int depth = 0;
        while(!q.isEmpty()){
            depth++;
            int n = q.size();
            double sum = 0;
            for(int i = 0;i < n;i++){
                TreeNode t = q.poll();
                if(t.left == null && t.right == null){
                    return depth;
                }
                if(t.left != null){
                    q.offer(t.left);
                }
                if(t.right != null){
                    q.offer(t.right);
                }
            }
        }
        return depth;
};
```
Time Complexity: O(N)
Space Complexity: O(N)

### Question 6 Level Order Successor

Given a binary tree and a node, find the level order successor of the given node in the tree. The level order successor is the node that appears right after the given node in the level order traversal.

This problem follows the Binary Tree Level Order Traversal pattern. We can follow the same BFS approach. The only difference will be that we will not keep track of all the levels. Instead we will keep inserting child nodes to the queue. As soon as we find the given node, we will return the next node from the queue as the level order successor.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  TreeNode *findSuccessor(TreeNode *root, int key){
        if(root == nullptr){
            return nullptr;
        }
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            TreeNode *t = q.front();
            q.pop();

            if(t->left != nullptr){
                q.push(t->left);
            }
            if(t->right != nullptr){
                q.push(t->right);
            }
            if(t->val == key){
                break;
            }
        }
        return q.front();
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static  TreeNode findSuccessor(TreeNode root){
        if(root == null){
            return 0;
        }
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            TreeNode t = q.poll();

            if(t.left != null){
                q.offer(t.left);
            }
            if(t.right != null){
                q.offer(t.right);
            }
            if(t.val == key){
                break;
            }
        }
        return q.peek();
};
```
Time Complexity: O(N)
Space Complexity: O(N)

### Question 7 Connect Level Order Siblings

Given a binary tree, connect each node with its level order successor. The last node of each level should point to a null node.

This problem follows the Binary Tree Level Order Traversal pattern. We can follow the same BFS approach. The only difference is that while traversing a level we will remember the previous node to connect it with the current node.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  void connect(TreeNode *root){

        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            TreeNode *prev = nullptr;
            int n = q.size();
            for(int i = 0;i < n;i++){
                TreeNode *t = q.front();
                q.pop();
                if(prev != nullptr){
                    prev->next = t;
                }
                prev = t;
                if(t->left != nullptr){
                    q.push(t->left);
                }
                if(t->right != nullptr){
                    q.push(t->right);
                }
            }        
        }
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static  void connect(TreeNode root){

        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            TreeNode prev = null;
            int n = q.size();
            for(int i = 0;i < n;i++){
                TreeNode t = q.poll();
                if(prev != null){
                    prev.next = t;
                }
                prev = t;
                if(t.left != null){
                    q.offer(t.left);
                }
                if(t.right != null){
                    q.offer(t.right);
                }
            }
        }
};
```
Time Complexity: O(N)
Space Complexity: O(N)

### Question 8 Connect All Level Order Siblings

Given a binary tree, connect each node with its level order successor. The last node of each level should point to the first node of the next level.

This problem follows the Binary Tree Level Order Traversal pattern. We can follow the same BFS approach. The only difference will be that while traversing we will remember (irrespective of the level) the previous node to connect it with the current node.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  void connect(TreeNode *root){
        queue<TreeNode*> q;
        q.push(root);
        TreeNode *cur = nullptr;
        TreeNode *prev = nullptr;
        while(!q.empty()){
            TreeNode *cur = q.front();
            q.pop();
            if(prev != nullptr){
                prev->next = cur;
            }
            prev = cur;
            if(cur->left != nullptr){
                q.push(cur->left);
            }
            if(cur->right != nullptr){
                q.push(cur->right);
            }
        }
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static  void connect(TreeNode root){

        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        TreeNode cur = null;
        TreeNode prev = null;
        while(!q.isEmpty()){
            cur = q.poll();
            if(prev != null){
                prev.next = cur;
            }
            prev = cur;
            if(cur.left != null){
                q.offer(cur.left);
            }
            if(cur.right != null){
                q.offer(cur.right);
            }
        }
};
```
Time Complexity: O(N)
Space Complexity: O(N)

### Question 9 Right view of a Binary Tree

Given a binary tree, return an array containing nodes in its right view. The right view of a binary tree is the set of nodes visible when the tree is seen from the right side.

This problem follows the Binary Tree Level Order Traversal pattern. We can follow the same BFS approach. The only additional thing we will be do is to append the last node of each level to the result array.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  vector<TreeNode*> traverse(TreeNode *root){
        queue<TreeNode*> q;
        q.push(root);
        vector<TreeNode*> res;
        while(!q.empty()){
            int n = q.size();
            for(int i = 0;i < n;i++){
                TreeNode *t = q.front();
                q.pop();
                if(i == n-1){
                    res.push_back(t);
                }
                if(t->left != nullptr){
                    q.push(t->left);
                }
                if(t->right != nullptr){
                    q.push(t->right);
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
    public static  List<TreeNode> traverse(TreeNode root){
        List<TreeNode> res = new ArrayList<>();
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            int n = q.size();
            for(int i = 0;i < n;i++){
                TreeNode t = q.poll();
                if(i == n-1){
                    res.add(t);
                }
                if(t.left != null){
                    q.offer(t.left);
                }
                if(t.right != null){
                    q.offer(t.right);
                }
            }
        }
        return res;
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
