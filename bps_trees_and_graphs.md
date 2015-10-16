# Trees and Graphs

### 树

#### 树的概念

树(tree)是一种能够分层存储数据的重要数据结构，树中的每个元素被称为树的节点，每个节点有若干个指针指向子节点。从节点的角度来看，树是由唯一的起始节点引出的节点集合。这个起始结点称为根(root)。树中节点的子树数目称为节点的度(degree)。在面试中，关于树的面试问题非常常见，尤其是关于二叉树(binary tree)，二叉搜索树(Binary Search Tree, BST)的问题。

所谓的二叉树，是指对于树中的每个节点而言，至多有左右两个子节点，即任意节点的度小于等于2。而广义的树则没有如上限制。二叉树是最常见的树形结构。二分查找树是二叉树的一种特例，对于二分查找树的任意节点，该节点存储的数值一定比左子树的所有节点的值大比右子树的所有节点的值小“(与之完全对称的情况也是有效的：即该节点存储的数值一定比左子树的所有节点的值小比右子树的所有节点的值大)。

基于这个特性，二分查找树通常被用于维护有序数据。二分查找树查找、删除、插入的效率都会于一般的线性数据结构。事实上，对于二分查找树的操作相当于执行二分搜索，其执行效率与树的高度(depth)有关，检索任意数据的比较次数不会多于树的高度。这里需要引入高度的概念：对一棵树而言，从根节点到某个节点的路径长度称为该节点的层数(level)，根节点为第0层，非根节点的层数是其父节点的层数加1。树的高度定义为该树中层数最大的叶节点的层数加1，即相当于于从根节点到叶节点的最长路径加1。由此，对于n个数据，二分查找树应该以“尽可能小的高度存储所有数据。由于二叉树第L层至多可以存储2^L个节点，故树的高度应在logn量级，因此，二分查找树的搜索效率为O(logn)。

直观上看，尽可能地把二分查找树的每一层“塞满”数据可以使得搜索效率最高，但考虑到每次插入删除都需要维护二分查找树的性质，要实现这点并不容易。特别地，当二分查找树退化为一个由小到大排列的单链表(每个节点只有右孩子)，其搜索效率变为O(n)。为了解决这样的问题，人们引入平衡二叉树的概念。所谓平衡二叉树，是指一棵树的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。通过恰当的构造与调整，平衡二叉树能够保证每次插入删除之后都保持平衡性。平衡二叉树的具体实现算法包括AVL算法和红黑算法等。由于平衡二叉树的实现比较复杂，故一般面试官只会问些概念性的问题。

#### 树型的概念

满二叉树(full binary tree)：如果一棵二叉树的任何结点，或者是叶节点，或者左右子树都存在，则这棵二叉树称作满二叉树。

完全二叉树(complete binary tree)：如果一棵二叉树最多只有最下面的两层节点度数可以小于2，并且最下面一层的节点都集中在该层最左边的连续位置上，则此二叉树称作完全二叉树。

#### “二叉树的遍历

二叉树的常见操作包括树的遍历，即以一种特定的规律访问树中的所有节点。常见的遍历方式包括：

前序遍历(Pre-order traversal)：访问根结点；按前序遍历左子树；按前序遍历右子树。

中序遍历(In-order traversal)：按中序遍历左子树；访问根结点；按中序遍历右子树。特别地，对于二分查找树而言，中序遍历可以获得一个由小到大或者由大到小的有序序列。

后续遍历(Post-order traversal)：按后序遍历左子树；按后序遍历右子树；访问根结点。

以上三种遍历方式都是深度优先搜索算法(depth-first search)。深度优先算法最自然的实现方式是通过递归实现，事实上，大部分树相关的面试问题都可以优先考虑递归。此外，另一个值得注意的要点是：深度优先的算法往往都可以通过使用栈数据结构将递归化为非递归实现。这里利用了栈先进后出的特性，其数据的进出顺序与递归顺序一致(请见Chapter 3 Stacks and Queues) 。

层次遍历(Level traversal)：首先访问第0层，也就是根结点所在的层；当第i层的所有结点访问完之后，再从左至右依次访问第i+1层的各个结点。层次遍历属于广度优先搜索算法(breadth-first search)。广度优先算法往往通过队列数据结构实现。

### Trie

字典树(trie or prefix tree)是一个26叉树，用于在一个集合中检索一个字符串，或者字符串前缀。字典树的每个节点有一个指针数组代表其所有子树，其本质上是一个哈希表，因为子树所在的位置(index)本身，就代表了节点对应的字母。节点与每个兄弟具有相同的前缀，这就是trie也被称为prefix tree的原因。

