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

leetcode 131, 132

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

> How many paths are there for a robot to go from (0,0) to (x,y), supposing it can only move down and move right.

解题分析：这是一个具有收敛性的数量问题，需要在x轴和y轴两个维度上使用二维DP。

递推关系：Paths(i, j) = Paths(i+1, j) + Paths(i, j+1); i和j分别表示起点的横纵坐标。DP table可以是一个二维数组，这样的解答在实际面试中已经足够好。但事实上，根据上述DP table的优化理论，我们至少可以选取一个维度来化简，因为我们只关心紧接的那一行/列的结果，而不在乎之前的行/列的结果，我们可以将新的结果叠加上去，这样可以仅用一个array作为DP table，详见解答。

参考解答：

```
int uniquePaths(int m, int n) {
    int ways[n] = {0};
    
    ways[n-1] = 1;
    for(int i = m - 1; i >= 0; --i)
        for(int j = n - 2; j >= 0; --j )
            ways[j] += ways[j+1];
    return ways[0];
}
```

> Given a value N, this N means we need to make change for N cents, and we have infinite supply of each of S = { S1, S2, .. , Sm} valued coins, how many ways can we make the change?

解题分析：本问题以及其他类似描述的Coin Change问题，都属于典型的分布在二维整数空间上的计数问题，用DP。

重要的是理解如下递推关系：
对于第j种coin，无非是选择和不选择使用两种可能(i是目标钱数，j是当前coin的下标)

ways(i, j) = ways(i-s(j), j) + ways(i, j-1);  i~[0,N], j~[1,m]

注意到在j这个维度上，只在意紧邻的上一步的结果，而不在意过去几步的结果，最后仍然是求和，因此上一步的局部解也只是当前解求和过程当中的一部分，完全可以覆盖，仅用一个变量表示，因此DP table可以简化为i方向一维空间。

参考解答：

```
int minNum(vector<int> S, int m, int n) {
    vector<int> table(n+1, INT::MAX);
    table[0] = 0;
    for(int i = 1; i <= n; i++)
        for(int j = 0; j < m; j++) {
            if( i >= s[j] && table[i] > table[i-s[j] )
                table[i] = table[i-s[j]] + 1;   
    }
    return table[n];
}
```

### 最长子序列类型的问题

对于“最长子序列”问题(即有限空间内，满足一定条件的最长顺序子序列)，本身具有很强的聚合性，可以以如下方式解答：用DP Table来记录以当前节点为末节点的序列的解(至少固定问题的一端，因此不是以“当前节点或之前节点”为末节点)的解，并根据递推关系，由问题空间的起点到达问题空间的终点。

> Find the longest increasing subsequence in an integer array. E.g, for array {1, 3, 2, 4}, return 3.

解题分析：用DP Table来记录以当前节点为末节点的序列的最大长度，其数值取决于当前节点之前的所有节点：如果当前节点对应的数组数值大于之前的某个节点，那么可以将当前节点对应的数组数值append在该节点的最长序列之后。最终，我们在DP table中将当前节点的结果更新为所有可能解的最大值。递推关系如下:

maxLength(i) = max{ maxLength(k), k = 0~i-1 and array[i] > array[k] } + 1;

另外，如果需要输出最长序列，那么无非就是对于每个节点额外记录一个index，该index是以当前节点为末节点的最长序列中，前驱元素在数组中的下标。

参考解答:

```
int longestIncreasingSubsequence( int arr[], int n) {
    vector<int> maxLength(n, 1);
    int global_max = 0;
    for (int i = 0; i < n; i++)   
        for(int j = 0; j < i; j++)
            if ( arr[i] > arr[j] && maxLength[j] + 1 > maxLength[i] )
                maxLength[i] = maxLength[j] + 1;

    for(int i = 0; i < n; i++)
        if ( global_max < maxLength[i] )
            global_max = maxLength[i];

    return global_max;
}
```

> Given the heights and weight of each person in the circus, compute the largest possible number of people in tower. (each person has to be both shorter and lighter than the person below him/her)

