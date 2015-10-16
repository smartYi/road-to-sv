# Array and String

### 数组

在C／C++中，标准的数组可以通过在栈(Stack)上分配空间，或者通过先声明指针，然后用new关键字(或者C中的malloc函数)，在堆(Heap)上动态的分配空间。举例如下：

    // 在栈上定义长度为arraySize的整型数组
    int array[arraySize];    
    // 在堆上定义长度为arraySize的整型数组
    int *array ＝ new int[arraySize];    

使用完后需要释放内存：

    delete[] array;

访问数组时要注意越界问题。

数组可以通过下标随机访问元素，所以在修改、读取某个元素的时候效率很高，具有O(1)的时间复杂度。在插入、删除的时候需要移动后面的元素，所以平均时间复杂度O(n)。通常，数组这个数据结构不是很适合增减元素。如果你想要的操作需要大量在随机位置增减元素，可以考虑其他的数据结构。在C++中，标准库提供vector容器，其本质上相当于一个长度可变的动态数组。

### 哈希表

哈希表(Hash Table)几乎是最为重要的数据结构，主要用于基于“键(key)”的查找，存储的基本元素是键-值对(key-value pair)。逻辑上，数组可以作为哈希表 的一个特例：键是一个非负整数。注意，通常哈希表会假设键是数据的唯一标识，相同的键默认表示同一个基本存储元素。

哈希表的本质是当使用者提供一个键，根据哈希表自身定义的哈希函数(hash function)，映射出一个下标，根据这个下标决定需要把当前的元素存储在什么位置。在一些合理的假设情况下，查找一个元素的平均时间复杂度是O(1)，插入一个元素的平摊(amortized)时间复杂度是O(1)。

当对于不同的键，哈希函数提供相同的存储地址时，哈希表就遇到了所谓的冲突(collision)。解决冲突的方式有链接法(chaining)和开放地址法(open addressing)两种。简单来说，链接法相当于利用辅助数据结构(比如链表)，将哈希函数映射出相同地址的那些元素链接起来。而开放地址法是指以某种持续的哈希方式继续哈希，直到产生的下标对应尚未被使用的存储地址，然后把当前元素存储在这个地址里。

通常而言，链接法实现相对简便，但是可能需要附加空间，并且利用当前空间的效率不如开放地址法高。开放地址法更需要合理设计的连续哈希函数，但是可以获得更好的空间使用效率。需要注意的是，过于频繁的冲突会降低哈希表的搜索效率，此时需要哈希表的扩张。

C++标准库中提供map容器，可以插入、删除、查找键-值对，底层以平衡二叉搜索树的方式实现，根据键进行了排序。严格来说，map并不是一个哈希表，原因是查找时间从O(1)变为了O(log n)，但是好处在于能够根据键值，顺序地输出元素，对于某些应用场景可能更为合适。在C++11中，标准库添加了unordered_map，更符合哈希表的传统定义，平均查找时间O(1)。

### 字符串

在C语言中，字符串(string)指的是一个以‘\0’结尾的char数组。关于字符串的函数通常需要传入一个字符型指针。然而，在C++中，String是一个类，并且可以通过调用类函数实现判断字符串长度，字串等等操作。

## 模式识别

+ 一般来说，一旦出现“unique”，就落入使用哈希表或者bitset来判断元素出现与否的范畴。
+ 一旦需要统计一个元素集中元素出现的次数，我们就应该想到哈希表。



### 使用哈希表

当遇到某些题目需要统计一个元素集中元素出现的次数，应该直觉反应使用哈希表，即使用std::unordered_map或std::map：键是元素，值是出现的次数。特别地，有一些题目仅仅需要判断元素出现与否(相当于判断值是0还是1)，可以用bitvector，即bitset，利用一个bit来表示当前的下标是否有值。

> Determine if all characters of a string are unique.

解题分析：这道题的关键在于“unique”。一般来说，一旦出现“unique”，就落入使用哈希表或者bitset来判断元素出现与否的范畴。进一步地，考虑如何建立键-值对：如果运用哈希表，我们可以直接用字符作为键，出现的次数作为值。特别地，由于“unique”只需要判断哈希表中是否已经存在当前键，所以我们可以通过insert函数的返回值作出相应的判断。

