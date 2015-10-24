# Amazon

+ 面试的时候，人家给你两个选择，一定要选一个，分析清楚各种优劣即可
+ 设计题的时候，一定要考虑 scale 的问题，并且要着重在这上面表现，多说一点，考虑各种情况。

## 算法题目

### leetcode: letter combination

输出 valid word

### 无向图距离

graph从一个source,然后输出指定的距离的node,无向图。我写的bfs level,中间 卡了一下,想了半天才写出来level 怎么做。

### 数组排序

两个数组,一个大,一个小。都是排好序的。然后大数组后面有几个空位置和小数组 长度相等,排好序的数组应该都在大数组上面。不要用额外的space

### 单词权数

给你一个字典一个,map<Character, Integer>存每个字母的 权,输入是一个List<Character>(可能有重复)让你求这些字母求可以组成最大权数的 word list(List<String>). 

### leetcode: Largest K element

经典题目

### leetcode 题目

一个 无序数组,判断它是否是连续序列,其中0是magic number。也是leetcode原题。

### @@@ 合并数组

将已经有序的N个ArrayList merge 为一个有序的List,LC原题,做完后因
为其中用到了Heap,让顺便实现了一下Heap的插入,中间卡壳了一下,想到Heap可以用数组表示后,才顺利解决。

首先讲了一下bruce force的方法,然后问了时间复杂度和空间复杂度。并且怎么推导出来的 都有问哦;

然后提到了用min heap, 然后写了除了constructor以外,所有min heap的构建方法。最后 问了问heap的内部机制

### 均匀分开数组

给两个列表,每个列表装着一样的元素,合并两个列表,并要求短的列表的元素把长列表元 素尽可能均衡分开。比如列表为AAAAA和BBBBBB,返回BABABABABAB;又比如 AAAAAA和BBBB,返回AABABABABA。

### 字母统计并查找

给一列城市的名字,并且告诉你你的家是哪个城市,并且每个城市的受欢迎指数是城市名字 里的辅音字母数除以元音字母数,比如Shanghai的指数就是5/3=1.66666,要返回和家乡 城市的指数最近的其它城市,没有的话返回NOT_FOUND。 题目都很简单,不需要复杂算法,就是在写之前想好edge cases,有很多刁钻的测试用例 需要注意,好好想清楚应该所有测试都能过。

### LRU Cache

扩展到集群,用Hash映射解决数据分布。

### 实现 heap

基础算法

### xml 文件读取算法

类似于 json 解析

### 缺一个整数

leetcode 原题n size 数组存储0 - n 缺一个整数 找出来, 画图油漆桶算法

### First N Prime Numbers

暴力

### Map-Reduce 的 Reducer方法

暴力

### 城市到达

给你一个API,input是一个城市,输出是邻近城市,问实现一个方法,判断 两个城市之间是否可以到达

BFS

### 城市路径

继续上问,实现一个方法,找出两个城市之间所有的路径。答:DFS

### 用户特殊请求

继续上问,如果client有很多特殊请求(如不想经过城市A,不想途转太多城
市)怎么办?答:Strategy Design Pattern

### @ isValidBST()

leetcode

validate BST using three different methods, array vs linked list, how to implement hashtable

### 数组单词

给你一个dictionary(all English word)和一个一维的character数组 (duplicated),找出所有可以用数组表示的字典里的单词

答:遍历字 典,判断是否能被表示

### @@ 偶数/奇数次元素

给一个array 找出里面出现奇数次的数字。我先用hashmap, 后来用int[],最后小哥说 no extra space

一个数出现odd times 其他even times 找这个数 实现2种方法

### 复制链表

linkedlist 每个节点多一个random pointer,问怎么复制,hashmap,很简单, 没让写代码。

### 求平方根

给一个平方数,求平方根。就/2 /2 /2。。。 log(n)的复杂度

### 循环链表删除

循环列表,删除指定节点,写完让想一些edge case 测试一下代码正确性

### 递归复制数组元素

给你一个array,不用循环把所有元素复制到另一个array里,用recursive

### 找中位数

无限输入流找中位数,follow up,如果memory非常小怎么办

### @ 有序数组的 two sum

先讲了用hashmap, 然后用binary search。follow up问了一下如果有duplicate 怎么办