解题分析：将People以Height进行排序，在高度相同的情况下，按照Weight其次排序。这样，把问题划归为以Weight为基准的LIS(Longest Increasing Subsequence)问题(因为选出的序列自然满足高度递增)，这样就与上一题 无甚区别。

> Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

解题分析：只观察以当前节点为末节点可能的最大sum，并记录一个global sum。对于当前节点，需要判断加入对应数组元素能否使得sum变大。递推公式如下

sum[i] = max(sum[i-1] + A[i], A[i])

因为需要连续的子数组，故计算当前的最大sum，只在乎前一次计算的结果，因此用一个变量每次覆盖即可 (正如我们之前关于简化DP空间的描述)。

参考解答：

```
int maxSubArray(int A[], int n) {
    if(n <= 0)  return 0;
    int max_sum = A[0], sum = A[0];
    for(int i = 1; i < n; i++){
        sum = max(sum + A[i], A[i]);
        if(sum > max_sum)
            max_sum = sum;
    }
    return max_sum;
}
```

> Suppose you are traveling along a circular route. On that route, we have N gas stations for you, where the amount of gas at station i is gas[i]. Suppose the size of the gas tank on your car is unlimited. To travel from station i to its next neighbor will cost you cost[i] of gas. Initially, your car has an empty tank, but you can begin your travel at any of the gas stations. Please return the smallest starting gas station's index if you can travel around the circuit once, otherwise return -1.

解题分析：这是一个比较难理解的问题，但其本质上还是选择一个序列，只不过这个序列是环形的。事实上，可以考虑对问题进行如下操作：对于第i个加油站，它能够给车子提供的净动力为array[i] = gas[i] – cost[i]。问题转化为，找到一个起始位置index，将array依此向左shift，即index->0(index对应新的数组下标0)，index+1->1…，使得对于任意0 <= i < n，满足序列和subSum(0, i)大于0。

首先，考虑什么情况下有解。经过上述转换，很明显有解的情况对应于sum(array)大于等于0。

那么，剩下的问题是在有解的情况下，如何选择一个正确的起始点。类似与一般序列问题，考虑将当前节点作为序列的末节点。如果从记录的开始节点(index)起，到当前节点的过程中，一旦出现subSum小于0，那么从开始节点到当前节点的所有节点都不能作为开始节点，因为在过程中一定会出现subSum小于0的情况，否则累计的结果不会为负。那么，开始点至少是index+1。另一方面，可以证明，如果从记录的开始点出发可以走到第n个加油站，即subSum(index, n)大于0，那么该开始点一定能走完全程。

参考解答：

```
int canCompleteCircuit(vector<int> &gas, vector<int> &cost) {
    int size = gas.size();
    int subSum = 0, sum = 0;
    int array[gas.size()];
    int index = 0;
   for(int i = 0; i < size; i++){
        array[i] = gas[i] - cost[i];
        sum += array[i];
    }
    if (sum < 0)
        return -1;
    for(int i = 0; i < size; i++) {
        subSum += array[i];
        if(subSum < 0) {
            subSum = 0;
            index = i + 1;
        }
    }
    return index;
}
```

> Please write a function to calculate the Longest Common Subsequence (LCS) given two strings.
LCS for input Sequences “ABCDGH” and “AEDFHR” is “ADH” of length 3.
LCS for input Sequences “AGGTAB” and “GXTXAYB” is “GTAB” of length 4.

解题分析：同时遍历两个序列，考虑以当前两个节点为末节点的序列的common subsequence长度。如果其对应的字符相等，那么可以使得LCS长度+1，即append当前字符；否则，保留较优的结果。递推关系如下：

    Length(i,j) = (str1[i-1] == str2[j-1]) ? Length(i-1, j-1) + 1 ： Max { Length(i,j-1), Length(i-1,j) }

参考解答：

