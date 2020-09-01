# Topic 6: In-place Reversal of a LinkedList


[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

In a lot of problems, we are asked to reverse the links between a set of nodes of a LinkedList. Often, the constraint is that we need to do this in-place, i.e., using the existing node objects and without using extra memory.

In-place Reversal of a LinkedList pattern describes an efficient way to solve the above problem. In the following chapters, we will solve a bunch of problems using this pattern.
### Question 1: Reverse a LinkedList

Given the head of a Singly LinkedList, reverse the LinkedList. Write a function to return the new head of the reversed LinkedList.

To reverse a LinkedList, we need to reverse one node at a time. We will start with a variable current which will initially point to the head of the LinkedList and a variable previous which will point to the previous node that we have processed; initially previous will point to null.

In a stepwise manner, we will reverse the current node by pointing it to the previous before moving on to the next node. Also, we will update the previous to always point to the previous node that we have processed.


### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static  ListNode* reverse(ListNode* head){
        if(head == NULL){
            return nullptr;
        }
        ListNode *cur = head;
        ListNode *prev = nullptr;
        while(cur){
            ListNode* next = cur->next;
            cur->next = prev;
            prev = cur;
            cur = next;
        }
        return prev;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static  ListNode reverse(ListNode head){
        if(head == null){
            return null;
        }
        ListNode cur = head;
        ListNode prev = null;
        while(cur){
            ListNode next = cur.next;
            cur.next = prev;
            prev = cur;
            cur = next;
        }
        return prev;
    }
};
```
Time complexity #
O(N)

Space complexity #
O(1)

### Question 2 Reverse a Sub-list
Given the head of a LinkedList and two positions ‘p’ and ‘q’, reverse the LinkedList from position ‘p’ to ‘q’.

The problem follows the In-place Reversal of a LinkedList pattern. We can use a similar approach as discussed in Reverse a LinkedList. Here are the steps we need to follow:

Skip the first p-1 nodes, to reach the node at position p.
Remember the node at position p-1 to be used later to connect with the reversed sub-list.
Next, reverse the nodes from p to q using the same approach discussed in Reverse a LinkedList.
Connect the p-1 and q+1 nodes to the reversed sub-list.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static ListNode* reverse(ListNode* head){
        if(p == q){
            return head;
        }
        ListNode *cur = head;
        ListNode *prev = nullptr;
        for(int i = 0; cur != nullptr && i < p - 1;i++){
            prev = cur;
            cur = cur->next;
        }
        ListNode *first = prev;
        ListNode *second = cur;
        for(int i = 0;cur != nullptr && i < q - p + 1; i++){
            ListNode *next = cur->next;
            cur->next = prev;
            prev = cur;
            cur = next;
        }
        if(first != nullptr){
            first->next = prev;
        }
        else{
            head = prev;
        }
        second->next = cur;
        return head;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static ListNode reverse(ListNode head){
        if(p == q){
            return head;
        }
        ListNode cur = head;
        ListNode prev = null;
        for(int i = 0; cur != null && i < p - 1;i++){
            prev = cur;
            cur = cur.next;
        }
        ListNode first = prev;
        ListNode second = cur;
        for(int i = 0;cur != null && i < q - p + 1; i++){
            ListNode next = cur.next;
            cur.next = prev;
            prev = cur;
            cur = next;
        }
        if(first != null){
            first.next = prev;
        }
        else{
            head = prev;
        }
        second.next = cur;
        return head;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 3 Reverse every K-element Sub-list
Given the head of a LinkedList and a number ‘k’, reverse every ‘k’ sized sub-list starting from the head.

If, in the end, you are left with a sub-list with less than ‘k’ elements, reverse it too.

The problem follows the In-place Reversal of a LinkedList pattern and is quite similar to Reverse a Sub-list. The only difference is that we have to reverse all the sub-lists. We can use the same approach, starting with the first sub-list (i.e. p=1, q=k) and keep reversing all the sublists of size ‘k’.

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

### Todos

 - Write MORE Tests
 - Add Night Mode

License
----

MIT
