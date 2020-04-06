---
published: true
layout: post
title: Two Pointer
date: '2020-04-06 14:50:00 -0000'
categories: Two Pointer
---
- Feature: Left pointer doesn't need to go back, i.e., when right pointer moves to the next position, left pointer doesn't need to start over from the beginning. Instead, it can stay at where it is or move forward.

- Tempalte: Right pointer will iterate through the list, calculate the results and decide when and how left pointer will move depending on the right pointer's value.

## Same Directions

1.Window Sum

Given an array of n integers, and a moving window(size k), move the window at each iteration from the start of the array, find the sum of the element inside the window at each moving.

* left won't move when right < k.
* When right >= k, sum will be added duirng each move of right, and left will move as well.
* Note if you add the sum into result before sum is updated, the last updated sum has to be added outside the loop.


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
