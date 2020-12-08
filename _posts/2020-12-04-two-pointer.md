---
published: true
layout: post
date: '2020-12-04 14:50:00 -0000'
categories: two-pointer
---
Two-pointer technique is commonly used to solve array, string and linked list problem. It often solves the problem in place. We have classified the two-pointer questions by its templates. The two pointer questions include *[sliding window](./SlidingWindow.md)*,  *[partition(merge sort, quick sort)](./Sort.md)*,  *[binary search](./BinarySearch.md)*, [*fast and slow pointer*](#Fast and Slow Pointer (Hare & Tortoise)), and [*left and right pointer*](Left and Right Pointer) problems. In this article, we will only describe  [*fast and slow pointer*](#Fast and Slow Pointer (Hare & Tortoise)), and [*left and right pointer*](Left and Right Pointer) .

*The main thought in this article is originated from [labuladong](https://labuladong.gitbook.io/algo/suan-fa-si-wei-xi-lie/shuang-zhi-zhen-ji-qiao), and [jiuzhang](https://www.jiuzhang.com/course/)*

#### Fast and Slow Pointer (Hare & Tortoise):

- It is mostly applied on the linked list or sorted array. The question usually requires O(1) extra space, i.e., no extra spaces.

- It is mostly used by single linked list, whose pointer can not traverse reversely.

- Fast and slow pointer will both start from the first element. The fast pointer will enumerate all elements. We will move slow pointer only when fast pointer's element satisfied some requirements.

- The fast and slow pointer is similar to sliding window. The difference is slow pointer is usually moving one step forward for each right element. In most cases, we need to consider what condition the fast pointer needs to satisfy to make slow pointer move.(要么slow pointer， fast pointer都走一步， 要么slow pointer不走， fast pointer走)。

  

#### Left and Right Pointer:

- It is often used for searching a group of elements satisfying some requirements in sorted array or linked list. The group can be a pair, 3 elements or even a sub array.
- In each iteration, either left and right pointer will move.（要么left pointer走一步， 要么right pointer走一步）。


### Problems

#### Fast and Slow Pointer (Hare & Tortoise):

###### [141. Linked List Cycle (Leetcode)](https://leetcode.com/problems/linked-list-cycle/)

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.

Return `true` *if there is a cycle in the linked list*. Otherwise, return `false`.

**Follow up:**

Can you solve it using `O(1)` (i.e. constant) memory?

 

Example 1:

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```

Example 2:

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

```
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.
```

Example 3:

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

```
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

 

Constraints:

- The number of the nodes in the list is in the range `[0, 104]`.
- `-105 <= Node.val <= 105`
- `pos` is `-1` or a **valid index** in the linked-list.

Analysis:

Linked List can only access each node's next element. If we want to know whether there is a cycle, we can let fast pointer moves 2 nodes every time and slow pointer moves 1 node every time. The fast pointer will meet the slow pointer if there is a cycle.

The cycle can only appear at the end of the tail element.

[Boundary Check](CleanCodePractice.md)

```java
public boolean hasCycle(ListNode head) {

    ListNode slow = head;
    ListNode fast = head;


    while(fast != null && fast.next != null) {

        slow = slow.next;           
        fast = fast.next.next;

        if(fast == slow) {
            return true;
        }

    }

    return false;
}
```

###### [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

Description:

Given a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.

**Notice** that you **should not modify** the linked list.

Follow up:

Can you solve it using `O(1)` (i.e. constant) memory?

 

Example 1:

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

Example 2:

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

```
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```

Example 3:

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

```
Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
```

Constraints:

- The number of the nodes in the list is in the range `[0, 104]`.
- `-105 <= Node.val <= 105`
- `pos` is `-1` or a **valid index** in the linked-list.

Analysis:

This question is extended from [141. Linked List Cycle(Leetcode)](#141. Linked List Cycle(Leetcode)). There is a tricky math equation. If we say slow pointer walks k distance when it meets with fast pointer, fast pointer walks 2k distance. k = head to cycle start + cycle start to meet-up spot because slow pointer walks k distance in total. Also, k = cycle start to meet up spot + meet up spot to cycle start, which is a cycle distance, which is exactly the fast pointer walks faster than slow pointer. As the picture showed in [labuladong](https://labuladong.gitbook.io/algo/suan-fa-si-wei-xi-lie/shuang-zhi-zhen-ji-qiao)

![LinkedListCycleII](E:\study\jiuzhang\Notes\linkedListCycleII.png)

​	So first we let the first and slow meet first time, and let any of them go back from head to move at the same speed until they meet again. The second meetup point is the cycle start time.

```java
public ListNode detectCycle(ListNode head) {
    if(head == null || head.next == null) {
        return null;
    }

    ListNode slow = head;
    ListNode fast = head;

    while(fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;

        if(slow == fast) {

            break;
        }
    }

    if(fast != slow) {

        return null;
    }

    slow = head;

    while(slow != fast) {
        fast = fast.next;
        slow = slow.next;
    }

    return slow;


}
```

######  [1609. Middle of the Linked List](https://www.lintcode.com/problem/middle-of-the-linked-list/description)

Description:

Given a non-empty, singly linked list with head node head, return a middle node of linked list.

If there are two middle nodes, return the second middle node.

Example 1:

```
Input: 1->2->3->4->5->null
Output: 3->4->5->null
```

Example 2:

```
Input: 1->2->3->4->5->6->null
Output: 4->5->6->null
```

Notice

The number of nodes in the given list will be between 1 and 100.

Analysis:

Fast pointer moves 2 steps, and slow moves 1 step. When fast reached to the end or becomes null, slow pointer will move to the middle pointer.

Find middle point of a linked list is the trick point of merge sort on a linked list.

```java
public ListNode middleNode(ListNode head) {

    ListNode slow = head;
    ListNode fast = head;

    while(fast != null && fast.next != null) {

        fast = fast.next.next;
        slow = slow.next;
    }


    return slow;

}
```

###### [166. Nth to Last Node in List](https://www.lintcode.com/problem/nth-to-last-node-in-list/description)

Find the nth to last element of a singly linked list. 

The minimum number of nodes in list is n.

```
Example 1:
	Input: list = 3->2->1->5->null, n = 2
	Output: 1


Example 2:
	Input: list  = 1->2->3->null, n = 3
	Output: 1
```

Analysis:

First we let the fast pointer go n step. Then we let slow and fast pointer go together until the fast pointer reaches null, the slow pointer will get the nth last element.

```java
    public ListNode nthToLast(ListNode head, int n) {
        
        ListNode fast = head;
        ListNode slow = head;
        
        while(n > 0) {
            fast = fast.next; 
            n--;
        }
        
        while(fast != null) {
            fast = fast.next;
            slow = slow.next;
        }
        
        return slow;
    }
```

######  [100. Remove Duplicates from Sorted Array](https://www.lintcode.com/problem/remove-duplicates-from-sorted-array/solution)

Description:

Given a sorted array, 'remove' the duplicates in place such that each element appear only once and return the 'new' length.

Do not allocate extra space for another array, you must do this in place with constant memory.

Example:

```
Input:  [1,1,2]
Output: 2	
Explanation:  uniqued array: [1,2]
```



Analysis:

 Since the array is sorted, the duplicates will stay together. It is easy to find them. However, if we need to remove duplicates, we need to relocate the rest of elements for each duplicate remove. If we can replace the duplicate with next non-duplicate element, we can avoid relocate all elements. We let slow pointer save the last unique element. As long as the element is different from slow pointer's value, we can copy it and update the slow pointer. After the right pointer exam the whole array, we will get all unique elements between 0 and slow.

```java
public int removeDuplicates(int[] nums) {
    if(nums == null || nums.length == 0) {
        return 0;
    }

    int slow = 0;
    int fast = 0;

    while(fast < nums.length) {
        if(nums[slow] != nums[fast]) {
            slow++;
            nums[slow] = nums[fast];
        }
        fast++;
    }

    return slow + 1;
}
```

###### [112. Remove Duplicates from Sorted List](https://www.lintcode.com/problem/remove-duplicates-from-sorted-list/description)

Description:

Given a sorted linked list, delete all duplicates such that each element appear only *once*.

```
Example 1:
	Input:  1->1->2->null
	Output: 1->2->null
	
Example 2:
	Input:  1->1->2->3->3->null
	Output: 1->2->3->null
```

Analysis: This question has similar thought as   [100. Remove Duplicates from Sorted Array](#100. Remove Duplicates from Sorted Array). The difference is for sorted array, we will copy value from fast to slow. For the linked list, we can directly change the slow pointer's next to fast element. One more thing needs to pay attention is after we get the last element as slow, we need to cut the rest of link list by `slow.next = null`. Otherwise, the last duplicate element will still be linked.

```java
public ListNode deleteDuplicates(ListNode head) {

    if(head == null) {
        return head;
    }

    ListNode slow = head;
    ListNode fast = head;

    while(fast != null) {
        if(slow.val != fast.val) {
            slow.next = fast;
            slow = slow.next;
        }
        fast = fast.next;
    }
    slow.next = null;

    return head;
}
```

###### [172. Remove Element](https://www.lintcode.com/problem/remove-element/description)

Description:

Given an array and a value, remove all occurrences of that value in place and return the new length.

The order of elements can be changed, and the elements after the new length don't matter.

```
Example:
	Input:  [0,4,4,0,0,2,4,4], value = 4
	Output: 4
	
	Explanation: 
	the array after remove is [0,0,0,2]
```

Analysis:

If want to delete an element which equals to the target without moving all other elements multiple times, we can try to use fast slow pointer. Slow pointer will move and copy right pointer's value when the element doesn't equal to the target.

```java
public int removeElement(int[] A, int elem) {

    if(A == null || A.length == 0) {
        return 0;
    }

    int fast = 0;
    int slow = 0;

    while(fast < A.length) {

        if(A[fast] != elem) {
            A[slow] = A[fast];
            slow++;

        }

        fast++;

    }

    return slow;


}
```

######  [539. Move Zeroes]()

Description:

Given an array `nums`, write a function to move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

Example 1:

```
Input: nums = [0, 1, 0, 3, 12],
Output: [1, 3, 12, 0, 0].
```

Example 2:

```
Input: nums = [0, 0, 0, 3, 1],
Output: [3, 1, 0, 0, 0].
```

Notice:

1. You must do this **in-place** without making a copy of the array.
2. Minimize the total number of operations.

Analysis:

This question is similar to [172. Remove Element](#172. Remove Element). This question's target element is 0. After we find all elements which are not equal to the target, we will wipe the rest element's to 0.



```java
public void moveZeroes(int[] nums) {

    int fast = 0;
    int slow = 0;

    while(fast < nums.length) {
        if(nums[fast] != 0) {
            nums[slow] = nums[fast];
            slow++;
        }

        fast++;
    }

    for(int i = slow; i < nums.length; i++) {
        nums[i] = 0;
    }

}
```

######  [610. Two Sum - Difference equals to target](https://www.lintcode.com/problem/two-sum-difference-equals-to-target/description)

Description:

Given an sorted array of integers, find two numbers that their `difference` equals to a target value.
return a list with two number like `[num1, num2]` that the difference of `num1` and `num2` equals to target value, and `num1` is less than`num2`.

Example 1:

```
Input: nums = [2, 7, 15, 24], target = 5 
Output: [2, 7] 
Explanation:
(7 - 2 = 5)
```

Example 2:

```
Input: nums = [1, 1], target = 0
Output: [1, 1] 
Explanation:
(1 - 1 = 0)
```

Notice:

It's guaranteed there is only one available solution.

Analysis:

This question is using a combination of left and right pointer and sliding window techniques. 

- Why the question can use sliding window template:

  When right pointer moves, the left pointer can move multiple steps or not move. However,  this question doesn't require a substring or subarray but only two points. Sliding window template is too fancy to use. 

- Why the question can use left and right pointer template:  

  The array is sorted and the question only requires 2 points satisfying the requirement. However, we need to modify the left and right pointer a bit different from other questions. In this question, the left and right pointer will go along the same direction. The left and right pointer template main point is either left or right element will move in each iteration. In this question,  If the left and the right pointer go towards each other, we can not decide which pointer shall move in one iteration. The moving of either left or right pointer will both make difference less. If the left and right pointer goes along the same direction, the difference will become greater when the right pointer moves, and difference will become less when the left pointer moves. Therefore, we will use left and right pointer technique but making the two pointers going along each other.



```java
    public int[] twoSum7(int[] nums, int target) {
        
        int fast = 0;
        int slow = 0;
        
        if(target < 0) {
            target *= (-1);
        }
        
        while(fast < nums.length) {
            if(slow == fast) {
                fast++;
                continue;
            }

            if(nums[fast] - nums[slow] == target) {
                return new int[]{nums[slow], nums[fast]};
            }
            
            if(nums[fast] - nums[slow] > target) {
                slow++;
            }else {
                fast++;
            }
            
            
        }
        
        return new int[]{-1, -1};
    }
```



#### Left and Right Pointer:

###### [56. Two Sum](https://www.lintcode.com/problem/two-sum/my-submissions)

Description:

Given an array of integers, find two numbers such that they add up to a specific target number.

The function `twoSum` should return *indices* of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are zero-based.

```
Example1:
numbers=[2, 7, 11, 15], target=9
return [0, 1]
Example2:
numbers=[15, 2, 7, 11], target=9
return [1, 2]
```

Notice:

You may assume that each input would have exactly one solution

Analysis:

- When the brute force time complexity is O(n ^ 3), and the input is bigger than 500, we shall think optimize the algorithm by sorting data and skipping some comparation, or by using more spaces to save some time. After sort, if it only makes sense to move either left or right pointer at one time, and ignoring all possibilities of moving the other pointer towards the moving pointer, we can try to use two pointers. 
- Left and right pointer will move towards each other until they meet. When left equals right, we won't consider this situation because it will become one not two sum.
- If we find a target, we can move any left or right pointer, or we can move both together.
- If we modify the left or right pointer in the while loop and use it before entering next loop, we need to check its boundary, i.e. left < right.

The brut force way is enumerating all sums of any two element, and find the pair can make sum == target. Time complexity will be O(n^2).  Usually, if we want to decrease the time complexity of O(n ^2) or O(n ^ c) (c is a constant which is way less than n), we can sort the array and see whether we can use two-pointer technique. This method will contain many duplicates.

Here we will show from an example, target == 13

|      | 3    | 4    | 5    | 7    | 8        | 11      |
| ---- | ---- | ---- | ---- | ---- | -------- | ------- |
| 3    | N/A  | <    | <    | <    | 11<13    | 14 > 13 |
| 4    |      | N/A  | <    | <    | 12<13    | >       |
| 5    |      |      | N/A  |      | 13 == 13 | >       |
| 8    |      |      |      | N/A  |          | >       |
| 9    |      |      |      |      | N/A      | >       |
| 10   |      |      |      |      |          | N/A     |

From the above table, we can see, 

- The results below diagonal will be the same result as the parts above diagonal. Therefore, we don't need to calculate them when  left > right. Here we will decrease the calculation half down. 
- When we find a sum > target, like 3 and 11, we don't need to check any number > 3 till 11 because other numbers are > 3, and their sum with 11 will be greater than the current sum. Here by calculating one cell, we can decrease the calculation by a column.
- When we find a sum < target, like 3 and 8, we don't need to check any number < 8 till 3 because other numbers are < 8, and their sum with 3 will be smaller than the current sum. Here by calculating one cell, we can decrease the calculation by a row.
- By either going down or left, we can finish all calculations in O(n)

***If a question relating to an array, and brute force time complexity is O(n ^ c) (c is a constant way less than n ), we can try to sort and see whether we can skip some calculations.***

```java
public class Solution {
    /**
     * @param numbers: An array of Integer
     * @param target: target = numbers[index1] + numbers[index2]
     * @return: [index1 + 1, index2 + 1] (index1 < index2)
     */
    public int[] twoSum(int[] numbers, int target) {
        int[] result = {-1, -1};
        
        Pair[] pairs = new Pair[numbers.length];
        
        for(int i = 0; i < numbers.length; i++) {
            
             pairs[i] = new Pair(numbers[i], i);   
        }
        
        Arrays.sort(pairs, new MyComparator());

        int left = 0;
        int right = pairs.length - 1;
        
        while(left < right) {
            if(pairs[left].num + pairs[right].num > target) {
                right--;
            }else if(pairs[left].num + pairs[right].num == target) {
                result[1] = Math.max(pairs[left].index, pairs[right].index);
                result[0] = Math.min(pairs[right].index, pairs[left].index);
                break;
            }else if(pairs[left].num + pairs[right].num < target) {
                left++;
            }
        }
        
        return result;
    }
}

class Pair {
    int num;
    int index;
    
    Pair(int num, int index) {
        this.num = num;
        this.index = index;
    }
}

class MyComparator implements Comparator<Pair> {
    public int compare(Pair a, Pair b) {
        return a.num - b.num;
    }
}
```

###### [587. Two Sum - Unique pairs](https://www.lintcode.com/problem/two-sum-unique-pairs/description)

Description:

Given an array of integers, find how many `unique pairs` in the array such that their sum is equal to a specific target number. Please return the number of pairs.

Example 1:

```
Input: nums = [1,1,2,45,46,46], target = 47 
Output: 2
Explanation:

1 + 46 = 47
2 + 45 = 47
```

Analysis:

This question is 2-sum + remove duplicate. 

- Since we only care about finding unique pair equals target, we don't need to remove duplicate when they are not equal to the target. If the sum is not equal to the target, the pointer will move automatically. 
- Before we remove duplicate, we need to collect the result first. That is to say `if(sum == nums[left] + nums[right]) result++`must run before `while(nums[left] == nums[left - 1]) left++` . Otherwise, if we remove duplicate first, we might miss a result. For example, if all elements are the same. We will remove duplicates till one element left. When there is only one element, we won't collect any result since it is not a two sum any more.

- We only need to remove either left or right side of duplicate.
- To check whether there is a duplicate, we can compare whether every two elements next to each other are equal or not. Or we can compare each element with former closet distinct element. 

Here I will list a **wrong solution** first. This solution will pass the test, but its correctness is caused by a coincidence. The solution will first check whether there is a duplicate, and will move left pointer to the last duplicated element. If all elements are the same. We will move the left pointer to the right pointer to make them equal. Left and right pointers equal make no scenes in two-sum question. However, here, we have not check left and right's position after we move left either, it will makes the program calculate a sum of one element twice. Since left and right will be the same only under the condition that left and right are duplicates, the one element adding up twice makes up the first sum-up before removing duplicates.

```java
public int twoSum6(int[] nums, int target) {

    if(nums == null || nums.length == 0) {
        return 0;
    }

    Arrays.sort(nums);

    int left = 0;
    int right = nums.length - 1;

    int result = 0;

    while(left < right) {
        if(left > 0 && nums[left] == nums[left - 1]) {
            left++;
            continue;
        }

        if(nums[left] + nums[right] == target) {
            result++;
            left++;
            right--;
        }else if(nums[left] + nums[right] < target) {
            left++;
        }else if(nums[left] + nums[right] > target) {
            right--;
        }

    }

    return result;

}
```

Here is the correct solution

```java
public int twoSum6(int[] nums, int target) {

    if(nums == null || nums.length == 0) {
        return 0;
    }

    Arrays.sort(nums);

    int left = 0;
    int right = nums.length - 1;

    int result = 0;

    while(left < right) {

        int sum = nums[left] + nums[right];

        if(sum == target) {
            result++;
            //Here we can move either left, right, or both
            left++;
			//right--; 
            
            //Here we can move remove duplicate either of from left or right or both
            while(left < right &&  nums[left] == nums[left - 1] ) {
                left++;
            }

        }else if(sum < target) {
            left++;
        }else if(sum > target) {
            right--;
        }

    }

    return result;

}
```

###### [533. Two Sum - Closest to target](https://www.lintcode.com/problem/two-sum-closest-to-target/description)

Description:

Given an array `nums` of *n* integers, find two integers in `nums` such that the sum is closest to a given number, *target*.

Return the absolute value of difference between the sum of the two integers and the target.

Example1

```
Input:  nums = [-1, 2, 1, -4] and target = 4
Output: 1
Explanation:
The minimum difference is 1. (4 - (2 + 1) = 1).
```

Example2

```
Input:  nums = [-1, -1, -1, -4] and target = 4
Output: 6
Explanation:
The minimum difference is 6. (4 - (- 1 - 1) = 6).
```

Analysis:

In this question, we still can use two-pointer technique. when we move left or right pointer, we can skip summing up each element, which are between the 2 pointers, with right or left pointer. We can draw the two-sum table as [56. Two Sum](#56. Two Sum). As long as all possible two sum are covered without duplicates(不重不漏), we can use two pointers. We know, sum will become bigger when we move left pointer, and will become smaller when we move right pointer. However, when the distance is not increasing or decreasing as sum changes. When sum < target, the bigger the sum the smaller the distance. When sum > target, the smaller the sum the smaller the distance. Since we are looking for the smallest distance, we can move the left and pointer based on above rules. We will compare all (left and right) moved distance, and get the smallest.

```java
public int twoSumClosest(int[] nums, int target) {
    if(nums == null || nums.length == 0 || nums.length == 1) {
        return -1;
    }

    Arrays.sort(nums);
    int left = 0;
    int right = nums.length - 1;

    int minDiff = Integer.MAX_VALUE;

    while(left < right) {
        if(target == nums[left] + nums[right]) {
            return 0;
        }else if(target > nums[left] + nums[right]) {
            minDiff = Math.min(minDiff, target - nums[left] - nums[right]);
            left++;
        }else if(target < nums[left] + nums[right]) {
            minDiff = Math.min(minDiff, nums[left] + nums[right] - target);
            right--;
        }

    }

    return minDiff;
}
```

###### [57. 3Sum](https://www.lintcode.com/problem/3sum/solution)

Description:

Given an array *S* of n integers, are there elements *a*, *b*, *c* in *S* such that `a + b + c = 0`? Find all unique triplets in the array which gives the sum of zero.

Example 1:

```
Input:[2,7,11,15]
Output:[]
```

Example 2:

```
Input:[-1,0,1,2,-1,-4]
Output:	[[-1, 0, 1],[-1, -1, 2]]
```

Notice

Elements in a triplet (a,b,c) must be in non-descending order. (ie, a ≤ b ≤ c)

The solution set must not contain duplicate triplets.

Analysis:

This question's solution will be based on the [Two Sum - Unique pairs](#587. Two Sum - Unique pairs). We can enumerate one element and find the target - element as the sum of other 2  elements in unvisited zone.

- The element has been visited won't be used as any two sum's element so that we can avoid duplicates. Since we will try all elements as the first element, we won't miss any combinations as well.(不重不漏)

- The unique pair requires the first element is unique, and two sum unique. Here we need to collect at least one result before we remove duplicate elements so that we won't miss the result of all elements are the same situation.

  [Remove Duplicates Trick among Consecutive Duplicates ](CleanCodePractice.md)

```java
public List<List<Integer>> threeSum(int[] numbers) {
    List<List<Integer>> result = new ArrayList<>();

    if(numbers == null || numbers.length == 0) {
        return result;
    }

    Arrays.sort(numbers);


    for(int i = 0; i < numbers.length; i++) {
        int left = i + 1;
        int right = numbers.length - 1;

        int target = 0 - numbers[i];

        while(left < right) {

            if(target < numbers[left] + numbers[right]) {
                right--;
            }else if(target > numbers[left] + numbers[right]) {
                left++;
            }else if(target == numbers[left] + numbers[right]) {
                List<Integer> list = new ArrayList<>();
                list.add(numbers[i]);
                list.add(numbers[left]);
                list.add(numbers[right]);
                result.add(list);
                left++;

                while(left < right && numbers[left] == numbers[left - 1]) {
                    left++;
                }


            }
        }

        while(i < numbers.length - 1 && numbers[i] == numbers[i + 1]) {
            i++;
        }

    }

    return result;
}
```

###### [59. 3Sum Closest](https://www.lintcode.com/problem/3sum-closest/solution)

Description:

Given an array S of *n* integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers.

Example 1:

```
Input:[2,7,11,15],3
Output:20
Explanation:
2+7+11=20
```

Example 2:

```
Input:[-1,2,1,-4],1
Output:2
Explanation:
-1+2+1=2
```

Challenge:

O(n^2) time, O(1) extra space

Notice:

You may assume that each input would have exactly one solution.

Analysis:

This question need to fix one number and get the closest 2sum to (target - one number)in the rest of array after that one number. We can also change to get the closest (2sum + one number) to the target. If (2sum + one number) > target, and to get the sum closer to the target, we need to make it smaller. Therefore, we will move right pointer. If (2sum + one number) <target, and to get the sum closer to the target, we need to make it bigger. If we find (2sum + one number) == target, it is an ever as close as possible element to the target, we can result the result directly.

```java
public int threeSumClosest(int[] numbers, int target) {

    Arrays.sort(numbers);

    int minSum = 0;
    int minDiff = Integer.MAX_VALUE;

    for(int i = 0; i < numbers.length; i++) {
        int left = i + 1;
        int right = numbers.length - 1;

        while(left < right) {
            int sum = numbers[left] + numbers[right] + numbers[i];

            if(target > sum) {
                if(target - sum < minDiff) {
                    minDiff = target - sum;
                    minSum = sum;
                }
                left++;

            }else if(target < sum) {
                if(sum - target < minDiff) {
                    minDiff = sum - target;
                    minSum = sum;
                }
                right--;
            }else if(target == sum) {
                return sum;
            }
        }
    }

    return minSum;

}
```

###### [58. 4Sum](https://www.lintcode.com/problem/4sum/description)

Description:

Given an array *S* of *n* integers, are there elements *a*, *b*, *c*, and *d* in *S* such that *a + b + c + d = target*?

Find all unique quadruplets in the array which gives the sum of target.

Example 1:

```
Input:[2,7,11,15],3
Output:[]
```

Example 2:

```
Input:[1,0,-1,0,-2,2],0
Output:
[[-1, 0, 0, 1]
,[-2, -1, 1, 2]
,[-2, 0, 0, 2]]
```

Notice

Elements in a quadruplet (*a,b,c,d*) must be in non-descending order. (ie, *a ≤ b ≤ c ≤ d*)
The solution set must not contain duplicate quadruplets.

Analysis:

Fix 2 points and get the sum as target - numbers[i] - numbers[j] in the rest of array after the two points.

```java
    public List<List<Integer>> fourSum(int[] numbers, int target) {
        
        List<List<Integer>> result = new ArrayList<>();
        
        if(numbers == null || numbers.length < 4) {
            return result;
        }
        
        Arrays.sort(numbers);
        for(int i = 0; i < numbers.length; i++) {
            for(int j = i + 1; j < numbers.length - 1; j++) {
                
                int left = j + 1;
                int right = numbers.length - 1;
                
                while(left < right) {
                    int sum = numbers[i] + numbers[j] + numbers[left] + numbers[right];
                    
                    if(target < sum) {
                        right--;
                    }else if(target > sum) {
                        left++;
                    }else if(target == sum) {

                        List<Integer> list = new ArrayList<>();
                        list.add(numbers[i]);
                        list.add(numbers[j]);
                        list.add(numbers[left]);
                        list.add(numbers[right]);
                        
                        result.add(list);
                                                
                        left++;
                        
                        while(left < right && numbers[left] == numbers[left - 1]) {
                            left++;
                        }
                        
                    }
                }
                
                while(j < numbers.length - 2 && numbers[j] == numbers[j + 1]) {
                    j++;
                }
            }
            
            while(i < numbers.length - 1 && numbers[i] == numbers[i + 1]) {
                i++;
            }
        }
        
        return result;
    }
```



######  [610. Two Sum - Difference equals to target](https://www.lintcode.com/problem/two-sum-difference-equals-to-target/description)

Description:

Given an sorted array of integers, find two numbers that their `difference` equals to a target value.
return a list with two number like `[num1, num2]` that the difference of `num1` and `num2` equals to target value, and `num1` is less than`num2`.

Example 1:

```
Input: nums = [2, 7, 15, 24], target = 5 
Output: [2, 7] 
Explanation:
(7 - 2 = 5)
```

Example 2:

```
Input: nums = [1, 1], target = 0
Output: [1, 1] 
Explanation:
(1 - 1 = 0)
```

Notice:

It's guaranteed there is only one available solution.
This question needs to be resolved in place.

Analysis: This question is a bit different from the 2 

```java
public int[] twoSum7(int[] nums, int target) {

    int left = 0;
    int right = 0;

    if(target < 0) {
        target *= (-1);
    }

    while(right < nums.length) {
        if(left == right) {
            right++;
            continue;
        }

        if(nums[right] - nums[left] == target) {
            return new int[]{nums[left], nums[right]};
        }

        if(nums[right] - nums[left] > target) {
            left++;
        }else {
            right++;
        }


    }

    return new int[]{-1, -1};
}
```



###### [382. Triangle Count](https://www.lintcode.com/problem/triangle-count/solution)

Description:

Given an array of integers, how many three numbers can be found in the array, so that we can build an triangle whose three edges length is the three numbers that we find?

Example 1:

```
Input: [3, 4, 6, 7]
Output: 3
Explanation:
They are (3, 4, 6), 
         (3, 6, 7),
         (4, 6, 7)
```

Example 2:

```
Input: [4, 4, 4, 4]
Output: 4
Explanation:
Any three numbers can form a triangle. 
So the answer is C(3, 4) = 4
```

Analysis:

If the sum of any two side is greater than the third side, the three sides can build a triangle. However, we only need to make sure the sum of 2 shorter sides is greater than the other longer side so that the three sides can build a triangle. Here we will enumerate all possible longest side from the 3rd element to the last. For every longest side, we will find all 2 elements whose sum are greater than the longest side. Since we want to enumerate the longest element, we only need to search its left(smaller) side for 2 sum whose value is greater than the longest element.

```java
public int triangleCount(int[] S) {

    if(S == null || S.length < 3) {
        return 0;
    }

    int count = 0;
    Arrays.sort(S);

    for(int i = S.length - 1; i > 1; i--) {

        int left = 0;
        int right = i - 1;

        while(left < right) {

            int sum = S[left] + S[right];

            if(S[i] < sum) {
                count += right - left;
                right--;

            }else if (S[i] >= sum) {
                left++;
            }
        }
    }

    return count;


}
```

977. ###### [(Leetcode) Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/)

Description:

Given an array of integers `A` sorted in non-decreasing order, return an array of the squares of each number, also in sorted non-decreasing order.

Example 1:

```
Input: [-4,-1,0,3,10]
Output: [0,1,9,16,100]
```

Example 2:

```
Input: [-7,-3,2,3,11]
Output: [4,9,9,49,121]
```

Note:

1. `1 <= A.length <= 10000`
2. `-10000 <= A[i] <= 10000`
3. `A` is sorted in non-decreasing order.

Analysis:

The array is sorted and we are trying to find its element's square value in order. Here we can tell, that the max square value can only appear at the two ends of the array, the beginning of the array or the end of the array. We can use two pointer to fill all results in one loop.

```java
public int[] sortedSquares(int[] A) {
    if(A == null || A.length == 0) {
        return new int[0];
    }

    int[] result = new int[A.length];

    int left = 0;
    int right = A.length - 1;

    int cur = result.length - 1;

    while(left <= right) {

        if(A[right] * A[right] > A[left] * A[left]) {
            result[cur] = A[right] * A[right];
            cur--;
            right--;
        }else {
            result[cur] = A[left] * A[left];
            cur--;
            left++;
        }

    }

    return result;


}
```
