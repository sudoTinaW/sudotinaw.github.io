---
published: true
layout: post
date: '2020-10-20 14:50:00 -0000'
categories: BFS
---
Comparing to DFS, BFS is usually used to find shortest path. Since it is using while loop, the debug is easier, and has lower risk of stackoverflow.

###### BFS Usage:

- It can be used to get a shortest path between a start and an end node in a simple graph (weight = 1 and no directions).

- It can be used to traverse a graph.
- It can be used in topological sort (weight = 1 with directions).

###### BFS Template:

We can implement BFS with queues in 3 different ways. 

- We can use 2 queues to store one level of nodes and its next level of nodes. 
- We can use one queue with a marker stored in the queue to store and separate 2 levels of nodes.
- We can use one queue with a marker stored outside the queue to store and separate 2 levels of nodes . 

Here we will use one queue with a maker outside the queue. Every time, before we add any new element belonging to the next level, we will save the current level's size. When we finish visiting the size of nodes in the queue, the rest of elements in queue will be the next level's elements. 

[labuladong](https://labuladong.gitbook.io/algo/di-ling-zhang-bi-du-xi-lie/bfs-kuang-jia) has offer a template. The template is a while (loop the queue) + a for (loop level by level only when we need level's information) + a for (loop all neighbors of a current level node)

- BFS template layer's loop is not necessary all the time. If our goal is visited all nodes in a graph, we can ignore layer's for loop.
- If The graph doesn't have a cycle, like a tree, we don't need to maintain the visited set even.
- We need to mark a node as visited when it is pushed into queue rather than when it is popped from the queue. That is because if we marked as visited when it is pushed into the queue, it will be only pushed and popped once. If we marked as visited when it is popped, it can be pushed multiple times already and waste memory. Every time, when we pop a node, we will push multiple nodes. Mark visit when push will skip more duplicated push operation.
- When we need to collect a node's value, we can collect either when it is pushed or popped. However, if we collect the data when it is pushed, we need to handle the first node as the special case. However, if we collect the node's value at pop operation, we won't have any special case. What is more, as above step, we have already make sure each node will be pushed and popped from the queue once, time complexity won't be influenced.

```java
// 计算从起点 start 到终点 target 的最近距离
int bfs(Node start, Node target) {
    Queue<Node> q; // 核心数据结构
    Set<Node> visited; // 避免走回头路

    q.offer(start); // 将起点加入队列
    visited.add(start);
    int step = 0; // 记录扩散的步数

    while (q not empty) {
        int sz = q.size();
        /* 将当前队列中的所有节点向四周扩散 */
        for (int i = 0; i < sz; i++) {
            Node cur = q.poll();
            /* 划重点：这里判断是否到达终点 */
            if (cur is target)
                return step;
            /* 将 cur 的相邻节点加入队列 */
            for (Node x : cur.adj())
                if (x not in visited) {
                    q.offer(x);
                    visited.add(x);
                }
        }
        /* 划重点：更新步数在这里 */
        step++;
    }
}
```

BFS usually use queue not stack, so that the members in the same level will be visited as the original order. If we are using stack, the order of members in the same level will be in reverse.

BFS **time complexity** will be O(n) and space will be O(n). Even though, we have multiple loops while + for. The worst of case of multiple loops won't happen at the same time. What is more, if you think how many times each node will be visited, it will be O(m + n). Therefore, the time complexity is O(n), but not O(n ^2) or O(n ^3).

###### Topological Sort

Topological Sort is used to offering a sort of elements having dependencies between each other.

-  A graph can have multiple topological sort. If a graph have a topological sort, it won't have cycles. 2 of its most famous usage are course selection and compiler compiling objects with dependencies. 
-  It can be implemented by DFS and BFS. DFS needs to use the program stack which are not allowed to be very deep. Therefore, it is not recommended. DFS cares about the outcoming degree of each node. Every round of calculation will start from any node with 0 outcoming degree. 
-  BFS is more often used. BFS cares about the incoming degrees. Every round of calculation will start from any node with 0 incoming degree. 
-  Both DFS and BFS time complexity will be O(m + n).

###### Bidirectional BFS

Some BFS questions have an optimization, which is bidirectional BFS. Bidirectional BFS will spread both from start and end point until their visited points meet. 

- Bidirectional BFS time complexity is also `O(n)` in general, but if the n is big, its time complexity can decrease to nearly `O(sqrt(n))`. The picture, from [labuladong](https://labuladong.gitbook.io/algo/di-ling-zhang-bi-du-xi-lie-qing-an-shun-xu-yue-du/bfs-kuang-jiashows), shows how bidirectional BFS can save time.

![](E:\study\jiuzhang\Notes\biBFS1.jpeg)

![biBFS2](E:\study\jiuzhang\Notes\biBFS2.jpeg)

- Bidirectional BFS can only be used in the question that we both know start and end.

- Bidirectional BFS template : Create 2 queues start queue and end queue. As long as 2 queues don't intersects with each other(visited set1 doesn't contain any element from visited set2), we will continue to move the pointer current queue to the shorter queue.

  ```java
  int bfs(Node start, Node target) {
      //Since bi directional search starts from its neighbors, it doesn't consider the case start is the target. You need to consider the conner case first.
      if(start == target) {
          return 0;
      }
      Queue<Node> q1;
      Queue<Node> q2;
      
      Set<Node> visited1;
      Set<Node> visited2;
      
      QueueCur<Node> qCur;
      Set<Node> visitedCur;
      Set<Node> visitedOp;
      
  
      q1.offer(start); 
      visited1.add(start);
      q2.offer(end);
      visited2.add(end);
      
      int step = 0; 
  
      while (q1 not empty && q2 not empty) {
          //Dynamically choose the shorter queue to visit
          if(q1.size() < q2.size()) {
              qCur = q1;
              visitedCur = visited1;
              visitedOp = visited2;
          }else {
              qCur = q2;
              visitedCur = visited2;
              visitedOp = visited1;
          }
          int sz = qCur.size();
          
          for (int i = 0; i < sz; i++) {
              Node cur = q.poll();
              
              for (Node x : cur.adj()) {
                  //Quit condition: there is an element intersiting with the other side of queue
                  if (x is in visitedOp) {
                      return step;
                  }
              	if(x is not in visitedCur) {
                      qCur.offer(x);
                      visitedCur.add(x);
                  }
          }
          
          step++;
      }
  }
  ```

  

### Problems

##### Traverse a Graph:

###### [69. Binary Tree Level Order Traversal](https://www.lintcode.com/problem/binary-tree-level-order-traversal/description)

Given a binary tree, return the *level order* traversal of its nodes' values. (ie, from left to right, level by level).

Example 1:

```
Input：{1,2,3}
Output：[[1],[2,3]]
Explanation：
  1
 / \
2   3
it will be serialized {1,2,3}
level order traversal
```

Notice:

- The first data is the root node, followed by the value of the left and right son nodes, and "#" indicates that there is no child node.
- The number of nodes does not exceed 20.

Analysis:

The tree is a also a graph. The difference is tree has no cycle, that is to say, we don't need to use a hash map to save whether a node is visited. If we have visited a node, we won't met this node again in the further visit.

Here we will use BFS template without visited hash map to implement level order traversal.

[*9. Data Serialization*](CleanCodePractice.md)

```java
public List<List<Integer>> levelOrder(TreeNode root) {

    List<List<Integer>> result = new LinkedList<>();

    if(root == null) {
        return result;
    }

    Queue<TreeNode> queue = new LinkedList<>();
    queue.add(root);

    while(!queue.isEmpty()) {

        int size = queue.size();

        List<Integer> list = new LinkedList<>();
        for(int i = 0; i < size; i++) {
            TreeNode cur = queue.poll();
            
            if(cur.left != null) {
                queue.offer(cur.left);
            }
            if(cur.right != null) {
                queue.offer(cur.right);
            }

            list.add(cur.val);
        }

        result.add(list);

    }

    return result;

}
```

###### [71. Binary Tree Zigzag Level Order Traversal](https://www.lintcode.com/problem/binary-tree-zigzag-level-order-traversal/description)

Description:

Given a binary tree, return the *zigzag level order* traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

Example:

```
Input:{3,9,20,#,#,15,7}
Output:[[3],[20,9],[15,7]]
Explanation:
    3
   / \
  9  20
    /  \
   15   7
it will be serialized {3,9,20,#,#,15,7}
```

Analysis:

We will use BFS to do level order traversal. For even level we will save the result list in original order directly. For odd level, we will save list in reverse order.

```java
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        
        List<List<Integer>> result = new LinkedList<>();
        
        if(root == null) {
            return result;
        }
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        
        int level = 0;
        
        while(!queue.isEmpty()) {
            
            int size = queue.size();
            
            boolean isEven = level % 2 == 0 ? true : false;
            List<Integer> list = new LinkedList<>();
            
            for(int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                if(node.left != null) {
                    queue.offer(node.left);
                }
                if(node.right != null) {
                    queue.offer(node.right);
                }
                list.add(node.val);
            }
            
            if(isEven) {
                result.add(list);

            }else {
                Collections.reverse(list);
                result.add(list);
            }
            level++;
            
        }
        
        return result;
    }
```

###### [242. Convert Binary Tree to Linked Lists by Depth](https://www.lintcode.com/problem/convert-binary-tree-to-linked-lists-by-depth/description)

Description:

Given a binary tree, design an algorithm which creates a linked list of all the nodes at each depth (e.g., if you have a tree with depth D, you'll have D linked lists).

Example 1:

```
Input: {1,2,3,4}
Output: [1->null,2->3->null,4->null]
Explanation: 
        1
       / \
      2   3
     /
    4
```

Analysis:

BFS level order traversal with build a linked list with dummy node. Here we need to pay attention that if BFS requires level information, we need to add a for loop and poll each element inside the level for loop.

```java
public List<ListNode> binaryTreeToLists(TreeNode root) {
    List<ListNode> result = new ArrayList<>();

    if(root == null) {
        return result; 
    }

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);


    while(!queue.isEmpty()) {

        int size = queue.size();
        ListNode dummy = new ListNode(-1);
        ListNode head = dummy;

        for(int i = 0; i < size; i++) {
            TreeNode cur = queue.poll();

            head.next = new ListNode(cur.val);
            head = head.next;

            if(cur.left != null) {
                queue.offer(cur.left);
            }
            if(cur.right != null) {
                queue.offer(cur.right);
            }


        }

        result.add(dummy.next);
    }

    return result;
}
```



###### [137. Clone Graph](https://www.lintcode.com/problem/clone-graph/description)

Description:

Clone an undirected graph. Each node in the graph contains a `label` and a list of its `neighbors`. Nodes are labeled uniquely.

You need to return a deep copied graph, which has the same structure as the original graph, and any changes to the new graph will not have any effect on the original graph.

Example1

```
Input:
{1,2,4#2,1,4#4,1,2}
Output: 
{1,2,4#2,1,4#4,1,2}
Explanation:
1------2  
 \     |  
  \    |  
   \   |  
    \  |  
      4   
```

Notice:

You need return the node with the same label as the input node.

Analysis:

This question requires to find all nodes, clone all the nodes, and copy each node's list using new node. Since we need to find all nodes, we can use BFS to traverse the whole graph. What's more, we need to be able to find its cloned node through original node. Therefore, we will save all original nodes as keys in a map rather than directly save in a list. We will save each cloned node as original node' s value. We we traverse original node's linked list, we can build its cloned node's list at the same time.

This question only needs to traverse all nodes without layer's requirement, therefore, we don't for-loop and size to manage layer's information. After we find and cloned all nodes, we can loop through the hash map to build connections between all cloned nodes. We can not build the connections before we get all cloned nodes. We will only cloned node when we start visit all its neighbors. 

```java
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
        
        if(node == null) {
            return null;
        }
        
        Map<UndirectedGraphNode, UndirectedGraphNode> map = new HashMap<>();
        
        Queue<UndirectedGraphNode> queue = new LinkedList<>();
        
        queue.offer(node);
        map.put(node, new UndirectedGraphNode(node.label));
        
        while(!queue.isEmpty()) {
            
            UndirectedGraphNode temp = queue.poll();
            
            for(UndirectedGraphNode nei : temp.neighbors) {
                if(!map.containsKey(nei)) {
                    queue.offer(nei);
                    map.put(nei, new UndirectedGraphNode(nei.label));
                }
            }
        }
        
        for(UndirectedGraphNode n: map.keySet()) {
            List<UndirectedGraphNode> list = map.get(n).neighbors;
            for(UndirectedGraphNode nei : n.neighbors) {
                list.add(map.get(nei));
            }
            
        }
        
        return map.get(node);
        
        
    }
```

###### [618. Search Graph Nodes](https://www.lintcode.com/problem/search-graph-nodes/description)

Given a `undirected graph`, a `node` and a `target`, return the nearest node to given node which value of it is target, return `NULL` if you can't find.

There is a `mapping` store the nodes' values in the given parameters.

Example 1:

```
Input:
{1,2,3,4#2,1,3#3,1,2#4,1,5#5,4}
[3,4,5,50,50]
1
50
Output:
4
Explanation:
2------3  5
 \     |  | 
  \    |  |
   \   |  |
    \  |  |
      1 --4
Give a node 1, target is 50

there a hash named values which is [3,4,10,50,50], represent:
Value of node 1 is 3
Value of node 2 is 4
Value of node 3 is 10
Value of node 4 is 50
Value of node 5 is 50

Return node 4
```

Notice: It's guaranteed there is only one available solution.

Analysis:

This question is giving a starting node, find the nearest the node with target value. It is necessary to traverse the whole graph, but it doesn't need the layer info either. Because BFS is naturally starting from the closest to furthest. So we don't need the layer for loop and the size. Since the graph might have cycle, we need to maintain a visited node hash table.

```java
public UndirectedGraphNode searchNode(Map<UndirectedGraphNode, Integer> values,
                                      UndirectedGraphNode node,
                                      int target) {


    Queue<UndirectedGraphNode> queue = new LinkedList<>();
    Set<UndirectedGraphNode> set = new HashSet<>();
    queue.add(node);
    set.add(node);

    UndirectedGraphNode result = null;

    while(result == null && !queue.isEmpty()) {

        UndirectedGraphNode cur = queue.poll();

        if(values.get(cur) == target) {
            result = cur;
            break;
        }

        for(UndirectedGraphNode nei : cur.neighbors) {
            if(!set.contains(nei)) {
                queue.offer(nei);
                set.add(nei);
            }
        }
    } 

    return result;

}
```

######  [431. Connected Component in Undirected Graph](https://www.lintcode.com/problem/connected-component-in-undirected-graph/description)

Find connected component in undirected graph.

Each node in the graph contains a label and a list of its neighbors.

(A connected component of an undirected graph is a subgraph in which any two vertices are connected to each other by paths, and which is connected to no additional vertices in the super graph.)

You need return a list of label set.

 Nodes in a connected component should sort by label in ascending order. Different connected components can be in any order.

Example 1:

```
Input: {1,2,4#2,1,4#3,5#4,1,2#5,3}
Output: [[1,2,4],[3,5]]
Explanation:

  1------2  3
   \     |  | 
    \    |  |
     \   |  |
      \  |  |
        4   5
```

Analysis:

BFS can find a node's all connected nodes. We don't need layer info, thus, we will ignore the layer's loop in the template. We need to collect all the connected nodes and mark them as visited. The next unvisited node will be in another connected component. We will check all nodes in the list and manage a visited set to tell whether it belongs to a connected component.

```java
public List<List<Integer>> connectedSet(List<UndirectedGraphNode> nodes) {
    List<List<Integer>> result = new LinkedList<>();
    
    Set<UndirectedGraphNode> set = new HashSet<>();
    
    for(UndirectedGraphNode n : nodes) {
        
        if(set.contains(n)) {
           continue; 
        }
        
        Queue<UndirectedGraphNode> queue = new LinkedList<>();
        List<Integer> list = new LinkedList<>();
        
        queue.offer(n);
        set.add(n);
        
        while(!queue.isEmpty()) {
            
            UndirectedGraphNode cur = queue.poll();
            list.add(cur.label);
            
            for(UndirectedGraphNode nei : cur.neighbors) {
                if(!set.contains(nei)) {
                    queue.offer(nei);
                    set.add(nei);
                }
            }
        }
        Collections.sort(list);
        result.add(list);
        
    }
    
    return result;
}
```

###### 178. Graph Valid Tree

Given `n` nodes labeled from `0` to `n - 1` and a list of `undirected` edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

Example 1:

```
Input: n = 5 edges = [[0, 1], [0, 2], [0, 3], [1, 4]]
Output: true.
```

Example 2:

```
Input: n = 5 edges = [[0, 1], [1, 2], [2, 3], [1, 3], [1, 4]]
Output: false.
```

You can assume that no duplicate edges will appear in edges. Since all edges are `undirected`, `[0, 1]` is the same as `[1, 0]` and thus will not appear together in edges.

Analysis:

To tell whether a graph is a tree, we need to know whether all nodes are connected without cycles. If all nodes are connected and no cycles, the edges must be n - 1. If the edges < n - 1, it means there must some vertices are not connected. If the edges > n - 1, it means there must be cycles that make some vertices are connected by more than once.

If the edges == n - 1. We still don't know whether it is the situation that some vertices are connected while some vertices have cycle, or the situation that all nodes are connected and only connected once. To see whether all nodes are connected, we can traverse the graph starting from any node and see whether we can visit all nodes from it.

This question only give the edges between any two nodes, if we want to traverse a graph using BFS, we have to get all its neighbors at once. Therefore, we need to transform the edge to a graph (a node : all its neighbors) structure. After that we will mark all visited nodes and see whether after traverse we can visit all nodes.

```java
public boolean validTree(int n, int[][] edges) {

    if(edges.length != n - 1) {
        return false;
    }

    if(edges == null || edges.length == 0 || edges[0].length == 0) {
        return true;    
    }

    Map<Integer, List<Integer>> map = new HashMap<>();

    for(int[] pair : edges) {
        List<Integer> l1 = map.getOrDefault(pair[0], new ArrayList<Integer>());
        l1.add(pair[1]);
        map.put(pair[0], l1);
        List<Integer> l2 = map.getOrDefault(pair[1], new ArrayList<Integer>());
        l2.add(pair[0]);
        map.put(pair[1], l2);
    }

    Queue<Integer> queue = new LinkedList<>();
    Set<Integer> set = new HashSet<>();

    queue.offer(edges[0][0]);
    set.add(edges[0][0]);

    while(!queue.isEmpty()) {

        Integer cur = queue.poll();

        List<Integer> neighbors = map.get(cur);

        for(int nei : neighbors) {

            if(!set.contains(nei)) {
                queue.offer(nei);
                set.add(nei);
            }
        }
    }

    return set.size() == n;



}
```

######  [127. Topological Sorting](https://www.lintcode.com/problem/topological-sorting/description)

Decription:

Given an directed graph, a topological order of the graph nodes is defined as follow:

- For each directed edge `A -> B` in graph, A must before B in the order list.
- The first node in the order can be any node in the graph with no nodes direct to it.

Find any topological order for the given graph. You can assume that there is at least one topological order in the graph.

Example:

For graph as follow:

![图片](https://media-cdn.jiuzhang.com/markdown/images/8/6/91cf07d2-b7ea-11e9-bb77-0242ac110002.jpg)

The topological order can be:

```
[0, 1, 2, 3, 4, 5]
[0, 2, 3, 1, 5, 4]
...
```

Analysis:

First we need to collect all node's indegree. We can choose a data structure to store this information. Here we will use a map (node : indegree) to save each node's indegree information. We will loop through all nodes in the graph list, if it has a neighbor, the neighbor's indegree will add 1. After we look through all node's neighbor, the node with 0 indegree, which is not any node's neighbor will not be in the map.

After we get all node's indegree map, we can push first batch of nodes which are not in the map onto the queue. Each time, we will pop one node out and add it onto the final result. When we visit this node's neighbors, we will decrease its neighbor's indegree by 1. What is more, we will keep pushing the unvisited node with updated indegree as 0 onto the queue until there is not any unvisited node.

```java
public ArrayList<DirectedGraphNode> topSort(ArrayList<DirectedGraphNode> graph) {

    ArrayList<DirectedGraphNode> result = new ArrayList<>();

    if(graph == null || graph.size() == 0) {
        return result;
    }

    Map<DirectedGraphNode, Integer> map = new HashMap<>();

    for(DirectedGraphNode cur : graph) {
        for(DirectedGraphNode nei : cur.neighbors) {
            int indegree = map.getOrDefault(nei, 0);
            map.put(nei, indegree + 1);
        }
    }

    Queue<DirectedGraphNode> queue = new LinkedList<>();
    Set<DirectedGraphNode> set = new HashSet<>();

    for(DirectedGraphNode node : graph) {
        if(!map.containsKey(node)) {

            queue.offer(node);
            set.add(node);
        }
    }

    while(! queue.isEmpty()) {
        DirectedGraphNode cur = queue.poll();
        result.add(cur);

        for(DirectedGraphNode nei : cur.neighbors) {

            int indegree = map.get(nei);
            map.put(nei, indegree - 1);

            if(map.get(nei) == 0 && !set.contains(nei)) {
                queue.offer(nei);
                set.add(nei);
            }
        }
    }

    return result;
}
```

##### Shortest Path

###### [814. Shortest Path in Undirected Graph](https://www.lintcode.com/problem/shortest-path-in-undirected-graph/solution)

Description:

Given an undirected graph in which each edge's length is 1, and two nodes from the graph. Return the length of the shortest path between two given nodes.

Example 1:

```
Input: graph = {1,2,4#2,1,4#3,5#4,1,2#5,3}, node1 = 3, node2 = 5
Output: 1
Explanation:
  1------2  3
   \     |  | 
    \    |  |
     \   |  |
      \  |  |
        4   5https://www.lintcode.com/help/graph/)
```

Analysis: BFS with level for loop template + maintain a level info.

```java
public int shortestPath(List<UndirectedGraphNode> graph, UndirectedGraphNode A, UndirectedGraphNode B) {

    Queue<UndirectedGraphNode> queue = new LinkedList<>();
    Set<UndirectedGraphNode> set = new HashSet<>();

    queue.offer(A);
    set.add(A);

    int length = 0;

    while(!queue.isEmpty()) {

        int size = queue.size();
        for(int i = 0; i < size; i++) {
            UndirectedGraphNode cur = queue.poll();

            if(cur == B) {
                return length;
            }

            for(UndirectedGraphNode nei : cur.neighbors) {

                if(!set.contains(nei)) {
                    queue.offer(nei);
                }
            }
        }
        length++;
    }

    return -1;

}
```

BFS Bi directional Solution:

```java
    public int shortestPath(List<UndirectedGraphNode> graph, UndirectedGraphNode A, UndirectedGraphNode B) {
        if(A == B) {
            return 0;
        }
        
        Queue<UndirectedGraphNode> q1 = new LinkedList<>();
        Queue<UndirectedGraphNode> q2 = new LinkedList<>();
        
        Set<UndirectedGraphNode> set1 = new HashSet<>();
        Set<UndirectedGraphNode> set2 = new HashSet<>();
        
        Queue<UndirectedGraphNode> qCur = null;
        Set<UndirectedGraphNode> setCur = null;
        Set<UndirectedGraphNode> setOp = null;
        
        q1.offer(A);
        set1.add(A);
        
        q2.offer(B);
        set2.add(B);
        
        int step = 0;
        
        while(!q1.isEmpty() && !q2.isEmpty()) {

            if(q1.size() <= q2.size()) {
                qCur = q1;
                setCur = set1;
                setOp = set2;
            } else {
                qCur = q2;
                setCur = set2;
                setOp = set1;
            }    
            
            step++;
            System.out.println(step);

            int size = qCur.size();
            for(int i = 0; i < size; i++) {
                System.out.println(step);

                UndirectedGraphNode cur = qCur.poll();
                System.out.println(cur.label + ",");
                for(UndirectedGraphNode nei : cur.neighbors) {
                    System.out.println(nei.label);
                    if(setOp.contains(nei)) {
                        return step;
                    }
                    
                    if(!setCur.contains(nei)) {
                        qCur.offer(nei);
                        setCur.add(nei);
                    }
                }
                
            }


        }
        
        return -1;
    }
```



###### [120. Word Ladder](https://www.lintcode.com/problem/word-ladder/my-submissions)

Description:

Given two words (*start* and *end*), and a dictionary, find the shortest transformation sequence from *start* to *end*, output the length of the sequence.
Transformation rule such that:

1. Only one letter can be changed at a time
2. Each intermediate word must exist in the dictionary. (Start and end words do not need to appear in the dictionary)

Example 1:

```
Input：start = "a"，end = "c"，dict =["a","b","c"]
Output：2
Explanation：
"a"->"c"
```

Example 2:

```
Input：start ="hit"，end = "cog"，dict =["hot","dot","dog","lot","log"]
Output：5
Explanation：
"hit"->"hot"->"dot"->"dog"->"cog"
```

Notice:

- Return 0 if there is no such transformation sequence.
- All words have the same length.
- All words contain only lowercase alphabetic characters.
- You may assume no duplicates in the word list.
- You may assume begin word and end word are non-empty and are not the same.

Analysis:

This is a tree problem. As we only care about the shortest transformation. We will use BFS to traverse level by level. The first level we met is the shortest. Here is the decision tree.

![](E:\study\jiuzhang\Notes\wordLadder.jpg)

We will replace the start word each position with a to z 26 characters. If the word is in the dictionary, we will collect it and add the length by 1. Then next level, we will repeat the same operations for all word in this level until we find the end word or traverse all possible combinations without found end.

One thing needs to pay attention, we need to add the end word into the dictionary. If the end word is not in the dictionary, when we find next, we will find an empty list, and we will not be able to find the end word.

```java
    public int ladderLength(String start, String end, Set<String> dict) {
        
        if(dict == null) {
            return 0;
        }
        
        dict.add(end);
        
        Queue<String> queue = new LinkedList<>();
        Set<String> set = new HashSet<>();
        
        queue.offer(start);
        set.add(start);
       
       int length = 1;
        
        while(!queue.isEmpty()) {
            
            int size = queue.size();
            length++;
            
            for(int i = 0; i < size; i++) {
                String cur = queue.poll();
                
                List<String> list = getNext(cur, dict);

                for(String s : list) {
                    if(s.equals(end)) {
                        return length;
                    }
                    
                    if(!set.contains(s)) {
                        queue.offer(s);
                        set.add(s);
                    }
                }          
            }
        }
        
        return 0;
        
        
    }
    
    private List<String> getNext(String str, Set<String> dict) {
        List<String> result = new ArrayList<>();
        
        char[] chars = str.toCharArray();
        
        for(int i = 0; i < chars.length; i++) {
            
            char original = chars[i];
            for(char c = 'a'; c <= 'z'; c++) {
                chars[i] = c;
                String newString = new String(chars);
                if(dict.contains(newString)) {
                    result.add(newString);
                }
            }
            chars[i]  original;

        }
        
        return result;
    }
```



###### [796. Open the Lock](https://www.lintcode.com/problem/open-the-lock/solution)

Description:

You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots: `'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'`. The wheels can rotate freely and wrap around: for example we can turn `'9'` to be `'0'`, or `'0'` to be `'9'`. Each move consists of turning one wheel one slot.

The lock initially starts at `'0000'`, a string representing the state of the 4 wheels.

You are given a list of `deadends` dead ends, meaning if the lock displays any of these codes, the wheels of the lock will stop turning and you will be unable to open it.

Given a `target` representing the value of the wheels that will unlock the lock, return the minimum total number of turns required to open the lock, or -1 if it is impossible.

Example 1:

```
Given deadends = ["0201","0101","0102","1212","2002"], target = "0202"
Return 6

Explanation:
A sequence of valid moves would be "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202".
Note that a sequence like "0000" -> "0001" -> "0002" -> "0102" -> "0202" would be invalid,
because the wheels of the lock become stuck after the display becomes the dead end "0102".
```

Notice:

1.The length of `deadends` will be in the range `[1, 500]`.
2.`target` will not be in the list `deadends`.
3.Every string in `deadends` and the string `target` will be a string of 4 digits from the 10,000 possibilities '0000' to '9999'.

Analysis:

We can consider one turn (either up or down) as one level of a decision tree. Therefore, we can use BFS to find the shortest path.

Here is the decision tree,

![openlock](E:\study\jiuzhang\Notes\openlock.jpg)

Every new level, you can change any digit of 4 digit, as long as long it is not in the upper level and it is not in the dead end set, you can put it into the new level. 

```java
    public int openLock(String[] deadends, String target) {
    
        Set<String> deadSet = new HashSet<>();
        for(String de : deadends) {
            deadSet.add(de);
        }
        if(deadSet.contains("0000")) {
            return -1;
        }
        
        Queue<String> queue = new LinkedList<>();
        Set<String> set = new HashSet<>();
        
        queue.offer("0000");
        set.add("0000");
        
        int times = 0;
        
        while(!queue.isEmpty()) {
            
            int size = queue.size();

            for(int i = 0; i < size; i++) {
                String s = queue.poll();
                if(s.equals(target)) {
                    return times;
                }
                List<String> list = getNext(s, deadSet);
                for(String next : list) {
                    if(!set.contains(next)) {
                        queue.offer(next);
                        set.add(next);
                    }
                }
            }
            times++;
            
        }
        
        return -1;
        
    }
    
    private List<String> getNext(String s, Set<String> deadends) {
        List<String> result = new ArrayList<>();
        char[] chars = s.toCharArray();
        
        for(int i = 0; i < 4; i++) {
            
            char original = chars[i];
            chars[i] = (char)((original - '0' + 10 - 1) % 10 + '0');
            String minus = new String(chars);
            chars[i] = (char)((original - '0' + 1) % 10 + '0');
            String plus = new String(chars);
            
            if(!deadends.contains(minus)) {
                result.add(minus);
            }
            if(!deadends.contains(plus)) {
                result.add(plus);
            }
            chars[i] = original;
        }
        
        return result;
    }
```

###### [794. Sliding Puzzle II](https://www.lintcode.com/problem/sliding-puzzle-ii/description)

Descriptions:

On a 3x3 board, there are 8 tiles represented by the integers 1 through 8, and an empty square represented by 0.

A move consists of choosing 0 and a 4-directionally adjacent number and swapping it.

Given an initial state of the puzzle board and final state, return the least number of moves required so that the initial state to final state.

If it is impossible to move from initial state to final state, return -1.

Example 1:

```
Input:
[
 [2,8,3],
 [1,0,4],
 [7,6,5]
]
[
 [1,2,3],
 [8,0,4],
 [7,6,5]
]
Output:
4

Explanation:
[                 [
 [2,8,3],          [2,0,3],
 [1,0,4],   -->    [1,8,4],
 [7,6,5]           [7,6,5]
]                 ]

[                 [
 [2,0,3],          [0,2,3],
 [1,8,4],   -->    [1,8,4],
 [7,6,5]           [7,6,5]
]                 ]

[                 [
 [0,2,3],          [1,2,3],
 [1,8,4],   -->    [0,8,4],
 [7,6,5]           [7,6,5]
]                 ]

[                 [
 [1,2,3],          [1,2,3],
 [0,8,4],   -->    [8,0,4],
 [7,6,5]           [7,6,5]
]                 ]
```

Analysis:

This question is actually using BFS to find the shortest path of a decision tree. Each node may have 4 directions change at most.  As long as all elements in the matrix are in same order. We can stop search. Therefore, in every compare, we need to compare the whole matrix with the final matrix. Here we will serialize the matrix into a string for easy comparation. However, we still need to know the '0' (x, y) index to calculate next level's matrix. *Here we will use index / (number of rows) to get x, (index % number of rows) to get y.*

Here is the decision tree.

![slidingPuzzleII](E:\study\jiuzhang\Notes\slidingPuzzleII.jpg)

```java
    public int minMoveStep(int[][] init_state, int[][] final_state) {
    
        Queue<String> queue = new LinkedList<>();
        Set<String> set = new HashSet<>();
        
        StringBuilder sb1 = new StringBuilder();

        for(int i = 0; i < init_state.length; i++) {
            for(int j = 0; j < init_state[0].length; j++) {
                sb1.append(init_state[i][j]);

            }

        }
        
        StringBuilder sb2 = new StringBuilder();
        for(int i = 0; i < final_state.length; i++) {
            for(int j = 0; j < final_state[0].length; j++) {
                sb2.append(final_state[i][j]);
            }
        }
        
        String initString = sb1.toString();
        String finalString = sb2.toString();

        
        queue.add(initString);
        set.add(initString);
        
        int moves = 0;
        
        int[] dx = {1, -1, 0, 0};
        int[] dy = {0, 0, 1, -1};
        
        while(!queue.isEmpty()) {
            int size = queue.size();
            for(int i = 0; i < size; i++) {
                
                String str = queue.poll();
                int index = str.indexOf('0');
                int x = index / 3;
                int y = index % 3;
                
                if(str.equals(finalString)) {
                    return moves;
                }

                for(int j = 0; j < 4; j++) {
                    int newX = x + dx[j];
                    int newY = y + dy[j];
                    
                    if(newX < 0 || newX >= 3 || newY < 0 || newY >= 3) {
                        continue;
                    }
                    
                    int newIndex = newX * 3 + newY;
                    String newStr = swap(str, index, newIndex);
                    if(!set.contains(newStr)) {
                        queue.offer(newStr);
                        set.add(newStr);
                    }
                    
                }
            }
            moves++;
            
        }
        
        
        return -1;
        
    }
    
    private String swap(String str, int index, int newIndex) {
        
        char[] chars = str.toCharArray();
        char temp = chars[newIndex];
        chars[newIndex] = '0';
        chars[index] = temp;
        
        return new String(chars);
    }
```

##### BFS in Matrix

A graph can be represented in 2 ways. 

- Map: (node, list of direct neighbors)
- 0 and 1Matrix: If any 2 nodes are connected, the value of indexes with 2 nodes will be 1, otherwise, it will be 0.

###### [433. Number of Islands](https://www.lintcode.com/problem/number-of-islands/solution)

Description:

Given a Boolean2D matrix, `0` is represented as the sea, `1` is represented as the island. If two 1 is adjacent, we consider them in the same island. We only consider up/down/left/right adjacent.

Find the number of islands.

Example 1:

```
Input:
[
  [1,1,0,0,0],
  [0,1,0,0,1],
  [0,0,0,1,1],
  [0,0,0,0,0],
  [0,0,0,0,1]
]
Output:
3
```

Analysis:

Traverse all nodes in the matrix. If the node starts with 1 and not visited, we will use BFS to visited all its connected elements. Every new turn of a BFS is a new connected component. We can collect all connected component after we visit all elements in matrix. ***Here we use 2 queues and one Boolean array to do BFS traverse in matrix***. 

```java
    public int numIslands(boolean[][] grid) {
       
        if(grid == null || grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        
        int rows = grid.length;
        int cols = grid[0].length;
        
        Queue<Integer> queueX = new LinkedList<>();
        Queue<Integer> queueY = new LinkedList<>();
        boolean[][] visited = new boolean[rows][cols];
        
        int[] dx = {0, 0, 1, -1};
        int[] dy = {1, -1, 0, 0};
        
        int result = 0;
        
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {

                if(!visited[i][j] && grid[i][j]) {
                    result++;
                    
                    queueX.offer(i);
                    queueY.offer(j);
                    visited[i][j] = true;
                    
                    while(!queueX.isEmpty() && !queueY.isEmpty()) {
                        
                        int x = queueX.poll();
                        int y = queueY.poll();
                        
                        for(int k = 0; k < 4; k++) {
                            
                            int newX = x + dx[k];
                            int newY = y + dy[k];
                            
                            if(newX >= 0 && newX < rows && newY >= 0 && newY < cols && !visited[newX][newY] && grid[newX][newY]) {
                                queueX.offer(newX);
                                queueY.offer(newY);
                                visited[newX][newY] = true;
                            }
                        }
                        
                    }
                }        
            }
        }
        
        return result;
    }
```

###### [611. Knight Shortest Path](https://www.lintcode.com/problem/knight-shortest-path/description)

Description: 

Given a knight in a chessboard (a binary matrix with `0` as empty and `1` as barrier) with a `source` position, find the shortest path to a `destination` position, return the length of the route.
Return `-1` if destination cannot be reached

Example 1:

```
Input:
[[0,0,0],
 [0,0,0],
 [0,0,0]]
source = [2, 0] destination = [2, 2] 
Output: 2
Explanation:
[2,0]->[0,1]->[2,2]
```

Example 2:

```
Input:
[[0,1,0],
 [0,0,1],
 [0,0,0]]
source = [2, 0] destination = [2, 2] 
Output:-1
```

Clarification

If the knight is at (*x*, *y*), he can get to the following positions in one step:

```
(x + 1, y + 2)
(x + 1, y - 2)
(x - 1, y + 2)
(x - 1, y - 2)
(x + 2, y + 1)
(x + 2, y - 1)
(x - 2, y + 1)
(x - 2, y - 1)
```

Notice:

source and destination must be empty.
Knight can not enter the barrier.
Path length refers to the number of steps the knight takes.

Analysis:

This question is still a matrix BFS. We have a start point and a destination point. We want to find the shortest path. The only variation is how to transform from one level to the next level. We can see from the clarification. The next level is not 4 directions neighbors but 8 diagonal directions. What is more, this question requires to find the shortest path, we need BFS template with level loop. As well, this question has given a point object, 1 queue is enough. However, we will still use 2D array to maintain  the visited set because by such way, we can get any element visited info in O(1).



```java
/**
 * Definition for a point.
 * class Point {
 *     int x;
 *     int y;
 *     Point() { x = 0; y = 0; }
 *     Point(int a, int b) { x = a; y = b; }
 * }
 */

public class Solution {
    /**
     * @param grid: a chessboard included 0 (false) and 1 (true)
     * @param source: a point
     * @param destination: a point
     * @return: the shortest path 
     */
    public int shortestPath(boolean[][] grid, Point source, Point destination) {
        
        if(grid == null || grid.length == 0 || grid[0].length == 0) {
            return -1;
        }
        int rows = grid.length;
        int cols = grid[0].length;
        
        Queue<Point> queue = new LinkedList<>();
        boolean[][] visited = new boolean[rows][cols];
        
        int[] dx = {1, 1, -1, -1, 2, 2, -2, -2};
        int[] dy = {2, -2, 2, -2, 1, -1, 1, -1};
        
        queue.offer(source);
        visited[source.x][source.y] = true;
        
        int steps = 0;
        
        while(!queue.isEmpty()) {
            
            int size = queue.size();
            
            for(int i = 0; i < size; i++) {
                
                Point cur = queue.poll();
                if(cur.x == destination.x && cur.y == destination.y) {
                    return steps;
                }
                
                for(int j = 0; j < 8; j++) {
                    
                    int newX = cur.x + dx[j];
                    int newY = cur.y + dy[j];
                    
                    if(newX >= 0 && newX < rows && newY >= 0 && newY < cols && !visited[newX][newY] && !grid[newX][newY]) {
                        queue.offer(new Point(newX, newY));
                        visited[newX][newY] = true;
                    }
                }
            }
            steps++;
        }
        
        return -1;
        
    }
}
```

###### [630. Knight Shortest Path II](https://www.lintcode.com/problem/knight-shortest-path-ii/description)

Descriptions:

Given a knight in a chessboard `n * m` (a binary matrix with 0 as empty and 1 as barrier). the knight initialze position is `(0, 0)` and he wants to reach position `(n - 1, m - 1)`, Knight can only be from left to right. Find the shortest path to the destination position, return the length of the route. Return `-1` if knight can not reached.

Example 1:

```
Input:
[[0,0,0,0],[0,0,0,0],[0,0,0,0]]
Output:
3
Explanation:
[0,0]->[2,1]->[0,2]->[2,3]
```

Clarification

If the knight is at (x, y), he can get to the following positions in one step:

```
(x + 1, y + 2)
(x - 1, y + 2)
(x + 2, y + 1)
(x - 2, y + 1)
```

Analysis:

It is exactly same logic as [611. Knight Shortest Path](#611. Knight Shortest Path). The source sets to (0, 0). The destination sets to (rows - 1, cols - 1). The jump changes from 8 possibilities to 4 possibilities.

```java
    public int shortestPath2(boolean[][] grid) {
        
        if(grid == null || grid.length == 0 || grid[0].length == 0) {
            return -1;
        }
        
        if(grid[0][0] == true) {
            return -1;
        }
        
        int rows = grid.length;
        int cols = grid[0].length;
        
        Queue<Integer> queueX = new LinkedList<>();
        Queue<Integer> queueY = new LinkedList<>();
        boolean[][] visited = new boolean[rows][cols];
        
        queueX.offer(0);
        queueY.offer(0);
        visited[0][0] = true;
        
        int step = 0;
        int[] dx = {1, -1, 2, -2};
        int[] dy = {2, 2, 1, 1};
        
        while(!queueX.isEmpty() && !queueY.isEmpty()) {
            
            int size = queueX.size();
            
            for(int i = 0; i < size; i++) {
                
                int x = queueX.poll();
                int y = queueY.poll();
                
                if(x == rows - 1 && y == cols - 1) {
                    return step;
                }
                
                for(int j = 0; j < 4; j++) {
                    int newX = x + dx[j];
                    int newY = y + dy[j];
                    
                    if(newX >= 0 && newX < rows && newY >= 0 && newY < cols && !visited[newX][newY] && !grid[newX][newY]) {
                        
                        queueX.offer(newX);
                        queueY.offer(newY);
                        visited[newX][newY] = true;
                    }
                }
            
            }
            step++;
        }
        
        return -1;

    }
```



###### [598. Zombie in Matrix](https://www.lintcode.com/problem/zombie-in-matrix/description)

Description:

Given a 2D grid, each cell is either a wall `2`, a zombie `1` or people `0` (the number zero, one, two).Zombies can turn the nearest people(up/down/left/right) into zombies every day, but can not through wall. How long will it take to turn all people into zombies? Return `-1` if can not turn all people into zombies.

Example 1:

```
Input:
[[0,1,2,0,0],
 [1,0,0,2,1],
 [0,1,0,0,0]]
Output:
2
```

Analysis:

This question is asking how many levels we can traverse the whole graph with. Traverse a graph by level, we can use BFS.  There is a special condition. There are walls in the graph. If we see a wall, we can not visit that node and its neighbors. After we visit all possible nodes, we can see whether the graph only contains zombies and walls. If not, it means there exits at least a people that can not be visited. We will return -1. Otherwise, zombies + walls shall equal to the total elements in the matrix. Since we need to get the number of levels, we need to use BFS template with level loop.



```java
    public int zombie(int[][] grid) {
        
        if(grid == null || grid.length == 0 || grid[0].length == 0) {
            return -1;
        }
        int rows = grid.length;
        int cols = grid[0].length;
        
        Queue<Integer> queueX = new LinkedList<>();
        Queue<Integer> queueY = new LinkedList<>();
        
        boolean[][] visited = new boolean[rows][cols];
        
        int walls = 0;
        int zombies = 0;
        
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(grid[i][j] == 1) {
                    queueX.offer(i);
                    queueY.offer(j);
                    visited[i][j] = true;
                    zombies++;
                } else if(grid[i][j] == 2) {
                    walls++;
                }
                
            }
        }
        
        int days = 0;
        int[] dx = {0, 0, 1, -1};
        int[] dy = {1, -1, 0, 0};
        
        while(!queueX.isEmpty() && !queueY.isEmpty()) {
            
            int size = queueX.size();
            
            for(int i = 0; i < size; i++) {
                int x = queueX.poll();
                int y = queueY.poll();
                
                for(int j = 0; j < 4; j++) {
                    int newX = x + dx[j];
                    int newY = y + dy[j];
                    
                    if(newX >= 0 && newX < rows && newY >= 0 && newY < cols && !visited[newX][newY] && grid[newX][newY] == 0) {
                        queueX.offer(newX);
                        queueY.offer(newY);
                        visited[newX][newY] = true;
                        zombies++;
                    }
                }
            }
            days++;
            
        }
        
        System.out.println(walls + zombies);
        
        return (walls + zombies) == rows * cols ? days - 1 : -1;
```
