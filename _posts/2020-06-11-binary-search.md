---
published: true
layout: post
date: '2020-06-11 14:50:00 -0000'
categories: Binary Search
---
## Basic Knowledge: 

1. What Type of Questions can be Solved by Binary Search Algorithm?

   1. Given an **sorted** array and a target, to find any,last,first position of target in the array. 

    2. During a search, if you can leave/cut half of the input every time.

2. Time Complexity : O(logn)


        T(N) = T(n / 2) + O(1)
             = T(n / 4) + O(1) + O(1)
             = T(n / 8) + O(1) + O(1) + O(1)
             ...
             = T(1) + logn * O(1)
             = O(log n)

3. Iterative Template:

   ```java
   public int binSearch(int[] nums, int target) {
   	//start and end are both valid indexes
   	int start = 0;
       int end = nums.length() - 1;
   	// start and end will stop when they are next to each other. 
       //By doing this, it can avoid only 2 elements left, and the range (start ~ mid or mid ~ end)
       // doesn't decrease anymore issue. 
       while(start + 1 < end) {
       	int mid = start + (end - start) / 2;
           if(nums[mid] == target) {
           ...
           }else if(nums[mid] < target) {
           	start = mid;
           }else {
           	end = mid;
           }
       }
       // At the end, we have 2 elements left, start and end. 
       // Here we will seperately check which one is the answer
       if(nums[start] == target) {
       	return start;
       }
       if(nums[end] == target) {
       	return end;
       }
       
   }
   ```


   ​    

 4. Recursion Template:

 		
        	

```java
    int binSearch(int[] nums, int target, int start, int end) {
       if(start > end) {
        	return -1;
        }
        
        int mid = start + (end - start) / 2;
        
        if(nums[mid] == target) {
        	return mid;
        }
        
        if(nums[mid] < target) {
        	reutrn binSearch(nums, target, mid + 1, end);
        }
        
        return binSearch(nums, start, mid - 1, target);
    
    }
```


Iterative Vs Recursion

In practice, recursion is not recommended. It can easily cause stack overflow. The recursion way will only be used when the iterative way is too complicated to implement.

## Problems

###### [457 Classical Binary Search](https://www.lintcode.com/problem/classical-binary-search/description)

Description： 

Find any position of a target number in a sorted array. Return `-1` if target does not exist.

Analysis:

Binary Search Template

```java
public int binarySearch(int[] array, int target) {
	
	int start = 0;
	int end = array.length - 1;
	
	while(start + 1 < end) {
		int mid = start + (end - start) / 2;
		if(array[mid] == target) {
			return mid;
		}else if(array[start] < array[mid]) {
			start = mid;
		}else {
            end = mid;
        }
	
	}
    if(array[start] == target) {
        return start;
    }
    if(array[end] == target) {
        return end;
    }
}
```



###### [14. First Position of Target](https://www.lintcode.com/problem/first-position-of-target/description)

Description: 

For a given sorted array (ascending order) and a `target` number, find the first index of this number in `O(log n)` time complexity. If the target number does not exist in the array, return `-1`.

Analysis:

Since target can be found multiple times, we shall let the mid go as left as possible, which means when target == mid, we won't return. Instead, we will let end = mid to make the next mid moves to the left.



```java
    public int binarySearch(int[] nums, int target) {
        
        int start = 0;
        int end = nums.length - 1;
        
        while(start + 1 < end) {
            
            int mid = start + (end - start) / 2;
            
            if(target <= nums[mid]) {
                end = mid;
            }else {
                start = mid;
            }
        }
        
        if(nums[start] == target) {
            return start;
        }
        
        if(nums[end] == target) {
            return end;
        }
        
        return -1;
    }
```



###### [61. Search for a Range](https://www.lintcode.com/problem/search-for-a-range/description)

Description: 

Given a sorted array of *n* integers, find the starting and ending position of a given target value.

If the target is not found in the array, return `[-1, -1]`.

Example:

```
Input:
[5, 7, 7, 8, 8, 10]
8
Output:
[3, 4]
```

Analysis: 

Go 2 rounds, find the first and last of the targets.

*[1. 2 ways to initialize an array with default value](CleanCodePractise.md)*