如果运用bitset，我们需要建立字符到整数下标的映射关系。通常，字符都是255个ASCII编码之一，所以可以利用ASCII 索引作为其整数下标的映射：a对应97，b对应98等等。此时，通常可以与面试官进行沟通，作出字符串中仅含有a-z，A-Z的合理假设，这样可以缩小bitset的空间需求。

复杂度分析：哈希表和bitset做法都需要扫描整个字符串，每次插入操作时间复杂度O(1)，假设字符串长度为n，则平均时间复杂度都是O(n)。空间上，每个合法字符都有可能出现，假设字符集大小为m，则平均空间是O(m)。哈希表的数据结构需要占用更多空间，所以bit-set是更合理的数据结构。

参考解答：

```
bool isUnique(string input) {
    bitset<256> hashMap;
    for (int i = 0; i < input.length(); i++) {
        if (hashMap[(int)input[i]]) {
            return false;
        }
        hashMap[(int)input[i]] = 1;
    }
    return true;
}
```

> Given two strings, determine if they are permutations of each other.

解题分析：对于这道题目，我们需要找到两个字符串之间的共同点，即通过某种映射，使得所有置换得到相同的结果。 考虑到置换的特性：无论如何变化，每个字符出现的次数一定相同。一旦需要统计一个元素集中元素出现的次数，我们就应该想到哈希表。于是，对于每个字符串，我们通过统计每个字符出现的次数把string映射成一个哈希表，最后比较两个哈希表是否相同。事实上，这种所谓的映射，其本身也是一个哈希的过程 (只不过哈希的结果是一个哈希表)：我们可以根据哈希的结果判断一个字符串集合中有多少字符串属于同一个置换。针对这道题目，我们还可以利用一些小技巧提前进行快速判断：如果两个字符串的长度不同，那它们一定不是一个置换。

复杂度分析：哈希表需要扫描整个字符串，每次插入操作时间复杂度O(1)，假设字符串的长度为n，则平均时间复杂度都是O(n)。最后比较两个hash是否相同，每个合法字符都有可能出现，假设字符集大小为m，则需要的时间复杂度是O(m)，故总的时间复杂度O(m+n)。空间上，平均空间是O(m)。

其他解法：还有一个比较有意思的解法是分别对每个字符串中的字符按照ASCII编码顺序进行排序。如果是一个置换，那么排序完的两个字符串应该相等。该解法利用了置换的传递性：如果A可以通过置换变成C，而B也可以通过置换变成C，那么A一定能通过置换变成B。这里我们只是通过对string中的字符排序，找出了一个比较容易实现的公共置换。这样做的时间复杂度是O(n log n)，空间复杂度是O(n)。

参考解答：

```
bool isPermutation(string stringA, string stringB) {
    if (stringA.length() != stringB.length()) {
        return false;
    }

    unordered_map<char, int> hashMapA;
    unordered_map<char, int> hashMapB;
    for (int i = 0; i < stringA.length(); i++) {
        hashMapA[stringA[i]]++;
        hashMapB[stringB[i]]++;
    }

    if (hashMapA.size() != hashMapB.size()) {
        return false;
    }

    unordered_map<char, int>::iterator it;
    for (it = hashMapA.begin(); it != hashMapA.end(); it++) {
        if (it->second != hashMapB[it->first]) {
            return false;
        }
    }
    return true;
}
```

> Given a newspaper and message as two strings, check if the message can be composed using letters in the newspaper.

解题分析：在什么情况下newspaper中出现的字符能够组成message？

首先，message中用到的字符必须出现在newspaper中。其次，message中任意字符出现的次数一定少于其在newspaper中出现的次数。一旦需要统计一个元素集中元素出现的次数，我们就应该想到哈希表。键是字符，值是字符在newspaper中出现的次数。那么，如果message中用到的字符没有出现在哈希表中，则构成失败；如果 message用到了某个哈希表中出现的字符，那我们可以将值减少，表示“消耗”了一个字符。如果最终某个值变成了负值，那么message中该字符出现的次数多于其在newspaper中出现的次数，构成失败。

复杂度分析：假设message长度为m ，newspaper长度为n，我们的算法需要hash整条message和整个newspaper，故时间复杂度O(m+n)。假设字符集大小为c，则平均空间是O(c)

参考解答：