2sum要求输出所有可能的pair,而且原数组可能有duplicate

### 3 sum

leetcode

### word ladder

介于1和2中间吧,只需要输出最短的一条路径就可以

### @ 矩阵 island

给一个矩阵,连通的非0元素组成一个island,问这个矩阵里island的数量. 

### min/max stack

先问了stack的pop, push, peek的时间复杂度。写了stack类的pop, push 和 peek方法。然 后让在自己设计的stack类的基础上实现min stack

### 分配 job

一个set里面装的是很多job, 每个job有开始的时间和结束的时间。现在需要分配这一堆job给 尽可能少的machine, 要求是每一个machine在同一个时间段只能做一个job,问怎么让 machine数量最少。

我用了bruth force的方法,in worst case, time complexity is O(n ^ 2), 当讨论怎么降到 O(n)的时候,没时间了

### 数组转 range

把一串连续的整数表示为一个range, range包含begin和end.比如3,4,5,6就表示为 begin=3,end=6的range

一个排好序的整数数组,返回所有的range. 比如1,2,3,5,7,8,10返回[1,3],[5,5],[7,8],[10,10] follow up是用迭代器实现。。。

### trie tree

给一个prefix字符串,找出字典中所有包含这个prefix的词

先问用什么数据结构表示这个字典,我说用trie

然后用dfs实现找词。面试官问bfs和dfs区别。

然后要求实现往trie里添加词的方法。

最后面试官提出apple和apples在我的代码里没法区分出来,经提醒我在trieNode中加一个 mark表示一个词的终止。然后重新修改了部分代码。

### @ 罗马数字转 Integer

口述Integer转罗马数字。我提出要把IX, IV这种也算一种特殊的罗马数字单位。

于是面试官出题: 给一个整数,返回这个整数对应的罗马数字中最大的单位代表的整数,比如8表示为罗马数字为VIII,最大单位为V,即5。 我用一次遍历实现的,面试官问怎么优化,我说可以二分搜索,复杂度降到O(logn)

### @@ compress string

input: aabccc output: 2ab3c 然后讨论可以不可以再优化

如果要 in place 怎么做

### reverse string

让我说出所有想到的方法 然后讨论复杂度 然后挑了其中一种写code

### @@ find lowest common ancester

最开始假设有parent, 然后没有parent的话怎么做,我说了一个top down approach,然后他说要优化,然后我说 了botom up,最后实现bottom up的代码。期间每种都要比较复杂度。

### 最长 leaf nodes 包含的 nodes 个数

binary tree找到距离最长的两个leaf nodes包含的nodes个数。

### 从 log 文件提取电话号码

从 log 文件提取电话号码

### 矩形重叠

一个window里有两个矩形,怎么判断它们有没有overlap

### decoding leetcode

给了一个dict,比如a->1,b->2...z->26, 那么“ab"->12; 现在给你一个数字,让你decoding。

### 存数据

有很多整数,和n台电脑,怎么把这些 整数分别存到不同的电脑上,第一台电脑上存得数要比第二台小,依次类推。

### 表达式求值

给了一个表达式让求值,比如7-10/5, 我说先转换成逆波兰式,然后用逆波 兰式求值。 他让我写逆波兰式求值部分的代码。

### substring 判断

给定一个string。比如说是“hello”。 判断输入的string是不是他的substring。输入 “ell” 返回true,输入“eo”返回false。(我面完这题觉得很简单,但后来同学告诉我, 这题要用kmp算法做)

写完之后,问如果“oh”和“ohel”这种也算是原string的substring,应该怎么办。(把两个元数组相加,其他代码不变)

### @ 地图搜索

输入是一个城市的地图的大小(m,n),和一个list,里面包含所有有locker的地理位置。 输出一个m*n的二位数组,每个单元的值为到最近locker的距离。问时间复杂度(这题要从 每个locker同时开始bfs)

### 二维数组搜索

给一个二维数组,都是整数,每行都是从小到大排列,每列也是从小到大。(但是第二行的 第一个不一定大于第一行最后一个),给一个target,判断是否存在于这个矩阵中。 先用mlgn,再用lgn做。(lgn的方法就是对整个二维数组做binary search,然后每次可以 把问题缩减为原来的3/4)

### 括号检验