[*2. Java only passes by value*](CleanCodePractise.md.md)

```java
    public int[] searchRange(int[] A, int target) {
        int[] result = new int[] {-1, -1};
        
        if(A == null || A.length == 0) {
            return result;
        }
        
        int start = 0;
        int end = A.length - 1;
        
        while(start + 1 < end) {
            
            int mid = start + (end - start) / 2;
            
            if(A[mid] >= target) {
                end = mid;
            }else {
                start = mid;
            }
        } 
        
        if(A[start] == target) {
            result[0] = start;
        }else if(A[end] == target) {
            result[0] = end;
        }
        
        start = 0;
        end = A.length - 1;
        
        while(start + 1 < end) {
            
            int mid = start + (end - start) / 2;
            
            if(A[mid] <= target) {
                start = mid;
            }else {
                end = mid;
            }
        } 
        
        if(A[end] == target) {
            result[1] = end;
        }else if(A[start] == target) {
            result[1] = start;
        }
        
        return result;
    }
```



###### [60 Search Insert Postion](https://www.lintcode.com/problem/search-insert-position/description)

Description: 

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume **NO** duplicates in the array.

Example:

`[1,3,5,6]`, 5 → 2

`[1,3,5,6]`, 2 → 1

`[1,3,5,6]`, 7 → 4

`[1,3,5,6]`, 0 → 0

Analysis: 

Here we need to consider 4 situations after the loop stops.  Some of the situation can be combined.

1. target == start or end. Directly return start or end.
2. if target < start, target must be the smallest value. The insertion point must be start (0).
3. if start < target < end, return end.
4. target > end, target must be the biggest value. The insertion point must be end + 1 (A.length + 1)

Here we learn from the template, the stopping point of start and end must be 2 elements, after the iteration, they must stop at 2 elements nearest to the target., and they must be next to each other.

```java
    public int searchInsert(int[] A, int target) {
       
        if(A == null || A.length == 0) {
            return 0;    
        }
        
        int start = 0;
        int end = A.length - 1;
        
        while(start + 1 < end) {
            int mid = start + (end - start) / 2;
            
            if(A[mid] == target) {
                return mid;
            }
            if(A[mid] < target) {
                start = mid;
            }else {
                end = mid;
            }
            
        }
        
        if(target <= A[start]) {
            return start;
        }
        
        if(target > A[end]) {
            return end + 1;
        }
        
        return end;
    }
```

###### [28 Search a 2D Matrix](https://www.lintcode.com/problem/search-a-2d-matrix/description)

Description:

Write an efficient algorithm that searches for a value in an *m* x *n* matrix.

This matrix has the following properties:

- Integers in each row are sorted from left to right.
- The first integer of each row is greater than the last integer of the previous row.

```
Example 1:
	Input:  [[5]],2
	Output: false
	
	Explanation: 
	false if not included.
	
Example 2:
	Input:  [
    [1, 3, 5, 7],
    [10, 11, 16, 20],
    [23, 30, 34, 50]
],3
	Output: true
	
	Explanation: 
	return true if included.
```

Analysis:

The question is looking for whether a butch of data contains a target element. First inspiration is using hash table. However, the data is sorted, and the time complexity is log(n + m), which leads to a binary search.

We search the 2D array twice. 

First we search which row it might be, that is searching the column 0 of all rows. The target can exit or not during the search. There are 5 situations as below,

```
1. target< matrix[start][0] : This means target is smaller than the smallest element in the matrix, and start and end index are positioned at the first 2 row. The result can be returned directly as false.

2. target == matrix[start][0]: This means we find the target, we can return true directly.

3. start < row < end: This means the target might be in the row start, so we need to set row to start and do a binary search again.
row == end : This means we find the target, we can return true directly.

4. end < row: This means the target might be in the last row, so we need to set row to end and do a binary search again.
```

If the program did not return directly (above situation 3 and 4), we will do a second binary search on the specified row.  If we did not find the target, we will return false.

[*3. 2D array empty check*](CleanCodePractise.md)

###### [38. Search a 2D Matrix II](https://www.lintcode.com/problem/search-a-2d-matrix-ii/description)

Description: 

