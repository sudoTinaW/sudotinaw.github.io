---
published: false
layout: post
date: '2020-10-20 14:50:00 -0000'
categories: Sliding Window
---
## Sliding Window

### Basic Knowledge: 

Sliding Window often applies on finding consecutive elements .When we need to find subarray or substring, we shall try sliding window technique first. Its has 2 difficulties, one is how to prove a question can use this technique. The other is how to implement it.

- How to prove a question can use sliding window technique:
  Sliding window is actually a greedy algorithm. To proof the current best choice can make the final best choice is the key thought of deciding whether we can use this technique for a question. Here we will offer the detailed proof, and offer a conclusion to help you quickly decide whether we can use this algorithm to solve the problem.

  - The left pointer moves when a window **qualifies** the question's requirement to get the **min** window.

    When the right pointer moves to a new position,

    If the window has qualified the requirement, we shall shrink the window to find the min. There is no need to enlarge the window by moving the left pointer backward.

    If the window disqualified the requirement, any point before the left pointer can not build a min valid window with the current right pointer. That is because any point before the left pointer must be able to build a valid window with a point before the current right pointer, and with the current right pointer the window can only get bigger, which can not be min window any more.

  - The left pointer moves when a window **disqualifies** the question's requirement to get the **max** window.

    When the right pointer moves to a new position,

    If the window still qualifies the requirement, any point before the left pointer can not make a valid window with the current right pointer. Thus the left pointer won't need to go backward.

    If the window disqualifies the requirement, we shall shrink the window to make it qualified instead of enlarging the window by moving the left pointer backwards.

  - The left pointer moves when a window **qualifies** the question's requirement to get a **fixed** window.

    When the right pointer moves to a new position,

    If the window has not reached to the fixed size, the left pointer will stay at the beginning, which is not possible to move backward.

    if the window has reached to the fixed size, the left pointer can only go one forward to maintain the window size, which is not possible to move backwards either.

- How to implement:

  - We have a template :

    - Right and left pointer in this template will both stop at the final result + 1 position. For the right pointer, since substring function requires[ ) boundary, the final result + 1 is what we need. For the left pointer, if we we need to collect its result inside while loop we need to collect it before the left pointer is updated.

    - If the left pointer can only move one step forward at most for every new right pointer, which is a fixed size window situation, we can change the inner `while` loop to `if `condition.

    - The left pointer moves when a window **qualifies** the question's requirement to get the **min/ fixed** window. Therefore, the inner loop condition shall be when window qualifies the question's requirement.

    - The left pointer moves when a window **disqualifies** the question's requirement to get the **max** window, the inner loop condition shall be when window disqualifies the question's requirement. Therefore, the inner loop condition shall be when window disqualifies the question's requirement.

    - The result will always be updated when the window qualifies the question's requirement. When the left pointer moves when a window qualifies the question's requirement, the result will be updated inside the left pointer's move condition. When the left pointer moves when a window disqualifies the question's requirement, the result will be updated outside the left pointer's while loop whenever a right pointer moves and left and updates to a new valid window.

      

  ```java
  /* 滑动窗口算法框架 */
  void slidingWindow(string s, string t) {
      int left = 0, right = 0;
      
      //统计合格的字符个数， 用以判断一个window是否包含所有要求字符
      int valid = 0; 
      while (right < s.size()) {
          // c 是将移入窗口的字符
          char c = s[right];
          // 右移窗口
          right++;
          // 进行窗口内数据的一系列更新
          ...
  
          /*** debug 输出的位置 ***/
          /********************/
  
          // 判断左侧窗口是否要收缩
          while (window needs shrink) {
              //collect result
              ...
              // d 是将移出窗口的字符
              char d = s[left];
              // 左移窗口
              left++;
              // 进行窗口内数据的一系列更新
              ...
        }
      }
  }
  ```

  

### Problems

###### [32. Minimum Window Substring](https://www.lintcode.com/problem/minimum-window-substring/solution)

Description:

Given two strings `source` and `target`. Return the minimum substring of `source` which contains each char of `target`.

Example 1:

```
Input: source = "abc", target = "ac"
Output: "abc"
```

Example 2:

```
Input: source = "adobecodebanc", target = "abc"
Output: "banc"
Explanation: "banc" is the minimum substring of source string which contains each char of target "abc".
```

Example 3:

```
Input: source = "abc", target = "aa"
Output: ""
Explanation: No substring contains two 'a'.
```

Notice:

1. If there is no answer, return `""`.
2. You are guaranteed that the answer is unique.
3. `target` may contain duplicate char, while the answer need to contain at least the same number of that char.

Analysis:

This question meets the situation of which left pointer moves when a window **qualifies** the question's requirement to get the **min** window. Here we will use the sliding window template,