假设我们要存储如下名字，年龄：

    Amy 12
    Ann 18
    Bob 30

则构成的字典树如下：

```
.                 root: level 0
a---------b       level 1
|         |  
m---n     o       level 2
|   |     |
y   n     b       level 3
|   |     |
12  18   30       level 4
```

由于Amy和Ann共享前缀a，故第二个字母m和n构成兄弟关系。

字典树以及字典树节点的原型：

```
class TrieNode {
    private:
        T mContent;
        vector<TrieNode*> mChildren;
    public:
        Node();
        ~Node();
        friend class Trie;
};
class Trie {
public:
    Trie();
    ~Trie();
    void addWord(string s);
    bool searchWord(string s);
    void deleteWord(string s);
private:
    TrieNode* root;
};
```

字典树的基本功能如下：

1) void addWord(string key, int value);

添加一个键:值对。添加时从根节点出发，如果在第i层找到了字符串的第i个字母，则沿该节点方向下降一层(注意，如果下一层存储的是数据，则视为没有找到)。否则，将第i个字母作为新的兄弟插入到第i层。将键插入完成后插入值节点。

2) bool searchWord(string key, int &value);

查找某个键是否存在，并返回值。从根节点出发，在第i层寻找字符串中第i个字母是否存在。如果是，沿着该节点方向下降一层；否则，返回false。

3) void deleteWord(string  key)

删除一个键:值对。删除时从底层向上删除节点，“直到遇到第一个有兄弟的节点(说明该节点向上都是与其他节点共享的前缀)，删除该节点。

### 堆与优先队列

通常所说的堆(Heap)是指二叉堆，从结构上说是完全二叉树，从实现上说一般用数组。以数组的下标建立父子节点关系：对于下标为i的节点，其父节点为(int)i/2，其左子节点为2i，右子节点为2i+1。堆最重要的性质是，它满足部分有序(partial order)：最大(小)堆的父节点一定大于等于(小于等于)当前节点，且堆顶元素一定是当前所有元素的最大(小)值。

堆算法的核心在于插入，删除算法如何保持堆的性质(以下讨论均以最大堆为例):

下移(shift-down)操作：下移是堆算法的核心。对于最大值堆而言，对于某个节点的下移操作相当于比较当前节点与其左右子节点的相对大小。如果当前节点小于其子节点，则将当前节点与其左右子节点中较大的子节点对换，直至操作无法进行(即当前节点大于其左右子节点)。

建堆：假设堆数组长度为n，建堆过程如下，注意这里数组的下标是从 1 开始的：

```
for i, n/2 downto 1
    do shift-down(A,i)
```

插入：将新元素插入堆的末尾，并且与父节点进行比较，如果新节点的值大于父节点，则与之交换，即上移(shift-up)，直至操作无法进行。

弹出堆顶元素：弹出堆顶元素(假设记为A[1]，堆尾元素记为A[n])并维护堆性质的过程如下：

```
output = A[1]
exchange A[1] <-> A[n]
heap size -= 1
shift-down(A,1)
return output
```

值得注意的是，堆的插入操作逐层上移，耗时O(log(n))，与二叉搜索树的插入相同。但建堆通过下移所有非叶子节点(下标n/2至1)实现，耗时O(n)，小于BST的O(nlog(n))。

通过上述描述，不难发现堆其实就是一个优先队列。对于C++，标准模版库中的priority_queue是堆的一种具体实现。

### 图

图(Graph)是节点集合的一个拓扑结构，节点之间通过边相连。图分为有向图和无向图。有向图的边具有指向性，即AB仅表示由A到B的路径，但并不意味着B可以连到A。与之对应地，无向图的每条边都表示一条双向路径。

图的数据表示方式也分为两种，即邻接表(adjacency list)和邻接矩阵(adjacency matrix)。对于节点A，A的邻接表将与A之间相连的所有节点以链表的形势存储起来，节点A为链表的头节点。这样，对于有V个节点的图而言，邻接表表示法包含V个链表。因此，链接表需要的空间复杂度为O(V+E)。邻接表适用于边数不多的稀疏图。但是，如果要确定图中边(u, v)是否存在，则只能在节点u对应的邻接表中以O(E)复杂度线性搜索。

