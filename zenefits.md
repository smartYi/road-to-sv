# Zenefits

> Input: { 'A1' :  '=C1*(C2-C3)',  'A2' : '=A3+B3', 'A3' : '1' ... },  Output: { 'A1' : 25, 'A2' : 50, 'A3' : 1 ...}. 同时热心的小印还提供了两个可用的method： tokenize('=C1*(C2-C3)') = ['C1', 'C2', 'C3',]， 和 exprEval('=C1*(C2-C3)', {'C1' : 1, 'C2' : 2, 'C3' : 3,}) = 1*(2-3) = -1。 第一个方法可以将一个表达式解析出所有的token，第二个方法则是给出一个已知value的token的map 和表达式，计算出表达式的值， 但是要求表达式中的每一个token，需要在map里存在。题目要求计算出每一个token对应的整数值。

楼主的solution：楼主觉得这个题目很眼熟，但是肯定是没有做过的，首先就直接想到了用DFS去计算出所有的token的值，这里tokenize方法十分有用，估计这也是另一种提示吧。大约20分钟code写完了，小印很快的发现了一些不足，让我想想有没有一些特殊的test case我这个算法是没办法通过的，也不知道怎么的，很快就反映出来如果A1 -> B1, B1 -> A1, 这种case会让我的算法出现stackoverflow。于是加上了一个判断。

> Sorted Linked List

高频

> implement sparse matrix addittion/multiplication

后来剩下最后10分钟，面试官给了提示，让我在5分钟内写完，我想大概是说给了你提示，5分钟搞不定，那就没救了。还好没有辜负他，等他上个厕所回来，我已经把code写完了。

> 给一个matrix,`{{'s','a','t'},{'b','y','e'},{'g','t','y'}}`, 一个字符串数组，`{"say", "yet"},` 让我找出在matrix将一些相连字符串连接起来，最终能形成给的字符串数组的字符串。怎么样定义相连呢？就是matrix周围的所有字符串都是相连的(包括对角线)，例如`[1,1]`相连的是`[0,0], [0,1],[0,2],[1,0],[1,2],[2,0],[2,1],[2,2]`。

典型的DFS + TrieTree. 我第一反应就是用TrieTree也把这些words组织起来，然后再用dfs在TrieTree里面找。小印哥可能没有想到我会选用TrieTree，于是有些惊讶，估计他的solution是准备用hashmap来做了。于是他反问我，我准备实现TrieTree吗，时间够不够。楼主装逼的说给我15分钟。后来也算是在15分钟内把code写完了。小印还比较满意，后面时间还够，就又闲聊了一些zenefits的事情。

> Box是一个类，然后他提供一个方法isSame(Box a, Box b) 来判断两个Box是不是相同的，题目input: 一个Box数组[Box, Box ....], 然后找出Majoriy， 也就是出现次数超过一半的Box。

这其实就典型的Majority问题，有著名的算法，大家可以自行Google

> 给一个matrix，R代表Robot, E代表entrance， X代表可走了路径， B代表block障碍。 然我求有几条unique的path。

    [ R, B, X]
    [ X, X, X]
    [ X, X, E]

不讲了，还是DFS， 怎么那么喜欢DFS。最花时间的是运行code，需要改一些编译错误，还是挺花时间的。建议大家多用用hackerank.

> robot best merge point. 给一个matrix，1代表robot, X代表障碍，找到一个点，这个点到所有的robot的距离之和要最小（有多个robot，有可能不只2个）。

    [
        0  0  0  0  0
        0  1  X  0  0
        0  X  0  0  0
        0  0  0  1  0
        0  0  0  0  0
    ]

Again DFS. 但是这道题有些厉害的地方是，需要从哪里开始进行DFS，DFS得到什么结果。楼主反正是在面试官的提示下找到这个重要的point

> 四个人过河，有一根蜡烛能烧15分钟，每个人分别花1，2，5，8分钟过河，每次过两个人，每次过桥必须拿着蜡烛。问，能在蜡烛烧完前所有人都可以过河嘛？

智力题

> LRU cache。 顺利写完。

Fibonacci 的几种写法，写出decision tree, 用recursive的方法减少重复计算。 估计是挂在这了。

> Two sum in BST

有个小的bug。.