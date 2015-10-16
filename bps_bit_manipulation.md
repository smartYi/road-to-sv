# Bit Manipulation

对于网络、操作系统、嵌入式系统等职位的面试，位运算也是常见的题目类型之一。所谓的位运算，是指按二进制进行的运算。常见运算包括求反，与运算，或运算，异或运算及位移。关于位运算的真值表，请参考“工具箱”给出的链接。

在C/C++中，基本的位运算符总结如下，其中运算符优先级为从上到下递减，且<<，>>优先级相同：

| 操作符 | 功能 | 用法 |
| :--: | :--: | :--: |
| ~ | 位求反 | ~var |
| << | 左移(乘法) | var << position |
| >> | 右移(除法) | var >> position |
| & | 位与 | var1 & var2 |
| ^ | 位异或 | var1 ^ var2 |
| 一条竖线 | 位或 | var1 竖线 var2 |

需要注意的是，位运算符只能用在带符号或无符号的char、short、int与long类型上。在实际应用中，建议用unsigned整型操作数，以免带符号操作数因为不同机器导致的结果不同：无符号数左移／右移默认移入的新比特是0。对于符号数，当最高位是1(代表负数)时，有的机器认为右移移入的新比特是1。此外，复杂的位运算建议都用括号强制计算顺序，而不是依赖于优先级，这样做可以增加可读性并避免错误。

用十六进制(hex)定义一个变量如下所示：

    unsigned short value = 0xFFFF;

等价于二进制(binary)定义：

    unsigned short value = 0b1111111111111111;

等价于十进制定义：

    unsigned short value = 65535;
    
## 模式识别

### 基本的位运算

最基本的操作包括获取位、设置位和清除位。获取位可以利用&1：&(0x1 << pos) ；设置位可以利用|1: | (0x1 << pos) ；清除位可以利用&0: &(~(0x1 << pos))。判断某位是否相同用^：(A & (0x1 << pos)) ^ (B & (0x1 << pos))。

> Determine the number of bits required to convert integer A to integer B

解题分析：通常情况下，当需要比较某比特位是否相同时，需要用位异或。如果A和B中某位比特不相同，则位异或得1。所以，题目等价为 A异或B，所得得结果里有几位是1。统计有几位是1可以通过反复Get lowest bit和右移1位(除2)实现。另一个常用技巧是n &= (n-1) 相当于clear最低的一位1。事实上，可以用一个哈希表在O(1)时间内得到一个整数中有多少个比特为1，具体见“工具箱”。

复杂度分析：假设整型有n位，则复杂度O(n)。

参考解答：

```
int numberOfDifferentBits(int A, int B) {
    int diff = A ^ B;
    int count = 0;
    while (diff > 0) {
        count += diff & (0x1);
        diff = diff >> 1;
/* or do the following:
        diff &= (diff – 1); // clear lowest 1 in diff
        count++;
*/
    }
    return count;
}
```

> Given an array of integers, every element appears twice except for one. Please write a function to find that single one.

解题分析：当遇到某些题目需要统计一个元素集中元素出现的次数，应该直觉反应使用哈希表，key是元素，value是出现的次数。扫描整个数组建立哈希表，再次扫描table看哪个元素出现了一次。这样做的时间复杂度O(n+n)。

事实上，能在面试中给出这样的解答已经足够好，而且这种解法具有普适性，应该首先想到。对于这道特殊的题目，能不能做的更好？我们考虑两个数如果相等，二进制表示有什么特点？很明显，当然是二进制表示每位比特都相等。能否通过某种二进制操作把两个相同整数变成常数？答案是异或：相同整数异或得0。如何把这个性质利用到本题？如果我们异或所有得元素，则出现两次的数都相互抵消，最后留下的就是单独的那个。

复杂度分析：扫描数组一次，复杂度O(n)。

参考解答：

```
int singleValue(vector<int> array) {
    int value = 0;
    for (int i = 0; i < array.size(); i++) {
        value ^= array[i];
    }
    return value;
}
```

