# Sorted Merge

You are given two sorted arrays, A and B, where A has a large enough buffer at the end to hold B. Write a method to merge B into A in sorted order

## Solution

```java
// basic merging process
public static void merge(int[] a, int[] b, int lastA, int lastB){
    int indexA = lastA - 1;
    int indexB = lastB - 1;
    int indexMerged = lastB + lastA - 1;

    while (indexB >= 0){
        if (indexA >= 0 && a[indexA] > b[indexB]){
            a[indexMerged--] = a[indexA--];
        }
        else {
            a[indexMerged--] = b[indexB--];
        }
    }
}
// Note that you don't need to copy the contents of A after running out of
// elements in B. They are already there.
```