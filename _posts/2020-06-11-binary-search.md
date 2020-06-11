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
         = T(1) + logn * O*(1)
         = O(log n)

3. Iterative tempalte:
		

