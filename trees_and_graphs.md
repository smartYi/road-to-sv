二叉树是每个节点最多有两个子树的树结构，子树有左右之分，二叉树常被用于实现二叉查找树和二叉堆。

二叉树的第i层至多有 `2^(i-1)` 个结点；深度为k的二叉树至多有 `2^k - 1` 个结点；对任何一棵二叉树T，如果其终端结点数为 n0, 度为2的结点数为 n2, 则 `n0 = n2 + 1`

一棵深度为 k, 且有 `2^(k−1)` 个节点称之为满二叉树；深度为 k，有 n 个节点的二叉树，当且仅当其每一个节点都与深度为 k 的满二叉树中，序号为 1 至 n 的节点对应时，称之为完全二叉树。完全二叉树中重在节点标号对应

# 树的遍历

+ 三个主要步骤
    + 对当前节点进行操作
    + 遍历左边子节点
    + 遍历右边子节点

顺序不同形成了不同的遍历方式，通常用递归的方式进行理解和实现

**深度优先**

先访问子节点，再访问父节点，根据顺序不同有三种方式

1. 前序 pre-order: 先根后左再右
2. 中序 in-order: 先左后根再右
3. 后序 post-order: 先左后右再根

**广度优先**

先访问根节点，沿着树的宽度遍历子节点，直到所有节点均被访问为止


## Binary Search Tree 二叉查找树

一颗二叉查找树(BST)是一颗二叉树，其中每个节点都含有一个可进行比较的键及相应的值，且每个节点的键都大于等于左子树中的任意节点的键，而小于右子树中的任意节点的键。

使用中序遍历可得到有序数组，这是二叉查找树的又一个重要特征。

二叉查找树使用的每个节点含有两个链接，它是将链表插入的灵活性和有序数组查找的高效性结合起来的高效符号表实现。

二叉树中每个节点只能有一个父节点(根节点无父节点)，只有左右两个链接，分别为左子节点和右子节点。

---

Tree and graph questions are rife with ambiguous details and incorrect assumptions. Be sure to watch out for the following issues and seek clarification when necessary

+ Trees vs. Binary Trees
+ Binary Tree vs. Binary Search Tree
	+ all left descendants <= n < all right descendants
	+ the definition of a binary search tree can vary slightly with respect to quality. Under some definitions, the tree cannot have duplicate values. In others, the duplicate values will be on the right or can be on either side. All are valid definitions, but you should clarify this with your interviewer.
+ Balanced vs. Unbalanced
+ Complete Binary Trees
	+ A complete binary tree is a binary tree in which every level of the tree is fully filled, except for perhaps the last level. To the extent that the last level is filled, it is filled left to right.
+ Full Binary Trees
	+ A full binary tree is a binary tree in which every node has either zero or two children. That is, no nodes have only one child.
+ Perfect Binary Trees
	+ A perfect binary tree is one that is both full and complete. All leaf nodes will be at the same level, and this level has the maximum number of nodes.

# Binary Tree Traversal

**In-Order Traversal**

left, current, right

	void inOrderTraversal(TreeNode node){
	    if (node != null){
	        inOrderTraversal(node.left);
	        visit(node);
	        inOrderTraversal(node.right);
	    }
	}

**Pre-Order Traversal**

current, left, right

	void preOrderTraversal(TreeNode node){
	    if (node != null){
	        visit(node);
	        preOrderTraversal(node.left);
	        preOrderTraversal(node.right);
	    }
	}

**Post-Order Traversal**

left, right, current

	void postOrderTraversal(TreeNode node){
	    if (node != null){
	        postOrderTraversal(node.left);
	        postOrderTraversal(node.right);
	        visit(node);
	    }
	}

# Binary Heaps (Min-Heaps and Max-Heaps)

A min-heap is a complete binary tree (that is, totally filled other than the right most elements on the last level) where each node is smaller than its children. The root, therefore, is the minimum element in the tree.

# Tries(Prefix Trees)

A trie is a variant of an n-ary tree in which characters are stored at each node. Each path down the tree may represent a word.

The * nodes are often used to indicate complete words.

Very commonly, a trie is used to store the entire (English) language for quick prefix lookups. While a hash table can quickly look up whether a string is a valid word, it cannot tell us if a string is a prefix of any valid words.

# Graphs

+ Graphs can be either directed or undirected. While directed edges are like a one-way street, undirected edges are like a two-way street.
+ The graph might consist of multiple isolated subgraphs. If there is a path between every every pair of vertices, it is called a "connected graph"
+ The graph can also have cycles. An "acyclic graph" is one without cycles

**Adjacency List**

Most common way to represent a graph. Every vertex stores a list of adjacent vertices. In an undirected graph, an edge like (a,b) would be stored twice

	class Graph{
	    public Node[] nodes;
	}

	class Node {
	    public String name;
	    public Node[] children;
	}

**Adjacency Matrices**

an NxN boolean matrix (where N is the number of nodes), where a true value at `matrix[i][j]` indicates an edge from node i to node j.

# Graph Search

DFS & BFS

**DFS Depth-First Search**

Note that pre-order and other forms of tree traversal are a form of DFS. The key difference is that when implementing this algorithm for a graph, we must check if the node has been visited. If we don't, we risk getting stuck in an infinite loop

	void search(Node root){
	    if (root == null) return;
	    visit(root);
	    root.visited = true;
	    for each (Node n in root.adjacent){
	        if (n.visited == false){
	            search(n);
	        }
	    }
	}

**BFS Breadth-First Search**

BFS uses a queue. In BFS, node `a` visits each of `a's` neighbors before visiting any of their neighbors. An iterative solution involving a queue usually works best.

	void search(Node root){
	    Queue queue = new Queue();
	    root.marked = true;
	    queue.enqueue(root);

	    while (!queue.isEmpty()){
	        Node r = queue.dequeue();
	        visit(r);
	        foreach(Node n in r.adjacent){
	            if (n.marked == false){
	                n.marked = true;
	                queue.enqueue(n);
	            }
	        }
	    }
	}

If you are asked to implement BFS, the key thing to remember is the use of the queue. The rest of the algorithm flows from this fact.

**Bidirectional Search**

Bidirectional search is used to find the shortest path between a source and destination node. It operates by essentially running two simultaneous breadth-first searches, on from each node. When their searches collide, we have found a path.