+ 难度：中等

给出一张有向图，设计一个算法判断两个点 s 与 t 之间是否存在路线。

样例

    如下图:
    A----->B----->C
     \     |
      \    |
       \   |
        \  v
         ->D----->E

    for s = B and t = E, return true
    for s = D and t = C, return false

## 题解

bfs dfs 均可，不要递归即可

```java
/**
 * Definition for Directed graph.
 * class DirectedGraphNode {
 *     int label;
 *     ArrayList<DirectedGraphNode> neighbors;
 *     DirectedGraphNode(int x) {
 *         label = x;
 *         neighbors = new ArrayList<DirectedGraphNode>();
 *     }
 * };
 */
public class Solution {
   /**
     * @param graph: A list of Directed graph node
     * @param s: the starting Directed graph node
     * @param t: the terminal Directed graph node
     * @return: a boolean value
     */
    public boolean hasRoute(ArrayList<DirectedGraphNode> graph,
                            DirectedGraphNode s, DirectedGraphNode t) {
        if (s == t) {
            return true;
        }
        if (graph == null || graph.size() == 0 || s == null || t == null) {
            return false;
        }
        Set<DirectedGraphNode> visited = new HashSet<DirectedGraphNode>();
        Stack<DirectedGraphNode> stack = new Stack<DirectedGraphNode>();
        stack.push(s);
        while (!stack.isEmpty()) {
            DirectedGraphNode node = stack.pop();
            if (visited.contains(node)) {
                continue;
            }
            visited.add(node);
            for (DirectedGraphNode neighbor : node.neighbors) {
                if (neighbor == t) {
                    return true;
                }
                stack.push(neighbor);
            }
        }
        return false;
    }
}

```