对于有V个节点的图而言，邻接矩阵用V*V的二维矩阵形式表示一个图。矩阵中的元素Aij表示节点i到节点j之间是否直接有边相连。若有，则Aij数值为该边的权值，否则Aij数值为0。特别地，对于无向图，由于边的双向性，其邻接矩阵的转置矩阵为其本身。邻接矩阵的空间复杂度为O(V^2)，适用于边较为密集的图。邻接矩阵在检索两个节点之间是否有边相连这样一个需求上，具有优势。

### 图的遍历

对于图的遍历(Graph Transversal)类似于树的遍历(事实上，树可以看成是图的一个特例)，也分为广度优先搜索和深度优先搜索。算法描述如下：

#### 广度优先

对于某个节点，广度优先会先访问其所有邻近节点，再访问其他节点。即，对于任意节点，算法首先发现距离为d的节点，当所有距离为d的节点都被访问后，算法才会访问距离为d+1的节点。广度优先算法将每个节点着色为白，灰或黑，白色表示未被发现，灰色表示被发现，黑色表示已访问。算法利用先进先出队列来管理所有灰色节点。一句话总结，广度优先算法先访问当前节点，一旦发现未被访问的邻近节点，推入队列，以待访问。

《算法导论》第22章图的基本算法给出了广度优先的伪代码实现，引用如下：

```
BFS(G, s)
For each vertex u exept s
    Do Color[u] = WHITE
        Distance[u] = MAX
        Parent[u] = NIL
Color[s] = GRAY
Distance[s] = 0
Parent[s] = NIL
Enqueue(Q, s)
While Q not empty
    Do u = Dequeue(Q)
        For each v is the neighbor of u
            Do if Color[v] == WHITE
                Color[v] = GRAY
                Distance[v] = Distance[u] + 1
                Parent[v] = u
                Enqueue(Q, v)
            Color[u] = BLACK”
```

#### 深度搜索

深度优先算法尽可能“深”地搜索一个图。对于某个节点v，如果它有未搜索的边，则沿着这条边继续搜索下去，直到该路径无法发现新的节点，回溯回节点v，继续搜索它的下一条边。深度优先算法也通过着色标记节点，白色表示未被发现，灰色表示被发现，黑色表示已访问。算法通过递归实现先进后出。一句话总结，深度优先算法一旦发现没被访问过的邻近节点，则立刻递归访问它，直到所有邻近节点都被访问过了，最后访问自己。

《算法导论》第22章图的基本算法给出了深度优先的伪代码实现，引用如下：

```
DFS(G)
For each vertex v in G
    Do Color[v] = WHITE
    Parent[v] = NIL
For each vertex v in G
    DFS_Visit(v)

DFS_Visit(u)
Color[u] = GRAY
For each v is the neighbor of u
    If Color[v] == WHITE
        Parent[v] = u
        DFS_Visit(v)
Color[u] = BLACK
```

### 单源最短路径问题

对于每条边都有一个权值的图来说，单源最短路径问题是指从某个节点出发，到其他节点的最短距离。该问题的常见算法有Bellman-Ford和Dijkstra算法。前者适用于一般情况(包括存在负权值的情况，但不存在从源点可达的负权值回路)，后者仅适用于均为非负权值边的情况。Dijkstra的运行时间可以小于Bellman-Ford。本小节重点介绍Dijkstra算法。

特别地，如果每条边权值相同(无权图)，由于从源开始访问图遇到节点的最小深度 就等于到该节点的最短路径，因此 Priority Queue就退化成Queue，Dijkstra算法就退化成BFS。

Dijkstra的核心在于，构造一个节点集合S，对于S中的每一个节点，源点到该节点的最短距离已经确定。进一步地，对于不在S中的节点，“我们总是选择其中到源点最近的节点，将它加入S，并且更新其邻近节点到源点的距离。算法实现时需要依赖优先队列。一句话总结，Dijkstra算法利用贪心的思想，在剩下的节点中选取离源点最近的那个加入集合，并且更新其邻近节点到源点的距离，直至所有节点都被加入集合。关于Dijkstra算法的正确性分析，可以使用数学归纳法证明，详见《算法导论》第24章，单源最短路径。 给出伪代码如下：

```
DIJKSTRA(G, s)
S = EMPTY
Insert all vertexes into Q
While Q is not empty
    u = Q.top
    S.insert(u)
    For each v is the neighbor of u
        If d[v] > d[u] + weight(u, v)
            d[v] = d[u] + weight(u, v)
            parent[v] = u
```

### 任意两点之间的最短距离

