# Sorting and Searching

## 常见的内排序算法

所谓的内排序是指所有的数据已经读入内存，在内存中进行排序的算法。排序过程中不需要对磁盘进行读写。同时，内排序也一般假定所有用到的辅助空间也可以直接存在于内存中。与之对应地，另一类排序称作外排序，即内存中无法保存全部数据，需要进行磁盘访问，每次读入部分数据到内存进行排序。我们在“2. 常见的外排序算法”中讨论该类问题。

### Merge Sort

合并排序(Merge Sort)是一种典型的排序算法，应用“分而治之(divide and conquer)”的算法思路：将线性数据结构(如array、vector或list )分为两个部分，对两部分分别进行排序，排序完成后，再将各自排序好的两个部分合并还原成一个有序结构。由于合并排序不依赖于随机读写，因此具有很强的普适性，适用于链表等数据结构。算法的时间复杂度O(nlogn)，如果是处理数组需要额外O(n)空间，处理链表只需要O(1)空间。算法实现如下：

```
void merge_sort( int array[], int helper[], int left, int right){
    if( left >= right )
        return;

    // divide and conquer: array will be divided into left part and right part
    // both parts will be sorted by the calling merge_sort
    int mid = right - (right - left) / 2;
    merge_sort( array, helper, left, mid );
    merge_sort( array, helper, mid + 1, right);

    // now we merge two parts into one
    int helperLeft = left;
    int helperLeft = left;
    int helperRight = mid + 1;
    int curr = left;
    for(int i = left; i <= right; i++)
        helper[i] = array[i];
    while( helperLeft <= mid && helperRight <= right ){
        if( helper[helperLeft] <= helper[helperRight] )
            array[curr++] = helper[helperLeft++];
        else
            array[curr++] = helper[helperRight++];
    }

    // left part has some large elements remaining. Put them into the right side
    while( helperLeft <= mid )
        array[curr++] = helper[helperLeft++];
}
```

当递归调用merge_sort返回时，array的左右两部分已经分别由子函数排序完成，我们利用helper数组暂存array中的数值，再利用两个while循环完成合并。helper数组的左右半边就是两个排序完的队列，第一个while循环相当于比较队列头，将较小的元素取出放入array，最后使得array的左半边由小到大排序完成。第二个while循环负责扫尾，把helper左半边剩余元素复制入array中。注意，此时我们不需要对helper右半边做类似操作，因为即使右半边有剩余元素，它们也已经处于array中恰当的位置。

关于合并排序更多理论方面的讨论，请见“工具箱”。

### Quick Sort (快速排序)

快速排序(Quick Sor)是最为常用的排序算法，C++自带的排序算法的实现就是快速排序。该算法以其高效性，简洁性，被评为20世纪十大算法之一(虽然合并排序与堆排序的时间复杂度量级相同，但一般还是比快速排序慢常数倍)。快速排序的算法核心与合并排序类似，也采用“分而治之”的想法：随机选定一个元素作为轴值，利用该轴值将数组分为左右两部分，左边元素都比轴值小，右边元素都比轴值大，但它们不是完全排序的。在此基础上，分别对左右两部分分别递归调用快速排序，使得左右部分完全排序。算法的平均时间复杂度是O(nlogn)，在最坏情况下为O(n^2)，额外空间复杂度O(logn)。算法实现如下：

```
int partition( int array[], int left, int right ) {
    int pivot = array[right];
    while( left != right ){
        while( array[left] < pivot && left < right)
            left++;
        if (left < right) {
            swap( array[left], array[right--]);
        }
        while( array[right] > pivot && left < right)
            right--;
        if( left < right )
            swap( array[left++], array[right]);
    }
    //array[left] = pivot;
    return left;
}
void qSort( int array[], int left, int right ){
    if( left >=right )
        return;
    int index = partition( array, left, right);
    qSort(array, left, index - 1);
    qSort(array, index + 1, right);
}
```

partition函数先选定数组right下标所指的元素作为轴值，用pivot变量存储该元素值。然后，右移left，即从左向右扫描数组，直到发现某个元素大于轴值或者扫描完成。如果某个元素大于轴值，则将该元素与轴值交换。该操作特性在于：保证交换后轴值左侧的元素都比轴值小。再次，左移right，即从右向左扫描数组，直到发现某个元素小于轴值或者扫描完成。如果某个元素小于轴值，则将该元素与轴值交换。该操作特性在于：保证交换后轴值右侧的元素都比轴值大。重复上述过程直到left和right相遇，最终相遇的位置即为轴值所在位置。由于上述操作的特性，最终轴值左侧的元素都比轴值小，轴值右侧的元素都比轴值大。

关于快速排序的更多理论讨论请见“工具箱”。C++标准模版库提供函数sort，实现快速排序的功能: sort( iterator first, iterator last, Compare comp ); // can aslo be pointers here

### Heap Sort(堆排序)

