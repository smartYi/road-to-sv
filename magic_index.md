# Magic Index

A magic index in an array A[1...n-1] is defined to be an index such that A[i] = i. Given a sorted array of distinct integers, write a method to find a magic index, if one exists, in array A.

FOLLOW UP

    What if the values are not distinct?

## Solution

```java
int magicIndex1(int[] array){
    for (int i = 0; i < array.length; i++){
        if (array[i] == i){
            return i;
        }
    }
    return -1;
}

int magicIndex2(int[] array){
    int start = 0;
    int end = array.length - 1;

    while (end > start){
        int mid = (start + end) / 2;
        if (array[mid] == mid){
            return mid;
        }
        else if (array[mid] > mid){
            end = mid - 1;
        }
        else {
            start = mid + 1;
        }
    }
    return -1;
}

// FOLLOW UP, Use the solution to help understanding
int magicFast3(int[] array){
    return magicFast3(array, 0, array.length - 1);
}

int magicFast3(int[] array, int start, int end){
    if (end < start)
        return -1;

    int midIndex = (start + end) / 2;
    int midValue = array[midIndex];
    if (midValue == midIndex){
        return midIndex;
    }

    // search left
    int leftIndex = Math.min(midIndex - 1, midValue);
    int left = magicFast3(array, start, leftIndex);
    if (left >= 0){
        return left;
    }

    // seach right
    int rightIndex = Math.max(midIndex + 1, midValue);
    int right = magicFast3(array, rightIndex, end);

    return right;
}
```