> Explain what the following code does: ( n & (n-1) ) == 0

解题分析：之前我们看到n &= (n-1) 相当于clear最低的一位1，我们用这个方法统计一个整数的二进制表示中有多少个1。那么( n & (n-1) ) = 0意味着该整数的二进制表示中只有一个1，即它是2的次方。事实上，如果不能立刻看出结果，不妨尝试从1开始列举n的值，看看有什么规律。根据观察到的结果再来倒推表达式的含义。

### 位掩码

对于这类题目，需要做的全部，就是选择合适的位掩码(bit mask)，然后与给定的二进制数进行基本位操作。而掩码，通常可以通过对~0，1 进行基本操作和加减法得到。例如，我们要构造一个第i到第j位为0，其他位为1的位掩码，则可以对~0进行左移操作获得形如111…0000的mask，再对~0进行右移操作，获得形如000…111的mask，最后通过位或(此处相当于相加)得到最终的位掩码。

在寻求得到一个特定的掩码时，还是利用最基本的获取位、设置位或清除位得到所需掩码的形态。另外，应当尽可能避免直接出现常数，比如使用32-i这样的情况(这里默认想要操作一个32bit的整型)，而应当定义一个意义明确的宏，以提高可读性：`#define INT_BIT_LENTH (32)`。

> Given N and M, 32bit integers, how to set i to j bits (bit position as 1,2,3,…32) of N as the value of bits in M.

For example, N = 00000000000000000000000001111011,

M = 00000000001000000011000000011000, i = 10, j = 20, then the result should be:

00000000001000000011000001111011

解题分析： 我们首先根据题意重现我们需要做的操作：

1. 我们需要从M中get第i到第j个比特 
2. 我们需要clear N中第i到第j个比特 
3. 我们需要set N中第i到第j个比特。

对于1)，根据基本操作，get bit需要&1，所以与M进行操作的bit mask在第i到第j位应当为1，其他位为0。对于2)， 根据基本操作，clear bit需要&0，所以与N进行操作的位掩码在第i到第j位应当为0，其他位为1。注意，这个位掩码刚好是前一项操作位掩码的位反运算。对于3)，我们只需要将1)，2)的操作结果进行位或即可。构造所需位掩码的过程如前所述，对~0进行基本操作和加减法即可。

参考解答：

```
#define INT_BIT_LENGTH (32)
void setBits(unsigned int &N, unsigned int M, int i, int j) {
    unsigned int max = ~0;
    unsigned int bitMask = (max << (INT_BIT_LENGTH - i) |
                            max >> j); //11..100..011..1
    N = (M & (~bitMask)) | (N & bitMask);
}
```

> Swap the neighboring odd and even bits in a integer (bit position as 1,2,3,…32).

解题分析： 同样的，我们首先根据题意重现我们需要做的操作：1)我们需要get奇数位比特，根据基本操作易得需要与形如1010…10的bit mask做位与。2) get偶数位比特，根据基本操作易得需要与形如0101…01的bit mask做位与。3) swap意味着将1)中得到的结果右移一位，与2)中得到的结果左移一位，然后做位或操作。由此可见，对于需要进行比特操作的题目，对题目要求进行分步，然后选择合适的bit mask，最后与给定二进制数进行基本位操作都是解题的关键。

参考解答：

```
int swapBits(int input) {
    return ((0xaaaaaaaa & input) >> 1) | ((0x55555555 & input) << 1);
}
```

## 工具箱

### 位运算的定义

[参考链接](http://en.wikipedia.org/wiki/Bitwise_operation)  给出的位运算定义。

### 关于位运算的深入讨论

请参考[链接](http://graphics.stanford.edu/~seander/bithacks.html#CountBitsSetTable)



Excerpt From: Yichao. “程序员面试白皮书.” iBooks. https://itun.es/us/zcOJ6.l