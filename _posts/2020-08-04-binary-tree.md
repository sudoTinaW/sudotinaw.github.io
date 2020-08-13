---
published: true
layout: post
date: '2020-08-04 14:50:00 -0000'
categories: Binary Tree
---
Analysis:

There are multiple solution to resolve this question. Here is [an article](https://blog.csdn.net/zhuiyisinian/article/details/107946790) about all methods. I will use a general way that can solve pre-order, in-order, post-order together.

First of all, we need to understand the recursion way to resolve the preorder traversal. In the recursion way, the method are loaded into stack and ready to run line by line. I will draw a stack diagram to demonstrate the details. As the picture see, we will push tree node 3, tree node 2 and add integer 1 to the stack. Then, we will pop integer 1 and save it to the result. Then we will do the same thing, push tree node 5, tree node 4, and integer 2 into the stack, and later pop the integer2.

As the picture described, each method (including its instructions and parameters) are saved in the stack. By iterative way, we will also use stack to save recursion's method's each parameter sets and do instructions iteratively. Inside each method,  we can use queue + while loop to run each instructions, or we can save the instructions reversely into a stack to avoid an extra loop. Here we will use this way. 

Because java can not save 2 types in one stack. We will create a customized object to bundle a tree node and marker(whether it is a value, or a node).

![BinaryTreeRecursionStack](E:\study\jiuzhang\Notes\BinaryTreeRecursionStack.JPG)

Since stack is popping in the reversed order of pushing. Here we need to save the nodes in reversed visited order, which means for preorder, we will push the nodes as right, left, and node itself. Right and left are pushed as subtree, node itself will be pushed as wait for visited.Most of Binary Tree questions can be resolved by its three ways of traversal. Pre-order and Post-order are more often used.

### Basic Knowledge

Binary Tree is expanded from linked list data structure. It has the character of inconsecutive data storage , and indirect data access(unlike array's direct access). **It can only be traversed by recursion**. 

There are 3 ways of traversal. Most of the issues can be resolved using the 3 ways of traversal. 

Pre-order(Top-down) Traversal,

```java
public void traverse(TreeNode root) { 
    //root will reslove the smaller issue for one node.
    traverse(root.left);
    traverse(root.right);
}
```

In-order Traversal,

```java
public void traverse(TreeNode root) { 
    traverse(root.left);
    //root will reslove the smaller issue for one node.
    traverse(root.right);
}
```

Post-order(Bottom-up) Traversal,

```java
public void traverse(TreeNode root) { 
    traverse(root.left);
    traverse(root.right);
    //root will reslove the smaller issue for one node.
}
```

Top-down and Bottom-up traversal are often used. 

###### Top-down Traversal (先做事，再分割成左右)

- Do what a node shall do.  明确一个节点要做的事
- Recursive call the node's left and right tree. 递归调用左右子树
- If we need to do something without changing the returning result(not include the class property), we can do it before the recursion. This is a top-down traversal.
- Note: The top-down traversal is traversing from top to bottom(由上至下的正向思考方式). Most of the time, the recursion method has a void return type. We can not remember each recursion's result. If we need remember each recursive call's result, we can save them in the class property or in the input parameter(object). 如果我们需要记录每层递归的一些答案， 我们只能通过全局变量记录，或者通过更改函数input parameter's property来记录（如果 input parameter 是 object）

###### Bottom-up Traversal (先分左右到底，再利用左右结果与node本身value做事)

- Recursive call the node's left and right tree. 递归调用左右子树
- Do what a node shall do with the left and right subtree returning result.  明确一个节点要做的事
- If we don't know the initial value and we need to use the bottom's value to build the final result,  shall call the recursion to divide the tree to the bottom and use the recursion's result to build the final result level by level up to the top. This is the post-order traversal, i.e. bottom-up.
- Note: The bottom-up is traversing from bottom to top（由下至上的逆向思考方式). Most of the time, the recursion method has a return type. After we call left and right sub tree recursion function, we suppose we get the result from both subtree and we need to use the returned result and node itself to calculate the final result.

Most of the questions are combined with two ways. To build some parameters using top-down, some parameters using post-order.

### Problem

######  [66. Binary Tree Preorder Traversal (Non-recursion Version)](https://www.lintcode.com/problem/binary-tree-preorder-traversal/note/224954)

Given a binary tree, return the preorder traversal of its nodes' values.

Example 1:

```
Input：{1,2,3}
Output：[1,2,3]
Explanation:
   1
  / \
 2   3
it will be serialized {1,2,3}
Preorder traversal
```

Analysis:

There are multiple solution to resolve this question. Here is [an article](https://blog.csdn.net/zhuiyisinian/article/details/107946790) about all methods. I will use a general way that can solve pre-order, in-order, post-order together.

First of all, we need to understand the recursion way to resolve the preorder traversal. In the recursion way, the method are loaded into stack and ready to run line by line. I will draw a stack diagram to demonstrate the details. As the picture see, we will push tree node 3, tree node 2 and add integer 1 to the stack. Then, we will pop integer 1 and save it to the result. Then we will do the same thing, push tree node 5, tree node 4, and integer 2 into the stack, and later pop the integer2.

As the picture described, each method (including its instructions and parameters) are saved in the stack. By iterative way, we will also use stack to save recursion's method's each parameter sets and do instructions iteratively. Inside each method,  we can use queue + while loop to run each instructions, or we can save the instructions reversely into a stack to avoid an extra loop. Here we will use this way. 

Because java can not save 2 types in one stack. We will create a customized object to bundle a tree node and marker(whether it is a value, or a node).

![](E:\study\jiuzhang\Notes\BinaryTreeRecursionStack.JPG)

Since stack is popping in the reversed order of pushing. Here we need to save the nodes in reversed visited order, which means for preorder, we will push the nodes as right, left, and node itself. Right and left are pushed as subtree, node itself will be pushed as wait for visited.

```java
public class Solution {
    /**
     * @param root: A Tree
     * @return: Preorder in ArrayList which contains node values.
     */
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        
        if(root == null) {
            return result;
        }
        Stack<Pair> stack = new Stack<>();
        
        stack.push(new Pair(root, false));
        
        while(!stack.isEmpty()) {
            
            Pair pair = stack.pop();
            
            if(!pair.isVisited) {
                
                if(pair.node.right != null) {
                    stack.push(new Pair(pair.node.right, false));
                }
                
                if(pair.node.left != null) {
                    stack.push(new Pair(pair.node.left, false));
                }
                
                stack.push(new Pair(pair.node, true));
            }else {
                result.add(pair.node.val);
            }
            
        }
        
        return result;
        
        
    }
}

class Pair {
    TreeNode node;
    boolean isVisited;
    
    Pair(TreeNode node, boolean isVisited) {
        this.node = node;
        this.isVisited = isVisited;
    }
}
```



###### [481. Binary Tree Leaf Sum](https://www.lintcode.com/problem/binary-tree-leaf-sum/description)

Description:

Given a binary tree, calculate the sum of leaves.

Example 1:

```
Input：{1,2,3,4}
Output：7
Explanation：
    1
   / \
  2   3
 /
4
3+4=7
```

Example 2:

```
Input：{1,#,3}
Output：3
Explanation：
    1
      \
       3
```

Analysis:

Here we can use both top-down and post-order(bottom-up).

top-down: 

Traverse all nodes and check whether the node is a leaf node. If it is true, add its value to the final sum result. The top-down method needs to use a class property to remember the final result. The top-down is easier to think, but has side effect.

```java
public int sum;

public int leafSum(TreeNode root) {
    traverse(root);
    return sum;
}

private void traverse(TreeNode cur) {
    
    if(cur == null) {
        return;
    }
    
    if(cur.right == null && cur.left == null) {
        sum += cur.val;
    }
    
    traverse(cur.left);
    traverse(cur.right);
}
```

Bottom-up (Post-order)： 

The binary tree's leaf sum equals the sum of its left sub tree's leaf sum and right sub tree's leaf sum. 

```java
    public int leafSum(TreeNode root) {
        if(root == null) {
            return 0;
        }
        
        int left = leafSum(root.left);
        int right = leafSum(root.right);
                
        if(root.right == null && root.left == null) {
            return root.val;
        }
        return left + right;
    }
```

###### [482. Binary Tree Level Sum](https://www.lintcode.com/problem/binary-tree-level-sum/description)

Description:

Given a binary tree and an integer which is the depth of the target level.

Calculate the sum of the nodes in the target level.

Example 1:

```
Input：{1,2,3,4,5,6,7,#,#,8,#,#,#,#,9},2
Output：5 
Explanation：
     1
   /   \
  2     3
 / \   / \
4   5 6   7
   /       \
  8         9
2+3=5
```

Example 2:

```
Input：{1,2,3,4,5,6,7,#,#,8,#,#,#,#,9},3
Output：22
Explanation：
     1
   /   \
  2     3
 / \   / \
4   5 6   7
   /       \
  8         9
4+5+6+7=22
```

Analysis:

top-down:

Traverse all nodes until the target level. If the node is in the target level, add its value to the final sum result.

```java
int sum;
public int levelSum(TreeNode root, int level) {
    traverse(root, level, 1);
    return sum;
}

private void traverse(TreeNode cur, int level, int height) {
    
    if(cur == null) {
        return;
    }
    
    if(level == height) {
        sum += cur.val;
        return;
    }
    
    traverse(cur.left, level, height + 1);
    traverse(cur.right, level, height + 1);
}
```

Bottom-up:

以root为顶点的树到targeted level 的level sum = left-subtree's level sum +  right subtree's level sum.

```java
public int levelSum(TreeNode root, int level) {
    if(root == null) {
        return 0;
    }
    
    if(level == 1) {
        return root.val;
    }
    
    int left = levelSum(root.left, level - 1);
    int right = levelSum(root.right, level - 1);
    
    return left + right;
    
}
```

###### [480. Binary Tree Paths](https://www.lintcode.com/problem/binary-tree-paths/description)

Description:

Given a binary tree, return all root-to-leaf paths.

Example 1:

```
Input：{1,2,3,#,5}
Output：["1->2->5","1->3"]
Explanation：
   1
 /   \
2     3
 \
  5
```

Example 2:

```
Input：{1,2}
Output：["1->2"]
Explanation：
   1
 /   
2     
```

Analysis:

top-down traversal: build the string during the traverse of the tree from top to down. Since the returning type is a `List<String>`, we can build the result through input parameter instead of using global variables.

*[5. Modifying input parameters during a method call](CleanCodePractice.md)*

Bottom-up: 

Get all left subtree's paths, and all right subtree's paths. Add the cur value to all left subtree's paths, and right subtree's paths in the front of all paths iteratively. 

Here we will only post the top-down traversal way since it is easier to think of.

```java
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<>();
        
        if(root == null) {
            return result;
        }
        traverse(root, "" + root.val, result);
        
        return result;
    }
    
    private void traverse(TreeNode cur, String path, List<String> result) {
        
        if(cur.left == null && cur.right == null) {
            result.add(path);
            
            return;
        }
        
        
        if(cur.left != null) {
            traverse(cur.left, path + "->" + cur.left.val, result);

        }
        if(cur.right != null) {
            traverse(cur.right, path + "->" + cur.right.val, result);
        }

    }
```

###### [469. Same Tree](https://www.lintcode.com/problem/same-tree/description)

Description:

Check if two binary trees are identical. Identical means the two binary trees have the same structure and every identical position has the same value.

Example 1:

```
Input:{1,2,2,4},{1,2,2,4}
Output:true
Explanation:
        1                   1
       / \                 / \
      2   2   and         2   2
     /                   /
    4                   4

are identical.
```

Example 2:

```
Input:{1,2,3,4},{1,2,3,#,4}
Output:false
Explanation:

        1                  1
       / \                / \
      2   3   and        2   3
     /                        \
    4                          4

are not identical.
```

Analysis:

Top-down:

Traverse all the nodes same time of tree a and tree b. Use a global variable to remember if they are the same or not.

Bottom-up:

If left subtree and right subtree are identical, and the cur value from a and b are the same, we can return true. Otherwise, return false.

```java
    public boolean isIdentical(TreeNode a, TreeNode b) {
        
        if(a == null && b == null) {
            return true;
        }
        
        if(a == null && b != null || a != null && b == null) {
            return false;
        }
        
        boolean isLeftTreeIdentical = isIdentical(a.left, b.left);
        boolean isRightTreeIdentical = isIdentical(a.right, b.right);
        
        return isLeftTreeIdentical && isRightTreeIdentical && a.val == b.val;
    }
```

###### [175. Invert Binary Tree](https://www.lintcode.com/problem/invert-binary-tree/description)

Description:

Invert a binary tree.left and right subtrees exchange.

Example 1:

```
Input: {1,3,#}
Output: {1,#,3}
Explanation:
	  1    1
	 /  =>  \
	3        3
```

Example 2:

```
Input: {1,2,3,#,#,4}
Output: {1,3,2,#,4}
Explanation: 
	
      1         1
     / \       / \
    2   3  => 3   2
       /       \
      4         4
```

Analysis:

Top-down: this question doesn't need a return type, so we will use top-down directly. We will swap left and right sub trees, and call recursions.  

Even though after the swap the traverse order of left and right will change, it won't influence the  final result. The question only requires invert all left and sub trees.

```java
    public void invertBinaryTree(TreeNode root) {
        
        if(root == null) {
            return;
        }
        
        TreeNode temp = root.right;
        root.right = root.left;
        root.left = temp;
        invertBinaryTree(root.left);
        invertBinaryTree(root.right);
    }
```

###### [97. Maximum Depth of Binary Tree](https://www.lintcode.com/problem/maximum-depth-of-binary-tree/description)

Description:

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

Example 1:

```
Input: tree = {}
Output: 0
Explanation: The height of empty tree is 0.
```

Example 2:

```
Input: tree = {1,2,3,#,#,4,5}
Output: 3	
Explanation: Like this:
   1
  / \                
 2   3                
    / \                
   4   5
it will be serialized {1,2,3,#,#,4,5}
```

Analysis:

Bottom-up:

This is a basic question. Since the method needs a return type, and the method doesn't need extra input parameters, we can  directly use the method signature with bottom-up. Each node's max depth is its max depth of its left subtree and right subtree + 1. 

```java
    public int maxDepth(TreeNode root) {
        if(root == null) {
            return 0;
        }
        
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        
        return Math.max(left, right) + 1;
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

This question is a combination of bottom-up and top-down traversal. The return value shall be the root of the tree after insertion. First we top-down traverse the tree to find the insertion place. **We will always add new nodes at the bottom of Binary Search Tree.** for each node, we traverse either its left subtree or right subtree., not both. After the recursion returned, we will link the newly inserted subtree's root to the current node.

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

###### [1008. Construct Binary Search Tree from Preorder Traversal Leetcode](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/)

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

This questions is just loop each node + insert each node into BST.

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

######  [93. Balanced Binary Tree](https://www.lintcode.com/problem/balanced-binary-tree/description)

Description:

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

```
Example  1:
	Input: tree = {1,2,3}
	Output: true
	
	Explanation:
	This is a balanced binary tree.
		  1  
		 / \                
		2  3

	
Example  2:
	Input: tree = {3,9,20,#,#,15,7}
	Output: true
	
	Explanation:
	This is a balanced binary tree.
		  3  
		 / \                
		9  20                
		  /  \                
		 15   7 

	
Example  3:
	Input: tree = {1,#,2,3,4}
	Output: false
	
	Explanation:
	This is not a balanced tree. 
	The height of node 1's right sub-tree is 2 but left sub-tree is 0.
		  1  
		   \                
		   2                
		  /  \                
		 3   4
```

Analysis:

Since this question requires return value and the value type is a primary type. We will use bottom-up method to resolve this question.

Bottom-up:

Left subtree is balanced, right subtree is balanced, and their heights' differences are not greater than 1, then we can say the tree is a balanced tree. Therefore, we need to return both heights and whether the current tree is a balanced tree. **If we need to return 2 different primary types for one method call, we can create an object and save those 2 return results into object's properties.**

```java
public class Solution {
    /**
     * @param root: The root of binary tree.
     * @return: True if this Binary tree is Balanced, or false.
     */
     
    
    public boolean isBalanced(TreeNode root) {
        
        ResultType result = helper(root);
        return result.isBalanced;
    }
    
    private ResultType helper(TreeNode cur) {
    
        if(cur == null) {
            return new ResultType(0, true);
        }
        
        ResultType left = helper(cur.left);
        ResultType right = helper(cur.right);
        
        if(!left.isBalanced || !right.isBalanced || Math.abs(left.depth - right.depth) > 1) {
            return new ResultType(Math.max(left.depth, right.depth) + 1, false);
        }
        
        return new ResultType(Math.max(left.depth, right.depth) + 1, true);
        
    } 
    
    
    
}

class ResultType {
    int depth;
    boolean isBalanced;
    
    ResultType(int depth, boolean isBalanced) {
        
        this.depth = depth;
        this.isBalanced = isBalanced;
    }
}
```

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

From the description, we can tell BST's character is all nodes in left subtree are smaller than root's value, and all nodes in right subtree are greater than root's value. Therefore, we need to maintain a max value for left subtree, and a min value for right subtree. 

Here we have 2 ways to save the max and min value for left and right subtree. We can save the min and max value in an return object or we can save them as the input parameters. If we are using the first way, we will use bottom-up to resolve the issue. If we are using the second way, we will use top-down traversal with a return type to solve this problem.

Top-down Traversal:

We can save each node's max and min range as the input parameter, and check if cur is in the min and max range. After call recursion, we will also need to validate if left sub tree is valid, and right sub tree is valid. 

```java
public boolean isValidBST(TreeNode root) {
        return traverse(root, Long.MAX_VALUE, Long.MIN_VALUE);
    }
    
private boolean traverse(TreeNode cur, long max, long min) {

    if(cur == null) {
        return true;
    }

    boolean left = traverse(cur.left, cur.val, min);
    boolean right = traverse(cur.right, max, cur.val);


    if(left && right && cur.val > min && cur.val < max) {
        return true;
    }

    return false;

}
```



At the beginning we don't know the root range. Root range can be any integer number, even the biggest integer. Therefore, we set the root range bigger than possible. This is the not the best way. If we only allow integer type (same data range as the result), we shall set the root as `NULL`.  Setting Root range as `NULL`  makes more sense, because root's true range is not between biggest and smallest number but unknown. Here I will post another version using `NULL` as the root range.

```java
    public boolean isValidBST(TreeNode root) {
        return traverse(root, null, null);
    }
    
    private boolean traverse(TreeNode cur, Integer max, Integer min) {
        
        if(cur == null) {
            return true;
        }
        
       if(min != null && cur.val <= min) {
           return false;
       }
       
       if(max != null && cur.val >= max) {
           
           return false;
       }
        
        return traverse(cur.left, cur.val, min) && traverse(cur.right, max, cur.val);
    }
```

Bottom-up:

We need to create a `ResultType`, which includes each node's range and their validation information. 

The current node will be valid if its left subtree and right subtree are valid, and cur's value is greater than left subtree's max value and less than right subtree's min value, the cur's value is valid. Since all one node cases are valid BST, we will set one node's min and max as null.

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

###### [376. Binary Tree Path Sum](https://www.lintcode.com/problem/binary-tree-path-sum/description)

Description:

Given a binary tree, find all paths that sum of the nodes in the path equals to a given number `target`.

A valid path is from root node to any of the leaf nodes.

Example 1:

```plain
Input:
{1,2,4,2,3}
5
Output: [[1, 2, 2],[1, 4]]
Explanation:
The tree is look like this:
	     1
	    / \
	   2   4
	  / \
	 2   3
For sum = 5 , it is obviously 1 + 2 + 2 = 1 + 4 = 5
```

Example 2:

```plain
Input:
{1,2,4,2,3}
3
Output: []
Explanation:
The tree is look like this:
	     1
	    / \
	   2   4
	  / \
	 2   3
Notice we need to find all paths from root node to leaf nodes.
1 + 2 + 2 = 5, 1 + 2 + 3 = 6, 1 + 4 = 5 
There is no one satisfying it.
```

Analysis:

Top-down:

Calculate sum and collect paths from root to all nodes during top-down traversal. When we reach to leaves and the sum is exactly target, we will collect this path to the final result. 

This question includes a backtracking skill point. Before we call the next level recursion, we will modify some properties. After the recursion return to the current level, we need to recover those modified properties.

```java
public List<List<Integer>> binaryTreePathSum(TreeNode root, int target) {
    List<List<Integer>> result = new ArrayList<>();
    
    if(root == null) {
        return result;
    }
    
    List<Integer> path = new ArrayList<>();
    path.add(root.val);
    
    traverse(root, target, path,  result, root.val);
    return result;
}

public void traverse(TreeNode cur, int target, List<Integer> path, List<List<Integer>> result, int sum) {
    
    if(cur == null) {
        return;
    }
    
    if(cur.left == null && cur.right == null && sum == target) {
        result.add(new ArrayList<>(path));
    }
    
    if(cur.left != null) {
        path.add(cur.left.val);
        traverse(cur.left, target, path, result, sum + cur.left.val);
        path.remove(path.size() - 1);
    }
    
    if(cur.right != null) {
        path.add(cur.right.val);
        traverse(cur.right, target, path, result, sum + cur.right.val);
        path.remove(path.size() - 1);
    }
}
```

Bottom-up:

Find all paths in left subtree and right subtree their sum equals target - root's value. Collect all left and right subtree's result.

######  [246. Binary Tree Path Sum II](https://www.lintcode.com/problem/binary-tree-path-sum-ii/description)

Your are given a binary tree in which each node contains a value. Design an algorithm to get all paths which sum to a given value. The path does not need to start or end at the root or a leaf, but it must go in a straight line down.

Example 1:

```plain
Input:
{1,2,3,4,#,2}
6
Output:
[[2, 4],[1, 3, 2]]
Explanation:
The binary tree is like this:
    1
   / \
  2   3
 /   /
4   2
for target 6, it is obvious 2 + 4 = 6 and 1 + 3 + 2 = 6.
```

Example 2:

```plain
Input:
{1,2,3,4}
10
Output:
[]
Explanation:
The binary tree is like this:
    1
   / \
  2   3
 /   
4   
for target 10, there is no way to reach it.
```



Analysis:

This question requires returning  a list of all paths. We can use both top-down and bottom-up. However, bottom-up needs to consider the question from result to requirements. Also, top-down won't introduce extra global variables because the result is an object already. Therefore, we will use top-down traversal for this question.

We will traverse top-down to build a path from root to leaf. For each node, we will iteratively calculate sum back up from each node to get sums not starting from root.

This question is actually a typical DFS question. DFS is actually a top-down traversal for multiple-branch trees. In the requirements, we can see the question is looking for **all paths**. DFS is looping each element in the same level horizontally and call recursion for each node.  this question, horizontally, we only have 2 subtrees. We will enumerate them by left and right. Here we need to pay attention, this question's iteration is different from the DFS template iteration. 

Time complexity is O(n^2).

```java
public List<List<Integer>> binaryTreePathSum2(TreeNode root, int target) {
    List<List<Integer>> result = new ArrayList<>();
    helper(root, target, new ArrayList<>(), result);
    return result;
}

private void helper(TreeNode cur, int target, List<Integer> subset, List<List<Integer>> result) {
    
    if(cur == null) {
        return;
    }
    
    subset.add(cur.val);
    
    int subSum = 0;
    for(int i = subset.size() - 1; i >= 0; i--) {
        subSum += subset.get(i);
        if(subSum == target) {
            result.add(new ArrayList<Integer>(subset.subList(i, subset.size())));
        }    
    }
    
    helper(cur.left, target, subset, result);
    helper(cur.right, target, subset, result);
    subset.remove(subset.size() - 1);
}
```

######  

###### [124. Binary Tree Maximum Path Sum(leetcode)](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

Given a **non-empty** binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain **at least one node** and does not need to go through the root.

```
Example 1:
Input: [-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

Output: 42
	
Example 2:
	Input:  For the following binary tree:

      1
     / \
    2   3
		
	Output: 6
```

Analysis: To solve this question, we shall know how to do the question [475. Binary Tree Maximum Path Sum II](#475. Binary Tree Maximum Path Sum II). The max path sum is the max value of left,right subtree and crossing root path sum. the max crossing root sum =(max of (with left subtree's top as root, max path sum) and 0) + max of (with right subtree's top as root, max path sum) and 0) + root's value.

```java
public class Solution {
    /**
     * @param root: The root of binary tree.
     * @return: An integer
     */
    public int maxPathSum(TreeNode root) {
        
        ResultType result = helper(root);
        return result.maxPathSum;
    }
    
    private ResultType helper(TreeNode cur) {
        
        if(cur == null) {
            return new ResultType(Integer.MIN_VALUE, 0);
        }
        
        ResultType left = helper(cur.left);
        ResultType right = helper(cur.right);
        
        int singlePathSum = Math.max(left.singlePathSum, right.singlePathSum) + cur.val;
        singlePathSum = Math.max(singlePathSum, 0);
        
        int maxPathSum = Math.max(left.maxPathSum, right.maxPathSum);
        maxPathSum = Math.max(maxPathSum, left.singlePathSum + cur.val + right.singlePathSum);
        
        return new ResultType(maxPathSum, singlePathSum);
    }
}

class ResultType {
    int maxPathSum;
    int singlePathSum;
    
    ResultType(int maxPathSum, int singlePathSum) {
        this.maxPathSum = maxPathSum;
        this.singlePathSum = singlePathSum;
    }
    
}
```

###### [475. Binary Tree Maximum Path Sum II](https://www.lintcode.com/problem/binary-tree-maximum-path-sum-ii/description)

Given a binary tree, find the maximum path sum from root.

The path may end at any node in the tree and contain at least one node in it.

Example 1:

```
Input: {1,2,3}
Output: 4
Explanation:
    1
   / \
  2   3
1+3=4
```

Example 2:

```
Input: {1,-1,-1}
Output: 1
Explanation:
    1
   / \
  -1 -1
```

Analysis: 

Bottom-up:

We can get left subtree's max path sum and right subtree's max path sum. Compare the left subtree and right subtree. If the greater path sum is **> 0**, we will add the path sum to with the root value and return. Otherwise, we will return root value itself.

```java
    public int maxPathSum2(TreeNode root) {
        
        if(root == null) {
            return 0;
        }
        
        int left = maxPathSum2(root.left);
        int right = maxPathSum2(root.right);
        
        return Math.max(Math.max(0, left), right) + root.val;
    }
```

###### [596. Minimum Subtree](https://www.lintcode.com/problem/minimum-subtree/solution)

Description:

Given a binary tree, find the subtree with minimum sum. Return the root of the subtree.

Example 1:

```
Input:
{1,-5,2,1,2,-4,-5}
Output:1
Explanation:
The tree is look like this:
     1
   /   \
 -5     2
 / \   /  \
1   2 -4  -5 
The sum of whole tree is minimum, so return the root.
```

Example 2:

```
Input:
{1}
Output:1
Explanation:
The tree is look like this:
   1
There is one and only one subtree in the tree. So we return 1.
```

Notice:

LintCode will print the subtree which root is your return node.
It's guaranteed that there is only one subtree with minimum sum and the given binary tree is not an empty tree.

Analysis: Get all sums for the all subtrees. Compare each sum and maintain a minimum for all trees.

```java
public class Solution {
    private TreeNode subtree = null;
    private int subtreeSum = Integer.MAX_VALUE;
    /**
     * @param root the root of binary tree
     * @return the root of the minimum subtree
     */
    public TreeNode findSubtree(TreeNode root) {
        helper(root);
        return subtree;
    }
    
    private int helper(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int sum = helper(root.left) + helper(root.right) + root.val;
        if (sum <= subtreeSum) {
            subtreeSum = sum;
            subtree = root;
        }
        return sum;
    }
}
```

Bottom-up:

minimum sum of a subtree is the minimum of (left, right, and left + right + root value).

```java
public class Solution {
    /**
     * @param root: the root of binary tree
     * @return: the root of the minimum subtree
     */
     
    public TreeNode findSubtree(TreeNode root) {
        ResultType result = traverse(root);
        return result.minNode;
    }
    
    private ResultType traverse(TreeNode cur) {
        
        if(cur == null) {
            return new ResultType(0, Integer.MAX_VALUE, null);
        }
        
        ResultType left = traverse(cur.left);
        ResultType right = traverse(cur.right);
        
        int sum = left.sum + right.sum + cur.val;
        
        TreeNode minNode = cur;
        int min = sum;
        
        if(left.min < right.min && left.min < sum) {
            min = left.min;
            minNode = left.minNode;
        }else if(left.min > right.min && right.min < sum){
            min = right.min;
            minNode = right.minNode;
        }
        
        return new ResultType(sum, min, minNode);
    }
}

class ResultType {
    int sum;
    int min;
    TreeNode minNode;
    
    ResultType(int sum, int min, TreeNode minNode) {
        this.sum = sum;
        this.min = min;
        this.minNode = minNode;
    }
}
```

###### [597. Subtree with Maximum Average](https://www.lintcode.com/problem/subtree-with-maximum-average/solution)

Descriptions:

Given a binary tree, find the subtree with maximum average. Return the root of the subtree.

Example 1:

```
Input：
{1,-5,11,1,2,4,-2}
Output：11
Explanation:
The tree is look like this:
     1
   /   \
 -5     11
 / \   /  \
1   2 4    -2 
The average of subtree of 11 is 4.3333, is the maximun.
```

Example 2:

```
Input：
{1,-5,11}
Output：11
Explanation:
     1
   /   \
 -5     11
The average of subtree of 1,-5,11 is 2.333,-5,11. So the subtree of 11 is the maximun.
```

Notice:

 the subtree which root is your return node.
It's guaranteed that there is only one subtree with maximum average.

Analysis:

Get all subtree's sum and its size to calculate the subtree's average. Maintain a class property of max average. This question needs to pay attention when node is null. The max average doesn't need to be calculated, instead, the leaf value is the avg. 

We shall avoid using division as much as possible.

*[6. Smallest number of integer and double](CleanCodePractice.md)*

```java
public class Solution {
    /**
     * @param root: the root of binary tree
     * @return: the root of the maximum average of subtree
     */
    
    double maxAvg;
    TreeNode maxNode;
    
    public TreeNode findSubtree2(TreeNode root) {
        maxAvg = -Double.MIN_VALUE;
        maxNode = null;
        
        traverse(root);
        
        return maxNode;
        
    }
    
    private ResultType traverse(TreeNode cur) {
        
        if(cur == null) {
            
            return new ResultType(0, 0);
        }
        
        ResultType left = traverse(cur.left);
        ResultType right = traverse(cur.right);
        
        int sum = left.sum + right.sum + cur.val;
        int count = left.count + right.count + 1;
        
        double avg = ((double) sum) / count;
        if(avg > maxAvg || maxNode == null) {
            maxAvg = avg;
            maxNode = cur;
        }
        
        return new ResultType(sum, count);
        
                
    }
    
    
}

class ResultType{
    int sum;
    int count;
    
    ResultType(int sum, int count) {
        this.sum = sum;
        this.count = count;
    }
}
```

###### [595. Binary Tree Longest Consecutive Sequence](https://www.lintcode.com/problem/binary-tree-longest-consecutive-sequence/solution)

Descriptions:

Given a binary tree, find the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from parent to child (`cannot be the reverse`).

Example 1:

```
Input:
   1
    \
     3
    / \
   2   4
        \
         5
Output:3
Explanation:
Longest consecutive sequence path is 3-4-5, so return 3.
```

Example 2:

```
Input:
   2
    \
     3
    / 
   2    
  / 
 1
Output:2
Explanation:
Longest consecutive sequence path is 2-3,not 3-2-1, so return 2.
```

Analysis:

If the root is not consecutive with neither right or left child, the length for that node will be 1. If the root is consecutive with one of them or is consecutive with both children, root length will the longer one. We will use a global variable to save the longest length for all nodes.

 

```java
public class Solution {
    /**
     * @param root: the root of binary tree
     * @return: the length of the longest consecutive sequence path
     */
     
    int longest;
    
    public int longestConsecutive(TreeNode root) {
        helper(root);
        return longest;
    }
    
    private int helper(TreeNode root) {
                
        if(root == null) {
            return 0;
        }
        
        int left = helper(root.left);
        int right = helper(root.right);
        
        int length = 1;
        
        if(root.left != null && root.val + 1 == root.left.val) {
            length = left + 1;
        }
        if(root.right != null && root.val + 1 == root.right.val) {
            length = Math.max(length, right + 1);
        }
        
        longest = Math.max(length, longest);
        
        return length;
    }
    
}
```



######  [88. Lowest Common Ancestor of a Binary Tree](https://www.lintcode.com/problem/lowest-common-ancestor-of-a-binary-tree/description)

Description:

Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.

The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.

Example 1:

```
Input：{1},1,1
Output：1
Explanation：
 For the following binary tree（only one node）:
         1
 LCA(1,1) = 1
```

Example 2:

```
Input：{4,3,7,#,#,5,6},3,5
Output：4
Explanation：
 For the following binary tree:

      4
     / \
    3   7
       / \
      5   6
			
 LCA(3, 5) = 4
```

Notice:

Assume two nodes are exist in tree.

Analysis:

The ancestor is the first node that diverse 2 nodes into left and right sub tree. **If we find a node, whose left subtree contains one target node, and right subtree contains another target node, the node is the lowest common ancestor** However, there is a special case, which is,  one of the target node is another target node's child. For this case, we shall return the first target node we found during the top-down traverse.  

We shall compare from the root to the leaf to see weather any node equal the target node (A or B). If we find any target node, we shall return it. if a node's left subtree contains a node, right subtree contains another node, the node is the lowest common ancestor. If we find target A and B are in the same subtree, then the first node we find in the top-down direction is the lowest common ancestor. 

This question is a variation of finding a node from binary search tree. Once we find a target, we will return that target directly from that node. If a node contains both targets on its left and right subtrees alone bottom-up returning direction, the node is the lowest common ancestor. We won't search the whole tree to the bottom, instead, we will return if we find any node A or B. If A or B is on the same side of tree, we need to find the topper one. Therefore, we have to return the first finding in the top-down direction, i.e., if we find a target, we shall return before we call the recursion.

```java
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode A, TreeNode B) {
        
        if(root == null) {
            return null;
        }
        
        if(root == A || root == B) {
            return root;
        }
        
        TreeNode left = lowestCommonAncestor(root.left, A, B);
        TreeNode right = lowestCommonAncestor(root.right, A, B);
        
        if(left != null && right != null) {
            return root;
        }
        
        if(left != null) {
            return left;
        }
        
        if(right != null) {
            return right;
        }
        return null;
        
        
    }
```

###### [474. Lowest Common Ancestor II](https://www.lintcode.com/problem/lowest-common-ancestor-ii/description)

Description:

Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.

The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.

The node has an extra attribute `parent` which point to the father of itself. The root's parent is null.

Example 1:

```
Input：{4,3,7,#,#,5,6},3,5
Output：4
Explanation：
     4
     / \
    3   7
       / \
      5   6
LCA(3, 5) = 4
```

Example 2:

```
Input：{4,3,7,#,#,5,6},5,6
Output：7
Explanation：
      4
     / \
    3   7
       / \
      5   6
LCA(5, 6) = 7
```



Analysis:

This question is  [88. Lowest Common Ancestor of a Binary Tree](#88. Lowest Common Ancestor of a Binary Tree) 's another solution. We can save both target nodes' parents into 2 lists. Then we find the first same parent which is the lowest common ancestor. Here we have a small trick to simplify the return condition. We can compare the 2 list from the end, and find the last same elements which are the result. If we compare from the beginning, the corner case, one node being another node's father, has to be considered as an extra.

```java
    public ParentTreeNode lowestCommonAncestorII(ParentTreeNode root, ParentTreeNode A, ParentTreeNode B) {
        
        ArrayList<ParentTreeNode> aPath = getRootPath(A);
        ArrayList<ParentTreeNode> bPath = getRootPath(B);
        
        
        int aIndex = aPath.size() - 1;
        int bIndex = bPath.size() - 1;
        
        ParentTreeNode result = null;
        
        while(aIndex >= 0 && bIndex >= 0) {
            
            if(aPath.get(aIndex) != bPath.get(bIndex)) {
                break;
            }
            
            result = aPath.get(aIndex);
            aIndex--;
            bIndex--;
        }
        
        return result;
        
    }
    
    private ArrayList<ParentTreeNode> getRootPath(ParentTreeNode node) {
        
        ArrayList<ParentTreeNode> result = new ArrayList<>();
        
        while(node != null) {
            result.add(node);
            node = node.parent;
            
        }
        
        return result;
    }
```

###### [578. Lowest Common Ancestor III](https://www.lintcode.com/problem/lowest-common-ancestor-iii/solution)

Description:

Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.
The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.
Return `null` if LCA does not exist.

Example1

```
Input: 
{4, 3, 7, #, #, 5, 6}
3 5
5 6
6 7 
5 8
Output: 
4
7
7
null
Explanation:
  4
 / \
3   7
   / \
  5   6

LCA(3, 5) = 4
LCA(5, 6) = 7
LCA(6, 7) = 7
LCA(5, 8) = null
```

Example2

```
Input:
{1}
1 1
Output: 
1
Explanation:
The tree is just a node, whose value is 1.
```

Notice

node A or node B may not exist in tree.
Each node has a different value

Analysis: This question is question [88. Lowest Common Ancestor of a Binary Tree](#88. Lowest Common Ancestor of a Binary Tree) + if we can find both nodes. Lowest Common Ancestor of a Binary Tree](#88. Lowest Common Ancestor of a Binary Tree) 's solution has been posted. Here we need to make sure whether we can find both nodes. We can use divide-conquer method. We need 2 Boolean values in the returning result to remember whether we found target A or B. If left subtree contains A, or right subtree contains A, or root == A, A exists in the tree. Same for the target B. After we can find both targets, [88. Lowest Common Ancestor of a Binary Tree](#88. Lowest Common Ancestor of a Binary Tree) 's returning is the answer. If any of them doesn't exist, we return null. The two recursion can be combined into one method. Besides we need to create a returning object, we need to pay attention the variables updating orders. Some variables has to be updated before recursion (top-down), some variables will update after recursion(bottom-up).

```java
public class Solution {
    /*
     * @param root: The root of the binary tree.
     * @param A: A TreeNode
     * @param B: A TreeNode
     * @return: Return the LCA of the two nodes.
     */
    public TreeNode lowestCommonAncestor3(TreeNode root, TreeNode A, TreeNode B) {
        
        ResultType result = helper(root, A, B);
        
        if(!result.foundA || !result.foundB) {
            return null;
        }
        
        return result.cur;
        
    }
    
    private ResultType helper(TreeNode root, TreeNode A, TreeNode B) {
        
        if(root == null) {
            return  new ResultType(false, false, root);
        }
        
        ResultType left = helper(root.left, A, B);
        ResultType right = helper(root.right, A, B);
        
        boolean foundA = left.foundA || right.foundA || root == A;
        boolean foundB = left.foundB || right.foundB || root == B;
        
        if(root == A || root == B) {
            return new ResultType(foundA, foundB, root);
        }
        if(left.cur != null && right.cur != null) {
            return new ResultType(foundA, foundB, root);
        }
        
        if(left.cur != null) {
            return new ResultType(foundA, foundB, left.cur);
        }
        if(right.cur != null) {
            return new ResultType(foundA, foundB, right.cur);
        }
        
        return new ResultType(foundA, foundB, null);
        
    }
}

class ResultType {
    boolean foundA;
    boolean foundB;
    TreeNode cur;
    
    ResultType(boolean foundA, boolean foundB, TreeNode cur) {
        this.foundA = foundA;
        this.foundB = foundB;
        this.cur = cur;
    }
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

This question is in-order traverse + only collect the numbers in the range from k1 to k2.

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

    traverse(cur.left, k1, k2, result);

    if(cur.val >= k1 && cur.val <= k2) {
        result.add(cur.val);
    }

    traverse(cur.right, k1, k2, result);

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
- The question is using In-order traversal, not like other questions using top-down or post-order. We can not build root before left subtree, neither after right subtree.

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

### Summery

###### Save the variable as input parameters' property, as class property, or returned result's property?

Variables can be saved into different ways, and sometimes, different ways can switch in between. Here I will summarize some general rules with example.

- **The variable's value, which can be calculated from top to bottom, is easier to save as input parameters.** Top-down is usually easier than bottom-up because top-down is reasoning from requirements to result. If the variable can be calculated by both top-down and bottom-up, we will choose top-down first.

  -  The root's initial value is known. Eg., in question  [106. Convert Sorted List to Binary Search Tree](#106. Convert Sorted List to Binary Search Tree) , the variable length. The root's length is known, and other children's values can be calculated in top-down directions.
  -  The root's initial value is unknown. Usually, the variable is a property of left and right sub tree. Even roots is unknown, it doesn't matter. All other children's value can be calculated in top-down directions. Eg., in question  [95. Validate Binary Search Tree](# 95. Validate Binary Search Tree) the variable min and max. We can calculate the min and max range before recursion.

- **The variable's value, which can be calculated from bottom to top and its initial value is not an input parameter, but a constant, shall be saved in returned result's property. Usually, if we need some median variables to help calculate the final result, we will save it in the returned result's property** If the variable at root's value is unknown, and it is hard to calculate its left and right child's value top-down neither. We will use bottom-up way. 

  - Eg., in question [93. Balanced Binary Tree](#93. Balanced Binary Tree), we don't know root's depth at the beginning, and we can not reasoning its left and right sub tree's depth either. Here it is better to save the depth in the returned result property.

- **Saving as class property variable is the most powerful one. It can be used with top-down, in-order and post-order traverse. Unlikely saving in the returning type (post-order) which can only be returned level by level, or saving in the input parameter (top-down) which can only be built level by level, it can be calculated in any level and in any direction (top-down or bottom up).  However, it has side effects, it is better to be avoid.**

  

  - **One of the class variable usage is, the variable can only be calculated from bottom to top and its initial value is not constant(which will get from method inputs) .**

    Since the variable is an input parameter, it has to be passed as an input parameter. However, it can only get updated in bottom-down direction, we have to save the updated variable's value in the returned result. 

    Eg., in question  [106. Convert Sorted List to Binary Search Tree](#106. Convert Sorted List to Binary Search Tree) , the variable linked list head. It is a method input parameter, and it can only be updated bottom-up. So we can save the variable both as input parameter and returned result property. Or we can save it as a class property.