Write an efficient algorithm that searches for a value in an m x n matrix, return the occurrence of it.

This matrix has the following properties:

- Integers in each row are sorted from left to right.
- Integers in each column are sorted from up to bottom.
- No duplicate integers in each row or column.

Example 1:

```
Input:
	[[3,4]]
	target=3
Output:1
```

Example 2:

```
Input:
    [
      [1, 3, 5, 7],
      [2, 4, 7, 8],
      [3, 5, 9, 10]
    ]
    target = 3
Output:2
```

Analysis:

This question is not a typically binary search question. Unlikely normal binary search, each step, we decide to search on left or right side. This question, each step, we decide to search its row or column. Also, we will start search from (top, right) / (bottom. left) corner to (bottom, left)/(top, right) corner, rather than find the target and break the search right away.

 It introduces an important data structure called increasing matrix. To use its sorted property, we need to start our search either from (top, right) or (bottom, left) corner. We can not start the search from (top, left)  or (bottom, right) corner. Because when we start from the  (top, right) or (bottom, left) corner, we can ignore one row or one column depending on the element smaller or greater than the target. However, starting from (top, left) or (bottom, right) , the row or column will both be greater or smaller than the target at the same time. Such that, you can not make decision on which direction to search next. **When the element is equal to the target, we can move both one row and one column diagonally  .**



target在左下或右上时， 其移动行或列时，可以根据与target的大小选择只移动其中一个方向。巧妙的运用了sort之后的结果减少了穷举的路径次数。



```java
public int searchMatrix(int[][] matrix, int target) {
    
    if(matrix == null || matrix.length == 0 || matrix[0] == null || matrix[0].length == 0) {
        return 0;
    }
    
    int m = matrix.length;
    int n = matrix[0].length;
    
    int count = 0;
    int row = 0;
    int col = n - 1;
    while(row < m && col >= 0) {
        if(matrix[row][col] == target) {
            count++;
            row++;
            col--;
        }else if(matrix[row][col] > target) {
            col--;
        }else {
            row++;
        }
        
    }
    
    return count;
    
}
```

###### [74 First Bad Version](https://www.lintcode.com/problem/first-bad-version/description)

Description

The code base version is an integer start from 1 to n. One day, someone committed a bad version in the code case, so it caused this version and the following versions are all failed in the unit tests. Find the first bad version.

You can call `isBadVersion` to help you determine which version is the first bad one. The details interface can be found in the code's annotation part.

Example:

```
Given n = 5:

    isBadVersion(3) -> false
    isBadVersion(5) -> true
    isBadVersion(4) -> true

Here we are 100% sure that the 4th version is the first bad version.
```

Analysis:

Binary search is designed to find a critical point(临界点). The input array is sorted, 0 is before 1. Also, the question is looking for first bad (last good + 1). Therefore, we can use binary search to solve this question.

Binary search always tries to include the answer between start and end. **At the end, if the answer is not the start neither the end, it means the answer is not in the input(corner case).**

The variation in this question is the comparison between A[mid] and target. Since the input is either 0 or 1. There are only 2 situations, < target and == target. If the A[mid] == target, we won't return result because we are looking for the first element == target. 

**In a binary search, mid will continue to move if there are duplicates. It will move to first or last target and stop.**



```java
    public int findFirstBadVersion(int n) {
        int start = 1;
        int end = n;
        
        
        while(start + 1 < end) {
            int mid = start + (end - start) / 2;
            
            if(!SVNRepo.isBadVersion(mid)) {
                start = mid;
            }else {
                end = mid;
            }
        }
        
        if(SVNRepo.isBadVersion(start)) {
            return start;
        }
       return end;
    }
```



###### [162 Find Peak Element(Leetcode)](https://leetcode.com/problems/find-peak-element/) 

Description:

A peak element is an element that is greater than its neighbors.

Given an input array `nums`, where `nums[i] ≠ nums[i+1]`, find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that `nums[-1] = nums[n] = -∞`.

Example 1:

```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```

Example 2:

```
Input: nums = [1,2,1,3,5,6,4]
Output: 1 or 5 
Explanation: Your function can return either index number 1 where the peak element is 2, 
             or index number 5 where the peak element is 6.
```

Analysis:

- This question can be resolved by Linear Scan.  We can find  one nums[i] > nums[i + 1] through a linear scan. Here we don't need to worry about nums[i] > nums[i - 1]. This is because, we could reach nums[i] as the current element only when the check nums[i] > nums[i + 1] failed for the previous (i - 1)th element, indicating that nums[i-1] < nums[i].

- This question have another solution which uses binary search.  **This is a new variation of binary search. Instead of comparing A[mid] and target, it compares A[mid], A[mid - 1], and A[mid + 1]**.  There are 3 situations as below,
  - mid > mid + 1 && mid > mid - 1 : peak, return
  - mid > mid + 1 && mid < mid - 1:  slope \  peak shall be at mid left. move end
  - mid < mid + 1 && mid > mid - 1: slope / peak shall be at mid right. move start.

```java
    public int findPeakElement(int[] nums) {
        
        int start = 0;
        int end = nums.length - 1;
        
        while(start + 1 < end) {
            
            int mid = start + (end - start) / 2;
            if(nums[mid] > nums[mid - 1] && nums[mid] > nums[mid + 1]) {
                return mid;
            }
            if(nums[mid] > nums[mid - 1] && nums[mid] < nums[mid + 1]) {
                start = mid;
            }else {
                end = mid;
            }
        }
        
        if(nums[end] > nums[start]) {
            return end;
        }
        return start;
        
    }
```

###### [585 Maximum Number in Moutain Sequence](https://www.lintcode.com/problem/maximum-number-in-mountain-sequence/description)

This question is the same as above question.

###### [159. Find Minimum in Rotated Sorted Array](https://www.lintcode.com/problem/find-minimum-in-rotated-sorted-array/description)

Description:

Suppose a sorted array in ascending order is rotated at some pivot unknown to you beforehand.

(i.e. , `0 1 2 4 5 6 7` might become `4 5 6 7 0 1 2`).

Find the minimum element.

Example 1:



```
Input：[4, 5, 6, 7, 0, 1, 2]
Output：0
Explanation：
The minimum value in an array is 0.
```

Example 2:

```
Input：[2,1]
Output：1
Explanation：
The minimum value in an array is 1.
```

Analysis:

This question is a variation of binary search as well. **The search usually compares A[start] and A[mid] in rotated sorted array **. This question only depends on the relationship of  A[start] and A[mid]. One corner case, we need to pay attention, when there is not a rotated increasing array. As our logic, start and end will stop at the end of the array. The result shall be the first element, neither start nor end.







###### [62 Search in Rotated Sorted Array](https://www.lintcode.com/problem/search-in-rotated-sorted-array/description)

Description:

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., `0 1 2 4 5 6 7` might become `4 5 6 7 0 1 2`).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

Example 1:

```
Input: [4, 5, 1, 2, 3] and target=1, 
Output: 2.
```

Example 2:

```
Input: [4, 5, 1, 2, 3] and target=0, 
Output: -1.
```

Analysis:

![](E:\study\jiuzhang\Notes\searchInRotatedSortedArray.JPG)

This question can be resolved by find the pivot and binary search 2 sorted parts. However, we can also combine the 2 steps into one.

**Binary Search main thought is reducing search range by half every time**. As the picture above, we will split the general situation by 2 parts,  A[mid] > A[start] and A[mid] < A[start].

- A[mid] > A[start] :
  - target > A[mid] : move start to mid.
  - target < A[mid]
    - target > A[start] : move end to mid
    - target < A[end] : move end to mid
- A[mid] < A[start]:
  - target < A[mid] : end move to mid
  - target > A[mid] : 
    - target > A[start] : end move to mid
    - target < A[start] : start move to mid

Why we need to compare mid with start first? Because if we only cut the graph horizontally by half, we can not decide which half shall we cut. The 2 pictures above cut the parts differently. However, if we cut the graph by  horizontally and vertically, we can decide which part we need to cut off. 

This question has a special case which the array is not rotated, that is A[start] < A[end]. If this is the case, we only need to do a normal binary search.



