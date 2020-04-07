---
published: true
layout: post
title: Two Pointer
date: '2020-04-06 14:50:00 -0000'
categories: Two Pointer
---
- Feature: Left pointer doesn't need to go back, i.e., when right pointer moves to the next position, left pointer doesn't need to start over from the beginning. Instead, it can stay at where it is or move forward.

- Tempalte: Right pointer will iterate through the list, calculate the results and decide when and how left pointer will move depending on the right pointer's value. 
	- left pointer moves 1 step when right pointer moves 1 step
    - left pointer move 0 or 1 step when right pointer moves 1 step
    - left pointer moves 0 or multiple steps when right pointer moves 1 step.
    - In general cases, the result will be updated after left is moved to its place.

## Same Directions

1. Window Sum

[https://www.lintcode.com/problem/window-sum/description](https://www.lintcode.com/problem/window-sum/description)

* left won't move when right < k.
* When right >= k,left will move when right moves.
* When right >= k, result will be added when right moves.
* Note result has been updated before left moves, so the last result can not be added inside the loop. The last result is added after the loop. 


      public int[] winSum(int[] nums, int k) {

          if(nums == null || nums.length == 0) {
              return new int[0]; 
          }

          int[] result = new int[nums.length - k + 1];

          int left = 0;
          int sum = 0;
          for(int right = 0; right < nums.length; right++) {
              if(right < k) {
                  sum += nums[right];
                  continue;
              }
              result[right - k] = sum;
              sum = sum - nums[left] + nums[right];
              left++;
          }

          //because the result is added before sum update, 
          //the last sum update won't be added into the result.
          // We add here manually.

          result[result.length - 1] = sum;

          return result;
      }
      
2. Two Sum - Difference equals to target

  https://www.lintcode.com/problem/two-sum-difference-equals-to-target/description

* Two differences usually invlove sorting and two pointer. Because after sorting, the difference between right pointer and left pointer has an order as well. The closer, the smaller the difference is. Therefore, the left pointer doesn't need to go back. If the current left doesn't meet the requirement, the elements on the left side of left pointer won't meet the requirement neither.
* Consider when left pointer needs to move, which is, left < right and pairs[right].value - pairs[left].value > target
* After left move, when the difference == target return result
* There are other details for this question
	* a - b or b - a, the result is the same. Therefore, target can be treated as positive all the time.
    * How to write a Comparator
    * Since the question requires return indexes, and we need to sort. Here we introduce Pair object to encapsulate the index and nums[index].
    * Since left pointer is moving inside a while loop, its boundary needs to tested all the time before it is used.
    
          public int[] twoSum7(int[] nums, int target) {
           int[] result = new int[2];

           if(nums == null || nums.length == 0) {
               return result;
           }

           if(target < 0) {
               target = -target;
           }

           Pair[] pairs = new Pair[nums.length];
           for(int i = 0; i < nums.length; i++) {
               pairs[i] = new Pair(i, nums[i]);
           }

           Arrays.sort(pairs, new MyComparator());

           int left = 0;
           for(int right = 1; right < pairs.length; right++) {
                while(left < right && pairs[right].value - pairs[left].value > target) {
                   left++;

                }
                if(left < right && pairs[right].value - pairs[left].value == target) {
                   result[0] = Math.min(pairs[right].index + 1, pairs[left].index + 1);
                   result[1] = Math.max(pairs[right].index + 1, pairs[left].index + 1);
                   return result; 
               }

           }
             return result;
          }
   
   
3. Remove Duplicates from Sorted Array

  https://www.lintcode.com/problem/remove-duplicates-from-sorted-array/solution

* Left pointer will move when nums[right] != nums[left].
* Copy the right pointer's value to the left pointer's place after left moves.


      public int removeDuplicates(int[] nums) {
        
        if(nums == null || nums.length == 0) {
            return 0;
        }
        int left = 0;
        for(int right = 0; right < nums.length; right++) {
            if(nums[right] != nums[left]) {
                left++;
                nums[left] = nums[right];
            }
            
        }
        return left + 1;
    
      }