堆排序(Heap Sort)利用了我们在Chapter 4 Trees and Graphs中提到的堆作为逻辑存储结构，将输入array变成一个最大值堆。然后，我们反复进行堆的弹出操作。回顾之前所述的弹出过程：将堆顶元素与堆末元素交换，堆的大小减一，向下移动新的堆顶以维护堆的性质。事实上，该操作等价于每次将剩余的最大元素移动到数组的最右边，重复这样的操作最终就能获得由小到大排序的数组。初次建堆的时间复杂度O(n)，删除堆顶元素并维护堆的性质需要O(logn)，这样的操作一共进行n次，故最终时间复杂度O(nlogn)。我们不需要利用额外空间，故空间复杂度O(1)。具体实现如下：

````
void heapSort(int array[], int size)  { 
    Heapify(array, size);
    for (int i = 0; i < size - 1; i++)
       popHeap(array);
}
```
Heapify和popHeap的实现参考Chapter 4 Trees and Graphs 。

### Bucket Sort (桶排序) 和 Radix Sort (基数排序)

桶排序(Bucket Sort) 和基数排序(Radix Sort)不需要进行数据之间的两两比较，但是需要事先知道数组的一些具体情况。特别地，桶排序适用于知道待排序数组大小范围的情况。其特性在于将数据根据其大小，放入合适的“桶(容器)”中，再依次从桶中取出，形成有序序列。具体实现如下：

```
void BucketSort(int array[], int n, int max)
{
    // array of length n，all records in the range of [0,max)
    int tempArray[n];
    int i;
    for (i = 0; i < n; i++)
        tempArray[i] = array[i];

    int count[max];    // buckets
    memset(count, 0, max * sizeof(int));

    for (i = 0; i < n; i++) // put elements into the buckets
        count[array[i]]++;
    for (i = 1; i < max; i++)
        count[i] = count[i-1] + count [i];  // count[i] saves the starting index (in array) of value i+1

    // for value tempArray[i], the last index should be count[tempArray[i]]-1
    for (i = n-1; i >= 0; i--)
        array[--count[tempArray[i]]] = tempArray[i];
}
```

该实现总的时间代价O(max+n)，适用于max相对n较小的情况。空间复杂度也为O(max+n)，用以记录原始数组和桶计数。

桶排序只适合max很小的情况，如果数据范围很大，可以将一个记录的值即排序码拆分为多个部分来进行比较，即使用基数排序。基数排序相当于将数据看作一个个有限进制数，按照由高位到低位(适用于字典序)，或者由低位到高位(适用于数值序)进行排序。排序具体过程如下：对于每一位，利用桶排序进行分类，在维持相对顺序的前提下进行下一步排序，直到遍历所有位。该算法复杂度为O(k*n)，k为位数(或者字符串长度)。直观上，基数排序进行了k次桶排序。具体实现如下：

```
void RadixSort(int Array[], int n, int digits, int radix)
{
    // n is the length of the array
    // digits is the number of digits
    int  *TempArray = new int[n];
    int *count = new int[radix]; // radix buckets
    int i, j, k;
    int Radix = 1; // radix modulo, used to get the ith digit of Array[j]
    // for ith digit
    for (i = 1; i <= digits; i++)  {
        for (j = 0; j < radix; j++)
            count[j] = 0;            // initialize counter
        for (j = 0; j < n; j++) {
            // put elements into buckets
            k = (Array[j] / Radix) % radix;  // get a digit
            count[k]++;
        }
        
        for (j = 1; j < radix; j++) {
            // count elements in the buckets
            count[j] = count[j-1] + count[j];
        }

        // bucket sort
        for (j = n-1; j >= 0; j--)  {
            k = (Array[j] / Radix ) % radix;
            count[k]--;
            TempArray[count[k]] = Array[j];
        }
        for (j = 0; j < n; j++) {
            // copy data back to array
            Array[j] = TempArray[j];
        }
        Radix *= radix;      // get the next digit
    }
}
```

与其他排序方式相比，桶排序和基数排序不需要交换或比较，它更像是通过逐级的分类来把元素排序好。

## 常见的外排序算法

外排序算法的核心思路在于把文件分块读到内存，在内存中对每块文件依次进行排序，最后合并排序后的各块数据，依次按顺序写回文件。相比于内排序，外排序需要进行多次磁盘读写，因此执行效率往往低于内排序，时间主要花费于磁盘读写上。我们给出外排序的算法步骤如下：

假设文件需要分成k块读入，需要从小到大进行排序

1. 依次读入每个文件块，在内存中对当前文件块进行排序(应用恰当的内排序算法)。此时，每块文件相当于一个由小到大排列的有序队列
2. 在内存中建立一个最小值堆，读入每块文件的队列头
3. 弹出堆顶元素，如果元素来自第i块，则从第i块文件中补充一个元素到最小值堆。弹出的元素暂存至临时数组
4. 当临时数组存满时，将数组写至磁盘，并清空数组内容。
5. 重复过程3)，4)，直至所有文件块读取完毕

## 快速选择算法 (quick selection algorithm)

快速选择算法能够在平均O(n)时间内从一个无序数组中返回第k大的元素。算法实际上利用了快速排序的思想，将数组依照一个轴值分割成两个部分，左边元素都比轴值小，右边元素都比轴值大。由于轴值下标已知，则可以判断所求元素落在数组的哪一部分，并在那一部分继续进行上述操作，直至找到该元素。与快排不同，由于快速选择算法只在乎所求元素所在的那一部分，所以时间复杂度是O(n)。关于算法复杂度的理论分析请见“工具箱”给出的参考资料。我们给出算法实现如下：

```
int partition( int array[], int left, int right ) {
    int pivot = array[right];
    while( left != right ){
        while( array[left] < pivot && left < right)
            left++;
            left++;
        if (left < right) {
            swap( array[left], array[right--]);
        }
        while( array[right] > pivot && left < right)
            right--;
        if( left < right )
            swap( array[left++], array[right]);
    }
    return left;
}