```
bool canCompose(string newspaper, string message) {
    unordered_map<char, int> hashMap;
    int i;
    if (newspaper.length() < message.length()) {
        return false;
    }

    for (i = 0; i < newspaper.length(); i++) {
        hashMap[newspaper[i]]++;
    }

    for (i = 0; i < message.length(); i++) {
        if (hashMap.count(message[i]) == 0) {
            return false;
        }
        if (--hashMap[message[i]] < 0) {
            return false;
        }
    }

    return true;
}
```

### 利用哈希表实现动态编程的思想

当处理当前节点需要依赖于之前的部分结果时，可以考虑使用哈希表记录之前的处理结果。其本质类似于动态编程(Dynamic Programming)，利用哈希表以O(1)的时间复杂度利用之前的结果。

> (2-Sum) Find a pair of two elements in an array, whose sum is a given target number. Assume only one qualified pair of numbers existed in the array, return the index of these numbers (e.g. returns (i, j), smaller index in the front).

解题分析：对于数组中的某个下标i，如何判断它是否属于符合条件的两个数字之一？最直观的方法是再次扫描数组，判断target – array[i]是否存在在数组中。这样做的时间复杂度是O(n^2)，效率不高的原因是没有保存之前的处理结果，每次都在做重复的工作。尽管效率不高，但是通过最直观的方法，我们发现处理当前节点需要依赖于之前的部分结果。如何保存之前的处理结果？可以使用哈希表。既然我们需要回答“target – array[i]是否存在在数组中”，那不妨把值作为键，通过询问哈希表是否含有所需要的键来得到我们需要的回答。最终，根据题目，我们需要返回下标，那么把下标作为哈希表的值也是非常自然了。

复杂度分析：我们可以先对数组中的每个元素进行上述哈希处理，然后再从头至尾扫描数组，判断对应的另一个数是否存在在数组中，时间复杂度O(n + n)。事实上，我们可以利用动态编程的思想，仅仅利用已经处理的部分解：哈希表只存储前驱节点的信息。对于当前节点，判断前驱中是否含有对应值。当处理完当前节点，把当前节点加入哈希表，作为已经处理的部分解。这样，时间复杂度可以进一步减少为O(n)。

参考解答：

```
vector<int> addsToTarget(vector<int> &numbers, int target) {
    unordered_map<int, int> numToIndex;
    vector<int> vi(2);
    for (auto it = numbers.begin(); it != numbers.end(); it++) {
        if (numToIndex.count(target - *it)) {
            vi[0] = numToIndex[target - *it] + 1;
            vi[1] = (int)(it - numbers.begin()) + 1;
            return vi;
        }
        numToIndex[*it] = (int)(it - numbers.begin());
    }
}
```

> Get the length of the longest consecutive elements sequence in an array. For example, given [31, 6, 32, 1, 3, 2],the longest consecutive elements sequence is [1, 2, 3]. Return its length: 3.

解题分析：如何判断当前节点i是否属于一个序列？如果array[i] – 1存在在数组中，那么array[i]就可以作为后继加入序列。类似地，如果array[i] + 1存在在数组中，那么array[i]就可以作为前驱加入序列。我们发现处理当前节点需要依赖于之前的部分结果：判断array[i] – 1，array[i] + 1是否存在于数组中。如何保存之前的处理结果？可以使用哈希表。很显然，键对应于数值。但对于这个问题，value并不是那么明显，需要进一步分析。

一般而言，键用于快速判断哈希表中有没有我们需要的元素，值提供我们需要的结果。在这题中，我们期望于获得怎样的部分解呢？我们需要知道现在已经构成的序列是怎样的。由于序列是一系列连续整数，所以只要序列的最小值以及最大值，就能唯一确定序列。“而所谓的“作为后继加入序列”，“作为前驱加入序列”，无非就是更新最大最小值。所以哈希表的值可以是一个记录最大／最小值的结构，用以描述当前节点参与构成的最长序列。

复杂度分析：根据上述算法，我们只要扫描一遍整个数组就能获得结果，时间复杂度O(n)，附加空间O(n)。

参考解答：

