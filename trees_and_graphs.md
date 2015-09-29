Tree and graph questions are rife with ambiguous details and incorrect assumptions. Be sure to watch out for the following issues and seek clarification when necessary

+ Trees vs. Binary Trees
+ Binary Tree vs. Binary Search Tree
	+ all left descendants \<= n \< all right descendants
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