int quick_select(int array[], int left, int right, int k)
{
    if ( left >= right )
        return array[left];
    int index = partition(array, left, right);
    int size = index - left + 1;
    if ( size == k )
        return array[left + k - 1]; // the pivot is the kth largest element
    else if ( size > k )
        return quick_select(array, left, index - 1, k);
    else
        return quick_select(array, index + 1, right , k - size);
}
```

> “Get the k largest elements in an array with O(n) expected time, they don’t need to be sorted.

解题分析：实际上和quick select的应用场景是一致的，先找到第k大的元素，再将数组重新整理，找出比第k大的元素小的所有元素。

> There are n points on a 2D plan, find the k points that are closest to origin ( x= 0, y= 0).

解题分析：在这里已知点的数量，因此k个点到原点的距离构成size确定的静态数组，应该对这个数组使用快速选择算法。

## 二分查找 (Binary search)

对于已排序的有序线性容器而言(比如数组，vector)，二分查找(Binary search)几乎总是最优的搜索方案。二分查找将容器等分为两部分，再根据中间节点与待搜索数据的相对大小关系，进一步搜索其中某一部分。二分查找的算法复杂度为O(logn)，算法复杂度的具体分析请见“工具箱”给出的参考资料。算法实现如下：

```
int binarySearch(int *array, int left, int right, int value) {
    if (left > right) {
        // value not found
        return -1;
    }

    int mid = right - (right - left) / 2;
    if (array[mid] == value) {
        return mid;
    } else if (array[mid] < value) {
        return binarySearch(array, mid + 1, right, value);
    } else {
        return binarySearch(array, left, mid - 1, value);
    }
}
```

对于局部有序的数据，也可以根据其局部有序的特性，尽可能地利用逼近、剪枝，使用二分查找的变种进行搜索。

## 模式识别

### 动态数据结构的维护

维护动态数据(data stream)的最大值、最小值或中位数，可以考虑使用堆。如果是动态数据求最大的k个元素，因为元素总数量不确定，不能使用quick select，这种情况下也应该用堆解决。

如果需要一个动态插入/删除的有序数据结构，那么可以使用二叉搜索树，因为它天生就是一个动态的有序数组，并且支持检索。

> Merge k sorted linked lists to be one sorted list.

解题分析：可以将k个sorted list想象成k个有序数据流，互相竞争插入到结果序列， 因此可以考虑使用一个最小值堆维护动态数据：将每个队头的元素加入一个堆，然后从堆中依次弹出最小数据。如果数据属于第i个list，则该list补充一个元素到堆。重复上述过程直到所有元素排序完成。事实上，该算法就是外排序算法的一个具体实现，区别仅仅在于这里略去了文件的读写操作。请仔细类比在“The Rules”部分提到的外排序算法，进一步加深理解。

复杂度分析：时间上，我们需要维护一个大小为k的最小值堆，每次维护复杂度O(logk)，由于一共有n个数据，每个数据都会加入最小值堆，故总体时间复杂度O(nlogk)。由于每个list至少有一个数据，故n一定大于等于k。相比于完全无序的n个数据排序(所需时间O(nlogn))，我们的算法将复杂度降至O(nlogk)。原因在于数据是部分有序的。事实上，在通常面试中，如果数据已经部分有序，我们理应能够实现时间复杂度优于O(nlogn)的算法。

空间上，我们需要大小为k的最小值堆。同时，在不允许破坏原有链表的情况下，我们需要额外O(n)的空间构建新链表，故总体空间复杂度O(n+k)。如果可以直接修改原有数据的next指针，则总体空间复杂度即为O(k)。至于是否能够破坏原始数据，需要与面试官进行沟通。

参考解答：

```
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(NULL) {}
};

struct cmp {
    bool operator() (const ListNode *a, const ListNode *b)
    {
        if (a->val < b->val)
            return false;
        else
            return true;
    }
};