```
int lcs( string str1, string str2) {
    vector<vector<int>> length( str1.size()+1, vector<int>(str2.size()+1);
    for(int i = 0; i < str1.size(); i++) {
        for(int j = 0; j < str2.size(); j++) {
            if(i == 0 || j == 0)
                length[i][j] = 0;
            else if (str1[i-1] == str2[j-1])
                length[i][j] = length[i-1][j-1] + 1;
            else
                length[i][j] = max(length[i-1][j], length[i][j-1]);
        }
    }
    return length[str1.size()][str2.size()];
}
```

特别地，如果当前节点的解，既依赖于前驱问题的解，又依赖于后驱问题的解，但这两部分又互相独立，则可以分别自左开始DP，计算从最左节点到当前节点的结果；自右开始DP，计算从最右节点到当前节点的结果；再用同一个DP Table来合并解。具体举例如下。

> Given an integer array, each element is a stock price on day i (i is the index). Design an algorithm to find the maximum profit. You may complete at most two transactions(one buy, one sell).

解题分析：假设在i位置买入，j位置卖出，那么对于i，j之间的某个节点，如何计算其利润？可以分成两部分计算：从i到当前节点的利润，这部分只和前驱问题有关；从当前节点到j的利润，这部分只依赖于后驱问题。并且这两部分相互独立，可以把结果叠加在DP Table上。直观上说，相当于在当天卖出又立刻买进，相当于增加了两次虚拟操“作。因此，可以以当前节点为分界线，第一次DP自左向右，只计算到当前为止可获得的最大收益(即从之前某天买入，当前卖出的最大收益)；第二次DP自右向左，计算从当前开始可获得的最大收益(即从当前买入，之后某天卖出的最大收益)。两部分收益之和即为总收益。

参考解答：

```
int maxProfit(vector<int> &prices) {   
    if(prices.empty())
        return 0;
        
    int n = prices.size();
    vector<int> dp(n, 0);      
        
    // left scan
    int minPrice = prices[0];
    for(int i = 0; i < n; ++i) {     
        dp[i] = prices[i] - minPrice;
        if(prices[i] < minPrice)
            minPrice = prices[i];          
    }
        
    int globalProfit = 0;
    
    // right scan
    int maxPrice = prices[n-1];    
    for(int i = n-1;i >= 0;--i) {
        if(prices[i] > maxPrice)
            maxPrice = prices[i];
        dp[i] += maxPrice - prices[i];
        globalProfit = max(globalProfit, dp[i]);          
    }
        
    return globalProfit;
}
```

> Given an array of integers, write a function to replace each element with the product of all elements other than that element.

解题分析：当前节点的解，既和左边的元素有关，又与右边的元素有关，两者相互独立，可以用双向DP。左遍历DP计算积累到目前为止的乘积，右遍历DP计算从目前开始到最后的乘积。

参考解答：

```
void replaceWithProducts(int elements[], int n) {
    int product = 1;
    vector<int> table(n,1);
    for(int  i = 0; i < n; i++){
        table[i] = product;
        product *= elements[i];
    }
  
    product = 1;
    for(i = n-1; i >=0 ; i--){
        elements[i] = table[i] * product;
        product *= elements[i];
    }
}
```

> You are given an array of n non-negative integers. Each value means the height of a histogram. Suppose you are pouring water onto them, what is the maximum water it can hold between a left bar and a right bar (no seperation)?

解题分析：当前节点的解，取决于左右两边的海拔高度，可以左一遍DP求出左侧海拔高度，再右一遍DP求出右侧海拔高度，同时积累结果：当前节点的储水量，等于左侧最高海拔与右侧最高海拔的较小值减去当前节点的海拔。

参考解答：

```
int trap(int A[], int n) {
    if(n <= 0)  return 0;      
    vector<int> dp(n,0);
        
    int left_max = 0, right_max = 0,water = 0;
        
    for(int i = 0; i < n; i++) {
        dp[i] = left_max;
        if(A[i] > left_max)
            left_max = A[i];
        }
        
    for(int i = n -1; i >= 0; i--) {
        if(min(right_max,dp[i]) > A[i])
            water += min(right_max,dp[i]) - A[i];
           
        if(A[i] > right_max)
            right_max = A[i];       
    }
    return water;
}
```