```java
 public int search(int[] A, int target) {
        
        if(A == null || A.length == 0) {
            return -1;
        }
        
        int start = 0;
        int end = A.length - 1;
        // if the array is not sorted, it is just a normal binary search
        if(A[start] < A[end]) {
             
             while(start + 1 < end) {
                 int mid = start + (end - start) / 2;
                 
                 if(A[mid] == target) {
                     return mid;
                 }
                 if(A[mid] < target) {
                     start = mid;
                 }else {
                     end = mid;
                 }
            }
            
            if(A[start] == target) {
                return start;
            }
            if(A[end] == target) {
                return end;
            }
            
            return -1;
            
        }
        
     // The array is rotated.
        while(start + 1 < end) {
            
            int mid = start + (end - start) / 2;
            
            if(A[mid] > A[start]) {
                if(target > A[mid] || (target < A[mid] && target < A[start])) {
                    start = mid;
                }else{
                    end = mid;
                } 
            }else {
                if(target < A[mid] || (target > A[mid] && target > A[start])) {
                    end = mid;
                }else {
                    start = mid;
                }
            }
        }
    
        if(A[start] == target) {
            return start;
        }
        if(A[end] == target) {
            return end;
        }
        
        return -1;

```

###### [63. Search in Rotated Sorted Array II](https://www.lintcode.com/problem/search-in-rotated-sorted-array-ii/description)

Description:

