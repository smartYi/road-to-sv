# Sort Stack

Write a program to sort a stack such that the smallest items are on the top. You can use an additional temporary stack , but you may not copy the elements into any other data structure (such as an array). The stack supports the following operations: push, pop, peek, and isEmpty.

## Solution

```java
public Stack<Integer> sort(Stack<Integer> s){
    Stack<Integer> r = new Stack<Integer>();

    while(!s.isEmpty()){
        int tmp = s.pop();
        while (!r.isEmpty() && r.peek() > tmp){
            s.push(r.pop());
        }
        r.push(tmp);
    }
    return r;
}
```