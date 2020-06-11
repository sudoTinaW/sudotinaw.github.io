---
published: true
layout: post
date: '2020-06-11 14:50:00 -0000'
categories: Two Pointer
---
1. What Type of Questions can be Soloved by Binary Search Alogrithm?

	1. Given an **sorted** array and a target, to find any,last,first position of target in the array. 

    2. During a search, if you can leave/cut half of the input every time.

2. Time Complexity : O(logn)


        T(N) = T(n / 2) + O(1)
             = T(n / 4) + O(1) + O(1)
             = T(n / 8) + O(1) + O(1) + O(1)
             ...
             = T(1) + logn * O(1)
             = O(log n)

3. Iterative tempalte:

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


