# Stacks and Queues

### 栈

栈(stack)是一种数据结构，可以实现后进先出(Last in first out, LIFO)。通常情况下，我们可以用栈作为辅助，实现深度优先算法(Depth first search, DFS)，或者将递归转为while循环。在本书第4章中，可以看到这样的实例。

事实上，递归本身就是相当于把函数本身一层一层地加到操作系统的内存栈上，所以利用栈数据结构去实现递归也是非常自然的：入栈操作相当于递归调用自身，出栈操作相当于递归返回。入栈操作的对象相当于需要被解决的问题，出栈对象相当于已经解决的子问题，通过共享的状态变量或返回值把子问题的结果传递出来。

最基本的栈至少包括入栈(push)和出栈(pop)，前者将一个元素放入栈内，后者将最后放入栈的元素弹出。

### 队列

与栈对称，队列(Queue)帮助实现先进先出(First in first out, FIFO)，我们可以用Queue作为辅助，实现广度优先算法(Breadth first search, BFS)。在第5章可以看到这样的实例。队列还可以作为buffer，构建一个生产者－消费者模型：生产者把新的元素加到队尾，消费者从队头读取元素。在有两个线程同时读取同一个队列时，需要考虑同步(synchronization)，具体问题在第11章中讨论。

事实上，栈 与队列可以视作封装好的链表，只是限制了访问和插入的自由。因此适用栈或队列的情境，也可以考虑使用更为强大的链表。

## 模式识别

+ 考虑到栈具有LIFO的特性，那么与之匹配地，最大值追踪方式也需要具有相同特性：不妨用另一个栈追踪最大值。
+ 遍历子树的过程是一个自上而下结构：从顶层出发，逐渐向下扩散。所以考虑递归或者栈。

### 通过栈实现特殊顺序的读取

由于栈具有LIFO的特性，如需实现任何特定顺序的读取操作，往往可以借助两个栈互相“倾倒”来实现特定顺序。事实上，在很多情况下，栈并不是实现这种读取顺序的最佳数据结构。但作为面试问题，往往面试官会很明确的说明用栈实现。此时，我们就应该立刻想到利用另一个栈作为辅助。

> Design a stack with push(), pop() and max() in O(1) time.

解题分析：在push的时候记录当前的最大值十分显然，只需要在插入当前值的时候比较是否比现在的最大值大，如果是，更新最大值即可。问题在于如何恰当处理pop的情况：直观想法是，当需要出栈的元素是当前最大值，则需要“回溯”到前一个最大值。考虑到栈具有LIFO的特性，那么与之匹配地，最大值追踪方式也需要具有相同特性：不妨用另一个栈追踪最大值。注意一个细节，当最大值重复入栈时，我们的“最大值栈”也需要重复记录该值。否则，在弹出第一个重复最大值的时候，我们就可能在最大值栈中丢失相应的信息。

复杂度分析：时间复杂度符合题目要求为O(1)。空间复杂度最坏情况附加的栈中需要存储每个元素，故额外使用O(n)空间。

参考解答：

```
class stackWithMax {
private:
stack<int> valueStack;
stack<int> maxStack;

public:
    void push(int);
    int pop();
    int max();
};

void stackWithMax::push(int value) {
    if (maxStack.empty() || maxStack.top() <= value) {
        maxStack.push(value);
    }
    valueStack.push(value);
}

int stackWithMax::pop() {
    int value = valueStack.top();
    valueStack.pop();
    if (value == maxStack.top()) {
        maxStack.pop();
    }
    return value;
}

int stackWithMax::max() {
    return maxStack.top();
}
```

> Given a stack structure, use it to implement a queue.

解题分析：这是一道比较常见的题目。 栈的输出顺序是LIFO，queue的输出顺序是FIFO，考虑到如果想利用栈实现特定顺序的读取操作，往往可以借助两个栈互相“倾倒”来实现特定顺序：当一个栈中的元素倾倒到另一个栈中，则原栈最后出栈的元素最先出栈，相当于实现了FIFO。

复杂度分析：出栈由于多了倾倒的过程，故时间复杂度降为O(n)。空间复杂度不变，因为并没有重复存储元素。

参考解答：

```
class Queue {
private:
    stack<int> inputStack;
    stack<int> outputStack;
public:
    void enqueue(int);
    int dequeue();
};

void Queue::enqueue(int value) {
    inputStack.push(value);
}

int Queue::dequeue() {
    int value;
    if (!outputStack.empty()) {
        value = outputStack.top();
        outputStack.pop();
        return value;
    }
    while (!inputStack.empty()) {
        outputStack.push(inputStack.top());
        inputStack.pop();
    }
    value = outputStack.top();
    outputStack.pop();
    return value;
}
```

> How to sort a stack in ascending order (i.e. pop in ascending order) with another stack?

