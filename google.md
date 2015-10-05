# Google

> 给一个先递增后递减的数组，找到最大值。这数组是非严格递增递减，也就是说有重复。

写了个Binary Search之后面试官问我如果输入是1233321怎么办

> Moving Average. 给的是一个stream,求前K个数的平均值

用个dequeue存K个数就行了

> 给你一个URL让你求这个URL访问到的页面的和。面试官给了一个API，输入URL返回URL所连接到的URL

写个DFS然后调用这个API，再用个Set去重就好了。

> Given a large integer array, count the number of triplets whose sum is less than or equal to an integer T

一开始猛然间以为是3-Sum的题，仔细想想不太一样，细节问题挺多。最后写了一个O(n2lgn)的算法，然后问大叔更简便的有木有，大叔迟疑了片刻说应该有数学相关的取巧办法。。

LeetCode新题3Sum Smaller，先排序，再双指针O(n^2)就可以，参考http://www.cnblogs.com/jcliBlogger/p/4736809.html

> Given an array of Ad (profit, start_time, end_time) and a timespan [0, T], find the max profit of Ads that fit the timespan.

先说了穷举法O(2^n), 然后说了贪心法（不是最优解），最后用DP解决。小哥态度灰常好，给了很有用的提示, 就像自家人啊。

> M x N large board, with only white and black blocks, all black blocks are connected (vertically or horizontally), find the minimum rectangle boundary that contains all black blocks.

还好心地给了提示和假定。感觉交流互动非常好，可惜最后差了一点，没能想出O(n^2)以内的算法。

第三题貌似是遍历玩board 记录 最left，最top，最bottom，最right坐标就可以了吧。。。。 应该是O(n*m).

如果n*m很大，还有一种方法是先指定一个黑点（如果不让指定，可以随机猜），因为黑点都是connected的，所以可以用递归搜索黑点，搜索的同时记录最left，最top，最bottom，最right坐标

就像图像填充里的floor fill，如果黑点的个数是K，复杂度就是O(K)

> Design a algorithm to initialize the board of Candy Crush Saga. With M x N board, Q types of candies. (Rules: no 3 for run after initialization, must contain at least one valid move at the beginning)

小哥说话很和气，先让我介绍了一个project，于是兴致勃勃地讲了做过的一个游戏。他于是拿出手机给我看了这个，说那就出一道游戏题吧

先把Candy简化成数字，类型数组就成了[0, 1, 2, ..., Q-1]。规则一共三条：随机（至少玩家看上去是）；不能一开始就有3个共线的；开局至少有一步能保证有消除（不然没法玩。。）

1. 我首先关注的是前两条规则，因为觉得第三条只是小修改（尽管如果初始化完成后，再修改成符合第三条，可能导致连锁反应）。因为“随机”一直在我心头挥之不去，所以首先想到的是遍历所有格子，每次随机挑一个Q放进去，但立刻意识到这样很有可能导致死锁，尤其Q很小的时候，然后举了个死锁的例子。
2. 因为之前考虑过Q很小，所以最简单就是{0, 1}两种，立刻想到国际象棋棋盘：两色相间一定能填满而且无冲突。然后想到能不能先按照国际象棋棋盘填满，然后在这个基础上进行”随机化“。假如我有个遵循前两条规则的函数random()，对棋盘进行随机化。因为是在01棋盘改，所以第一遍下来可能还是很”假“，但既然这个函数是遵循前两条规则的，那么大可放心的多执行几遍。就像PS的滤镜叠加使用~ 然后开始讨论这个random()函数，大体思路是遍历棋盘，对每个格子有0.5 的概率进行”尝试修改“。(随机化就是一个慢慢tweak的过程，这个参数后面提到要根据实际效果调整）。尝试修改的算法就是：q = random(0, Q); 以q为中心向上下左右四个方向，一共有6条长度为3的线会造成潜在冲突，因此逐个检查一遍，假如无冲突就把当前位置替换为q。最后根据实际效果决定是否再来几次~
3. 然后就剩第三条规则：开局至少有一步能走。我上面阐述的时候就一直有个感觉，每局开始看似随机但一定有定势。然后让小哥打开手机游戏开了两局看了下，果然，每局开头一定会有{V型, _/型} 中的一种或两种排列，保证挪一步就能消除。跟小哥聊了这个想法之后，我的做法就是01棋盘生成后，随机选一个排列，比如V型，然后在棋盘上随机选一个（也可以多个）位置，把这个位置画出的V全部mark成不可修改。然后在这个基础上跑上面提到的random()算法。第三条规则也可以有很多随机性，必须类型选择，类型对应位置、个数的选择。

> 给一个整数n，返回前n个fibonacci number相邻pair的最后一个digit。听起来有点绕，其实不难，比如：

    n = 8
    fibonacci: [0, 1, 1, 2, 3, 5, 8, 13]
    return: [ (0, 1), (1, 1), (1, 2), (2, 3), (3, 5), (5, 8), (8, 3) ]

很简单，先生成前n个number到list，然后第二遍loop返回pair。

follow up：怎样确定有没有cycle？如果n很大怎么办？

维护一个visited set，每次检查一下。

很多solution: 可以用long， 用string， 或者cache，或者distributed system。

然后问最多需要查多少次就可以确定cycle的存在？

这个自己脑抽了，理解了好久才明白过来，就是计算有多少个可能的pair，10 * 10。

> 给一颗二叉树，返回重复的subtree。比如：

                      1
                    /   \
                  2      3
                 /     /   \
               4      2     4
                     /
                    4
                    
结果应该返回[ ( 2 -> 4), (4) ] 两颗树。

也很简单，BFS遍历，存每个substree到list里，然后用双重循坏找。其中需要写一个helper function判断两棵树是否相同。

> 设计一个fraction number 的class，要求实现equals和String toDecimal()

toDecimal是leetcode原题，equals的话注意先得到gcd，然后除了之后再比较。还有分子分母是否为0，符号不一样等等都需要考虑。细节挺多的。

> 给一个二维boolean array， true代表greyed， 要找出所有可能的正方形

    0 1 0
    0 0 0
    1 0 0

一共有8个正方形（边长为1的7个，为2的1个，为3的0个）。注意matrix的边长可能不等。

用DP对matrix先预处理，方法有点类似之前地里面经出现的计算matrix中rectangle面积的题，dp[j]代表从(0, 0) 到 （i, j) 里面所有可用的grid的数量。具体方法大家可以自己思索一下。\

