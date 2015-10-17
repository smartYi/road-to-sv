# Recursion and Dynamic Programming

递归和动态编程(Dynamic Programming, DP)是算法类问题中的难点所在。算法的核心在于找到状态转移方程，即如何通过子问题解决原问题。在“The Rules”部分，我们先介绍递规和DP的普适特性；再通过“模式识别”，从题目的关键字出发，判断什么样的题目适合用递归和DP，并且总结算法模版

### Build approach from sub-problems to the final destination

递归和动态编程能解决的问题都有一个特性：原问题(problem)可以分解成若干个子问题(sub-problem)，只有先解决了子问题才能进一步解决原问题。子问题的解决方式形式上与原问题一致。从题目描述来看，可以提示我们尝试用递归、DP解决的关键词有：compute nth element (value, sum, max, etc.), return all the paths, return all the combinations, return all the solutions…

既然动规与递归都能解决相同类型的问题，那么DP和递归有什么不同？最大的区别在于，DP存储子问题的结果，当子问题已经被计算过，直接返回结果。因此，当需要重复计算子问题时，DP的时间效率高很多，但需要额外的空间。

特别地，具有聚合属性的问题(Aggregate)，例如在所有组合中寻找符合特定条件的特解(比如二叉树求一条从根节点到叶节点和为定值的路径，或第n个元素)，或最优解(包括最值)，或总和，或数量的问题(其实看一下SQL里的聚合函数(aggregate function)就明白了)。因为这些问题它们只需要一个聚合的或者特殊的结果，而不是所有满足条件的集合，所以它们具有很强的收敛性质。这类问题往往也可以用DP来解决。

本章节将问题处理的每一个最小的元素/步骤，称为节点，就好比一维/二维/三维数组中的一个element，或者每一次递归中独立解决的那个元操作。 我们把节点空间“两端收敛”的问题，归结为收敛结构；将节点空间“发散”的问题，归结为发散结构。形象地说，收敛问题是由若干个子问题共同决定当前状态，即状态的总数逐渐“收敛”，例如斐波那契数列问题(前两个节点决定当前节点)；发散问题是当前状态会衍生出多个下一状态，例如遍历已知根节点的二叉树(下一层的状态以指数形式增加)。抽象地说，能够在多项式时间内解决的问题，是收敛问题(P类问题)，不能在多项式内解决的问题(如阶乘级或指数级)，是发散问题(NP类问题)。定义“收敛”和“发散”是为了方便本章节描述和区分这两类问题，并非是公认的准则。

### 递归的空间与时间成本

对系统层面上说，操作系统是利用函数栈来实现递归，每次操作可视为栈里的一个对象。递归的时间成本随递归深度n(单条路径中递归调用的次数)成指数增长；空间复杂度为O(n)。

### 自底向上与自顶向下

从子问题解决原问题, 无非是两种方法，自底向上(Bottom-Up)与自顶向下(Top-Down)，形式上前者对应iteration，利用循环将结果存在数组里，从数组起始位置向后计算；后者对应recursion，即利用函数调用自身实现，如果不存储上一个状态的解，则为递归，否则就是DP。举个斐波那契数列(0,1,1,2,3,5…)的例子：

1) 自底向上

```
int array[n] = {0};
array[1] = 1;
for (int i = 2; i < n; i++)
    array[i] = array[i-1] + array[i-2];
```

这里，为了说明方法，采用数组存储结果，空间复杂度O(n)。事实上，额外空间可以进一步缩小到O(1)：利用几个变量记录之前的状态即可。由于我们记录了子问题的解，故给出的方法就是DP。事实上，自底向上的方式往往都是通过动态编程实现。

2) 自顶向下

```
int Fibonacci(int n)
{
    if(n == 0)
        return 0;
    if(n == 1)
        return 1;
    return Fibonacci(n-1) + Fibonacci(n-2);
}
```

为计算Fibonacci的第n个元素，我们先自顶向下地计算子问题：第n-1个元素和第n-2个元素。由于没有存储子问题的运算结果，我们给出的方法是递归。然而，Fibonacci(n-1)与Fibonacci(n-2)包含很多重复的子问题，所以DP效率更高。如果用一个全局数组，将子问题的解存储到数组的对应位置，在重复计算的时候直接读取计算结果，那么就是DP的解法。