ListNode *mergeKLists(vector<ListNode *> &lists) {
    priority_queue<ListNode *, vector<ListNode *>, cmp> heap;

    for (int i = 0; i < lists.size(); i++) {
        if (lists[i]) {
            // for the corner case: [{}]
            heap.push(lists[i]);
        }
    }

    ListNode *prevNode = NULL;
    ListNode *head = NULL;
    ListNode *curNode = NULL;
    while (!heap.empty()) {
        curNode = heap.top();
        heap.pop();
        if (head == NULL) {
            head = curNode;
        }
        if (curNode->next) {
            heap.push(curNode->next);
        }
        if (prevNode) {
            prevNode->next = curNode;
        }
        prevNode = curNode;
    }
    return head;
}
```

> Having a stream of integers, design a data structure to support easy look up the number of values less than or equal to a given value.

解题分析：需要一个动态的数据结构，支持快速检索，并且满足一定的有序性，这样才能维护这个特殊属性(给定值在这个有序结构中的相对位置)，使用二叉搜索树。(注意堆没有其他数据结构例如哈希表的辅助，无法支持给定key的快速检索。)

参考解答：

```
class Node{
    private:
        Node*trackhelp(Node*rt,int x);
    public:
        Node *left;
        Node *right;
        int key;
        int left_cnt;
        void track(int x);
        int getRank(int x);
        Node(int key,Node*l = NULL,Node*r =NULL,int cnt = 0):key(key),left(l),right(r),left_cnt(cnt){};
};

Node *Node::trackhelp(Node*rt, int x){
    if( rt == NULL) return new Node(x);
    if( x <= rt->key ){
        rt->left = trackhelp(rt->left,x);
        rt->left_cnt++;
    } else {
        rt->right = trackhelp(rt->right,x);
    }
    return rt; 
}

void Node::track( int x){
     trackhelp(this,x);
}

int Node::getRank(int x){
    if(this == NULL) return -1;
    if(x == key)
       return left_cnt;
    if(x < key)
      return left->getRank(x);
    if(x > key){
        if(right->getRank(x) == -1) return -1;
        return left_cnt + 1 + right->getRank(x); 
    }
}
```

### 对于有序／部分有序容器的搜索， 用二分查找(binary search)。

> Find i in a given array that arr[i] == i.

解题分析：本题最直观的想法是线性扫描整个数组，逐一检查元素值与下标是否相同。这样做的时间复杂度是O(n)，但是很明显的缺陷在于这种做法完全没有利用到数组是有序的特性。由于题目中出现了关键字“sorted”，结合“The Rules”中提到的规律：对于有序线性容器的搜索，二分查找或其变种基本上是解题的最佳方法，我们需要尝试设计一种类似于二分查找的算法。通常，我们可以尝试自己举个实例，便于发现普遍规律性：

| Index | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
| -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
| Value | -7 | -2 | 0 | 3 | 7 | 9 | 10 | 12 | 13 |

在此例中，A[3] = 3。同时，不难发现一个规律：A[3]左侧的数据满足value < index，A[3]右侧的数据满足value > index。反而言之，如果当前数据满足value < index，则需要搜索的数据必然在其右侧，如“果当前数据满足value > index，则需要搜索的数据必然在其左侧。这样，实际上就可以利用二分查找：比较当前数据值和其下标，选择恰当的半边进行下一步搜索。

复杂度分析：时间复杂度同二分查找，为O(logn)。

参考解答：

```
int indexSearch(int *array, int left, int right) {
    if (left > right) {
        // value not found
        return -1;
    }

    int mid = right - (right - left) / 2;    
    if (array[mid] == mid) {
        return mid;
    } else if (array[mid] < mid) {
        return indexSearch(array, mid + 1, right);
    } else {
        return indexSearch(array, left, mid - 1);
    }
}
```

> An array is sorted without duplicates. However, someone mysteriously shifted all the elements in this array (e.g. 1,2,3,4,5 -> 5,1,2,3,4). Implement a function to find an element in such array (return -1 if no such element).

解题分析：如果是在完全有序的数组中进行搜索，最优的解法无疑是二分查找。本题中，平移操作后的数组不再是完全有序了，因此我们不能直接应用二分查找。回顾二分查找算法：二分查找将容器等分为两部分，再根据中间节点与待搜索数据的相对大小关系，进一步搜索其中某一部分。算法本质在于，通过数据之间的相互比较，每次我们只需要搜索容器的某一半边。事实上，对于本题而言，既然数据具备局部有序的特性，如果通过适当的条件判断，每次能够减半搜索范围，那么我们同样可以达到二分查找的效果。关键问题在于：如何通“过数据之间的相互比较，确定需要检索的那一半数据？

首先，我们可以通过一个实例观察平移后的数组有什么特性：假设数组0,1,2,3,4,5,6,7,8,9,10通过平移变为7,8,9,10,0,1,2,3,4,5,6，要求搜索4。根据二分查找的算法，我们将4与容器的中间元素进行比较，由此确定需要继续搜索的半边。本例中，中间元素为1，即将数组切割为7,8,9,10,0,1与1,2,3,4,5,6。不难发现，每次切割都保证至少有一半数组是完全有序的，并且，我们可以通过比较切割后两半数组各自的头尾数据大小，确定哪一半是完全有序的。

现在，我们进一步考虑如何减半搜索范围。由于至少有一半数组是有序的(并且我们知道数据的范围)，那么可能有两种情况：

1. 待搜索元素落在有序的那一半(即大于最左元素且小于最右元素)。
2. 待搜索元素不在有序的那一半。

对于情况1，我们只需要搜索那半边即可。对于情况2，我们只需搜索另一半边即可。这样，无论出现哪种情况，我们都可以减半搜索范围。

总结一下我们的算法：

1. 通过中间元素将数组划分为两个半边
2. 通过比较切割后两半数组各自的头尾数据大小，确定哪一半是完全有序的
3. 判断待搜索元素落在哪个半边，减半搜索范围

复杂度分析：由于我们每次能够减半搜索范围，故时间复杂度与二分查找相同，为O(logn)。

参考解答：

```
int searchInRotatedArrayHelper(int array[], int left, int right, int target) {
    if (left > right) {
        // exit: target not found
        return -1;
    }
    int mid = right - (right - left) / 2;
    if (array[mid] == target) {
        // exit: target found
        return mid;
    }

    if (array[left] <= array[mid]) {
        // left half is completely sorted
        if (target >= array[left] && target <= array[mid]) {
            // target in the sorted part
            return searchInRotatedArrayHelper(array, left, mid - 1, target);
        } else {
            // target not in the sorted part
            return searchInRotatedArrayHelper(array, mid + 1, right, target);
        }
    } else {
        // right half is completely sorted
        if (target >= array[mid] && target <= array[right]) {
            // target in the sorted part
            return searchInRotatedArrayHelper(array, mid + 1, right, target);
        } else {
            // target not in the sorted part
            return searchInRotatedArrayHelper(array, left, mid - 1, target);
        }
    }
}

