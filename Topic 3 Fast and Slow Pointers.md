# Topic 3: Fast and Slow Pointers


[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

The Fast & Slow pointer approach, also known as the Hare & Tortoise algorithm, is a pointer algorithm that uses two pointers which move through the array (or sequence/LinkedList) at different speeds. This approach is quite useful when dealing with cyclic LinkedLists or arrays.

By moving at different speeds (say, in a cyclic LinkedList), the algorithm proves that the two pointers are bound to meet. The fast pointer should catch the slow pointer once both the pointers are in a cyclic loop.

One of the famous problems solved using this technique was Finding a cycle in a LinkedList. Let’s jump onto this problem to understand the Fast & Slow pattern.

### Question 1 LinkedList Cycle

Given the head of a Singly LinkedList, write a function to determine if the LinkedList has a cycle in it or not.

Example 1:
```sh
Following LinkedList has a cycle:
head->1->2->3->4->5->6 (6->3)

Following LinkedList doesn't have a cycle:
head->2->4->6->8->10->null
```
Imagine two racers running in a circular racing track. If one racer is faster than the other, the faster racer is bound to catch up and cross the slower racer from behind. We can use this fact to devise an algorithm to determine if a LinkedList has a cycle in it or not.

Imagine we have a slow and a fast pointer to traverse the LinkedList. In each iteration, the slow pointer moves one step and the fast pointer moves two steps. This gives us two conclusions:

If the LinkedList doesn’t have a cycle in it, the fast pointer will reach the end of the LinkedList before the slow pointer to reveal that there is no cycle in the LinkedList.
The slow pointer will never be able to catch up to the fast pointer if there is no cycle in the LinkedList.
If the LinkedList has a cycle, the fast pointer enters the cycle first, followed by the slow pointer. After this, both pointers will keep moving in the cycle infinitely. If at any stage both of these pointers meet, we can conclude that the LinkedList has a cycle in it. Let’s analyze if it is possible for the two pointers to meet. When the fast pointer is approaching the slow pointer from behind we have two possibilities:

The fast pointer is one step behind the slow pointer.
The fast pointer is two steps behind the slow pointer.
All other distances between the fast and slow pointers will reduce to one of these two possibilities. Let’s analyze these scenarios, considering the fast pointer always moves first:

If the fast pointer is one step behind the slow pointer: The fast pointer moves two steps and the slow pointer moves one step, and they both meet.
If the fast pointer is two steps behind the slow pointer: The fast pointer moves two steps and the slow pointer moves one step. After the moves, the fast pointer will be one step behind the slow pointer, which reduces this scenario to the first scenario. This means that the two pointers will meet in the next iteration.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static bool hasCycle(ListNode *head){
        ListNode *fast = head;
        ListNode *slow = head;
        while(fast != nullptr && fast->next != nullptr){
            fast = fast->next->next;
            slow = slow->next;
            if(fast == slow){
                return true;
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
    public static boolean hasCycle(ListNode head){
        ListNode fast = head;
        ListNode slow = head;
        while(fast != null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow){
                return true;
            }
        }
        return false;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 2 Find the length of the cycle
Given an array of sorted numbers, remove all duplicates from it. You should not use any extra space; after removing the duplicates in-place return the new length of the array.
 - Example 1:
```sh
Following LinkedList has a cycle:
head->1->2->3->4->5->6 (6->3)

Following LinkedList doesn't have a cycle:
head->2->4->6->8->10->null
```
We can use the above solution to find the cycle in the LinkedList. Once the fast and slow pointers meet, we can save the slow pointer and iterate the whole cycle with another pointer until we see the slow pointer again to find the length of the cycle.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static int findCycleLength(ListNode *head){
        ListNode *fast = head;
        ListNode *slow = head;
        while(fast != nullptr && fast->next != nullptr){
            fast = fast->next->next;
            slow = slow->next;
            if(fast == slow){
                return calculateLength(slow);
            }
        }
        return 0;
        }
    static int calculateLength(ListNode *slow){
        int len = 1;
        ListNode *current = slow->next;
        while(current != slow){
            current = curent->next;
            len++;
        }
        return len;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static int findCycleLength(ListNode head){
        ListNode fast = head;
        ListNode slow = head;
        while(fast != null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow){
                return calculateLength(slow);
            }
        }
        return 0;
    }
    
    public static int calculateLength(ListNode slow){
        int len = 1;
        ListNode current = slow.next;
        while(current != slow){
            current = curent.next;
            len++;
        }
        return len;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 3 Start of LinkedList Cycle

Given the head of a Singly LinkedList that contains a cycle, write a function to find the starting node of the cycle.

If we know the length of the LinkedList cycle, we can find the start of the cycle through the following steps:

Take two pointers. Let’s call them pointer1 and pointer2.
Initialize both pointers to point to the start of the LinkedList.
We can find the length of the LinkedList cycle using the approach discussed in LinkedList Cycle. Let’s assume that the length of the cycle is ‘K’ nodes.
Move pointer2 ahead by ‘K’ nodes.
Now, keep incrementing pointer1 and pointer2 until they both meet.
As pointer2 is ‘K’ nodes ahead of pointer1, which means, pointer2 must have completed one loop in the cycle when both pointers meet. Their meeting point will be the start of the cycle.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static ListNode *findCycleStart(ListNode *head){
        ListNode *fast = head;
        ListNode *slow = head;
        int len = 0;
        while(fast != nullptr && fast->next != nullptr){
            fast = fast->next->next;
            slow = slow->next;
            if(fast == slow){
                len = calculateLength(slow);
                break;
            }
        }
        return findStart(head, len);
        }
    static int calculateLength(ListNode *slow){
        int len = 1;
        ListNode *current = slow->next;
        while(current != slow){
            current = curent->next;
            len++;
        }
        return len;
    }
    
    static ListNode *findStart(ListNode *head, int len){
        ListNode *p1 = head;
        ListNode *p2 = head;
        while(len--){
            p1 = p1->next;
        }
        while(p1 != p2){
            p1 = p1->next;
            p2 = p2->next;
        }
        return p2;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static int findCycleLength(ListNode head){
        ListNode fast = head;
        ListNode slow = head;
        int len = 0;
        while(fast != null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow){
                len = calculateLength(slow);
                break;
            }
        }
        return findStart(head, len);
    }
    
    public static int calculateLength(ListNode slow){
        int len = 1;
        ListNode current = slow.next;
        while(current != slow){
            current = curent.next;
            len++;
        }
        return len;
        
    static ListNode findStart(ListNode head, int len){
        ListNode p1 = head;
        ListNode p2 = head;
        while(len--){
            p1 = p1.next;
        }
        while(p1 != p2){
            p1 = p1.next;
            p2 = p2.next;
        }
        return p2;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

Time Complexity #
As we know, finding the cycle in a LinkedList with ‘N’ nodes and also finding the length of the cycle requires O(N). Also, as we saw in the above algorithm, we will need O(N) to find the start of the cycle. Therefore, the overall time complexity of our algorithm will be O(N).

Space Complexity #
The algorithm runs in constant space O(1).

### Question 4 Happy Number

Any number will be called a happy number if, after repeatedly replacing it with a number equal to the sum of the square of all of its digits, leads us to number ‘1’. All other (not-happy) numbers will never reach ‘1’. Instead, they will be stuck in a cycle of numbers which does not include ‘1’.

 - Example 1:
```sh
Input: 23   
Output: true (23 is a happy number)  
```
 - Example 2:
```sh
Input: 12   
Output: false (12 is not a happy number)  
```
The process, defined above, to find out if a number is a happy number or not, always ends in a cycle. If the number is a happy number, the process will be stuck in a cycle on number ‘1,’ and if the number is not a happy number then the process will be stuck in a cycle with a set of numbers. As we saw in Example-2 while determining if ‘12’ is a happy number or not, our process will get stuck in a cycle with the following numbers: 89 -> 145 -> 42 -> 20 -> 4 -> 16 -> 37 -> 58 -> 89

We saw in the LinkedList Cycle problem that we can use the Fast & Slow pointers method to find a cycle among a set of elements. As we have described above, each number will definitely have a cycle. Therefore, we will use the same fast & slow pointer strategy to find the cycle and once the cycle is found, we will see if the cycle is stuck on number ‘1’ to find out if the number is happy or not.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static bool find(int num){
        int slow = num;
        int fast = calculateSquare(calculateSquare(num));
        while(slow != fast){
            slow = calculateSquare(slow);
            fast = calculateSquare(calculateSquare(fast));
        }
        return slow == 1;
    }
    static int calculateSquare(int num){
            int sum = 0;
            while(num){
                sum += (num % 10)^2;
                num = num/10;
            }
            return sum;
        }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static boolean find(int num){
        int slow = num;
        int fast = calculateSquare(calculateSquare(num));
        while(slow != fast){
            slow = calculateSquare(slow);
            fast = calculateSquare(calculateSquare(fast));
        }
        return slow == 1;
    }
   public  static boolean calculateSquare(int num){
            int sum = 0;
            while(num){
                sum += (num % 10)^2;
                num = num/10;
            }
            return sum;
        }
};
```
Time Complexity #
The time complexity of the algorithm is difficult to determine. However we know the fact that all unhappy numbers eventually get stuck in the cycle: 4 -> 16 -> 37 -> 58 -> 89 -> 145 -> 42 -> 20 -> 4

This sequence behavior tells us two things:

If the number NN is less than or equal to 1000, then we reach the cycle or ‘1’ in at most 1001 steps.
For N > 1000N>1000, suppose the number has ‘M’ digits and the next number is ‘N1’. From the above Wikipedia link, we know that the sum of the squares of the digits of ‘N’ is at most 9*9 M, or 81M (this will happen when all digits of ‘N’ are ‘9’).
This means:
N1 < 81M
As we know M = log(N+1)
Therefore: N1 < 81 * log(N+1) => N1 = O(logN)
This concludes that the above algorithm will have a time complexity of O(logN).

Space Complexity #
The algorithm runs in constant space O(1).

### Question 5 Middle of the LinkedList

Given the head of a Singly LinkedList, write a method to return the middle node of the LinkedList.
If the total number of nodes in the LinkedList is even, return the second middle node.

 - Example 1:
```sh
Input: 1 -> 2 -> 3 -> 4 -> 5 -> null
Output: 3 
```
 - Example 2:
```sh
Input: 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> null
Output: 4
```
 - Example 3:
```sh
Input: 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> null
Output: 4
```
One brute force strategy could be to first count the number of nodes in the LinkedList and then find the middle node in the second iteration. Can we do this in one iteration?

We can use the Fast & Slow pointers method such that the fast pointer is always twice the nodes ahead of the slow pointer. This way, when the fast pointer reaches the end of the LinkedList, the slow pointer will be pointing at the middle node.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static ListNode *findMiddle(ListNode *head){
        ListNode *slow = head;
        ListNode *fast = head;
        while(fast != nullptr && fast->next != nullptr){
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static ListNode findMiddle(ListNode head){
        ListNode slow = head;
        ListNode fast = head;
        while(fast != null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }
};
```
Time Complexity #
O(N)

Space Complexity #
The algorithm runs in constant space O(1).

### Question 6 Palindrome LinkedList

Given the head of a Singly LinkedList, write a method to check if the LinkedList is a palindrome or not.

Your algorithm should use constant space and the input LinkedList should be in the original form once the algorithm is finished. The algorithm should have O(N)O(N) time complexity where ‘N’ is the number of nodes in the LinkedList.

 - Example 1:
```sh
Input: 2 -> 4 -> 6 -> 4 -> 2 -> null
Output: true
```
 - Example 2:
```sh
Input: 2 -> 4 -> 6 -> 4 -> 2 -> 2 -> null
Output: false
```
As we know, a palindrome LinkedList will have nodes values that read the same backward or forward. This means that if we divide the LinkedList into two halves, the node values of the first half in the forward direction should be similar to the node values of the second half in the backward direction. As we have been given a Singly LinkedList, we can’t move in the backward direction. To handle this, we will perform the following steps:

We can use the Fast & Slow pointers method similar to Middle of the LinkedList to find the middle node of the LinkedList.
Once we have the middle of the LinkedList, we will reverse the second half.
Then, we will compare the first half with the reversed second half to see if the LinkedList represents a palindrome.
Finally, we will reverse the second half of the LinkedList again to revert and bring the LinkedList back to its original form.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static bool isPalindrome(ListNode *head){
        if(head == nullptr || head->next = nullptr){
            return true;
        }
        ListNode *fast = head;
        ListNode *slow = head;
        while(fast != nullptr && fast->next != nullptr){
            fast = fast->next->next;
            slow = slow->next;
        }
        ListNode *second = reverse(slow);
        ListNode *copySecond = second;
        while(head != nullptr && second != nullptr){
            if(head->val != second->val){
                break;
            }
            head = head->next;
            second = second->next;
        }
        reverse(copySecond);
        if(head == nullptr || second == nullptr){
            return true;
        }
        return false;
    }
    static ListNode *reverse(ListNode *head){
            ListNode *cur = head;
            ListNode *prev = nullptr;
            ListNode *next = cur->next;
            while(cur != nullptr){
                ListNode *next = cur->next;
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
    public static boolean isPalindrome(ListNode head){
        if(head == null || head.next = null){
            return true;
        }
        ListNode fast = head;
        ListNode slow = head;
        while(fast != null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
        }
        ListNode second = reverse(slow);
        ListNode copySecond = second;
        while(head != null && second != null){
            if(head.val != second.val){
                break;
            }
            head = head.next;
            second = second.next;
        }
        reverse(copySecond);
        if(head == null || second == null){
            return true;
        }
        return false;
    }
    static ListNode reverse(ListNode head){
            ListNode cur = head;
            ListNode prev = null;
            ListNode next = cur.next;
            while(cur != null){
                ListNode next = cur.next;
                cur.next = prev;
                prev = cur;
                cur = next;
            }
            return prev;
        }
};
```
Time Complexity #
O(N)

Space Complexity #
The algorithm runs in constant space O(1).

### Question 7 Rearrange a LinkedList

Given the head of a Singly LinkedList, write a method to modify the LinkedList such that the nodes from the second half of the LinkedList are inserted alternately to the nodes from the first half in reverse order. So if the LinkedList has nodes 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> null, your method should return 1 -> 6 -> 2 -> 5 -> 3 -> 4 -> null.

Your algorithm should not use any extra space and the input LinkedList should be modified in-place.

 - Example 1:
```sh
Input: 2 -> 4 -> 6 -> 8 -> 10 -> 12 -> null
Output: 2 -> 12 -> 4 -> 10 -> 6 -> 8 -> null 
```
 - Example 2:
```sh
Input: 2 -> 4 -> 6 -> 8 -> 10 -> null
Output: 2 -> 10 -> 4 -> 8 -> 6 -> null
```
This problem shares similarities with Palindrome LinkedList. To rearrange the given LinkedList we will follow the following steps:

We can use the Fast & Slow pointers method similar to Middle of the LinkedList to find the middle node of the LinkedList.
Once we have the middle of the LinkedList, we will reverse the second half of the LinkedList.
Finally, we’ll iterate through the first half and the reversed second half to produce a LinkedList in the required order.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static void reorder(ListNode *head){
        if(head == nullptr || head->next == nullptr){
            return;
        }
        ListNode *fast = head;
        ListNode *slow = head;
        while(fast != nullptr && fast->next != nullptr){
            slow = slow -> next;
            fast = fast -> next -> next;
        }
        ListNode *first = head;
        ListNode *second = reverse(slow);
        while(first != nullptr && second != nullptr){
            ListNode *temp = first->next;
            first->next = second;
            first = temp;
            temp = second->next;
            second->next = first;
            second = temp;
    }
    if(first != nullptr){
        first->next = nullptr;
    }
}
    static ListNode *reverse(ListNode *head){
            ListNode *cur = head;
            ListNode *prev = nullptr;
            while(cur != nullptr){
                ListNode *next = cur->next;
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
    public static void reorder(ListNode head){
        if(head == null || head.next == null){
            return;
        }
        ListNode fast = head;
        ListNode slow = head;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode first = head;
        ListNode second = reverse(slow);
        while(first != null && second != null){
            ListNode temp = first.next;
            first.next = second;
            first = temp;
            temp = second.next;
            second.next = first;
            second = temp;
    }
    if(first != null){
        first.next = null;
    }
}
    public static ListNode reverse(ListNode head){
            ListNode cur = head;
            ListNode prev = null;
            while(cur != null){
                ListNode next = cur.next;
                cur.next = prev;
                prev = cur;
                cur = next;
            }
            return prev;
        }
};
```
Time Complexity #
O(N)

Space Complexity #
The algorithm runs in constant space O(1).

### Question 8 Cycle in a Circular Array

We are given an array containing positive and negative numbers. Suppose the array contains a number ‘M’ at a particular index. Now, if ‘M’ is positive we will move forward ‘M’ indices and if ‘M’ is negative move backwards ‘M’ indices. You should assume that the array is circular which means two things:

 - If, while moving forward, we reach the end of the array, we will jump to the first element to continue the movement.
 - If, while moving backward, we reach the beginning of the array, we will jump to the last element to continue the movement.
Write a method to determine if the array has a cycle. The cycle should have more than one element and should follow one direction which means the cycle should not contain both forward and backward movements.

 - Example 1:
```sh
Input: [1, 2, -1, 2, 2]
Output: true
Explanation: The array has a cycle among indices: 0 -> 1 -> 3 -> 0
```
 - Example 2:
```sh
Input: [2, 2, -1, 2]
Output: true
Explanation: The array has a cycle among indices: 1 -> 3 -> 1  
```
 - Example 3:
```sh
Input: [2, 1, -1, -2]
Output: false
Explanation: The array does not have any cycle.
```
This problem involves finding a cycle in the array and, as we know, the Fast & Slow pointer method is an efficient way to do that. We can start from each index of the array to find the cycle. If a number does not have a cycle we will move forward to the next element. There are a couple of additional things we need to take care of:

 - As mentioned in the problem, the cycle should have more than one element. This means that when we move a pointer forward, if the pointer points to the same element after the move, we have a one-element cycle. Therefore, we can finish our cycle search for the current element.

 - The other requirement mentioned in the problem is that the cycle should not contain both forward and backward movements. We will handle this by remembering the direction of each element while searching for the cycle. If the number is positive, the direction will be forward and if the number is negative, the direction will be backward. So whenever we move a pointer forward, if there is a change in the direction, we will finish our cycle search right there for the current element.

### Solution:
 - C++ Solution:
```sh
Class solution{
public:
    static bool loopExists(const vector<int> &arr){
        for(int i = 0; i < arr.size();i++){
            bool isForward = arr[i] > 0;
            int slow = i;
            int fast = i;
            do{
                slow = next(arr, slow, isForward);
                fast = next(arr, fast, isForward);
                if(fast != -1){
                   fast = next(arr, fast, isForward);
                }
            }while(fast != -1 && slow != -1 && fast != slow);
            if(slow != -1 && fast == slow){
                return true;
            }
        }
        return false;
    }
    static int next(const vector<int> &arr, int curr, bool isForward){
            bool dir = arr[cur]>0;
            if(dir != isForward){
                return -1;
            }
            int pos = (arr[curr] + curr + arr.size())%arr.size();
            if(pos == curr){
                return -1;
            }
            return pos;
        }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
Class solution{
    public static boolean loopExists(int[] arr){
        for(int i = 0; i < arr.length;i++){
            boolean isForward = arr[i] > 0;
            int slow = i;
            int fast = i;
            do{
                slow = next(arr, slow, isForward);
                fast = next(arr, fast, isForward);
                if(fast != -1){
                   fast = next(arr, fast, isForward);
                }
            }while(fast != -1 && slow != -1 && fast != slow);
            if(slow != -1 && fast == slow){
                return true;
            }
        }
        return false;
    }
    static int next(int[] arr, int curr, boolean isForward){
            boolean dir = arr[cur]>0;
            if(dir != isForward){
                return -1;
            }
            int pos = (arr[curr] + curr + arr.length)%arr.length;
            if(pos == curr){
                return -1;
            }
            return pos;
        }
};
```
Time Complexity #
O(N^2)

Space Complexity #
The algorithm runs in constant space O(1).
### Todos

 - Write MORE Tests
 - Add Night Mode

License
----

MIT