**我们再次强调：动态编程的核心在于，如果在一个问题的解决方案中，子问题被重复计算，那么就可以利用记录中间结果，达到用空间换取时间的目的。**

以下如不额外说明，动态编程特指迭代形式的自底向上的动态编程，并将自顶向下，递归形式，在递归过程中用哈希表记录中间计算结果的DP，称作Memorization。

Memorization的一般形式是: 建立以input为键，以output为value的哈希表：

```
T func(N node, HashTable<N, T>& cache) {
    If (cache.contains(node)) {
        return cache.get(node);
    }
    …
    T sub_res = func(next_node, cache);
…
    T res = G( sub_res … );  //当前子问题的解，依赖于更小的子问题(s)
    cache.set(node, res);
    return res;
}
```

从解决问题的角度来说，用自底向上的DP，固然通常可以节省递归本身的空间开销，但有很多缺点和局限：较难理解，边界条件较难处理，只适用于问题的节点空间是离散的整数空间，必须一步步邻接、连续地计算(无论是不是每一个节点的结果都会被用到)。而Memorization，则灵活得多：可以从递归形式轻易修改得到，也更符合普遍的思维过程，并且没有上面说的这些局限，子问题只有在被需要的时候才会被计算。尤其是在某些情况下，不仅需要aggregate的结果，还需要获得achieve这个结果的路径，这时候就算用自底向上的DP，也需要记录prev节点，最后需要递归回溯得到路径，那么节省递归空间开销的优势，也荡然无存了。

尽管有上述局限, 自底向上的DP，仍然可以作为很好的思维和编程训练，熟练之后写法也很简洁精妙，本章节也会举很多例子，但完全不用过于痴迷这类技巧性较强的解法，否则就是买椟还珠了。毕竟，从面试的角度讲，在这么短时间内，期望面试者用这类DP解法写出一个完整程序，并不是非常普遍的。

### 算法策略

以下回顾一些利用到DP思想的经典算法策略：

+ 分而治之(Divide and Conquer)
    + 这里只谈狭义的D&C，即将问题分成几个部分，每一部分相互独立，互不重叠，假定每个部分都可以得到解决来进行递归调用，合并每一部分的结果。
    + 例如Merge Sort， Quick Sort (Merge Sort的divide容易，但Conquer/Merge复杂，Quick Sort的divide复杂，但Conquer/Merge容易)
+ 动态编程(Dynamic Programming)
    + 尽可能不重复计算每个子问题，而是将计算结果存储下来，以确定后驱问题的解。
    + 与贪心算法的区别是，会记录下所有可能通向全局最优解的局部解，以便在计算后驱问题时综合考虑多个前驱问题的解。
+ 贪婪算法(Greedy Algorithm)
    + 只做出当下最优的判断，并且以此为基础进行下一步计算。当前判断最优时，不考虑对全局/未来的影响，所以所以从全局来说并不能保证总是最优。
    + 贪心算法每次更新当前的最优解。如Dijkstra算法就是贪心算法的实例之一。
+ 回溯 (Backtracking)
    + 一种暴力(穷举)的深度优先搜索法：搜索，直到节点空间的尽头，然后再返回到上次的节点，再往其他方向深度搜索。
    + 树或图的DFS是回溯的实例之一。

## 模式识别

### 用动态编程(自底向上)解决收敛结构问题

具有强收敛性/Aggregate属性的问题(承前所述，是指关于特解，或最值，或总和，或数量的问题)，都可以用整数坐标映射所有节点，且当前节点的解只依赖于前驱节点(无论是顺序还是倒序)。那么，这类问题往往可以用DP解决。解决的关键是建立子问题的解之间的递推关系：
f(n) = G[f(n-1), f(n-2), … , f(1)] 或
f(i, j) = G[f(i-1, j-1), f(i, j -1), f(i-1,j)]
其中G[ ]表示子问题到原问题的映射关系，例如对于斐波那契数列，有递推式：
f(n) = G[f(n-1), f(n-2)] = f(n-1) + f(n-2)