### 用Memorization (Top-Down)解决收敛结构问题

Memorization是自顶向下形式的动态编程，并且受到的制约更少 ，自然也可以用来解决前述的问题(但空间上可能效率不及自底向上形式的DP)。

Memorization的核心在于，在原有递归框架下，存储子问题的计算结果，在重复计算子问题时返回已经计算的值。

值得注意的是，这里所谓的“重复计算子问题”，在自顶向下结构下必须与前驱节点无关，因为子问题并不知道原问题是如何到达当前节点的。举例来说，求二叉树从根节点到叶节点的权值最大路径，对于当前节点到叶节点的路径与之前如何到达当前节点没有关系，只要计算当前节点到叶节点的路径，就一定是重复的计算，可以直接返回结果。作为反例，在一个字母矩阵当中寻找词典中的单词，当前路径能否构成单词，不仅与之后走的过程有关，也与之前的过程有关。因此，从当前节点出发，哪怕走过相同的路径，也不能看成是重复计算的子问题。在回溯部分我们会进一步讲解。

> Given a set of boxes, each one has a square bottom and height of 1. Please write a function to return the tallest stack of these boxes. The constraint is that a box can be put on top only when its square bottom is restrictively smaller.

解题分析：放置在当前盒子上的盒子序列是否满足条件，与当前盒子下面已经放好的盒子(前驱条件)无关，只与将要放在当前盒子上的盒子序列有关(后驱节点)，并且在计算后驱条件时，以每个盒子为底的最长序列必然会被反复计算。因此可以将盒子作为Memorization的key，以满足条件的最长序列作为Memorization的value。

参考解答：

```
vector<Box> createStackDP( Box boxes[], const int num, Box bottom, unordered_map< Box, vector<Box> >& stackCache) {
    vector<Box> max_stack;
    int max_height = 0;
    vector<Box> new_stack;
  
    // memorization
    if( stackCache.count( bottom ) > 0 )
        return stackCache[ bottom ];
    else {
        for( int i = 0; i < num; i++ ) {
            if( Box[i].canBeAbove( bottom ) ) {
                // solve subproblem
                new_stack = createStackDP( boxes, num, Box[i], stackCache );
            }
            if( new_stack.size() > max_height ) {
                max_height = new_stack.size();
                max_stack = new_stack;
            }
        }
    }
  
    max_stack.insert( max_stack.begin(), bottom );
    stackCache[ bottom ] = max_stack;
    return max_stack;
}
```

> Given a string and a dictionary of words, please write a function to add space into the string, such that the string can be completely segmented into several words, where every word appears in the given dictionary.

解题分析：本题事实上是e.g.3的另一种描述，为的是让你了解自上而下一样可以解决用自底向上解决的问题。考虑使用Memorization的理由是：要求满足特定条件的所有解，当前节点以后的拆分方法是否满足条件，与当前节点之前的拆分方法无关。并且在计算子问题时，会有子串的拆分被重复计算。因此可以用substring作为Memorization的key，拆分后的结果集合作为Memorization的value。

参考解答：

```
vector<string> wordBreak(string s, unordered_set<string> &dict, unordered_map<string,vector<string> >& cache) {
    // memorization
    if(cache.count(s))
        return cache[s];
        
    vector<string> vs;
        
    if(s.empty()) {
        vs.push_back(string());
        return vs;
    }
        
    for(int len = 1; len <= s.size(); ++len ) {
        string prefix = s.substr(0,len);
        if(dict.count(prefix) > 0) {          
            string suffix = s.substr(len);

            // solve subproblem
            vector<string> segments = wordBreak(suffix,dict,cache);          
            for(int i = 0; i < segments.size(); ++i) {
                if(segments[i].empty())
                    vs.push_back(prefix);
                else
                    vs.push_back(prefix + " " + segments[i]);
            }
        }
    }
        
    cache[s] = vs;
    return vs;
}

vector<string> wordBreak(string s, unordered_set<string> &dict) {
    if(s.empty())
        return vector<string>();
    unordered_map<string,vector<string> > cache;
    return wordBreak(s,dict,cache);
}
```

