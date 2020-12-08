---
published: true
layout: post
date: '2020-08-18 14:50:00 -0000'
categories: Binary Search Tree
---
Binary Search Tree is one of the commonly used binary tree. Its definition is all nodes on the left subtree are <= the node, and all nodes on the right subtree are >= the node.

The blog is written based on [labuladong](https://labuladong.gitbook.io/algo/shu-ju-jie-gou-xi-lie/er-cha-sou-suo-shu-cao-zuo-ji-jin) git book.

### Basic Knowledge
BST's value has orders. Therefore, most of its operations doesn't need to search both right and left subtree, and its template is a bit different from the general traverse.

```java
void BST(TreeNode root, int target) {
    if (root.val == target)
        // 找到目标，做点什么
    if (root.val < target) 
        BST(root.right, target);
    if (root.val > target)
        BST(root.left, target);
}
```

Also, BST's in-order traverse is sorted. It is commonly used with in-order traversal.

###### [95. Validate Binary Search Tree](https://www.lintcode.com/problem/validate-binary-search-tree/description)

Description:

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.
- A single node tree is a BST

Example 1:

```
Input:  {-1}
Output：true
Explanation：
For the following binary tree（only one node）:
	      -1
This is a binary search tree.
```

Example 2:

```
Input:  {2,1,4,#,#,3,5}
Output: true
For the following binary tree:
	  2
	 / \
	1   4
	   / \
	  3   5
This is a binary search tree.
```

Analysis:

Top-down Traversal:

If a tree is a BST. All nodes on the left subtree must be less than its parent node, and all nodes on right subtree must be greater than its parent node. ***When we visit left or right subtree, we can not get root's value (because there is not a pointer from child to parent). Therefore, we need to pass root's value as a parameter.  What's more, left and right subtree's operation are different. If we want left and right subtree run the same operation(recursion required), we need to generalize the operation.*** Here, we will test whether a node is in a range. If it is a node in the left subtree, we will set its left range as negative infinity to make sure the left boundary check won't influence the whole result. Same as right node, we will set each right node's right boundary as positive infinity. From the picture, we can see the right boundary will be updated when we visit a node's left subtree, and the left boundary will be updated when we visit a node's right subtree. If we finish traverse the tree and all nodes are in their correct boundary, we can say BST is valid. If there is any node not in the range, we can stop traverse and return false right away.

![](E:\study\jiuzhang\Notes\validateBST.jpg)

This question can not use the template to choose only one subtree to traverse.

```java
    public boolean isValidBST(TreeNode root) {
            return traverse(root, Long.MAX_VALUE, Long.MIN_VALUE);
        }
        
    private boolean traverse(TreeNode cur, long max, long min) {
    
        if(cur == null) {
            return true;
        }
        
        if(cur.val <= min || cur.val >= max) {
            return false;
        }
    
        boolean left = traverse(cur.left, cur.val, min);
        boolean right = traverse(cur.right, max, cur.val);
    
        if(!left || !right) {
            return false;
        }
    
        return true;
    
    }
```

At the beginning we don't know the root range. Root range can be any integer number, even the biggest integer. Therefore, we set the root range bigger than possible. This is the not the best way. If we only allow integer type (same data range as the result), we shall set the root as `NULL`.  Setting Root range as `NULL`  makes more sense, because root's true range is not between biggest and smallest number but unknown. Here I will post another version using `NULL` as the root range.

```java
    public boolean isValidBST(TreeNode root) {
        
        return helper(root, null, null);
    }
    
    
    private boolean helper(TreeNode cur, Integer min, Integer max) {
        
        if(cur == null) {
            return true;
        }
        
        if((min != null && cur.val <= min) || (max != null && cur.val >= max)) {

            return false;
        }
        
        boolean left = helper(cur.left, min, cur.val);
        boolean right = helper(cur.right, cur.val, max);
        
        if(!left || !right) {
            return false;
        }

        
        return true;

    }
```

Bottom-up:

From bottom to top, If a node's left subtree and right subtree are valid, and its value is greater than left subtree's max value and less than right subtree's min value, it is a valid BST. Therefore, we need to remember each node's min and max in the result set so that we can use left's max and right's min value. Here we will create `ResultType` return object, which includes each node's max and min and their validation result. 

Here we need to pay attention of some details, each `ResultType` saves its real range which are `(left.min, right.max)`. When we use the left and right subtree's result to validate the BST, the current node's value shall be in the range `(left.max, right.min)` .

```java
public class Solution {
    /**
     * @param root: The root of binary tree.
     * @return: True if the binary tree is BST, or false
     */
    public boolean isValidBST(TreeNode root) {
        ResultType result = divideConquer(root);
        return result.isValid;
    }
    
    private ResultType divideConquer(TreeNode cur) {
        
        if(cur == null) {
            return new ResultType(true);
        }


        ResultType left = divideConquer(cur.left);
        ResultType right = divideConquer(cur.right);
    
        if(!left.isValid || !right.isValid) {
            return new ResultType(false);
        }
        
        if((left.max != null && cur.val <= left.max.val) 
           || (right.min != null && cur.val >= right.min.val)) {
            return new ResultType(false);
        }
        
        ResultType result = new ResultType(true);
        result.min = left.min == null ? cur : left.min;
        result.max = right.max == null ? cur : right.max;
        return result;
    }
}

class ResultType {
    TreeNode max;
    TreeNode min;
    boolean isValid;
    
    ResultType(boolean isValid) {
        this.isValid = isValid;
        max = null;
        min = null;
        
    }
}
```

###### [1524. Search in a Binary Search Tree](https://www.lintcode.com/problem/search-in-a-binary-search-tree/description)

Description:

Given the root of a binary search tree (BST) and a `value`.

Return the node whose value equals the given `value`. If such node doesn't exist, return `null`.

Example 1:

```
Input: value = 2
        4
       / \
      2   7
     / \
    1   3
Output: node 2
```

Example 2:

```
Input: value = 5
        4
       / \
      2   7
     / \
    1   3
Output: null
```

Analysis:

It is the template. if we traverse to the bottom and there isn't such node, we will return null. Otherwise, it will return directly without going to the bottom.

```java
public TreeNode searchBST(TreeNode root, int val) {
    
    if(root == null) {
        return null;
    }
    
    if(root.val == val) {
        return root;
    }
    
    if(root.val > val) {
        return searchBST(root.left, val);
    }
    
    if(root.val < val) {
        return searchBST(root.right, val);
    }
    
    return null;
    
}
```


###### [11. Search Range in Binary Search Tree](https://www.lintcode.com/problem/search-range-in-binary-search-tree/solution)

Given a binary search tree and a range `[k1, k2]`, return node values within a given range in ascending order.

Example 1:

```
Input：{5},6,10
Output：[]
        5
it will be serialized {5}
No number between 6 and 10
```

Example 2:

```
Input：{20,8,22,4,12},10,22
Output：[12,20,22]
Explanation：
        20
       /  \
      8   22
     / \
    4   12
it will be serialized {20,8,22,4,12}
[12,20,22] between 10 and 22
```

Analysis:

This question is a variation of [1524. Search in a Binary Search Tree](#**1524. Search in a Binary Search Tree**). The difference between template and this question is the condition to call the recursion. The [1524. Search in a Binary Search Tree](#**1524. Search in a Binary Search Tree**) will call recursion on either left or right side. This question will call recursion as the following situation,

```
if cur.val < k1, call recursion only on cur.right subtree, i.e. if cur.val >= k1 call recursion on cur.left subtree.
if cur.val > k2, call recursion only on cur.left sbutree, i.e., if cur.val <= k2 call recursion on cur.right subtree.
if k1 <= cur.val <= k2, call recursion on both cur.left, and cur.right subtree
Combined above situation, 
When cur.val > k1 we call cur.left. 
When cur.val < k2 we call cur.right.
```

For this question, we can use both pre-order or in-order, since we are collecting the result alone the traverse not by return type.

This question can be also resolved by traverse all nodes and collect the result at the same time. This way will traverse more nodes than the solution above. Here we won't post the solution.

```java
    public List<Integer> searchRange(TreeNode root, int k1, int k2) {
        
        List<Integer> result = new ArrayList<>();
        traverse(root, k1, k2, result);
        return result;
        
    }
    
    private void traverse(TreeNode cur, int k1, int k2, List<Integer> result) {
        
        if(cur == null) {
            return;
        }
        
        if(cur.val >= k1 && cur.val <= k2) {
            result.add(cur.val);
        }
        
        if(cur.val > k1) {
            traverse(cur.left, k1, k2, result);
        }
        
        if(cur.val < k2) {
            traverse(cur.right, k1, k2, result);
        }
        
    }
```

######  [85. Insert Node in a Binary Search Tree](https://www.lintcode.com/problem/insert-node-in-a-binary-search-tree/description)

Description:

Given a binary search tree and a new tree node, insert the node into the tree. You should keep the tree still be a valid binary search tree.

```
Example 1:
	Input:  tree = {}, node = 1
	Output:  1
	
	Explanation:
	Insert node 1 into the empty tree, so there is only one node on the tree.

Example 2:
	Input: tree = {2,1,4,3}, node = 6
	Output: {2,1,4,3,6}
	
	Explanation: 
	Like this:



	  2             2
	 / \           / \
	1   4   -->   1   4
	   /             / \ 
	  3             3   6
		
```

Analysis:

The insertion requires 2 things, first, we need to locate the inserting place. Second, we need to add the node to the inserting point. Since we need to add node, we need to return a root of the tree after insertion.*(一旦涉及到修改node的操作，我们就要返回被改后node所在分支的“root”)*

 First we top-down traverse the tree to find the insertion place. **We will always add new nodes at the bottom of Binary Search Tree.** for each node, we traverse either its left subtree or right subtree, not both. After the recursion returned, we will link the newly inserted subtree's root to the current node.

```java
    public TreeNode insertNode(TreeNode root, TreeNode node) {
        
        if(root == null) {
            return node;
        }

        if(root.val > node.val) {
            TreeNode left = insertNode(root.left, node);
            root.left = left;

        }else {
            TreeNode right = insertNode(root.right, node);
            root.right = right;
        }
        
        return root;
        
    }
    
```

###### [(Leetcode) 1008. Construct Binary Search Tree from Preorder Traversal](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/)

Description:

Return the root node of a binary **search** tree that matches the given `preorder` traversal.

*(Recall that a binary search tree is a binary tree where for every node, any descendant of `node.left` has a value `<` `node.val`, and any descendant of `node.right` has a value `>` `node.val`. Also recall that a preorder traversal displays the value of the `node` first, then traverses `node.left`, then traverses `node.right`.)*

It's guaranteed that for the given test cases there is always possible to find a binary search tree with the given requirements.

Example 1:

```
Input: [8,5,1,7,10,12]
Output: [8,5,10,1,7,null,12]
```

Constraints:

- `1 <= preorder.length <= 100`
- `1 <= preorder[i] <= 10^8`
- The values of `preorder` are distinct.

Analysis:

This questions is a variation of [85. Insert Node in a Binary Search Tree](#85. Insert Node in a Binary Search Tree) . We need to loop the node list + insert each node into BST.

```java
public TreeNode bstFromPreorder(int[] preorder) {
    
    if(preorder == null || preorder.length == 0) {
        return null;
    }
    
    TreeNode root = new TreeNode(preorder[0]);
    
    for(int i = 1; i < preorder.length; i++) {
        TreeNode newNode = new TreeNode(preorder[i]);
        insert(root, newNode);
    }
    
    return root;
}

private TreeNode insert(TreeNode cur, TreeNode newNode) {
    
    if(cur == null) {
        return newNode;
    }
    
    if(newNode.val < cur.val) {
        cur.left = insert(cur.left, newNode);
    }else if(newNode.val > cur.val) {
        cur.right = insert(cur.right, newNode);
    }
    
    return cur;
}
```

###### [87. Remove Node in Binary Search Tree](https://www.lintcode.com/problem/remove-node-in-binary-search-tree/solution)

Description:

Given a root of Binary Search Tree with unique value for each node. Remove the node with given value. If there is no such a node with given value in the binary search tree, do nothing. You should keep the tree still a binary search tree after removal.

Example 1

```plain
Input: 
Tree = {5,3,6,2,4}
k = 3
Output: {5,2,6,#,4} or {5,4,6,2}
Explanation:
Given binary search tree:
    5
   / \
  3   6
 / \
2   4
Remove 3, you can either return:
    5
   / \
  2   6
   \
    4
or
    5
   / \
  4   6
 /
2
```

Example 2

```plain
Input: 
Tree = {5,3,6,2,4}
k = 4
Output: {5,3,6,2}
Explanation:
Given binary search tree:
    5
   / \
  3   6
 / \
2   4
Remove 4, you should return:
    5
   / \
  3   6
 /
2
```

Analysis:

First we can use the template to find the node that we are going to delete. Once find the node, we don't need to traverse till the bottom anymore. Therefore, we will use pre-order traversal.  Similar to the question [85. Insert Node in a Binary Search Tree](#85. Insert Node in a Binary Search Tree) , since we need to modify the node, we need to return its root. 

Second, when we find the node, we will delete the node in the following situations,

- If the node doesn't have any child, set the node to null and return it.

- If the node has only one child, set the node equals to the only child, and only child becomes null.

- If the node has 2 children,

  - Find the right sub tree min node.

  - exchange the value in the min node and the current node. 

  - delete the min node.

    Note: Usually, we don't exchange the value inside the node. Instead we will exchange the node. Because the value can be very big chuck of data, the exchange will take a lot of time. Exchange node reference is usually faster. However, if we want to change the reference here, we need to get each deleted node's parent node. It will complicate the main thought of the problem. We will not use it.

```java
    public TreeNode removeNode(TreeNode root, int value) {
        
        if(root == null) {
            return null;
        }
        
        if(root.val == value) {
            
            if(root.left == null) {
                return root.right;
            }
            
            if(root.right == null) {
                return root.left;
            }
            
            if(root.left != null && root.right != null) {
                
                TreeNode node = getMin(root.right);
                root.val = node.val;
                root.right = removeNode(root.right, node.val);
                
            }
            
        } else if(root.val < value) {
            root.right = removeNode(root.right, value);
        } else if(root.val >= value) {
            root.left = removeNode(root.left, value);
        }
        
        return root;
        
    }
    
    
    private TreeNode getMin(TreeNode cur) {
        
        while(cur.left != null) {
            cur = cur.left;
        }
        
        return cur;
        
    }
```



###### [106. Convert Sorted List to Binary Search Tree](https://www.lintcode.com/problem/convert-sorted-list-to-binary-search-tree/solution)

Description:

Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

```
Example 1:
	Input: array = 1->2->3
	Output:
		 2  
		/ \
		1  3
		
Example 2:
	Input: 2->3->6->7
	Output:
		 3
		/ \
	   2   6
		     \
		      7

	Explanation:
	There may be multi answers, and you could return any of them.
```

  Analysis:

This question can be resolved by 2 ways. First, we can split the list from middle, the left side will be middle point's left sub tree, and the right side will be middle point's right sub tree. Time complexity will be O(n^2) because we need O(n) time to find the middle point, and there are n nodes. Second, we can build the tree using in-order traversal. By using in-order traversal, we can build the tree in order of sequential access of linked list. Thus we can avoid finding middle point time to decrease the time complexity to O(n). Here we will only show the in-order traversal solution.

This question is a bit different from other binary tree questions from 2 aspects,

- Most questions are given a binary tree as input. Which means, dividing work is naturally done (left and right subtree). However, this question we need to divide the list first. To divide the list, we usually need to remember the start and end index of each division. (The thought is similar to **Merge Sort**) . For the linked list, since we can not access the element using start and end index, we will modify to use start node and length.
- The question is using In-order traversal, not like other questions using top-down or bottom-up method. We can not build root before left subtree, neither after right subtree.

We will build the left subtree, create root node, build right subtree. Then connect the root with left and right subtree. The list pointer will move after we create a new tree node. To remember the moving pointer, we can use a global variable, or pass as input and return moved pointer as one of the return results.

 

```java
public class Solution {
    /*
     * @param head: The first node of linked list.
     * @return: a tree node
     */
     
    private ListNode cur;
    
    public TreeNode sortedListToBST(ListNode head) {
        int length = getLength(head);
        cur = head;
        TreeNode root = build(length);
        return root;
        
    }
    
    private TreeNode build(int length) {
        
        if(length <= 0) {
            return null;
        }
        
        if(cur == null) {
            return null;
        }
        
        TreeNode left = build(length / 2);
        TreeNode root = new TreeNode(cur.val);
        cur = cur.next;
        TreeNode right = build(length - length / 2 - 1);
        
        root.left = left;
        root.right = right;
        
        return root;
    }
    
    private int getLength(ListNode head) {
        
        int size = 0;
        while(head != null) {
            head = head.next;
            size++;
        }
        return size;
    }
    
}
```



###### [448. Inorder Successor in BST](https://www.lintcode.com/problem/inorder-successor-in-bst/description?_from=ladder&&fromId=177)

Description:

Given a binary search tree ([See Definition](http://www.lintcode.com/problem/validate-binary-search-tree/)) and a node in it, find the in-order successor of that node in the BST.

If the given node has no in-order successor in the tree, return `null`.

Example 1:

```
Input: {1,#,2}, node with value 1
Output: 2
Explanation:
  1
   \
    2
```

Example 2:

```
Input: {2,1,3}, node with value 1
Output: 2
Explanation: 
    2
   / \
  1   3
```

Challenge:

O(h), where h is the height of the BST.

Notice:

It's guaranteed *p* is one node in the given tree. (You can directly compare the memory address to find p)

Analysis:

If the node p doesn't have a right subtree, the successor will be its closet parent. If the node has a right subtree, the successor will be right subtree's left most node. Here we will draw those 2 conditions. 

- From the picture, we can see, the stop condition is the pointer becomes null. When we traverse the tree, we either traverse left subtree or right subtree, not both. That is to say, when we traverse left subtree, we don't need to save the right subtree's operation, which will run after left subtree.  Therefore, we can use loop easily, instead of recursion. 

- From the picture, we can see, the successor is the parent of last left child (left child may be null). Therefore, we need to save the parent node every time when we move to the left subtree. 
- One thing we need to pay attention, we are not stopping the loop even when we find the target p. We will continue to move to its right.

![](E:\study\jiuzhang\Notes\inorderSuccessor.jpg)

```java
    
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        
        TreeNode next = null;
        while(root != null) {
            
            if(root.val <= p.val) {
                root = root.right;
            }else {
                next = root;
                root = root.left;
            }
        
        }
        
        return next;
        
    }
```