一个string里面有括号,怎么判断这些括号是合法的,拓展: 如果还有中括号,大 括号呢

### 比节点大的最小节点

给一个binary search tree里面的一个节点,找到比这个节点大的最小节点

### 日期间隔

输入两个时期,有天,月,年,计算出这两个日期差多少天

### 迷宫出路

一个迷宫, 指定起始点和终点,找任意一条路线即可。自己设计数据结构存路线和迷宫。

### 回文串

是否是回文串

### 两个栈实现队列

两个栈实现一个队列

### 排序基本

heapsort quicksort怎么做 给你数据模拟一下流程

### unique ip

给你一个file 里头是IP地址 然后统计多少个unique的

### string 统计

统计一个string里头各字母出现了多少 排序输出字母和对应次数

### 最大连续数组和

给定一个数组 求连续k个数之和的最大值 以及从哪个index开始

### 打僵尸

就是一圈zombies,给你一 个starting point,然后每隔k步你shoot一个zombie,完后依次下去直到剩下一个 zombie,让你输出最后剩下的zombie。我用linkedlist做的,然后while loop每次shoot一个zombie,num-1删除那个listnode,直到最后num变成1跳出循环。

### 数组中元素出现次数

给你一个sorted array,完后输入一个数,让你output它在array里面出现的次数。这道题比 较tricky的地方是,corner case的考虑。每个人都能想到binary search, 但是在处理 binary search的时候有些corner case要处理好。

---

## 设计题目

### poker game

OOD design a poker game and different methods in each class.1

### 电梯设计

经典设计题

### 解析表达式

解析表达 式,例如“1*2 + 3”,关键要用面向对象的思想设计。需要可以拓展,支持括号,支持开 方之类的。这题感觉自己没思路,瞎答了一通

### Parking Lot

传统题目

### 设计一个机场调度系统

和面试官多 交流有哪些实体需要抽象吧,要想清楚业务流程里的参与者和用例,还有之后的耦合内聚怎么优化等等。

### 查询机制

给一个数组和一个范围,要返回范围里的最小值,然后扩 展到如何缓存这个查询。一开始搞我以为是算法题,DP?排序?扯了半天,后来发现原来 是OOD,囧。。。关键是如何解耦缓存查询和实时查询这俩类,然后扯了扯工厂模式,依 赖注入等等。

### 设计电话本

http://www.cs.gordon.edu/courses/cs211/AddressBookExample/

### 生物分类

设计数据结构表示生物学门纲目科属种

### OOD (Tic-tac Game) Followups

CC150

### HashTable出现很多collision怎么办

(Rehashing)load factor

### truck tracking system

要求实现查询truck的地点,每天的行程之类 的。一开始写了一个truck class,然后在引导下一点一点的增加功能

### 火箭打彗星

火箭每次发射之后需要五分钟准备时间,雷达会监控彗星出现的距离,每 个火箭只能打最近的一个彗星,问用什么data structure 记录彗星的情况,要求时间复杂度 O(1)的

### 图书推荐系统

类似facebook, twitter那样的好友系统,可以看评价,可以写评价,有好友
列表一类的。

### 顺序 hashmap

重写一个HashMap,多加一个功能,可以按照insert的顺序输出出来

### 设计一个邮件系统

新题目

### Java 基础

概念问了 reflection(我不 清楚。。),garbage collector具体是怎么工作的以及几种GC的区别(我也不会), abstract class跟 interface区别(就知道这个。。) 然后让实现一个interface 实现两个功 能: 一个是能够check input的单词拼写有没有错误 二是可以给你alternative words。这 个我答的不太好。我开始说用trie,他说用一般的,就用hashSet

### deck of cards

给一个deck of cards, 实现两个methods,一个是 deal,这个method可以发一张random的card。 另一个method是shuffle,就是类似 reset deck。

## 家具质量测试

OO design:要给一个家具工厂的所有家具做质量测试(压力测试,是否易燃等等)

---

> Round Robin，求average waiting time，就是 http://www.1point3acres.com/bbs/thread-142143-1-1.html 这个帖子里说的这个题，友情提示楼里有代码~

TODO

> 0，1的list，每天更新，每个单元的新值看前一天的左右邻居，一样就是1，不一样就是0，求n天以后的list。

TODO

> Shortest Job First

地里很多人贴过面经了