另一个关于图常见的算法是，如何获得任意两点之间的最短距离(All-pairs shortest paths)。直观的想法是，可以对于每个节点运行Dijkstra算法，该方法可行，但更适合的算法是Floyd-Warshall算法。

Floyd算法的核心是动态编程，利用二维矩阵存储i，j之间的最短距离，矩阵的初始值为i，j之间的权值，如果i，j不直接相连，则值为正无穷。动态编程的递归式为：d(k)ij = min(d(k-1)ij, d(k-1)ik+ d(k-1)kj)  (1<= k <= n)。直观上理解，对于第k次更新，我们比较从i到j只经过节点编号小于k的中间节点(d(k-1)ij)，和从i到k，从k到j的距离之和(d(k-1)ik+ d(k-1)kj)。Floyd算法的复杂度是O(n^3)。关于Floyd算法的理论分析，请见《算法导论》第25章，每对顶点间的最短路径。 给出伪代码如下：

```
FLOYD(G)
Distance(0) = Weight(G)
For k = 1 to n
    For i = 1 to n
        For j = 1 to n
Distance(k)ij = min(Distance (k-1)ij, Distance (k-1)ik+ Distance(k-1)kj)  
Return Distance(n)
```

## 模式识别

### 利用Divide and Conquer(D&C)策略判断树、图的性质

对于树和图的性质，一般全局解依赖于局部解。通常可以用DFS来判断子问题的解，然后综合得到当前的全局结论。

值得注意的是，当我们在传递节点指针的时候，其实其代表的不只是这个节点本身，而是指对整个子树、子图进行操作。只要每次递归的操作对象的结构一致，我们就可以选择Divide and Conquer(事实上对于树和图总是如此，因为subgraph和subtree仍然是graph和tree结构)。实现函数递归的步骤是：首先设置函数出口，就此类问题而言，递归出口往往是node == NULL。其次，在构造递归的时候，不妨将递归调用自身的部分视为黑盒，并想象它能够完整解决子问题。以二叉树的中序遍历为例，函数的实现为：

```
void InOrderTraversal(TreeNode *root) {
    if (root == NULL) {
        return;
    }
    InOrderTraversal(root->left);
    root->print();
    InOrderTraversal(root->right);
}
```

想象递归调用的部分InOrderTraversal(root->left)／InOrderTraversal(root->right)能够完整地中序遍历一棵子树，那么根据中序遍历“按中序遍历左子树；访问根结点；按中序遍历右子树”的定义，写出上述实现就显得很自然了。

> Determine if a binary tree is a balanced tree.

解题分析： 回顾二叉树是否平衡的定义：一颗二叉树是平衡的，当且仅当左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。同时，注意特例，空树是一棵平衡二叉树。子树平衡是全局问题的一部分，因此可以通过子问题的结果来推断全局的结果：判断高度差的绝对值不超过1，以及递归左右子树都为平衡二叉树。

计算高度的子函数我们也可以用递归来实现。首先，考虑递归的出口：当node为空的时候，高度应该为0。其次，如果node不为空，那么这棵树的高度应该为左右子树中的高度较高者加1。

参考解答：

```
int level(TreeNode *root){
    if(root ==  NULL)
        return 0;
    return max(level(root->left), level(root->right)) + 1;
}
bool isBalanced(TreeNode* root){
    if(root ==  NULL)
       return true;
    int factor = abs(level(root->left) - level(root->right));
    return factor < 2 && isBalanced(root->right) && isBalanced(root->left);
}
```

复杂度分析：对于这个实现，isBalanced函数对于每个节点只运行一次。然而，level函数会进行很多重复的计算：每次进入isBalanced函数，调用level会遍历一遍所有子节点。原因在于level函数没有记忆性，当输入为同一个节点时，level函数会再次进行完整的计算。

一种改进方式是，可以考虑利用动态编程的思想，将TreeNode指针作为键，高度作为值，一旦发现节点已经被计算过，直接返回结果，这样，level函数对每个节点只计算一次。另一种更为巧妙的方法是，isBalanced返回当前节点的高度，用-1表示树不平衡。 将计算结果自底向上地传递，并且确保每个节点只被计算一次，复杂度O(n)。

```
#define INBALANCE (-1)
int isBalancedHelper(TreeNode *root) {
    if (root == NULL) {
       return 0;
    }
    int leftHeight = isBalance(root->left);
    if (leftHeight == INBALANCE) {
        return INBALANCE;
    }
    int rightHeight = isBalance(root->right);
    if (rightHeight == INBALANCE) {
        return INBALANCE;
    }

    if (abs(leftHeight - rightHeight) > 1) {
        return INBALANCE;
    }

    return max(leftHeight, rightHeight) + 1;// return height
}

bool isBalancedTree(TreeNode *root) {
    if(isBalancedHelper(root) == INBALANCE)
        return false;
    else
        return true;
}
```

