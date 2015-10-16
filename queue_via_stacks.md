# Queue via Stacks

Implement a MyQueue class which implements a queue using two stacks

## Solution

```java
/**
 * StackNewest has the newest elements on top and stackOldest has the oldest
 * elements on top. When we dequeue an element, we want to remove the oldest
 * element first, and so we dequeue from stackOldest. If stackOldest is
 * empty, then we want to transfer all elements from stackNewest into this
 * stack in reverse order. To insert an element, we push onto stackNewest,
 * since it has the newest elements on top
 */
class MyQueue<T>{
    Stack<T> stackNewest, stackOldest;

    public MyQueue(){
        stackNewest = new Stack<T>();
        stackOldest = new Stack<T>();
    }

    public int size(){
        return stackNewest.size() + stackOldest.size();
    }

    public void add(T value){
        stackNewest.push(value);
    }

    public void shiftStack(){
        if (stackOldest.isEmpty()){
            while(!stackNewest.isEmpty()){
                stackOldest.push(stackNewest.pop());
            }
        }
    }

    public T peek(){
        shiftStack();
        return stackOldest.peek();
    }

    public T remove(){
        shiftStack();
        return stackOldest.pop();
    }
}
```