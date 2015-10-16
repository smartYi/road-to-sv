# Stack of Boxes

You have a stack of n boxes, with width wi, heights hi, and depths di. The boxes cannot be rotated and can only be stacked on top of one another if each box in the stack is strictly larger than the box above it in width, height, and depth. Implement a method to compute the height of the tallest possible stack. The height of a stack is the sum of the heights of each box.

## Solution

We can think about the recursive algorithm as making a choice, at each step, whether to put a particular box in the stack. (We will again sort our boxes in descending order by a dimension, such as height.)

First, we choose whether or not to put box 0 in the stack. Take one recursive path with box 0 at the bottom and one recursive path without box 0. return the better of the two opitions.

Then, we choose whether or not to put box 1 in the stack. Take one recursive path with box 1 at the bottom and one path without box 1. Return the better of the two options.

```java
int createStack(ArrayList<Box> boxes){
    Collections.sort(boxes, new BoxComparator());
    int[] stackMap = new int[boxes.size()];
}

int createStack(ArrayList<Box> boxes, Box bottom, int offset, int[] stackmap){
    if (offset >= boxes.size())
        return 0;

    Box newBottom = boxes.get(offset);
    int heightWithBottom = 0;
    if (bottom == null || newBottom.canBeAbove(bottom)) {
        if (stackMap[offset] == 0){
            stackMap[offset] = createStack(boxes, newBottom, offset + 1, stackMap);
            stackMap[offset] += new Bottom.height;
        }
        heightWithBottom = stackMap[offset];
    }

    // without this bottom
    int heightWithoutBottom = createStack(boxes, bottom, offset + 1, stackMap);

    // return better of two options
    return Math.max(heightWithBottom, heightWithoutBottom);
}
```