解题分析：本题明确要求利用两个栈实现一种特定的输出顺序。由于栈限制了输出的顺序，所以要调整栈内部的元素顺序比较困难。但从另一个角度想，我们也许可以在入栈的时候就控制插入的位置，使得栈内的元素都是有序的，即只要保证新栈的元素一定是有序的即可。同时，考虑到栈具有LIFO的输出性质，将一个栈中的元素倾倒到另一个栈，在倾倒回来，元素之间保持原有顺序。

于是我们可以构建如下算法：假设有Stack A和Stack B，Stack A中的元素没有顺序。我们需要把Stack A中的数据有序地加入到Stack B中。每当我们从Stack A中取出一个元素，当该元素不符合Stack B的当前排列顺序时，我们倾倒Stack B的元素，直到该元素可以按顺序入栈。然后，我们恢复之前倾倒的元素。根据栈“将一个栈中的元素倾倒到另一个栈，在倾倒回来，元素之间保持原有顺序”的性质，特别地，我们可以将Stack A看成性质中的“另一个栈”。

复杂度分析：由于调整一个元素的顺序可能要求将之前的n个元素来回倾倒，故时间复杂度O(n^2)。

参考解答：

```
stack<int> sort(stack<int> &input) {
    stack<int> output;
    while (!input.empty()) {
        int value = input.top();
        input.pop();
        while (!output.empty() && output.top() < value) {
            input.push(output.top());
            output.pop();
        }
        output.push(value);
    }
    return output;
}
```
### “Save for later”问题

有一类问题有这样的特性：当前节点的解依赖后驱节点。也就是说，对于某个当前节点，如果不能获知后驱节点，就无法得到有意义的解。这类问题可以通过栈(或等同于栈的若干个临时变量)解决：先将当前节点入栈，然后看其后继节点的值，直到其依赖的所有节点都完备时，再从栈中弹出该节点求解。某些时候，甚至需要反复这个过程：将当前节点的计算结果再次入栈，直到其依赖的后继节点完备。

> Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is a valid parentheses string. For example, “(([]))” is valid, but “(]” or “((“ is not.”

解题分析：比较明显的一件事是，为了判断输入是否有效，我们需要从头到尾地扫描整个字符串。问题在于，对于某个字符，我们并不能立刻判断是否有效，因为当前节点的解依赖后驱节点。这种情况下，我们可以尝试使用栈作为辅助结构： 需要先将当前节点入栈，然后看其后继节点的值，直到其依赖的所有节点都完备时，再从栈中弹出该元素求解。具体地，当当前节点是一个左括号，我们就把它入栈，直到读到其所依赖的右括号，再从栈中弹出该节点。如果当前节点是右括号，则栈顶必须是与之相依赖的左括号，否则输入无效。当扫描完整个字符串，栈应该为空，否则输入无效。

复杂度分析：时间复杂度O(n)。辅助栈最多有n个数据入栈，故额外空间复杂度O(n)

参考解答：

```
bool isLeftParentheses (char input) {
    return input == '(' || input == '[' || input == '{';
}

bool isMatchParentheses(char left, char right) {
    switch (left) {
        case '(':
            return right == ')';
        case '[':
            return right == ']';
        case '{':
        return right == '}';
    }
    return false;
}

bool isValidParentheses(string input) {
    stack<char> parenthesesStack;
    for (int i = 0; i < input.length(); i++) {
        if (isLeftParentheses(input[i])) {
            parenthesesStack.push(input[i]);
        } else {
            if (parenthesesStack.empty() || !isMatchParentheses(parenthesesStack.top(), input[i])) {
                return false;
            }
            parenthesesStack.pop();
        }
    }
    return parenthesesStack.empty();
}
```

### 用栈解决自上而下结构的问题

所谓的自上而下(Top-Down)结构，从逻辑理解的角度来看，实际上就是一种树形结构，从顶层出发，逐渐向下扩散，例如二叉树的遍历问题。 在实际运算的时候，我们先解决子问题，再利用子问题的结果解决当前问题。从算法角度而言，就是利用递归。用递归解决自上而下结构的问题，详见第7章。

由于栈的LIFO特性，可以利用栈数据结构消除递归。递归通常用函数调用自身实现，在调用的时候系统会分配额外的空间，并且需要用栈指针记录函数返回的位置，故额外开销(overhead)比较大。但在实际工作或面试中，往往用栈或者用递归的区别不大，按照自己擅长的方式做就可以。在使用栈的时候， 每个子问题应当被看成是同样类型的对象(object)，将该对象按照自上而下“的方向入栈。然后通过while循环，调用栈的pop()函数弹出栈顶元素并访问，直至栈清空。这样，后入栈的子问题会优先被弹出，相当于实现了递归。

> Given a binary tree, implement the In-Order Traversal using a stack

解题分析：中序遍历(In-Order Traversal)的规则是：1) 先遍历左子树 2) 访问当前节点 3) 遍历右子树。我们先逻辑上重现整个遍历过程，对于某个上层节点，先向左下降到下一层，解决子问题，回到当前节点，向右下降到下一层。很显然，遍历子树的过程是一个自上而下结构：从顶层出发，逐渐向下扩散。实际计算时，我们先解决子问题(遍