### 用回溯法( 自上而下 )解决发散结构问题

对于发散性问题(例如“所有组合”，“全部解”)，可以选取其问题空间“收敛”的一端作为起点，沿着节点发散的方向(或者说，当前节点的多种选择)进行递归，直到

1. 当前节点“不合法” 或 
2. 当前节点发散方向搜索完毕，才会return

举例来说，考虑树的遍历：根节点方向就是“收敛”的一端，节点发散的方向就是子节点。对于某个树的节点，其孩子就是当前决策的多种选择。当达到叶节点是，其孩子为NULL，即达到“不合法”的边界条件。回溯法的核心在于选择哪些方向/决策，才是最合理，不重复的。所谓“剪枝”(pruning)，就是指：只选择尽可能少的、可能到达“胜利条件”的方向，而不是搜索当前节点的所有发散方向。这样，可能将幂指数级的复杂度降低到阶乘级。

值得注意的是，invalid前的最末节点未必意味着胜利(不是所有的问题走通就算满足条件)，胜利的节点也未必代表不需要继续走下去(比如寻找到一个单词之后，继续走下去可能能找到以这个单词为前缀的另一个单词)。因此我们强烈推荐将invalid的判定与胜利条件的判定总是分开，即使在某些题目中它们是一致的。当然，如果经过充分剪枝之后，所有搜索只会沿着“正确”的方向行进，那么当前节点“不合法”往往也就意味着胜利条件。

如果需要记录决策的路径，可以用vector<int> &path沿着搜索的方向记录，在满足胜利条件时记录当前path(通常是将path存入vector<vector<int>> &paths)。

注意，我们传入的path是引用形式，属于全局变量。Backtracking(回溯)本身隐含的含义是，在访问完这个节点返回时，需要恢复原本的状态(即回到该节点)，以访问其他路径。具体实现时，意味着需要:

1. 在return前，删除path中的当前节点。
2. 如果搜索的方向有出现环路的可能，那么可以使用bool []或unordered_map来记录该节点是否已被使用，在访问时以及return前维护。

如果以传值形式传入path，由于path成了局部变量，故在某些情况下不需要显式回溯，相当于把状态复制给了子问题。可能有人觉得这样做比较直观，但其缺点是需要额外的空间。

回溯法的典型模板如下所示：  

```
void backtracking( P node, vector<P> &path, vector<vector<P> >&paths ){
        if(!node )  // invalid node
            return;

        path.push_back(node);

        bool success =  ;  // condition for success
        if( success )  
            paths.push_back( vector<P>(path.begin(),path.end()) ); // don't return here
        
        for( P next: all directions )
            backtracking( next, path, paths );
        path.pop_back();
        return;
}
```

> Given n pairs of parentheses, generate all valid combinations of parentheses. E.g. if n = 2, you should return ()(), (())

解题分析：由于题目要求找出所有解，故属于发散性的DP，用backtracking。核心在于：对于当前节点，也哪些可用的选择。对本题而言：

+ 如果所剩的左括号大于0，那么继续添加左括号一定是一种决策；
+ 如果所剩的左括号少于右括号，那么补充右括号也一定是一种决策；

应该按照决策的选择方向进行回溯。

参考解答：

```
void parenthesesCombination(int leftRem, int rightRem, string &path, vector<string> &paths ){
    if(leftRem < 0 || rightRem < 0)
        return;
        
    if(leftRem > 0) {
        // make choice        
        path.push_back('(');
        parenthesesCombination(leftRem-1,rightRem,path,paths);

        // backtracking       
        path.pop_back();
    }
        
    if(leftRem < rightRem) {
        // make choice
        path.push_back(')');
        rightRem -= 1;
        if(rightRem == 0)
            paths.push_back(path);    // winning
        parenthesesCombination (leftRem, rightRem, path, paths);

        // backtracking        
        path.pop_back();          
    }       
}

vector<string> generateParenthesis(int n) {        
    vector<string> res;
    if(n <= 0)  
        return res;      
    string path;
    parenthesesCombination(n, n, path, res);
        
    return res;
}     
```
> Please write a function to find all ways to place n queens on an n×n chessboard such that no two queens attack each other.

