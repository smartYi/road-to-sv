# Search in Rotated Array

Given a sorted array of n integers that has been rotated an unknown number of times, write code to find an element in the array. You may assume that the array was originally sorted in increasing order

## Solution

```java
/**
 * One half of the array must be ordered normally. We can therefore look at
 * the normally ordered half to determine whether we should search the left
 * or right half.
 *
 * The tricky condition is if the left and the middle are identical. We can
 * check if the rightmost element is different. If it is, we can search just
 * the right side. Otherwise, we have no choice but to search both halves
 */

int search(int a[], int left, int right, int x){
    int mid = (left + right) / 2;
    if (x == a[mid]){
        return mid;
    }
    if (right < left){
        return -1;
    }

    /**
     * Either the left or right half must be normally ordered. Find out
     * which side is normally ordered, and then use the normally ordered
     * half to figure out which side to search to find x
     */
    if (a[left] < a[mid]){ // Left is normally ordered
        if (x >= a[left] && x < a[mid]){
            return search(a, left, mid - 1, x); // Search left
        }
        else {;
            return search(a, mid + 1, right, x); // Search right
        }
    }
    else if (a[mid] < a[left]){ // Right is normally ordered
        if (x > a[mid] && x <= a[right]){
            return search(a, mid + 1, right, x); // Search right
        }
        else {
            return search(a, left, mid - 1, x); // Search left
        }
    } else if (a[left] == a[mid]){ // Left half is all repeats
        if (a[mid] != a[right]){ // If right is different, search it
            return search(a, mid + 1, right, x);
        }
        else { // Else, we have to search both halves
            int result = search(a, left, mid - 1, x); // Search left
            if (result == -1){
                return search(a, mid + 1, right, x); // Search right
            }
            else{
                return result;
            }
        }
    }
    return -1;
}
```