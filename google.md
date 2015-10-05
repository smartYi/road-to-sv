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