int searchInRotatedArray(int array[], int n, int target) {
    return searchInRotatedArrayHelper(array, 0, n - 1, target);
}
```

> Given a sorted array of integers with duplicates. Implement a function to get the start and end position of a given value.

解题分析：对于完全排序数组的搜索问题，首先应该想到二分查找：我们可以通过二分查找找到相应的元素。其次，题目要求返回该元素的起始和终止位置。那么，我们可以基于二分查找返回的结果，向左向右依次做线性扩展，即查看下一个元素是否依然符合条件。这样做可以得到正确的结果，但是在最坏情况下，该算法复杂度为O(n)。例如，数组为1,1,1,1,1，给定的元素也为1。那么，在我们做线性扩展的时候，我们会遍历数组中的每一个元素。

如何效率更高地找到元素的起始和终止位置？考虑到数组是完全排序的，即被目标值分割的左右半边仍然分别有序，满足局部有序的特征，于是我们可以进一步继续做二分查找：即对左右两个区间分别继续搜索目标元素，在这个过程中，更新目标值出现的最左位置和最右位置。这样，我们可以以O(logn)的复杂度快速获得起始和终止位置。

复杂度分析：始终在做二分查找，没有线性查找，因此平均时间复杂度是O(logn)。

参考解答：

```
void searchRangeHelper(int array[], int left, int right, int target, int &begin, int &end) {
    if (left > right) {
        return;
    }

    int mid = right - (right - left) / 2;
    if (array[mid] == target) {
        if (mid < begin || begin == -1) {
            begin = mid;
        }
        if (mid > end) {
            end = mid;
        }
        searchRangeHelper(array, left, mid - 1, target, begin, end);
        searchRangeHelper(array, mid + 1, right, target, begin, end);
    }
    else if (array[mid] < target) {
        searchRangeHelper(array, mid + 1, right, target, begin, end);
    }
    else {
        searchRangeHelper(array, left, mid - 1, target, begin, end);
    }
}

