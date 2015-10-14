# Yahoo

## Phone Screen

一定要注意问清楚细节，要求我做什么

> Use minimum counts of coins to represent a number

从大到小排列硬币，然后动态规划，其中 dp[i] 表示面值为 i 时的最小硬币数。递推公式为

+　dp[0] = 0
+　dp[i] = min{dp[i-A[j]} + 1

这里 A[j] 表示第 j 个面值，比如现在要求 dp[10] 有面值为 1,2,5 的硬币，那么就应该找 dp[5], dp[8] 和 dp[9] 的最小值，加一表式从那个值到当前值最少需要的硬币数量。

---

> Lognest Palindrome Substring

这题从两个角度来考虑即可，一个是以每个字符为中间来进行扩张，另一个就是 manacher 算法。代码如下

```java
public String longestPalindrome_3(String s) {
    int n = s.length();
    int idx = 0, maxLen = 0;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j <= 1; ++j) {
            boolean isP = true;
            for (int k = 0; i - k >= 0 && i + j + k < n && isP; ++k) {
                isP = (s.charAt(i - k) == s.charAt(i + j + k));
                if (isP && (j + 1 + k*2) > maxLen) {
                    idx = i - k; maxLen = j + 1 + k*2;
                }
            }
        }
    }
    return s.substring(idx, idx + maxLen);
}

public String longestPalindrome_4(String s) {
    int n = s.length();
    int idx = 0, maxLen = 0;
    StringBuffer sb = new StringBuffer();
    sb.append('^');
    for (int i = 0; i < n; ++i) {
        sb.append('#');
        sb.append(s.charAt(i));
    }
    sb.append("#$");
    n = 2 * n + 3;
    int mx = 0, id = 0;
    int[] p = new int[n];
    Arrays.fill(p,0);
    for (int i = 1; i < n - 1; ++i) {
        p[i] = (mx > i) ? Math.min(p[2 * id - i], mx - i) : 0;
        while (sb.charAt(i + 1 + p[i]) == sb.charAt(i - 1 - p[i])) ++p[i];
        if (i + p[i] > mx) {
            id = i; mx = i + p[i];
        }
        if (p[i] > maxLen) {
            idx = i; maxLen = p[i];
        }
    }
    idx = (idx - maxLen - 1) / 2;
    return s.substring(idx, idx + maxLen);
}
```

---

> Boggle game, word search

题目大概要求就是在一个 MxN 的矩阵中找到单词，所以可以使用 Trie Tree 来存储字典(具体怎么写可以暂时不管)，然后对于每一点进行各个方向的 dfs。

从 matrix 的每一个 slot 出发进行回溯，知道超过 matrix 的边缘，或者当前 slot 已经被访问呢过，或者路径超过了最长单词的长度，就三种 invalid 条件。

胜利条件是当前的路径组成一个词典中的单词，但胜利条件之后应当继续回溯（统一地，对任何回溯问题，在处理完胜利条件之后都应该继续回溯，只不过很多时候胜利节点就是末节点，因此回溯下去也会因为 invalid 条件返回而已）

考虑对这个问题剪枝，可以将字典改写为 trie tree，这样在每一步查询时，可以根据当前路径序列是否是一个有效的 prefix 来进行判断，可以大幅提高搜索的效率。

Trie Tree 的代码

```java
public class Trie {
	private Node root;
	 
    public Trie(){
        root = new Node(' '); 
    }
 
    public void insert(String word){
    	if(isWordExisted(word) == true) return;
        Node current = root; 
        for(int i = 0; i < word.length(); i++){
            Node child = current.subNode(word.charAt(i));
            if(child != null){ 
                current = child;
            } else {
                 current.childList.add(new Node(word.charAt(i)));
                 current = current.subNode(word.charAt(i));
            }
            current.count++;
        } 
        // Set isEnd to indicate end of the word
        current.isEnd = true;
    }
    public boolean isWordExisted(String word){
	    Node current = root;
        
	    for(int i = 0; i < word.length(); i++){    
            if(current.subNode(word.charAt(i)) == null)
                return false;
            else
                current = current.subNode(word.charAt(i));
        }
        if (current.isEnd == true) return true;
        else return false;
    }
    
    public boolean isPrefixExisted(String word){
	    Node current = root;
	    for(int i = 0; i < word.length(); i++){    
            if(current.subNode(word.charAt(i)) == null)
                return false;
            else
                current = current.subNode(word.charAt(i));
        }
        return true;
    }
}

class Node {
    char content; // the character in the node
    boolean isEnd; // whether the end of the words
    int count;  // the number of words sharing this character
    LinkedList<Node> childList; // the child list
  
    public Node(char c){
        childList = new LinkedList<Node>();
        isEnd = false;
        content = c;
        count = 0;
    }
  
    public Node subNode(char c){
        if(childList != null){
	        for(Node eachChild : childList){
	            if(eachChild.content == c){
	                 return eachChild;
	            }
        	}
        }
        return null;
   }
}
```

然后是 Boggle Class 的代码

```java
public class Boggle {
	
	public static void main(String[] args) {
		char[][] board = {{'d', 'g', 'h', 'i'}, {'k', 'l', 'p', 's'}, {'y', 'e', 'u', 't'}, {'e', 'o', 'r', 'n'}};
		String path = "input.in";
		ArrayList<String> allWordsList = findAllValidWords(board, path);
		for (String str : allWordsList) {
			System.out.println(str);
		}
	}
	// find all the valid words in the board, and the dictionary can be found in "path"
	public static ArrayList<String> findAllValidWords(char[][] board, String path) {
		ArrayList<String> allWordsList = new ArrayList<String>();
		if (board == null || board.length == 0 || board[0].length == 0) return allWordsList;
		// trie stores the dictionary
		Trie trie = generateTrie(path);
		for (int i = 0; i < board.length; i++) {
			for (int j = 0; j < board[0].length; j++) {
				boolean[][] isVisited = new boolean[board.length][board[0].length];
				helper(board, i, j, new ArrayList<Character>(), isVisited, trie, allWordsList);
			}
		}
		return allWordsList;
	}
	
	public static void helper(char[][] arr, int i, int j, ArrayList<Character> list, boolean[][] isVisited, Trie trie, ArrayList<String> allWordsList) {
		if (i < 0 || j < 0 || i >= arr.length || j >= arr[0].length || isVisited[i][j]) return;
		list.add(arr[i][j]);
		isVisited[i][j] = true;
		String word = convertToString(list);
		// the prefix doesn't exist in the dictionary
		if (!trie.isPrefixExisted(word)) {
			list.remove(list.size() - 1);
			isVisited[i][j] = false;
			return;
		}
		//satisfy the condition
		if (word.length() >= 3 && trie.isWordExisted(word)) {
			allWordsList.add(word);
		}
		//different directions
		helper(arr, i - 1, j, list, isVisited, trie, allWordsList);
		helper(arr, i - 1, j - 1, list, isVisited, trie, allWordsList);
		helper(arr, i - 1, j + 1, list, isVisited, trie, allWordsList);
		helper(arr, i, j - 1, list, isVisited, trie, allWordsList);
		helper(arr, i, j + 1, list, isVisited, trie, allWordsList);
		helper(arr, i + 1, j, list, isVisited, trie, allWordsList);
		helper(arr, i + 1, j - 1, list, isVisited, trie, allWordsList);
		helper(arr, i + 1, j + 1, list, isVisited, trie, allWordsList);
		list.remove(list.size() - 1);
		isVisited[i][j] = false;
	}
	// convert arraylist to string
	public static String convertToString(ArrayList<Character> list) {
		StringBuilder sb = new StringBuilder();
		for (Character c : list) {
			sb.append(c);
		}
		return sb.toString();
	}
	// create trie based on the dictionary 
	public static Trie generateTrie(String path) {
		Trie trie = new Trie();
		try {
			BufferedReader br = new BufferedReader(new FileReader(path));
			String strWord = "";
			while ((strWord = br.readLine()) != null) {
				trie.insert(strWord);
			}
			br.close();
		} catch (IllegalArgumentException e) {
			System.out.println(e.getMessage());
			return null;
		} catch (IOException e) {
			System.out.println(e.getMessage());
			return null;
		}
		
		return trie;
	}
}
```
---

> 经典的停车场设计题 

考察面向对象的能力，。

现实中对事物的处理的方法和软件设计的面向对象的方式是非常的相似的。现在假设我们正采用面向对象的方法为停车场设计一套软件

1. 你设计的目的是什么？（即明确需求）。为了管理停车场中的空车位，还是统计停车场中各类车的种类，还是协助残障人寻找车位等等。
2. 设计中主要的对象是什么？（车，停车位，整个停车场，停车计时等等；而对这个抽象的概念还有很多的子类（轿车，卡车，残疾助动车）；停车位的同样会有残障车位的子类。）
3. 还漏掉了什么东西了吗？我们用什么方式来停车计费呢？是按时间收费还是免费？我们可以添加一个叫Permission的类用来处理不同的付费方式。Permission类的两个子类可以分别应对付费停车和免费停车。这样的话每个停车位就应该有个方法得到一个Permission对象来决定收费问题。
4. 我们这个设计中又如何来查询某辆是在停车位上？采用怎么样的方法来实现效率会高一些呢？

---

> 实现linkedlist 

LinkedList 和 ArrayList 一样，都实现了 List 接口，但其内部的数据结构有本质的不同。LinkedList 是基于链表实现的（通过名字也能区分开来），所以它的插入和删除操作比 ArrayList 更加高效。但也是由于其为基于链表的，所以随机访问的效率要比 ArrayList 差。

看一下 LinkedList 的类的定义：

	public class LinkedList<E>
	    extends AbstractSequentialList<E>
	    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
	{}

LinkedList 继承自 AbstractSequenceList，实现了 List、Deque、Cloneable、java.io.Serializable 接口。AbstractSequenceList 提供了List接口骨干性的实现以减少实现 List 接口的复杂度，Deque 接口定义了双端队列的操作。

在 LinkedList 中除了本身自己的方法外，还提供了一些可以使其作为栈、队列或者双端队列的方法。这些方法可能彼此之间只是名字不同，以使得这些名字在特定的环境中显得更加合适。

---

> 语言的基础知识

Python, C++

> 把一个linked list里的元素两两交换。

TODO

> 在一棵binary search tree里找到小于某个值的最大元素。

TODO

> 设计一个地铁售票机。

TODO

> implement Singleton.

TODO

> 位运算

复习

> java，设计address book。问要实现哪些功能。add, remove, get

TODO

> @@ 找出一个Binary tree 的 deepest node

TODO, BFS

> compress string

TODO

> Valid 一串 id : 假设id从1开始，到N (N 已知)， validate 一下这串id是否是从1到N每个数字都出现，且仅出现了一次。 输入是一个array包含了这些数字，输出是true （valid）或者false （invalid）

我用了类似于bucket sort的方法，见到一个数字就减去1，放到相应的index里面。遇到重复的或者超出数组范围的就返回false，否则继续检查下一个index，直到所有数字通过测试，返回true。

> longgest common sequence of letters

leetcode longgest common prefix, trie-tree

> Product of Array Exclude itself

lintcode, 另一个array来存储从左到右或者是从右到左的连续product

> Array a has ten elements ,array b has nine elements that are from array a, find the missing one, do not use extra space

TODO

> @@@ remove duplicated character from string without changing character order

hashset 做

> @@@ find least common ancestor

第二题的tree node还给了parent，所以只要向上找最先出现的公共parent就行就行了，非常简单。

in binary tree or binary search tree

> @@ min stack

TODO

> atoi

atoi的时候 我问了他好多test case， 比如溢出， 比如invalid char 的处理，比如 nagetive / positive， 他让我实现了一个最base 的 one，其他的问题以throw exception的方式抛出。

> 斐波那奇数列不用递归

dp 搞起

> @@ 1到100缺一个数找到它

先用公式去减，优化的化就二分

> leetcode 积水题目

TODO

> 反转字符串同时判断回文

TODO

> 写个bst class，各种操作写出signature即可，然后让在class里面写一个找successor的方法

TODO

> pow(x,n)

因为n是一个整数，所以顿时简单了很多，迅速的给出了一个recursive的方法

## On Site

> strStr

leetcode

> Unique Path

leetcode

> 数组里找最大连续sum，说了半天不要 
用brute force

leetcode

> lru implementation + sychronization analys (哪些方法需要 synchronize， 为什么)

这一题感觉时间略紧张，要在40分钟不到的时间里写完，还要求尽量把code 的readability 弄好一点

> 给定两个array, 从每个array的最后一个元素起，交叉放置两个array里面的元素，形成一个新的array。就像洗牌一样，交叉放置，从array1的最后一个开始

TODO

> tree 的 dfs traversal

TODO


> 写hash function

TODO

> Valid Telephone number

TODO

> 给一个数字，转化成字符串，有多少种可能
比如123，1=>a, 2=>b, 3=>c; 12=>l, 3=>c;  1=>a, 23=>w

TODO

> 设计一个算法比较字符串间的相似度，返回[0,1]间的一个数

open question


1. 白人大叔, 问了大概50个CS fundamental questions: 
先是OOD:
encapisulation, inherentance是啥, 有啥好处, 什么时候用inheritance vs composition. 1point 3acres 璁哄潧
Polymorphism:
Static polymorphism
dynamic polymorphism
Why it is useful?
Design patterns:
what is a design patterns?
how many categories do you know?
然后一堆multi-threading和OS问题:
Read lock? Write lock? Reader-Writer lock (shared-exclusive) lock? Nested lock? Conditional variable?. 

Deadlock? How to prevent deadlock?
Pthread related..
mandatory lock vs advisory lock


2. 印度哥哥, Code to implement a function to calculate 一个函数在一个点的导数


3. 女印度director, 吃饭闲聊加reverse a linked list. 说这个组还挺受公司重视

4. 中国哥哥, Merge k-sorted array and find top k elements in a BST. 楼主写太快了时间用了只有一半,然后他就走了,凉了楼主一个人做了快半个小时.

5. 一脸严肃的印度大妈. split a linked list. 用python写完了说我不懂Python, 再让用C++写一遍... 啥是virtual function. overloading vs overwriting


第一轮是个口音标准的印度阿姨，问了resume的project，然后接着问了在very very very long string找not duplicate.
第二轮是个abc。同问了resume，同时在撸主讲的时候问了怎么存储前n个最少数据。coding是copy random pointer。. From 1point 3acres bbs
面完这两轮撸主觉得表现还不错。然后问题来了。。。第三轮。。烙印三哥。。。。
第三轮。。一开始看我做个分布式的project，然后开始疯狂攻势。假设distributed system 各种scenario问应对方法。。撸主上课也没讲这么深入好木好。。撸主各种糊弄。然后coding叫我用pthread implement thread safe class. 神马？！pthread? 撸主也不熟好木好。。问能不能用java，幸亏可以。写了一大堆，然后各种不满意让我改。面试时间都过了45分钟，还抓着撸主问什么process vs. thread, process 之间怎么communicate。。期间撸主只听懂烙印50%口音。。。面完之后直接感觉又悬了。

