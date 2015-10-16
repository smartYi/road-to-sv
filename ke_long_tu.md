# 克隆图

克隆一张无向图，图中的每个节点包含一个label和一个列表neighbors

LintCode Online Judge的无向图序列化：

图节点有唯一的label。

使用#作为一个分隔符，分隔节点的label和每个相邻节点neighbors。比如，序列化图{0,1,2#1,2#2,2}共有三个节点,因此包含两个个分隔符#。

1. 第一个节点label为0，存在边从节点0链接到节点1和节点2
2. 第二个节点label为1，存在边从节点1连接到节点2
3. 第三个节点label为2，存在边从节点2连接到节点2(本身),从而形成自环。

我们能看到如下的图：

       1
      / \
     /   \
    0 --- 2
         / \
         \_/

## 题解

使用BFS来解决此问题。用一个Queue来记录遍历的节点，遍历原图，并且把复制过的节点与原节点放在MAP中防止重复访问。

图的遍历有两种方式，BFS和DFS

这里使用BFS来解本题，BFS需要使用queue来保存neighbors

但这里有个问题，在clone一个节点时我们需要clone它的neighbors，而邻居节点有的已经存在，有的未存在，如何进行区分？

这里我们使用Map来进行区分，Map的key值为原来的node，value为新clone的node，当发现一个node未在map中时说明这个node还未被clone，

将它clone后放入queue中处理neighbors。

使用Map的主要意义在于充当BFS中Visited数组，它也可以去环问题，例如A--B有条边，当处理完A的邻居node，然后处理B节点邻居node时发现A已经处理过了

处理就结束，不会出现死循环。

queue中放置的节点都是未处理neighbors的节点。

```java
/**
 * Definition for undirected graph.
 * class UndirectedGraphNode {
 *     int label;
 *     ArrayList<UndirectedGraphNode> neighbors;
 *     UndirectedGraphNode(int x) { label = x; neighbors = new ArrayList<UndirectedGraphNode>(); }
 * };
 */
public class Solution {
    /**
     * @param node: A undirected graph node
     * @return: A undirected graph node
     */
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
        if (node == null) {
            return null;
        }

        UndirectedGraphNode root = null;

        // store the nodes which are cloned.
        HashMap<UndirectedGraphNode, UndirectedGraphNode> map =
            new HashMap<UndirectedGraphNode, UndirectedGraphNode>();

        Queue<UndirectedGraphNode> q = new LinkedList<UndirectedGraphNode>();

        q.offer(node);
        UndirectedGraphNode rootCopy = new UndirectedGraphNode(node.label);

        // 别忘记这一行啊。orz..
        map.put(node, rootCopy);

        // BFS the graph.
        while (!q.isEmpty()) {
            UndirectedGraphNode cur = q.poll();
            UndirectedGraphNode curCopy = map.get(cur);

            // bfs all the childern node.
            for (UndirectedGraphNode child: cur.neighbors) {
                // the node has already been copied. Just connect it and don't need to copy.
                if (map.containsKey(child)) {
                    curCopy.neighbors.add(map.get(child));
                    continue;
                }

                // put all the children into the queue.
                q.offer(child);

                // create a new child and add it to the parent.
                UndirectedGraphNode childCopy = new UndirectedGraphNode(child.label);
                curCopy.neighbors.add(childCopy);

                // Link the new node to the old map.
                map.put(child, childCopy);
            }
        }

        return rootCopy;
    }
}

```