vector<int> searchRange(int A[], int n, int target) {
    int begin = -1, end = -1;

    searchRangeHelper(A, 0, n - 1, target, begin, end);

    vector<int> ans;
    ans.push_back(begin);
    ans.push_back(end);
    return ans;
}
```

> Check if an element is in a M x N matrix, each row and column of which is sorted.

解题分析：首先我们可以构造一个矩阵：

    1    5    10   20
    2    6    11   30
    7    9    12   40
    8    15   31   41

如果要在上述矩阵中找到9，应该如何计算？最简单的方法显然是遍历每行每列，这样的时间复杂度是O(n^2)，而且完全没有利用到矩阵已经部分有序的特性。

进一步观察矩阵，任何元素都将矩阵划分为4个部分：

     I  | II
    III | IV

根据矩阵的特性，同行同列中元素的大小关系已知，并且I区的所有数据都比当前元素小，IV区的所有数据都比当前元素大。II，III两区数据与当前元素没有明确的相对大小关系。因此，我们的每次操作必须保证没有II区或III区，即从右上角或者左下角开始搜索。不妨假设从右上角(20)开始搜索：

1. 比较20与9，左移
2. 比较10与9，左移
3. 比较5与9，下移
4. 比较6与9，下移
5. 找到9

不难发现，每次当前元素大于待搜索元素，我们左移，否则下移。

对于有序容器的搜索，能不能用二分查找？对于本例，我们不能使用二分查找及其变种。原因是：二分查找的关键在于，当前元素将容器分为两个部分，并且通过比较当前元素和待搜索元素的大小，我们能够确定两者的相对位置关系，进而缩小搜索范围。但是对于本例，当前元素和待搜索元素大小关系并不能确定两者相对位置。

复杂度分析：假设矩阵有M行N列，则我们至多下移M次，左移N次，即算法复杂度O(M+N)。

参考解答：

```
bool isElementInMatrix(int **matrix, int M, int N, int target) {
    int row = 0;
    int column = N - 1;

    while (row < M && column >= 0) {
        if (matrix[row][column] == target) {
            return true;
        } else if (matrix[row][column] < target) {
            row++;
        } else {
            column--;
        }
    }
    return false;
}
```

> Implement sqrt(x), which returns the square root of value x.

解题分析：首先我们需要明确开根号的性质：

1. 负数无效
2. 若x为0，则返回0
3. 若x属于(0,1)，则sqrt(x)属于(x,1)
4. 若x为1，则返回1
5. 若x大于1，则sqrt(x)属于(1,x)

此外，若x > y，则sqrt(x) > sqrt(y)。情况1，2，4可以作为特例，而对于通常情况(情况3，情况5)，我们发现如下两个特性：

1. 解落在已知区间
2. 存在相对大小关系

进一步地，我们可以将sqrt(x)的所有“候选数”看成是分布在有限区间上的有序数列，对于每个元素，我们通过平方操作比较与待搜索数x的相对大小关系。很明显，这就是二分查找的思想。

复杂度分析：由于我们利用了二分查找的思想，故复杂度为O(log(x/precision))

参考解答：

```
double mySqrtHelper(double x, double lowBound, double highBound) {
    double precision = 0.00001;
    double sqrt = lowBound / 2 + highBound / 2;
    if (abs(sqrt * sqrt - x) <  precision) {
        return sqrt;
    } else if (sqrt * sqrt - x > 0) {
        return mySqrtHelper(x, lowBound, sqrt);
    } else {
        return mySqrtHelper(x, sqrt, highBound);
    }
}

double mySqrt(double x) {
    if (x < 0)
        return ERROR;
    if (x == 0) {
        return 0;
    }

    if (x == 1) {
        return 1;
    }

    if (x < 1) {
        return mySqrtHelper(x, x, 1);
    } else {
        return mySqrtHelper(x, 1, x);
    }
}
```

> Find the median of the elements of two sorted arrays.

解题分析：对于含有n个数的数组，若n为奇数，则中位数array[n/2+1]，若n为偶数，则中位数为 (array[n/2] + array[n/2+1]) / 2。所以我们当前的目标是设计一种算法能够返回第k大的元素。对于查找第k大的数，最先想到的应该是快速选择算法，但是该算法并没有利用数组已经有序的特性，故时间复杂度O(m+n)。与二分查找相似，快速选择算法的核心在于快速缩小搜索范围。

对于本例，我们应该如何缩小搜索范围呢？假设数组分别记为A，B。当前需要搜索第k大的数，于是我们可以考虑从数组A中取出前m个元素，从数组B中取出k-m个元素。由于数组A，B分别排序，则A[m]大于从数组A中取出的其他所有元素，B[k-m] 大于数组B中取出的其他所有元素。此时，尽管取出元素之间的相对大小关系不确定，但A[m]与B[k-m]的较大者一定是这k个元素中最大的。那么，较小的那个元素一定不是第k大的，它至多是第k-1大的：因为它小于其他未被取出的所有元素，并且小于取出的k个元素中最大的那个。为叙述方便，假设A[m]是较小的那个元素。那么，我们可以进一步说，A[1], A[2]…A[m-1]也一定不是第k大的元素，因为它们小于A[m]，而A[m]至多是第k-1大的。因此，我们可以把较小元素所在数组中选出的所有元素统统排除，并且相应地减少k值。这样，我们就完成了一次范围缩小。特别地，我们可以选取m=k/2。

复杂度分析：每次缩小范围之后k值基本上折半，故时间复杂度O(logn)。

参考解答：

```
double helper(int A[], int m, int B[], int n, int k){
    // find the kth largest element
    if(m > n)
        return helper(B, n, A, m, k);//make sure that the second one is the bigger array;
    if(m == 0)
        return B[k - 1];
    if(k == 1){
        return min(A[0], B[0]);
    }
    int pa = min(k / 2, m); // assign k / 2 to each of the array and cut the smaller one
    int pb = k - pa;
    if (A[pa-1] <= B[pb-1])
        return helper(A + pa, m - pa, B, n, k - pa);
    return helper(A, m, B + pb, n - pb, k - pb);
}