Follow up for [Search in Rotated Sorted Array](http://www.lintcode.com/problem/search-in-rotated-sorted-array/):

What if **duplicates** are allowed?

Would this affect the run-time complexity? How and why?

Write a function to determine if a given target is in the array.

Example 1:

```
Input:
[]
1
Output:
false
```

Example 2:

```
Input:
[3,4,4,5,7,0,1,2]
4
Output:
true
```



Analysis:

If duplicates are allowed, we can not use binary search any more. So the time complexity will be O(n). **Binary search requires every time we can reduce the search range by half.** If there is a duplicate, we can not decide which direction it can move to anymore. For example, two array 111101 and 101111, we can not decide which direction we can move to by comparing mid with target.

```
The code will be traverse the array to see whether you can find the element.

```

[**65. Median of 2 Sorted Arrays**](https://www.lintcode.com/problem/median-of-two-sorted-arrays/description)

Description:

There are two sorted arrays *A* and *B* of size *m* and *n* respectively. Find the median of the two sorted arrays.

Clarification:

The definition of the median:

- The median here is equivalent to the median in the mathematical definition.
- The median is the middle of the sorted array.
- If there are n numbers in the array and n is an odd number, the median is A[(n-1)/2].
- If there are n numbers in the array and n is even, the median is (A[n / 2] + A[n / 2 + 1]) / 2.
- For example, the median of the array A=[1,2,3] is 2, and the median of the array A=[1,19] is 10.

Example 1

```plain
Input:
A = [1,2,3,4,5,6]
B = [2,3,4,5]
Output: 3.5
```

Example 2

```plain
Input:
A = [1,2,3]
B = [4,5]
Output: 3
```



Analysis:

This question is a hard level question. The first brute force way will be merge 2 sorted array until kth element. The time complexity will be O(k). There is a faster way to do it. The faster way most likely will be O(log k)  time complexity. When we see log(k), we will think about using binary search.

**binary search key point is trying to narrow the search range by half every time**. Here we need find a way to decrease half of the search range in O(1) time.  If we merge 2 sorted array and find the kth element. We only need to merge array of k-length. Same as this thought, we only need to search first k-length elements.

Imagine there are k-length elements. We start our search from 0. The difference between the normal binary search and this imagination is our target is the end. We need to approach our target by only moving start. If as the imagination, we have a k-length sorted array, we can access the kth element in O(1). However, this question has 2 arrays, we can not access kth element without a traverse. By using binary search, we can skip some parts during the traverse of the final imaginary sorted array. Since we are looking for the end, our end is fixed. We can only move 2 starts.

if A[k / 2 - 1] < B[k / 2 - 1], the k length element will not be in the first k / 2 length of A. Because If we pick k/ 2 length elements from A and k / 2 length elements from B, our kth element will be the last element of either A or B. However, we know A[k / 2 - 1] < B[k / 2 - 1], A[k / 2 - 1] will not be the last element, neither any smaller elements of A. Now we can narrow the search range of A from(0, k) to (k/2, k).

Similar, if B[k / 2 - 1] < A[k / 2 - 1], we can  narrow the search range of B from(0, k) to (k/2, k).

Since we use length and starting point to narrow the search range. There isn't an obvious start, mid, end. We will use a more general binary search implementation as recursion. 

For the recursion method, 

First, we need to define the method signature

- Here we need to find the value of k-length element. Our return type shall be this value. 
- The method input shall include the solution's input, which are int[] A and int[] B
- The variation of each call shall be in the input as well, which a starting point and length

The signature will look like,

```java
int findKth(int[] A, int[] B, int aStart, int bStart, int length)
```

Second, we need to consider the base case,

Here we have 3 variables may change in each recursion call, aStart,  bStart, and length. aStart and bStart will increase to A.length or B.length maximum. length will decrease until 1.

The base will look like,

```java
if(aStart >= A.length) {
// if A is all cut and become empty, then the length element will be B[bStart + length - 1]
    return B[bStart + length - 1];
}
if(bStart >= B.length) {
//if B is all cut and become empty, the the length element will be A[aStart + length - 1]
    return A[aStart + length - 1];
}
if(length == 1) {
//if A and B both search range only contains 1 length element, then return the min one as the first 1.
    return Math.min(A[aStart], b[bStart]);
}
```



Third, we will call recursion,

```java
if (A[aStart + length / 2 - 1] < B[bStart + length / 2 - 1]) {
// dump first half of A, which increase the aStart by length / 2.
    findKth(A, B, aStart + length / 2, bStart, length - length / 2);
}else {
    //dump first half of B, which increase the bStart by length / 2.
    findKth(A, B, aStart, bStart + length / 2, length - length / 2);
}
```



Here is a special case, before we use A[aStart + length / 2 - 1] or B[bStart +length / 2 - 1], we need to make sure index is inbound. If aStart + length / 2 - 1 or bStart + length/ 2 - 1 is out of bound, we will cut the length / 2 in the opposite array. Because, for example, if aStart + length / 2 - 1 is beyond the range, which means the left elements' length in A is less than length / 2. Thus, if the kth element is in B,  it must appear somewhere after length/ 2. Therefore, we will cut first half length of B.

Here are the logic for the special case,

```java
if(aStart + k / 2 - 1 >= A.length) {
	findKth(A, B, aStart, bStart + length / 2, length - length / 2);
  }else {
    findKth(A, B, aStart + length / 2, bStart, length - length / 2);
    
}            
```

[*4. Replace Multiplication and Division with Addition and Decrease*](CleanCodePractise.md)

 

```java
    public double findMedianSortedArrays(int[] A, int[] B) {
        
        int n = A.length + B.length;
        
        if (n % 2 == 0) {
            return (
                findKth(A, B, 0, 0, n / 2) + 
                findKth(A, B, 0, 0, n / 2 + 1)
               
            ) / 2.0;
        }
        
        return findKth(A, B, 0, 0, n / 2 + 1);
    }
    
    //aStart and bStart is the starting index, k is the length
    private int findKth(int[] A, int[] B, int aStart, int bStart, int k) {
        if(aStart >= A.length) {
            return B[bStart + k - 1];
        }
        
        if(bStart >= B.length) {
            return A[aStart + k - 1];
        }
        
        if(k == 1) {
            return Math.min(A[aStart], B[bStart]);
        }
        
        if(bStart + k / 2 - 1 >= B.length) {
            aStart = aStart + k / 2;
            // bStart = bStart + k / 2;
        } else if(aStart + k / 2 - 1 >= A.length) {
            // aStart = aStart + k / 2;
            bStart = bStart + k / 2;
            
        }else if(A[aStart + k / 2 - 1] < B[bStart + k/ 2 - 1]){
            aStart = aStart + k / 2;
        }else {
            bStart = bStart + k / 2;
        }
        
        return findKth(A, B, aStart, bStart, k - k / 2);
        
    }

```

###### [600 Smallest Rectangle Enclosing Black Pixels](https://www.lintcode.com/problem/smallest-rectangle-enclosing-black-pixels/description)

Description:

An image is represented by a binary matrix with `0` as a white pixel and `1` as a black pixel. The black pixels are connected, i.e., there is only one black region. Pixels are connected horizontally and vertically. Given the location `(x, y)` of one of the black pixels, return the area of the smallest (axis-aligned) rectangle that encloses all black pixels.



Example 1:

```
Input：["0010","0110","0100"]，x=0，y=2
Output：6
Explanation：
The upper left coordinate of the matrix is (0,1), and the lower right coordinate is (2,2).
```

Example 2:

```
Input：["1110","1100","0000","0000"], x = 0, y = 1
Output：6
Explanation：
The upper left coordinate of the matrix is (0, 0), and the lower right coordinate is (1,2).
```



Analysis:

The brute force way is to search the whole matrix and to remember the top most, and bottom most position. Search the matrix again to find the left most and right most element. Using the 4 elements position calculate the rectangle Area. The time complexity will be O(mn) + O(mn). With this method we have one condition did not use.

Since the matrix is full of "0" and "1". If 0 and 1 are sorted, we can use binary search. This question has another condition, a given location(x, y). If we split the matrix by this "1" location, we can make sure "0" and "1" will be sorted. Thus we can use binary search separately on the split parts as following,

With y as a column, we need to find the left most and right most column,

- In the column range of (0, y), search each row, find the first column that has 1, and set it as left boundary.
- In the column range of (y, n - 1), search each row, find the last column that has 1, and set it as right boundary.

With x as a row boundary, we need to find the top most and bottom most column,

- In the row range of (0, x), search each column, find the first row that has 1, and set it as top boundary.
- In the row range of(x, m - 1), search each column, find the last row that has 1, and set it as bottom boundary.

The time complexity is O(n log m) + O(m log n).

After we get the position of left (top) and right(bottom), we need to calculate its length. We can think in this way, when left == right, the length will be 1. Therefore, the length between left and right will be right - left + 1.

###### [460 Find K Closest Elements](https://www.lintcode.com/problem/find-k-closest-elements/description)

Description:

Given `target`, a non-negative integer `k` and an integer array `A` sorted in ascending order, find the `k` closest numbers to `target` in `A`, sorted in ascending order by the difference between the number and target. Otherwise, sorted in ascending order by number if the difference is same.

Example 1:

```
Input: A = [1, 2, 3], target = 2, k = 3
Output: [2, 1, 3]
```

Example 2:

```
Input: A = [1, 4, 6, 8], target = 3, k = 3
Output: [4, 1, 6]
```

Analysis:

First the question is looking for a starting point(may or may not in a sorted array) which is closest to the target. Then from the starting point, we use 2 pointers to search the 2 directions of the array to find the k closest element.

One detail we need to pay attention during the implementation is when the target is before the first element or after the last element. The searching result will not be accurate any more. The start and end index will either stop at the very beigining of the array or the very last 2 element of the array. The two situation needs to be considered separately. 

Another detail we need to watch out is when the two pointer moves towards 2 directions, we need to consider the pointer out of buond situation.



```java
    public int[] kClosestNumbers(int[] A, int target, int k) {
        int[] result = new int[k];
        
        if(A == null || A.length == 0) {
            return result;
        }
        
        int start = 0;
        int end = A.length - 1;
        
        while(start + 1 < end) {
            
            int mid = start + (end - start) / 2;
            if(A[mid] > target) {
                end = mid;
            }else {
                start = mid;
            }
        }
        
      //If the target is the smallest, we want the for loop below start < 0 starts working from index 0.
        if(target < A[start]) {
            start = -1;
            end = 0;
        }
       //If the target is the largest, we want the for loop below end >= A.length working from A.length - 1. 
        if(target > A[end]) {
            end = A.length;
            start = A.length - 1;
        }
        
        
        for(int i = 0; i < k; i++) {
            
            if(start < 0) {
                result[i] = A[end];
                end++;
                continue;
            }
            
            if(end >= A.length) {
                result[i] = A[start];
                start--;
                continue;
            }

            if(Math.abs(target - A[start]) > Math.abs(target - A[end])) {
                result[i] = A[end];
                end++;
            }else {
                result[i] = A[start];
                start--;
            }
            
        }
        
        return result;
    }
```