> Given a list of boarding passes, each boarding pass contains two cities, say A-B, generate travel itinerary.  

Typical topology sort

> moving range average. Given an input array，always output the average of k item, known as window size.

use a counter and cur variable to keep track of current information.

> Given a sorted input array, write an O(lgN) algorithm to find if there is a number has appears more than 1/4 times

Binary search. Say value of current mid is k, search first appearance index of k on left and last index of mid element on right. The number of times k appears is rightIndex-leftIndex+1.

>  rotate a pixel picture by 90 degree

build a mapping function between old index and new index.

> 给int n，求n所有factors，然后问问算法的running time

开方之后遍历一波

> 上的follow up，给distinct primes list，回传所有由这些primes组成的数字。再follow up，那给的primes有重复呢？

要好好想一想

> 给个字典，要求找出一对单词。所有字符都不同。然后长度乘积最大

    cat 
    dog 
    feed 
    pull 
    space

+ cat and dog share no letters, and have a product of 3*3 = 9 
+ space and dog share no letters, and have a product of 15 
+ space does not work with either feed (e) or pull (p) 
+ feed and pull is the best answer for this dictionary (4*4 = 16) 

The dictionary will only contain lower case letters (a-z) 
=> return the numeric value of word length product, e.g. 16 for the example above

> 给一个list，长度未知，里面全是质数，然后给一个k，要求输出前k个正整数，这k个数都能被list里的质数因式分解

lz一看到这个题目就闻到了一股递归的气息，然后用大概10分钟blablabla讲怎么用stack存每次分解后的子结果。。谈笑风生了一会儿发现面试官没声音了。。之后他说不如你先写个brute－force，我去研究一下你说的算法。。

> 实现这么一个函数 void longestSubstr(string s, int m). 在s中，找到最长的字串， 使其恰好含有m个distinct char.

比如：

+ 输入 aabbccedf, 3 返回 aabbcc（含有a b c三个不同的char）
+ 输入 abcdbcedf，3 返回 bcdbc （含有b c d 三个）

> 给一个字符串s由单词组成， 比如“i have a dream”。 要求把这个字符串添到一个m x n的网格里，同一个单词不能被cut off，每一句之间空格相连。问最多添满多少个整句。

follow up （m and n are much larger than the length of s, 怎么办）

> 有一个2维数组A。实现两个函数：1) void update(int x, int y, int v), 就是更新A[x][y]的值(v).  2) int regionalSum(int x1, int y1, int x2, int y2)：就是求（x1, y1）和（x2, y2）构成矩形的所有元素和。

系统很少用带update而经常使用reginalSum, 如何设计减少复杂度。

> 给一个二维矩阵，
实现两个方法，set(int x, int y), sum(int x, int y, int val)
set 方法设一个点的值， sum得到这一点到左上角（0， 0）点左右值的和。

（1）set多，sum少
（2）set少，sum多

第二种情况主要是要在set时就把sum算好，sum就是constent time了，但是set要注意更新。

> LeetCode原题，search in 2D matrix

leetcode

> oil pipeline

其实就是在一个坐标轴上若干个点找距离各点之和最近的一个点，先sort这一组点，再找它们的median，median就是要找的这个点。如果点的数量是双数，那么求的这个点就可以取中间两点之间任意一个点。

> Given a string S, return minimum number of chars that you can add to its back to obtain a palindrome. Explain the complexity

TODO

> Write a function to return expected number of tosses of an unbiased or biased coin until there are m (>= 1) heads in a row,supposing the head probability in a toss is p: 0 < p <= 1. If the result is not integer, round it.

TODO