解题方向：本题是经典的八皇后问题。由于需要找到所有解，属于发散性问题。对于每一行，我们枚举可以将当前的皇后放在哪一列，所有目前为止可行的列都可以作为选择进行DP，即每次选择方向时都经过充分剪枝，使得每一步都朝着胜利条件前进。这样，最后得到的解“一定是需要的解。

复杂度分析：回溯总共n步，每次供选择的方向为n。经过剪枝之后，可以认为复杂度小于n!。

参考解答：

```
bool checkValid( int row1, int col1,int *rowCol ) {  
    for( int row2 = row1 - 1; row2 >= 0; row2--) {
        if( rowCol[row2] == col1 )
            return false;
        if( abs(row1 - row2) == abs( rowCol[row2] - col1 ) )
            return false;
    }
    return true;
}

void placeQ( int row, int rowCol[], vector<int*>& res ) {
    if (row == GRID_SIZE) {
        //winning
        int p[GRID_SIZE];
        for( int i =0; i < GRID_SIZE; i++)
            p[i] = rowCol[i];
        res.push_back(p);
        return;
    }

    int col = 0;
    for(col = 0; col < GRID_SIZE; col++) {
        if( checkValid(row, col, rowCol) ) {
            rowCol[row] = col;
            placeQ( row+1, rowCol, res);
            // because we rewrite rowCol[row] everytime, 
            // so backtracking is inferred here
        }     
    }
}
```

> Given a collection of integers that might contain duplicates, return all possible subsets.

解题分析：如果不存在重复，那么对于当前节点，分为选取当前元素和不选取当前元素两条路径；

当存在重复时，那么对于当前节点，也可以分为选取当前元素和不选取当前元素。但是，如果没有选择当前元素，那么也一定不能选择后驱节点中与当前元素重复的任何元素，否则会产生完全重复的路径；

至于如果选取了当前元素，那么之后这个支线内部的重复问题，支线自然会解决，不需要关心。原因在于，子问题和原问题相互独立，子问题不需要关心如何来到当前节点。这样既保证了对于set{1,2,2}，subset{1, 2}只被选择一次，也保证了{1,2,2}会被选择在内。

```
void subsetsWithDup(int index, const vector<int> &S, vector<int> &path, vector<vector<int> > &paths) {
    if(index == S.size())
        return;
    for(int i = index; i < S.size(); i++) {
        if( i != index && S[i] == S[i-1])
            continue;
        path.push_back(S[i]);
        paths.push_back(path);
        subsetsWithDup(i+1, S, path, paths);
        path.pop_back();
    }
}

vector<vector<int> > subsetsWithDup(vector<int> &S) {
    vector<vector<int>> paths;
    vector<int> path;
    paths.push_back(path);
    sort(S.begin(), S.end());
    subsetsWithDup( 0, S, path, paths );
    return paths;
}
```

> Given a collection of numbers that might contain duplicates, return all possible unique permutations.

解题分析：选择后驱元素中的一个与当前节点交换，然后再将后面的节点作为子问题考虑。由于有重复元素，而重复元素对当前节点的影响是相同的，因此应该去重：把相同元素的替换作为同一个回溯方向/选择来处理，一旦发现是已经处理过的相同元素，则直接跳过。

参考解答：