```
struct Bound {
    int high;
    int low;
    Bound(int h = 0, int l = 0) {
        high = h;
        low  = l;
    }
};

int lengthOfLongestConsecutiveSequence(vector<int> &num) {
    unordered_map<int, Bound> table;
    int local;
    int maxLen = 0;
    for (int i = 0; i < num.size(); i++) {
        if (table.count(num[i])) {
            continue;
        }
        local = num[i];
        
        int low = local, high = local;
        
        if (table.count(local - 1)) {
            low = table[local - 1].low;
        }
        if (table.count(local + 1)) {
            high = table[local + 1].high;
        }
        
        table[low].high = table[local].high = high;
        table[high].low = table[local].low = low;
        
        if (high - low + 1 > maxLen) {
            maxLen = high - low + 1;
        }
    }
    return maxLen;
}
```

### String相关问题的处理技巧

通常，纯粹的字符串操作难度不大，但是实现起来可能比较麻烦，边界情况(edge case)比较多。例如把字符串变成数字，把数字变成字符串等等。这时候需要与面试官进行沟通，明确他们期望的细节要求，再开始写代码。同时，可以利用一些子函数，使得代码结构更加清晰。考虑到时间限制，往往有的时候面试官会让你略去一些过于细节的实现。此外，不妨看看经典字符串算法，比如判断子串等，这也是比较常见的面试题。

> “Given input -> "I have 36 books, 40 pens2."; output -> "I evah 36 skoob, 40 2snep." (Suppose punctuation mark may only have period or comma)

解题分析：题意比较简单：把每个以空格或符号为间隔的单词逆向输出，如果遇到纯数字，则不改变顺序。自然而然地，每次处理分为两个步骤：(1)判断是否需要逆向 (2)逆向当前单词。这样就可以分为两个子函数：一个负责判断，另一个负责逆向。然后进行分段处理。另外，请注意如果括号中的假设没有给出，可以与面试官试探去要这个假设。

参考解答：

```
bool isPunctuationOrSpace(char *character) {
    return *character == ' ' || *character == ',' || *character == '.';
}

bool isNumber(char *character) {
    return *character >= '0' && *character <= '9';
}

bool needReverse(char *sentence, int *offset) {
    int length = (int)strlen(sentence);
    bool needReverse = false;
    *offset = 0;
    while (!isPunctuationOrSpace(sentence + (*offset)) && (*offset) < length) {
        if (!isNumber(sentence + (*offset))) {
            needReverse = true;
        }
        (*offset)++;
    }
    return needReverse;
}

void reverseWord(char *word, int length) {
    int i = 0, j = length - 1;
    while (i < j) {
        swap(*(word + i), *(word + j));
        i++;
        j--;
    }
}

void reverseSentence(char *sentence) {
    int length = (int)strlen(sentence);
    int offset;
    for (int i = 0; i < length;) {
        if (needReverse(sentence + i, &offset)) {
            reverseWord(sentence + i, offset);
        }
        i += (offset + 1);
    }
}
```

> Replace space in the string with “%20”. E.g. given "Big mac", return “Big%20mac
>
> Replace “%20” substring with space. E.g. given “Big%20mac”, return “Big mac”

解题分析：对于要求原处(in-place)的删除或修改，可以用两个int变量分别记录新下标与原下标，不断地将原下标所指的数据写到新下标中。

这里有个小规律：如果改动后字符串长度增大，则先计算新字符串的长度，再从后往前对新字符串进行赋值；反之，则先从前往后顺序赋值，再计算长度。

## 工具箱

### 栈和堆

栈主要是指由操作系统自动管理的内存空间。当进入一个函数，操作系统会为该函数中的局部变量分配存储空间。事实上，系统会分配一个内存块，叠加在当前的栈上，并且利用指针指向前一个内存块的地址。函数的局部变量就存储在当前的内存块上。当该函数返回时，系统“弹出”内存块，并且根据指针回到前一个内存块。所以，栈总是以后进先出(LIFO)的方式工作。通常，在栈上分配的空间不需要用户操心。

堆是用来存储动态分配变量的空间。对于堆而言，并没有像栈那样后进先出的规则，程序员可以选择随时分配或回收内存。这就意味着需要程序员自己用命令回收内存，否则会产生内存泄漏(memory leak)。在C/C++中，程序员需要调用free/delete来释放动态分配的内存。在Java，Objective-C (with Automatic Reference Count)中，语言本身引入垃圾回收和计数规则帮助用户决定在什么时候自动释放内存。

