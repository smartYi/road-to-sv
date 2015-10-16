# 找出有向图中的弱联通分量

请找出有向图中弱联通分量的数目。图中的每个节点包含其邻居的 1 个标签和1 个列表。 （一个有向图中的相连节点指的是一个包含 2 个通过直接边沿路径相连的顶点的子图。）

样例

    给定图:
    A----->B  C
     \     |  |
      \    |  |
       \   |  |
        \  v  v
         ->D  E <- F

    返回 {A,B,D}, {C,E,F}. 图中有 2 个相连要素，即{A,B,D} 和 {C,E,F} 。

挑战

    将原素升序排列。

## 题解

Union Find

```java
/**
 * Definition for Directed graph.
 * class DirectedGraphNode {
 *     int label;
 *     ArrayList<DirectedGraphNode> neighbors;
 *     DirectedGraphNode(int x) { label = x; neighbors = new ArrayList<DirectedGraphNode>(); }
 * };
 */
public class Solution {
    /**
     * @param nodes a array of Directed graph node
     * @return a connected set of a directed graph
     */
    public List<List<Integer>> connectedSet2(ArrayList<DirectedGraphNode> nodes) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        for(DirectedGraphNode node : nodes){
            for(DirectedGraphNode n : node.neighbors){
                int fa = find(map, node.label);
                int ch = find(map, n.label);
                map.put(fa, ch);
            }
        }
        HashMap<Integer, ArrayList<Integer>> record = new HashMap<Integer, ArrayList<Integer>>();

        for(DirectedGraphNode node : nodes){
            int val = find(map, node.label);
            if(!record.containsKey(val)){
                record.put(val, new ArrayList<Integer>());
            }
            record.get(val).add(node.label);
        }

        for(int key : record.keySet()){
            ArrayList<Integer> sub = new ArrayList<Integer>();
            sub.addAll(record.get(key));
            res.add(sub);
        }
        return res;

    }

    private int find(HashMap<Integer, Integer> map, int v){
        if(!map.containsKey(v)){
            map.put(v, v);
            return v;
        }
        while(map.get(v) != v) v = map.get(v);
        return v;
    }
}

```