```
void permuteHelper(int index, vector<int> &num, vector<vector<int>> &paths ) {      
    if(index > num.size()) {
        return;
    }

    if(index == num.size()) {
        paths.push_back(num);
    }
              
    unordered_set<int> used;
    for(int i = index; i < num.size(); i++ ) {
       // handle duplicates        
        if(used.count(num[i]))
            continue;

        // make choice
        swap(num,index,i);
        permuteHelper(index+1,num,paths);
        // backtracking
        swap(num,index,i);        
        used.insert(num[i]);
    }    
}

vector<vector<int>> permuteUnique(vector<int> &num) {
    vector<vector<int>> paths;
    permuteHelper(0,num,paths);
    return paths;
}
```

> Solve a boggle game(http://en.wikipedia.org/wiki/Boggle), with a dictionary given as unordered_set.

解题分析：

1)	猜词游戏(boggle game)实际上就是寻找有限路径，判断路径能否组成单词。从matrix的每一个slot出发进行回溯，直到超过matrix的边缘，或者当前slot已经被访问过(环路可能)，或者路径超过了最长单词的长度，就三种invalid条件。胜利条件是当前的路径组成一个词典中的单词，但胜利条件之后应当继续回溯(统一地，对任何回溯问题，在处理上胜利条件之后都应该继续回溯，只不过很多时候胜利节点就是末节点，因此回溯下去也会因为invalid条件返回而已)。

2)	考虑对这个问题剪枝，可以将dictionary改写成prefix tree，这样才每一步查询时，可以根据当前路径序列是否是一个有效的prefix，增加一个invalid的条件，大幅提高搜索的效率。Prefix tree的DFS，可以与问题的回溯同步进行，“可以与问题的回溯同步进行，matrix单条搜索路径中的节点，对应prefix tree单条搜索路径中的节点。

利用回溯的另一种情景是：就算是处理收敛结构问题，如果无论从哪一端出发，都避免不了“(部分)当前节点的解依赖后驱节点”(也就是说，当前节点，如果不能获知后驱节点，就无法得到有意义的解)的情况，那么可以也用回溯解决。(请参考Chapter 3 Stacks and Queues的叙述)

### 用Divide and Conquer 解决独立子问题

如果能将问题由几个孤立但类似的部分组成，则可以优先选择使用D&C策略：将问题分割解决，再合并结果。特别地，如果期望将问题的复杂度由O(n)进一步降低到O(logn)，一般总是可以联想到使用D&C策略，将问题分割而治。

> Implement pow(x, n).

解题分析：由于pow(x, n)相当于n个x相乘，左半边乘积与右半边相互独立且类似，所以可以用D&C策略进行二分。注意，二分之后需要处理n大于0，等于0，小于0等情况。”

参考解答：

```
double pow(double x, int n) {      
    if (n == 0)
        return 1.0;
    if( abs(x) < numeric_limits<double>::epsilon() )
        return 0.0;
    double half = pow(x,n/2);
    if(n%2 == 0)
        return half*half;
    else if(n > 0)
        return half*half*x;
    else
        return half*half/x;
}
```

> Evaluate an infix notation( without parentheses ). E.g 4+3*2 returns 10

解题分析：对于一个不包含括号的算术式，如果看到*或\，则应该立刻计算。如果看到+或-，则应该解决加减号之后表达式的子问题(divide)，并且把子问题的结果叠加到当前结果(conquer)。在这种情况下，由于当前节点已经读入了字符(加号或减号)，我们需要吐出这个字符，目的是将其作为正负号传给子问题。在读取数字的时候，我们可以用sscanf，关于sscanf的具体解释请见http://www.cplusplus.com/reference/cstdio/sscanf/。

参考解答：

```
double readNum( char * &str) {
    double num;
    sscanf( str, "%lf", &num );
    str++;
    while( isdigit(*str) || *str == '.' )
        str++;
    return num;
}

double evaluate( char *str) {
    if( *str == '\0' )
        return 0.0;
  
    double res = 1.0;
    char op = '*';
  
    while( op == '*' || op == '/' ) {
        if( op == '*' )
            res *= readNum(str);
        else
            res /= readNum(str);
        op = *str++;
    }
  
    return res + evaluate( --str);
}
```