> Check if a binary tree is a Binary Search Tree (BST), you may assume there is no element having same value.

解题分析： 首先考虑BST的定义：对于二分查找树的任意节点，该节点存储的数值一定比左子树的所有节点的值大比右子树的所有节点的值小。并且中序遍历能够获得从小到大排列的有序数组。

根据BST的中序遍历特性，非常直观的想法是：可以通过中序遍历将这棵二叉树线性化，然后遍历数据，考察数组是否有序。然而，这样的方法并不是最优的，原因在于我们至少遍历整棵树，而不是一旦发现该树不是BST立刻返回结果。此外，遍历完成后还需要再遍历一遍额外的数组空间以判断是否有序，故该方法整体额外开销较大。

事实上，问题的定义符合采用D&C的条件：子树是BST的是原问题的一部分；对于递归调用的函数而言，节点代表的树与子节点代表的树是同样的结构。考虑到对于BST的判断，我们需要左子树均小于当前节点，右子树均大于当前节点，故可以将当前节点作为最小或最大值传入。为了函数接口的统一，一个小技巧是：可以同时传入最小／最大值，并且将初始值设为`INT_MIN`，`INT_MA`X，这样，其左子树所有节点的值必须在 `INT_MIN`及根节点的值之间，其右子树所有节点的值必须在根节点的值以及`INT_MAX`之间。“从递归的角度看，不难得出递归方式是：判断`root->value > min && root->value < max` (root->value为`INT_MIN`或`INT_MAX`的情况需要特殊处理)，以及递归左右子树都为BST。同时，注意特例，空树是一棵BST，该特例可以最为递归的出口。

当然这题也可以直接中序遍历然后检查输出是否按顺序。

复杂度分析：根据解题分析的描述，算法需要遍历所有节点，故时间复杂度为O(n)。

参考解答：

```
bool helper(TreeNode *root, int min, int max){
    if(!root) return true;
    if((root->val < max || (root->val == INT_MAX && root->right == NULL)) &&
        (root->val > min || (root->val == INT_MIN && root->left == NULL)) &&
        helper(root->left, min, root->val) &&
        helper(root->right, root->val, max))
            return true;
    return false;
}

bool isValidBST(TreeNode *root) {
    return helper(root, INT_MIN, INT_MAX);
}
```

> Tree1 and Tree2 are both binary trees nodes having value, determine if Tree2 is a subtree of Tree1.

解题分析： 根据题意，比较容易想到的是我们需要实现一个matchTree辅助函数，用以判断根节点为root1和root2的两棵树是否完全相等。判断两棵树相等的定义符合采用D&C的条件：原问题的答案取决于左子树右子树都相等这两个子问题 。对于递归的实现，出口为root1 == NULL && root2 == NULL，递归函数需要判断当前节点是否相等，左子树是否相等和右子树是否相等。

其次，考虑在什么情况下我们需要调用matchTree：当Tree1的当前节点与Tree2的根节点相等时，我们有兴趣调用matchTree判断以Tree1当前节点为根的子树是否与Tree2相等。那么如果不相等怎么办？我们同样可以利用递归的思想，“将当前节点的左子树或右子树是否包含Tree2作为子问题，通过递归调用自身获得结果。

复杂度分析：假设Tree1有M个节点，Tree2有N个节点。最坏情况下，对于Tree1的每个节点，都需要调用matchTree函数，并且matchTree在基本遍历完Tree2后才能返回结果。此时，时间复杂度为O(MN)。

参考解答：

```
bool subTree(TreeNode *root1, TreeNode *root2){
    if (root2 == NULL) {
        return true;
    }
    if (root1 == NULL) { //we have exhauste the root1 already
        return false;
    }
    if (root1->val == root2->val) {
        if (matchTree(root1, root2)) {
            return true;
        }
    }
    return isSubTree(root1->left, root2) || isSubTree(root1->right, root2);
}

bool matchTree(TreeNode *root1, TreeNode *root2){
    if (root1 == NULL && root2 == NULL) {
        return true;
    }
    if (root1 == NULL || root2 == NULL) {
        return false;
    }
    if (root1->val != root2->val) {
        return false;
    }
    return matchTree(root1->left, root2->left) &&
        matchTree(root1->right, root2->right);
}
```

