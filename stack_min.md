# Stack Min

How would you design a stack which, in addition to push and pop, has a function min which returns the minimum element? Push, pop and min should all operate in O(1) time

## Solution

```java
/**
 * use an additional stack to keep track of the mins
 */
class MinStack extends Stack<Integer>{
    Stack<Integer> s;
    public MinStack(){
        s = new Stack<Integer>();
    }

    public void push(int value){
        if (value <= min()){
            s.push(value);
        }
        super.push(value);
    }

    public Integer pop(){
        int value = super.pop();
        if (value == min()){
            s.pop();
        }
        return value;
    }

    public int min(){
        if (s.empty()){
            return Integer.MAX_VALUE;
        }
        else {
            return s.peek();
        }
    }
}
```