```java
public String minWindow(String source , String target) {

    int[] targetFreq = new int[256];
    int[] sourceFreq = new int[256];

    char[] targetChars = target.toCharArray();
    char[] sourceChars = source.toCharArray();

    int uniqueCount = 0;
    for(char c : targetChars) {
        targetFreq[c]++;
        if(targetFreq[c] == 1) {
            uniqueCount++;
        }
    }


    int left = 0;
    int right = 0;

    int start = 0;
    int end = Integer.MAX_VALUE;

    int valid = 0;
    while(right < sourceChars.length) {

        char c = sourceChars[right];
        right++;

        if(targetFreq[c] > 0) {
 		//Here we will increase the source frequency and then check its validation, because we only need to check the elements 			//inside a window.
            sourceFreq[c]++; 
			//The valid counter can only be increased at "==". If it is ">", its validation must have been counted. We shall not 			 //count multiple times.
            if(sourceFreq[c] == targetFreq[c]) {
                valid++;
            }
        }

        while(valid == uniqueCount) {

            if(right - left < end - start) {
                start = left;
                end = right;
            }


            char d = sourceChars[left];
            left++;

            if(targetFreq[d] > 0) {
                if(sourceFreq[d] == targetFreq[d]) {
                    //The valid counter is better decreased at "==". The valid can also decrease at the "<" condition, but it 						//only works when valid == uniqueCount.
                    valid--;
                }
                //Here we will check the element and then move the window, becuase we only need to check the elements inside
                //a window.
                sourceFreq[d]--;
            }



        }

    }

    return end == Integer.MAX_VALUE? "" : source.substring(start, end);



}
```

###### [604. Window Sum](https://www.lintcode.com/problem/window-sum/solution)

Description:

Given an array of n integers, and a moving window(size k), move the window at each iteration from the start of the array, find the `sum` of the element inside the window at each moving.

Example 1

```
Input：array = [1,2,7,8,5], k = 3
Output：[10,17,20]
Explanation：
1 + 2 + 7 = 10
2 + 7 + 8 = 17
7 + 8 + 5 = 20
```

Analysis:

This question is variation of situation of left pointer moves when a window **qualifies** the question's requirement to get a **fixed** window. We shall collect result whenever the window satisfy the requirement. Here when right - left = k, we shall collect the result.



```java
public int[] winSum(int[] nums, int k) {
    if(nums == null || nums.length == 0 || k <= 0) {
        int[] result = new int[0];
        return result;
    }
    
    int[] result = new int[nums.length - k + 1];
    
    int right = 0;
    int left = 0;
    
    int sum = 0;
    while(right < nums.length) {
        
        int in = nums[right];
        right++;
        
        sum += in;

        if(right - left == k) {
            
            result[left] = sum;
            
            int out = nums[left];
            left++;
            
            sum -= out;
        }
    }
    
    return result;
}
```

[*Create an Empty Array*](CleanCodePractice.md)

######  [1169. Permutation in String](https://www.lintcode.com/problem/permutation-in-string/description)

Description:

Given two strings `s1` and `s2`, write a function to return true if `s2` contains the permutation of `s1`. In other words, one of the first string's permutations is the `substring` of the second string.

Example 1:

```
Input:s1 = "ab" s2 = "eidbaooo"
Output:true
Explanation: s2 contains one permutation of s1 ("ba").
```

Example 2:

```
Input:s1= "ab" s2 = "eidboaoo"
Output: false
```

Notice:

1. The input strings only contain lower case letters.
2. The length of both given strings is in range [1, 10,000].

Analysis:

This question can be translated to check whether s2 contains a substring constructed by all characters from s1. Since this is a substring problem, we shall try to use sliding window first. 

This question is variation of situation of left pointer moves when a window **qualifies** the question's requirement to get a **fixed** window. 