double findMedianSortedArrays(int A[], int m, int B[], int n) {
    int total = m + n;
    if(total % 2 == 0){
        return (helper(A, m, B, n, total / 2) + helper(A, m, B, n, total / 2 + 1)) / 2;
    }
    return helper(A, m, B, n, total / 2 + 1);
}
```

> Given a server that has requests coming in. Design a data structure such that you can fetch the count of the number requests in the last second, minute and hour.

解题分析：对于这个问题，比较容易想到的是对于每个request，我们都分配一个当时的时间戳(timestamp)，并且将request根据时间戳排序，则时间戳构成一个完全有序的序列。这样，当我们要搜索一分钟前的request时，可以用二分查找快速定位。其次，如果我们找到了对应的request，如何最快速地知道从这个request至现在一共发生了多少次其他请求？我们可以采用计数的方式，对于每个request，分配一个计数，用以表示这是第几个request。我们只需要用当前计数减去某个request的计数，就可以知道从那个request至现在一共发生了多少次其他请求。

复杂度分析：我们应用二分查找寻找某个特定时间的request，算法涉及的时间复杂度为O(logn)。

参考解答：

注意，我们简化了overflow的处理，采用long int并且默认整形数据不会越界。读者可以进一步考虑如何处理越界。

```
#include <time.h>
#include <sys/time.h>
long now() {
    struct timeval time;
    gettimeofday(&time, NULL);
    return (time.tv_sec * 1000000 + time.tv_usec);
}
“class HitCounter {
private:
    deque<pair<long, int>> hits;
    long last_count = 0;
    const int second = 1000000;
    const int minute = 60 * second;
    const int hour = 60 * minute;

    void prune() {
        auto old = upper_bound(hits.begin(), hits.end(), make_pair(now() - 1 * hour, -1));
        if (old != hits.end()) {
            hits.erase(hits.begin(), old);
        }
    }

public:
    void hit() {
        hits.push_back(make_pair(now(), ++last_count));
        prune();
    }

    long hitsInLastSecond() {
        auto before = lower_bound(hits.begin(), hits.end(), make_pair(now() - 1 * second, -1));
        if (before == hits.end()) { return 0; }
        return last_count - before->second + 1;
    }

    long hitsInLastMinute() {
        auto before = lower_bound(hits.begin(), hits.end(), make_pair(now() - 1 * minute, -1));
        if (before == hits.end()) { return 0; }
        return last_count - before->second + 1;
    }
    long hitsInLastHour() {
        auto before = lower_bound(hits.begin(), hits.end(), make_pair(now() - 1 * hour, -1));
        if (before == hits.end()) { return 0; }
        return last_count - before->second + 1;
    }
};
```

### 数据范围有限、离散

数据范围有限、离散(或存在大量重复数据，即密集数据)的排序问题，一般可以使用桶排序。对于有限位数的数据(如string, vector<int>, int)，可以利用基数排序进行数值序或词典序排序。

> Sort a large number of people by their ages.

解题分析： 人的寿命是有限的，即数据都处于[0,150]。在数据最大值已知的情况下，通常桶排序效率最高，为O(n)。对于本例，由于数据为十进制数，并且至多3位。故我们还可以考虑用基数排序，时间复杂度与桶排序近似。

复杂度分析：同桶排序，O(n)。

参考解答：请参考桶排序的代码实现

> Reorder an array of strings so that anagrams appear together.

解题分析： 这并不是一个完全排序的问题，而只要求分类，并且类别一定是有限个，选择用桶排序，类别数量等于桶的数量。对一般的桶排序，每一个不同的值就对应一个桶，而这里则是所有相同字母异序词(anagram)对应一个桶，因此需要找到一个哈希函数，要求是所有相同字母异序词的哈希值相同，对应同一个桶。一种做法是将字符串排序后的结果作为字符串的哈希值。

复杂度分析：假定字符串平均长度为k，数量为n，那么平均复杂度为O(klogk*n)。

参考解答：

```
void reOrderWithAnagrams( vector<string> strs ) {
    unordered_map<string, list<string>> buckets;
    for (auto it = strs.begin(); it != strs.end(); it++) {
       string curr_str = *it;
       sort(curr_str.begin(), curr_str.end());
       buckets[curr_str].push_back(*it); // Add this string to corresponding bucket
    }
    // Post processing, reorder the strings with the bucket 
    int index = 0;
    for (auto it = buckets.begin(); it != buckets.end(); ++it) {
        list<string> &anagrams = it->second;
        for (auto local_it = anagrams.begin(); local_it != anagrams.end(); ++local_it){
            strs[index] = *local_it;
            index++;
        }
    }
    return;
} 
```

### Scalability & Memory Limits 问题

对这类问题一般采用Divide & Conquer策略，即对问题进行预处理，将问题的输入进行分割、归类(sorting)，放入相应的桶(单机上的某一块Chunk，或者分布式系统中的一台单机)，再对每个桶进行后期处理，最后合并结果。

整个过程中应该用到哈希函数: 对于Memory Limits问题，一般可以直接利用哈希函数建立对象到索引的直接映射；对Scalability问题，一般可以用哈希表来记录对象与存储该对象的机器之间的映射，在该机器上进一步做映射以获得索引。

> A library is trying to build up a smart computer-aided look up system: user may input a list of key words, and the system shall provide all books that “contain these words. How to implement such query? (A library may have millions of books)

解题分析：对于文章中单词的检索，一般采用倒排索引(Inverted index) ，其本质就是单词到文档集合的一个哈希，例如：

    apple -> doc 1, doc 2
    banana -> doc 2

对于文档集合，可以进一步利用各种集合操作，例如交集，并集等，获得对应的结果。如果题目中书本数目不大，并且出现的单词总个数不多，那么可以直接将倒排索引建立在一部机器上。在查询时，对于用户给定的每个单词，通过哈希表查找出对应的文档集合，最后对所有集合求交集，即可实现查询功能。但是，当文档／单词量巨大时，如何实现可扩展性？我们需要以一定的规律将哈希表分配到多台机器上，即完成对哈希表的拆分和分块存储。例如，machine1可以负责首字母为a的所有单词，machine2可以负责首字母为b的单词，以此类推。事实上，这个过程相当于引入另一层hash进行数据分流：建立对象与存储该对象的机器之间的映射。

> Design a scalable social network system and support looking up connection between two users.

解题分析：与上题类似，首先我们考虑数据量不大的情况，即所用用户数据存储在同一部机器上。要查询两个用户之间的联系无非就是广度优先搜索(请回顾Chapter 4 Trees and Graphs)。进一步地，如果数据量很大，那么我们需要用多台机器存储对象。不妨采用与上题类似的思路：引入另一层hash进行数据分流，建立对象与存储该对象的机器之间的映射。通常用户都有一个唯一的ID，我们可以利用ID的前几位数字，决定将该用户数据存放在哪台机器上。这样，广度搜索的查询流程变为：

1. 获得用户的好友ID列表
2. 根据ID获得机器
3. 访问机器获得对应的用户节点
4. 进行下一步递归

应用该方法我们需要注意一下问题：

1. 访问另一台机器的成本可能比较高，因此应该尽量把该机器上的所有相关数据一次性读取完毕。
2. 广度搜索需要记录某个节点是否已经访问过以避免循环访问，我们可以另外开辟一个哈希表记录已经访问的用户节点。
3. 注意合理地简化问题：如果两个用户之间需要多于3重关系才能联系上，我们可以把他们归类为“陌生人”。那么，我们可以将关系链限制在3重关系以内，以减少搜索成本。

> Suppose you are given an extremely large set of URLs, ~billion for example. How do you de-duplicate(remove duplicates)?

解题分析：对于本题，我们还是套用惯用的模版：

1. 先考虑小数据量的情况 使用哈希表
2. 数据量很大 使用多台机器，引入另一层hash进行数据分流
分流方式可以根据机器数量，采用URL的前若干个字符进行数据到机器号的映射。

## 工具箱

### 合并排序

关于合并排序的理论分析，请见参考资料：
《算法导论》(Introduction to Algorithms, 2nd Edition), Thomas H. Corman, Charles E.Leiserson, Ronald L.Rivest, Clifford Stein, 第2章，算法入门，2.3.1 分治法

### 快速排序

关于快速排序的理论分析，请见参考资料：
《算法导论》(Introduction to Algorithms, 2nd Edition), Thomas H. Corman, Charles E.Leiserson, Ronald L.Rivest, Clifford Stein, 第7章，快速排序

### 快速选择算法 (quick selection algorithm)

关于快速选择算法的理论分析，请见参考资料：“《算法导论》(Introduction to Algorithms, 2nd Edition), Thomas H. Corman, Charles E.Leiserson, Ronald L.Rivest, Clifford Stein, 第9章，中位数和顺序统计学

### 二分查找 (binary search)

关于二分查找的理论分析，请见参考资料：
《算法导论》(Introduction to Algorithms, 2nd Edition), Thomas H. Corman, Charles E.Leiserson, Ronald L.Rivest, Clifford Stein, 第12章，二叉查找树

### 动态数据结构的维护

本章涉及的排序与搜索通常可以由如下数据结构实现，我们在此做一下比较：

二叉查找树(binary search tree, BST): 可以看成一个动态的有序数组。建立BST(对应std::map )通常可以满足有序要求，将检索对象作为Tree Node的key，将检索内容作为Tree Node的value。利用BST本身的性质，在插入／删除时都能够维持其有序的特性。此外，BST的节点可以方便地记录其子树的信息，并在节点间传递全局信息，方便检索。当BST反复插入／删除时，可能导致树的不平衡，影响检索效率。此时，应该使用更为高级的平衡二叉查找树或者红黑树来维护动态数据。

堆：能够很方便地维护动态数据的最大值、最小值或中位数。priority_queue相比简单的队列更适用于需要优先级的情境。与队列类似，该结构适合 push和pop，但不方便更新和检索(仅仅部分有序)。
链表: 适合快速的更新、插入和删除，但不方便检索。可以注意到链表同时也可以是队列的底层实现方式，因此适合使用队列的情境，一般也可以使用更为强大的链表。

哈希表: 为了实现更快速的非排序检索(O(1))，如unordered_map以及array、 set、bitset<int>。

### 倒排索引(Inverted index)

http://en.wikipedia.org/wiki/Inverted_index

