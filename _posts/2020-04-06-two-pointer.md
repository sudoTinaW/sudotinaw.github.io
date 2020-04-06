---
published: true
---
# Two Pointer
## Same Directions

- Feature: Left pointer doesn't need to go back, i.e., when right pointer moves to the next position, left pointer doesn't need to start over from the beginning. Instead, it can stay at where it is or move forward.

- Tempalte: Right pointer will iterate through the list, deal with the left pointer depends on right pointer's value and requirements.

### LintCode Questions

1.Window Sum

Given an array of n integers, and a moving window(size k), move the window at each iteration from the start of the array, find the sum of the element inside the window at each moving.

* result length is nums.length - k + 1
* calculate the first k sum
* Iterate right pointer from 1, to nums.length - k, for each right pointer 's value, remove left pointer's value and add right pointer's value.


      public int[] winSum(int[] nums, int k) {
              if(nums == null || nums.length < k || k <= 0) {
                  return new int[0];
              }
              int[] result = new int[nums.length - k + 1];
              int sum = 0;
              for(int i = 0; i < k; i++) {
                  sum += nums[i];
              }
              result[0] = sum;
              for(int i = 1; i < nums.length - k + 1; i++) {
                  sum = sum - nums[i - 1] + nums[i + k - 1];
                  result[i] = sum;
              }

              return result;

          }