### 需要了解的常见容器及方法

+ Vector / ArrayList
+ HashSet, HashTable
+ Map / Dictionary

### C 字符串常见函数

“相关函数通常定义在string.h中，简要介绍如下常见函数，更多信息请参考[这里](http://www.cplusplus.com/reference/cstring/)：

```
//copy source string to destination string
char *strcpy ( char *destination, const char *source );    

Example:
char str1[] = "Sample string";
char str2[SAMPLE_STRING_SIZE];
strcpy (str2,str1);

//Appends a copy of the source string to the destination string.
char *strcat ( char *destination, const char *source );    

Example:
char str[STRING_SIZE] = "string is ";
strcat(str, "concatinated");    //str becomes "string is concatinated

// Returns a pointer to the first occurrence of character in the C string str (NULL if not found).
char *strchr ( const char *str, int character );    

Example:
char string[STRING_LENGTH] = "This is a string";
int firstOccurPos = (int)(strchr(string, 'i') – string);    // firstOccurPos equals to 2 (here we assume string contains character 'i')

// Returns a pointer to the last occurrence of character in the C string str.
char *strrchr ( char *str, int character );    

// Returns a pointer to the first occurrence of str2 in str1, or a NULL pointer if str2 is not part of str1.
char *strstr (char *str1, const char *str2 );    

// Returns the length of the C string str.
size_t strlen ( const char *str );    

Example:
char string[STRING_LENGTH] = "This is a string";
int length = strlen(string);    // length equals to 16

// convert char string to a double
double atof (const char *str);    

// convert char string to an int
int atoi (const char *str);    
```

### C++ String 类常见函数

由于String类重载了+, <, >, =, ==等运算符，故复制，比较是否相等，附加子串等都可以用运算符直接实现。简要介绍其他常见函数如下，更多信息请参考[这里](http://www.cplusplus.com/reference/string/string/ )：

```
// Searches the string for the first occurrence of the str, returns index
size_t find (const string& str, size_t pos = 0) const;    

// Returns a newly constructed string object with its value initialized to a copy of a substring starting at pos with length len.
string substr (size_t pos = 0, size_t len = npos) const; 

Example:
string str("This is a string");
string subStr = str.substr(10,6);    // subStr equals to "string", with length 6

// erase characters from pos with length len
string &erase (size_t pos = 0, size_t len = npos); 

Example:
string str("This is a string");
str.erase(0,10);    // str becomes "string", with length 6 after erasure

// Returns the length of the string, in terms of bytes.
size_t length();    

Example:
string str ("This is a string");
int length = str.length();    // length equals to 16
```

### String Matching 的常见算法

简单介绍两种比较易于实现的字符串比较算法，下述算法假设在长度为n的母串中匹配长度为m的子串。更详细的解释请见[这里](http://en.wikipedia.org/wiki/String_searching_algorithm)。

Brute-Force算法： 顺序遍历母串，将每个字符作为匹配的起始字符，判断是否匹配子串。时间复杂度 O(mn)。

Rabin-Karp算法：将每一个匹配子串映射为一个哈希值。例如，将子串看做一个多进制数，比较它的值与母串中相同长度子串的哈希值，如果相同，再细致地按字符确认字符串是否确实相同。顺序计算母串哈希值的过程中，使用增量计算的方法：扣除最高位的哈希值，增加最低位的哈希值。因此能在平均情况下做到O(m+n)。

Example:

为描述简单，假设字符串只含有十进制数字，母串为"1234"待匹配子串为"23"，定义hash函数把字符串转成整数值。

首先计算子串hash：值为23。

然后计算母串前两个字符的hash：值为12，与子串不符合，需要后移。此时我们扣除最高位"1"的“hash，增加新的最低位"3"的hash，得到新的hash值23，与子串相符。

通过按字符比较发现的确匹配成功，可以返回母串匹配上的下标。

伪代码：

```
int RabinKarp(string s[1..n], string pattern[1..m])
hpattern = hash(pattern[1..m]);  hs = hash(s[1..m]);
for i from 1 to n-m+1
if hs = hpattern
     if s[i..i+m-1] = pattern[1..m]
return i
hs = hash(s[i+1..i+m])
       return not found
```