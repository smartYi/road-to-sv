# 单词接龙

给出两个单词（start和end）和一个字典，找到从start到end的最短转换序列

比如：

1. 每次只能改变一个字母。
2. 变换过程中的中间单词必须在字典中出现。

样例

    给出数据如下：
    start = "hit"
    end = "cog"
    dict = ["hot","dot","dog","lot","log"]
    一个最短的变换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog"，
    返回它的长度 5

注意

+ 如果没有转换序列则返回0。
+ 所有单词具有相同的长度。
+ 所有单词都只包含小写字母。

## 题解

一开始看到string会以为是类似array的问题，但是其实是graph的问题 找最短路径，应该用BFS而不是DFS BFS搜索，最先搜索到的一定是最短路径

这种题，肯定是每次改变单词的一个字母，然后逐渐搜索，很多人一开始就想到用dfs，其实像这种求最短路径、树最小深度问题bfs最适合，可以参考我的这篇博客bfs（层序遍历）求二叉树的最小深度。本题bfs要注意的问题：

和当前单词相邻的单词是：对当前单词改变一个字母且在字典中存在的单词
一开始我使用每个dict里面的word和curr比较是否是neighbour的方式，但是这个会随着dict越大计算次数越多，会超时

使用每个位置改变26个字母来找neighbours的方法，只需要使用固定的26 * len(word)的time

找到一个单词的相邻单词，加入bfs队列后，要从字典中删除，因为不删除的话会造成类似于hog->hot->hog的死循环。而删除对求最短路径没有影响，因为我们第一次找到该单词肯定是最短路径，即使后面其他单词也可能转化得到它，路径肯定不会比当前的路径短（如果要输出所有最短路径，则不能立即从字典中删除，具体见下一题）

```java
public class Solution {
    /**
      * @param start, a string
      * @param end, a string
      * @param dict, a set of string
      * @return an integer
      */
    public int ladderLength(String start, String end, Set<String> dict) {
         if(start == null || end == null || start.equals(end)) return 0;

        Queue<String> from = new LinkedList<String>();
        HashSet<String> record = new HashSet<String>();
        from.offer(start);
        record.add(start);
        int count = 1;
        while(!from.isEmpty()){
            Queue<String> to = new LinkedList<String>();
            while(!from.isEmpty()){
                String word = from.poll();
                if(word.equals(end)) return count;
                for(int i = 0; i < word.length(); i++){
                    for(char c = 'a'; c <= 'z'; c++){
                        char[] arr = word.toCharArray();
                        if(arr[i] != c){
                            arr[i] = c;
                            String newWord = new String(arr);
                            if(end.equals(newWord)) return count+1;
                            if(!record.contains(newWord) && dict.contains(newWord)){
                                record.add(newWord);
                                to.add(newWord);
                            }
                        }
                    }
                }
            }
            count++;
            from = to;
        }
        return count;
    }
}

```