We will still use that template, the main difference from [32. Minimum Window Substring](#Minimum Window Substring) is the left pointer moves forward when the size reaches to s1's length. 

```java
public boolean checkInclusion(String s1, String s2) {

    char[] s1Chars = s1.toCharArray();
    char[] s2Chars = s2.toCharArray();

    int[] targetFreq = new int[128];
    int[] sourceFreq = new int[128];

    int unique = 0;
    for(char c : s1Chars) {
        targetFreq[c]++;
        if(targetFreq[c] == 1) {
            unique++;
        }
    }


    int left = 0;
    int right = 0;

    int valid = 0;
    while(right < s2Chars.length) {

        char in = s2Chars[right];
        if(targetFreq[in] > 0) {
            sourceFreq[in]++;
            if(targetFreq[in] == sourceFreq[in]) {
                valid++;
            }
        }
        right++;
		//Here we change while to if since left pointer will only move one step at most for each right pointer.
        if(right - left == s1Chars.length) {

            if(valid == unique) {
                return true;
            }

            char out = s2Chars[left];


            if(targetFreq[out] > 0) {

                if(sourceFreq[out] == targetFreq[out]) {

                    valid--;
                }
                sourceFreq[out]--;


            }
            left++;
        }

    }

    return false;
}
```

###### [647. Find All Anagrams in a String](https://www.lintcode.com/problem/find-all-anagrams-in-a-string/description)

Description:

Given a string `s` and a **non-empty** string `p`, find all the start indices of `p`'s anagrams in `s`.

Strings consists of lowercase English letters only and the length of both strings **s** and **p** will not be larger than 40,000.

The order of output does not matter.

Example 1:

```
Input : s =  "cbaebabacd", p = "abc"
Output : [0, 6]
Explanation : 
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```

Analysis:

This question is the same question with [1169. Permutation in String](1169. Permutation in String). This question we will collect different result.

```java
public List<Integer> findAnagrams(String s, String p) {

    List<Integer> result = new ArrayList<>();

    if(p.length() > s.length()) {
        return result;
    }

    int[] sFreq = new int[128];
    int[] pFreq = new int[128];

    char[] sChars = s.toCharArray();
    char[] pChars = p.toCharArray();

    int unique = 0;

    for(char c : pChars) {
        pFreq[c]++;
        if(pFreq[c] == 1) {
            unique++;
        }
    }

    int left = 0;
    int right = 0;

    int valid = 0;
    while(right < sChars.length) {

        char in = sChars[right];
        right++;

        if(pFreq[in] > 0) {
            sFreq[in]++;

            if(sFreq[in] == pFreq[in]) {
                valid++;
            }
        }

        if(right - left == pChars.length) {

            if(valid == unique) {
                result.add(left);
            }
            char out = sChars[left];
            left++;

            if(pFreq[out] > 0) {
                if(sFreq[out] == pFreq[out]) {
                    valid--;
                }
                sFreq[out]--;
            }
        }
    }

    return result;

}
```

###### [384. Longest Substring Without Repeating Characters](https://www.lintcode.com/problem/longest-substring-without-repeating-characters/description)

Description:

Given a string, find the length of the longest substring without repeating characters.

Example 1:

```
Input: "abcabcbb"
Output: 3
Explanation: The longest substring is "abc".
```

Example 2:

```
Input: "bbbbb"
Output: 1
Explanation: The longest substring is "b".
```

Analysis:

Since this question is asking for longest substring. We shall try sliding window first.  It meets the situation of left pointer moves when a window **disqualifies** the question's requirement to get the **max** window.

This question uses the template in a bit different way. The result shall be collected after left pointer moving and before right pointer moving. Also, we only need one window hash table instead of 2 (source and target) hash table to compare. All elements shall be updated when window changes instead of only elements inside of target window shall be updated.

```java
public int lengthOfLongestSubstring(String s) {
    if(s == null || s.length() == 0) {
        return 0;
    }

    int maxLen = 1;

    int[] freq = new int[128];
    char[] sChars = s.toCharArray();


    int left = 0;
    int right = 0;

    while(right < sChars.length) {

        char in = sChars[right];
        right++;

        freq[in]++;

        while(freq[in] > 1) {
            char out = sChars[left];
            left++;
            freq[out]--;
        }

        maxLen = Math.max(maxLen, right - left);
    }

    return maxLen;
}
```

###### [386. Longest Substring with At Most K Distinct Characters](https://www.lintcode.com/problem/longest-substring-with-at-most-k-distinct-characters/solution)

Description:

Given a string *S*, find the length of the longest substring *T* that contains at most k distinct characters.

Example 1:

```
Input: S = "eceba" and k = 3
Output: 4
Explanation: T = "eceb"
```

Example 2:

```
Input: S = "WORLD" and k = 4
Output: 4
Explanation: T = "WORL" or "ORLD"
```

Analysis:

This question meets the situation of left pointer moves when a window **disqualifies** the question's requirement to get the **max** window. Left pointer will move when the distinct character is more than k. We will still use one hash table to save the words frequency inside the window.

```java
public int lengthOfLongestSubstringKDistinct(String s, int k) {
    int result = 0;
    
    int[] sFreq = new int[128];
    char[] sChars = s.toCharArray();
    
    int left = 0;
    int right = 0;
    
    int valid = 0;
    while(right < sChars.length) {
        
        char in = sChars[right];
        right++;

        sFreq[in]++;
        
        if(sFreq[in] == 1) {
            valid++;
        }
        
        while(valid > k) {
            
            char out = sChars[left];
            left++;
            
            sFreq[out]--;
            if(sFreq[out] == 0) {
                valid--;
            }
        }
        
       result = Math.max(right - left, result); 
        
    }
    
    return result;
}
```