解决这类问题的时候，可以把上述递推关系写在手边，这样做非常有利于理清算法实现的思路。实际实现算法时，往往以问题的一端为循环开端，另一端为循环终止条件， 将当前节点的解(或往往是，以当前节点为末节点的问题的解，抑或是以当前两个坐标为输入的问题的解)用DP table(即数组)记录下来(如果当前节点只由之前紧接的若干个节点决定，那么用若干个变量也够了)，数组的下标即为子问题的输入变量，也就是递推关系中的函数参数，只是把f(i,j)表示成array[i][j]而已。

如果问题除了要计算动归终点的数值以外，还需要记录具体的到达路径，则可记录每个节点的前驱节点(prev[n])或前驱路径(vector<vector<int> > prev)，然后用终点出发通过backtracking处理成path。这时候记录的前驱们都是经过了DP的剪枝，每一条路径都是符合条件的正确路径。

注意，如果出现类似于“所有解”，“所有路径”等关键词，则用自上而下方法更为直接。之后我们也会给出例子。

> Suppose we have a ladder which has n steps. Each time you can either climb 1 or 2 steps. Please write a function to calculate how many distinct ways that can you climb to the top?

解题分析：本问题描述了一个数量问题，属于前述的强收敛(聚合)性问题，可以用DP。DP的核心在于递推关系：当前节点的值可以由前驱走一步到达，或者前前驱走两步到达，即CountOfWays(n) = CountOfWays(n–1) + CountOfWays(n-2)；由于当前节点只与紧邻的两个节点决定，所以只需要2个临时变量来表示前驱节点的解即可，而不用DP table，因为更老的解我们不需要关心。在实现时，往往边界条件直接用if…then return value的形式，成为递归的出口。

参考解答：

```
int climbStairs(int n) {
    if(n <= 1 ) return 1;
    if(n == 2)  return 2;
    
    int p = 1, q = 2, curr;
    for(int i = 3; i <= n; ++i ){
        curr = p + q;
        p = q;
        q = curr;
    }
    return curr;
}
```

> Compute the nth prime

解题分析：在数学理论中，存在埃拉托斯特尼筛法(sieve of Eratosthenes)，用来解决素数判定问题。如果面试中碰到这个问题，面试官绝对不是希望获得那样的解答，因为这里考察的是编程技能，而不是高深的数学知识。

第n个素数仍然是一个特解问题，我们需要考虑DP。事实上，素数的定义隐含了一个递推关系，即如果n是素数，那么n不能被1 ~ n-1的所有素数整除(事实上，可以优化为1~sqrt(n))，即当前节点与之前的所有节点有关:

+ Prime(n) = G ( Prime(n-1), Prime(n-2), Prime(n-3) … Prime(1));
+ Prime(1) = 2
+ G( )表示不能整除的关系。

参考解答：

```
int GetNthPrime( int n) {
    list<int> primes (1, 2);    // init list: length 1, value 2 (first prime)
    int number = 3;
    while(primes.size() != n) {
        bool isPrime = true;
        for(auto it = primes.begin(); it != primes.end() && (*it)*(*it) <= number; it++) {
            if(number % (*it) == 0)
                isPrime = false;
        }

        if (isPrime) {
            primes.push_back(number);
        }
        number += 2;    // even numbers greater than 2 are not prime
    }
    return *(primes.rbegin());
}
```

> Given a string and a dictionary of words, please write a function to determine if that string can be completely segmented into several words, where every word appears in the given dictionary.

解题分析：这个问题属于在所有组合中，寻求一个特解以满足一定的条件。对于这样的判定性问题，它具有很强的聚合性，可以用DP。

DP的关键在于寻找递推关系：考虑string的前n个字符，那么对于第i个字符(i [0, n))，如果[0,i)可以由一个或多个单词组成(前驱节点，可以直接读取DP table中计算过的结果)，并且[i, n)是一个单词(这一段我们不能去借助局部解，因为目前为止，我们只有[0,1), [0,2) … [0, n-1)的结果，需要利用字典判断)，那么，[0, n)的结果就是true。

参考解答：