> Compute the depth of a binary tree.

解题分析： 回顾树的高度定义：从根节点到某个节点的路径长度称为该节点的层数(level)，根节点为第0层，非根节点的层数是其父节点的层数加1。树的高度定义为该树中层数最大的叶节点的层数加1。判断树的高度符合D&C的 条件：对于某个节点，其高度为左子树和右子树高度的较大者加1，即原问题依赖于两个子问题。对于递归的实现，出口为传入节点为空，此时应该返回高度0。

复杂度分析：该算法遍历树的所有节点，故复杂度O(n)。

参考解答：

```
int treeDepth(TreeNode *node) {
    if (node == NULL)
        return 0;
    else
        return max(treeDepth(node->left), treeDepth(node->right)) + 1;
}
```

### 树的路径问题

有一类关于树的问题是， 要求找出一条满足特定条件的路径 。对于这类问题，通常都是传入一个vector记录当前走过的路径(为尽可能模版化，统一记为path)，传入path的时候可以是引用，可以是值，具体见下面的分析。还需要传入另一个vector引用记录所有符合条件的path(为尽可能模版化，统一记为result)。注意， result可以用引用或指针形式，相当于一个全局变量，或者就开辟一个独立于函数的成员变量。由于path通常是vector<int> ，那么result就是vector<vector<int>>。当然，那个特定条件，也是函数的一个输入变量。

在解答此类问题的时候，通常都采用DFS来访问，利用回溯思想，直到无法继续访问再返回。值得注意的是，如果path本身是以引用(reference)的形式传入，那么需要在返回之前消除之前所做的影响(回溯)。因为传引用(Pass by reference)相当于把path也看作全局变量，对path的任何操作都会影响其他递归状态，而传值(pass by value)则不会。传引用的好处是可以减小空间开销。在本书第7章中，我们会继续进一步介绍递归和回溯。

> Get all the paths (always starts from the root) in a binary tree, whose sum would be equal to given value.

解题分析：正如上文的叙述，寻找满足条件的路径，用path记录当前走过的路径，用result记录所有符合条件的path，用DFS进行探索。 对于递归函数，传入的节点相当于当前子树的根：当传入节点为空时，说明我们走完了当前的path，直接返回，即达到递归函数的出口。由于根节点必须要出现在path中，所以我们先将当前节点push到path中去。此时，如果当前节点的数值等于sum，说明找到了一个可行解，故把该递归状态下的path加入到answer中。进一步，我们需要调用函数自身解决左子树和右子树的子问题。由于当前节点已经加入path，那么自然传递给子问题的sum变为sum – root->val，根的左右孩子成为了子问题的根。

复杂度分析：时间上，上述解法遍历每个节点，故时间复杂度O(n)。空间上，如果path以引用的方式传入，则额外空间为O(n)；如果path以值的方式传入，则在递归函数的底层会有2^h个path拷贝，h为树的高度。故复杂度为O(2^h*n)。

参考解答：

这里对path 用传值的方法。

```
void pathSumHelper(vector<int> path, vector<vector<int>> &result, TreeNode *root, int sum){
    if(root == NULL)
        return;
    path.push_back(root->val);
    if(root->val == sum){
        answer.push_back(path);
    }
    pathSumHelper(path,answer,root->left, sum - root->val);
    pathSumHelper(path,answer,root->right, sum - root->val);
}

vector<vector<int> > pathSum(TreeNode *root, int sum) {
    vector<int> path;
    vector<vector<int>> result;
    pathSumHelper(path, result, root, sum);
    return result;
}
```

> Get all the paths (always starts from the root and ends at leaf) in a binary tree, whose sum would be equal to given value.

解题分析： 本题的处理办法基本与上题完全一致，唯一的区别在于现在要求一定是root-to-leaf paths，故当path和为给定值的时候还需要判断当前节点是否是叶节点。

复杂度分析：时间、空间复杂度均为O(n)。

参考解答：

这里对path 用传引用的方法。

```
void pathSumHelper(vector<int> &path, vector<vector<int>> &result, TreeNode *root, int sum){
    if(root == NULL)
        return;
    path.push_back(root->val);
    if(root->left == NULL && root->right == NULL && root->val == sum){
        result.push_back(path);
    }
    pathSumHelper(path,answer,root->left, sum - root->val);
    pathSumHelper(path,answer,root->right, sum - root->val);
    path.pop_back();
}

vector<vector<int> > pathSum(TreeNode *root, int sum) {
    vector<int> path;
    vector<vector<int>> result;
    pathSumHelper(path, result, root, sum);
    return result;
}
```

