# Word Ladder

Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord, such that:

Only one letter can be changed at a time
Each intermediate word must exist in the word list
For example,

Given:

    beginWord = "hit"
    endWord = "cog"
    wordList = ["hot","dot","dog","lot","log"]
    As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
    return its length 5.

Note:

+ Return 0 if there is no such transformation sequence.
+ All words have the same length.
+ All words contain only lowercase alphabetic characters.

## Solution

分析：这种题，肯定是每次改变单词的一个字母，然后逐渐搜索，很多人一开始就想到用dfs，其实像这种求最短路径、树最小深度问题bfs最适合，可以参考我的这篇博客bfs（层序遍历）求二叉树的最小深度。本题bfs要注意的问题：

+ 和当前单词相邻的单词是：对当前单词改变一个字母且在字典中存在的单词
+ 找到一个单词的相邻单词，加入bfs队列后，要从字典中删除，因为不删除的话会造成类似于hog->hot->hog的死循环。而删除对求最短路径没有影响，因为我们第一次找到该单词肯定是最短路径，即使后面其他单词也可能转化得到它，路径肯定不会比当前的路径短（如果要输出所有最短路径，则不能立即从字典中删除，具体见下一题）
+ bfs队列中用NULL来标识层与层的间隔，每次碰到层的结尾，遍历深度+1


```java
public class Solution {
    public int ladderLength(String beginWord, String endWord, Set<String> dict) {
        Queue<String> queue = new LinkedList<String>();
        queue.offer(beginWord);
        dict.remove(beginWord);
        int length = 1;
        
        //BFS level traverse
        while (!queue.isEmpty()) {
            int count = queue.size();
            for (int i = 0; i < count; i++){
                String current = queue.poll();
                for (char c = 'a'; c <= 'z'; c++) {
                    for (int j = 0; j < current.length(); j++) {
                        if (c == current.charAt(j)) {
                            continue;
                        }
                        String tmp = replace(current, j, c);
                        if (tmp.equals(endWord)) {
                            return length + 1;
                        }
                        if (dict.contains(tmp)){
                            queue.offer(tmp);
                            dict.remove(tmp);
                        }
                    }
                }
            }
            length++;
        }
 
        return 0;
    }
 
    private String replace(String s, int index, char c) {
        char[] chars = s.toCharArray();
        chars[index] = c;
        return new String(chars);
    }
}
```