```
bool wordBreak(string s, unordered_set<string> &dict) {
    int begin = 0, end = 0;
    string word;
    bool words[s.size()+1] = {0};
    words[0] = true;

    for( int i = 1; i < s.size() + 1; i++) {
        words[i] = false;
    }
    for( end = 0; end < s.size(); end++) {
        for( begin = 0; begin <= end; begin++) {
            if( words[begin] && dict.find( s.substr(begin, end-begin+1) )
                != dict.end()) {
                words[end + 1] = true;
                break;
            }
        }
    }
    return words[s.size()];
}
```

> Given a string s, we can partition s such that every segment is a palindrome (e.g, ‘abba’ is a palindrome, ‘a’ is a palindrome, ‘ab’ is not). Please write a function to return the minimum cuts needed for a palindrome parti“tioning, given string s.

解题分析：本题是最小值问题，有很强的聚合性，用DP。

本题可以分为两个部分：判断字串是不是回文(palindrome)，如何切割原字符串。

对于问题的第一部分，实际上也是一个递归问题，递推关系是：

isPalindrome( i , j ) = (value(i) == value(j)) AND ( isPalindrome(i+1, j-1) OR j – i <= 1 ) ， i和j分别表示substring的首坐标和尾坐标。

注意i的前驱坐标是i+1，j的前驱坐标是j-1， 所以在具体处理时，前者是倒序遍历，后者是顺序遍历，其实也是利用了自底向上，自底向上的处理方向总是与直观理解的方向相反，这样可以确保每次都能调用已经计算过的结果。

再来考虑cut的问题，因为首坐标的遍历顺序是倒序，因此可以将minCut(i)定义为：将原字符串最末字符到第i个字符视为子串，依题意处理这个字串需要的最少cut 数量。所谓的“最少”，同样是一个DP问题，递归式如下(请深刻理解该递归式，所有DP相关的最大／最小问题都可以总结为类似的递归式)：
minCut(i) = min(minCut(j+1)+1)，for i <= j < n, and substring(i , j) is palindrome。

直观上说，如果substring(i , j)是回文，那么一种分割方式就是将i到j视为一个子串，j+1到字符串按照minCut(j+1)的方式分割。这样，minCut(i)需要在minCut(j+1)的基础上再分割一次(substring(i , j))。最终，最外层的最小值符号确保保存minCut(i)是所有这些分割中最优的一个。

注意对于最小值，可以初始化一个显然的最坏的解，在这里就是每一个字符都需要分割的情况。

参考解答：

int minCut(string s) {
    if(s.empty())   return 0;
    vector<vector<bool> > palin(s.size(), vector<bool>(s.size(),false));
    vector<int> minCut(s.size()+1,0);
    for(int i = 0; i <= s.size(); i++)
        minCut[i] = s.size() - i - 1;
       
    for(int i = s.size() - 1; i >= 0; i--) {
        for(int j = i; j < s.size();++j) {              
            if(s[i] == s[j] && ( j - i <= 1 || palin[i+1][j-1] ) ) {
                palin[i][j] = true;
                minCut[i] = min(minCut[j+1]+1, minCut[i]);
            }              
        }
    }
       
    return minCut[0];     
}

特别地，具有简单“聚合”性质的问题，如最值或者求和问题，往往可以进一步优化DP Table的空间。应该更确切地说，如果只在乎紧邻的前一个的局部解，而不在乎前几个局部解的问题，就可以接受每次在计算当前解的时候，替换掉那个最优解。 毕竟，我们只在乎最值，而不在乎次最值(假定在乎次最值，那么就需要两个变量，以此类推)；只在乎总和，而不在乎总和的一部分(所以只要放心地叠加上去就行了，覆盖过去的总和)。其实最简单的例子就是求一个数组内的数的最值/总和：我们自左向右遍历数组，存储当前的最值或者和。所存储的恰恰就是从下标0到当前下标的计算结果，是原问题的一个子问题。在这个过程中，“我们只需要一个全局变量就够了，因为总是可以把过去的局部最优解替换掉。

对于DP table的优化仍然是一个比较tricky的优化，不用强求，建议先解决问题，再寻求简化。用一个数组而不是若干个变量解决问题绝不会成为你面试时的red flag。