> Get all the paths (from any node to any other node with deeper level) in a binary tree, whose sum would be equal to given value.

解题分析： 经过前两道题目的分析，本题的大体思路应该已经比较容易想到了：用path记录当前走过的路径，用result记录所有符合条件的path，利用DFS进行左子树和右子树的探索。本题的特别之处在于，所求的path并不一定需要从root开始，即之前的题目都是知道固定的起始点，寻找终点，而本题起始点不定。进一步考虑path中存放的路径，该路径是由root到当前节点经过的所有节点，且当前节点一定是path的终点。那么，不妨换一个角度思考：以当前节点为终点，是否存在一个起始节点，使得路径上的节点数字和为给定值。这样，“对于每个path数组，应该从数组尾(当前节点，即终点)，反向搜索，寻求起始节点。一旦找到，则将该段path加入answer。

复杂度分析：对于处于第i层的节点，从终点往根节点反向搜索需要的复杂度为i。对于二叉树，第i层有2^i个节点，故复杂度为`i*2^i`。对于深度为d的二叉树，整体复杂度为：
`1*2^1+2*2^2+…+d*2^d = 2(d - 1)*2^d + 2`
又`d = logn`，故整体时间复杂度`O(nlogn)`。

参考解答：

```
void pathSumHelper(vector<int> path, vector<vector<int>> &answer, TreeNode *root, int sum){
    if(root == NULL)
        return;
    path.push_back(root->val);
    int tempSum = sum;
    vector<int> partialPath;
    for (int i = (int)path.size() - 1; i >= 0; i--) {
        tempSum -= path[i];
        partialPath.push_back(path[i]);
        if (tempSum == 0) {
            answer.push_back(partialPath);
        }
    }
    pathSumHelper(path, answer,root->left, sum);
    pathSumHelper(path, answer,root->right, sum);
}
vector<vector<int> > pathSum(TreeNode *root, int sum) {
    vector<int> path;
    vector<vector<int> > answer;
    pathSumHelper(path, answer,root,sum);
    return answer;
}
```

### 树和其他数据结构的相互转换

这类题目要求将树的结构转化成其他数据结构，例如链表、数组等，或者反之，从数组等结构构成一棵树。前者通常是通过树的遍历，合并局部解来得到全局解，而后者则可以利用D&C的策略，递归将数据结构的两部分分别转换成子树，再合并。

> Covert a binary tree to linked lists. Each linked list is correspondent to all the nodes at the same level.

解题分析： 本题相当于层次遍历，当遍历完一层时将构成的链表加入链表集合。本题的核心在于如何判断某层已经遍历完成。回顾层次遍历，我们将节点逐次放入优先队列，然后一次弹出，并将左右子节点放入队列。我们可以引入一个特别的哑节点，用来作为层与层之间的分隔符。每当发现当前节点是哑节点，则说明当前层已经遍历完毕，也意味着下一层的所有节点都已经进入队列。此时，立刻再推送一个哑节点入队，作为下一层的分隔符。

复杂度分析：算法需要层次遍历树，故时间复杂度O(n)。同时，需要额外的O(n)空间将树中的元素存储到链表。

参考解答：

```
bool isDummyNode(TreeNode *node)
{
    return (node->left == node);
}

vector<list<TreeNode *>> linkedListsFromTree(TreeNode *root)
{
    vector<list<TreeNode *>> result;
    list<TreeNode *> levelList;
    queue<TreeNode *> nodeQueue;
    TreeNode *currentNode;

    TreeNode dummyNode;
    dummyNode.left = &dummyNode;

    if (!root) {
        return answer;
    }

    nodeQueue.push(&dummyNode);
    nodeQueue.push(root);

    while (!nodeQueue.empty()) {
        currentNode = nodeQueue.front();
        if (isDummyNode(currentNode)) {
            if (!levelList.empty()) {
                answer.push_back(levelList);
                levelList.clear();
            }
            nodeQueue.pop();
            if (nodeQueue.empty()) {
                break;
            } else {
                nodeQueue.push(&dummyNode);
            }

        } else {
            levelList.push_back(currentNode);
            if (currentNode->left) {
                nodeQueue.push(currentNode->left);
            }
            if (currentNode->right) {
                nodeQueue.push(currentNode->right);
            }
            nodeQueue.pop();
        }
    }
    return result;
}
```

> Convert a sorted array( increasing order ) to a balanced BST.

解题分析： 将数据结构分成左右两部分，分别构造两个二叉树，再用中间数与这两个局部解合并得到当前的全局解。因为数组已经排序好，因此这个二叉树一定满足BST的条件(左边的任何数一定小于中间数小于右边的任何数)，在选择分界点的时候选择中点，那么也就能满足平衡条件。

复杂度分析：算法遍历整个数组，故时间复杂度O(n)，需要额外O(n)空间存储树的节点。

参考解答：

```
TreeNode* helper(vector<int>num, int first, int last){
    if(first > last){
        return NULL;
    }
    if (first == last) {
        TreeNode* parent = new TreeNode(num[first]);
        return parent;
    }
    int mid = (first + last) / 2;
    TreeNode *leftchild = helper(num, first, mid - 1);
    TreeNode *rightchild = helper(num, mid + 1, last);
    TreeNode *parent = new TreeNode(num[mid]);
    parent->left = leftchild;
    parent->right = rightchild;
    return parent;
}

TreeNode *sortedArrayToBST(vector<int> &num) {
    if(num.size() == 0)
        return NULL;
    if(num.size() == 1){
        TreeNode* parent = new TreeNode(num[0]);
        return parent;
    }
    int first = 0;
    int last = (int)num.size() - 1;
    return helper(num, first, last);
}
```

### 寻找特定节点

此类题目通常会传入一个当前节点，要求找到与此节点具有一定关系的特定节点：例如前驱、后继、左／右兄弟等。

对于这类题目，首先可以了解一下常见特定节点的定义及性质。在存在指向父节点指针的情况下，通常可以由当前节点出发，向上倒推解决。如果节点没有父节点指针，一般需要从根节点出发向下搜索，搜索的过程就是DFS。

> In-order traverse a binary tree with parent links, find the next node to visit given a specific node.

解题分析： 根据中序遍历的性质，我们可以分几种情况来考虑目标节点与给定节点的关系：1，如果该节点有右子树，那么，中序后继节点就是右子树中最左的节点。2，如果没有右子树，那么考虑该节点与其父节点的关系：如果它是父节点的左孩子，那么，父节点就是它的后继。3，如果它是父节点的右孩子，那么我们可以向上倒推，直到某个节点(或者不存在这样的节点，返回空指针)是其父节点的左孩子。

复杂度分析：最坏情况下，考虑一棵树只有右孩子，而输入恰好是最右节点。在这个情况下，我们需要向上倒推遍历所有节点，此时复杂度O(n)。平均情况下，复杂度为O(h)，h为树的高度。

参考解答：

```
TreeNode *leftMostNode(TreeNode *node)
{
    if (!node) {
        return NULL;
    }
    “while (node->left) {
        node = node->left;
    }
    return node;
}

bool isLeftChild(TreeNode *node, TreeNode *parent)
{
    return (parent->left == node);
}

TreeNode *inOrderSuccessor(TreeNode *node)
{
    if (!node) {
        return NULL;
    }
    if (node->right) {
        return leftMostNode(node->right);
    }

    TreeNode *parent = node->parent;
    while (parent && !isLeftChild(node, parent)) {
        node = parent;
        parent = node->parent;
    }
    return parent;
}
```

> In-order traverse a binary search tree without parent links, find the next node to visit given a specific node.

解题分析： 对于该节点有右子树的情况，由于我们不需要利用父节点信息倒推，故搜索过程与上题一致：中序后继节点就是右子树中最左的节点。在其他情况下，我们需要从根部开始搜索。对于根节点，我们应该如何判断继续搜索左子树还是右子树？对于BST，中序遍历的后继节点就是值比当前节点大的所有节点中最小的那个。因此，一旦根节点大于当前节点，我们存储当前节点，并且往数值减小的方向搜索(左子树)；一旦根节点小于当前节点，我们继续往数值增大的方向搜索(右子树)。这样，当算法执行完成，我们存储的最后一个节点一定恰好大于给定节点，即是给定节点的中序遍历后继。

“复杂度分析：算法复杂度同上题：平均情况下，复杂度为O(h)，h为树的高度。

参考解答：

```
TreeNode *inOrderSuccessor(TreeNode *node, TreeNode *root)
{
    if (!node) {
        return NULL;
    }
    if (node->right) {
        return leftMostNode(node->right);
    }

    TreeNode *successor = NULL;
    while (root) {
        if (root->val > node->val) {
            successor = root;
            root = root->left;
        } else {
            root = root->right;
        }
    }
    return successor;
}
```