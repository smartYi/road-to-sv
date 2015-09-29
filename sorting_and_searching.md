+ Bubble Sort | Runtime: O(n^2) | Memory: O(1)
+ Selection Sort | Runtime: O(n^2) | Memory: O(1)
+ Merge Sort | Runtime: O(n log(n)) | Memory: Depends
+ Quick Sort | Runtime: O(n log(n)) O(n^2) - worst case | Memory: O(log(n))
+ Radix Sort | Runtime: O(kn)

# Merge Sort

Merge sort divides then array in half, sorts each of those halves, and then merges them back together. Each of those halves has the same sorting algorithm applied to it. Eventually, you are merging just two single-element arrays. It is the “merge” part that does all the heavy lifting.

The merge method operates by copying all the elements from the target array segment into a helper array, keeping track of where the start of the left and right halves should be (`helperLeft` and `helperRight`).

	void mergesort(int[] array){
	    int[] helper = new int[array.length];
	    mergesort(array, helper, 0, array.length - 1)
	}

	void merge(int[] array, int[] helper, int low, int middle, int high){
	    for (int i = low; i <= high; i++){
	        helper[i] = array[i];
	    }

	    int helperLeft = low;
	    int helperRight = middle + 1;
	    int current = low;

	    while (helperLeft <= middle && helperRight <= high){
	        if (helper[helperLeft] <= helper[helperRight]){
	            array[current] = helper[helperLeft];
	            helperLefting;
	        }
	        else {
	            array[current] = helper[helperRight];
	            helperRight++;
	        }
	        current++;
	    }

	    int remaining = middle - helperLeft;
	    for (int i = 0; i <= remaining; i++){
	        array[current + i] = helper[helperLeft + i];
	    }
	}

You may notice that only the remaining elements from the left half of the helper array are copied into the target array. Why not the right half? The right half doesn't need to be copied because it's already there.

# Quick Sort

In quick sort, we pick a random element and partition the array, such that all numbers that are less than the partitioning element come before all elements that are greater than it. The partitioning can be performed efficiently through a series of swaps.

	void quickSort(int arr[], int left, int right){
	    int index = partition(arr, left, right);
	    if (left < index - 1){
	        quickSort(arr, left, index - 1);
	    }
	    if (index < right){
	        quickSort(arr, index, right);
	    }
	}

	int partition(int arr[], int left, int right){
	    int pivot = arr[(left + right) / 2];
	    while (left <= right){
	        while (arr[left] < pivot) left++;
	        while (arr[right] > pivot) right--;

	        // Swap elements, and move left and right indices
	        if (left <= right){
	            swap(arr, left, right);
	            left++;
	            right--;
	        }
	    }
	    return left;
	}

# Radix Sort

Takes advantage of the fact that integers have finite number of bits. In radix sort, we iterate through each digit of the number, grouping numbers by each digits.

# Binary Search

注意加一和减一的问题

	int binarySearch(int[] a, int x){
	    int low = 0;
	    int high = a.length - 1;
	    int mid;

	    while (low <= high){
	        mid = (low + high) / 2;
	        if (a[mid] < x){
	            low = mid + 1;
	        }
	        else if (a[mid] > x){
	            high = mid - 1;
	        }
	        else {
	            return mid;
	        }
	    }
	    return -1;
	}

	int binarySearchRecursive(int[] a, int x, int low, int high){
	    if (low > high) return -1;

	    int mid = (low + high) / 2;
	    if (a[mid] < x){
	        return binarySearchRecursive(a, x, mid + 1, high);
	    }
	    else if (a[mid] > x){
	        return binarySearchRecursive(a, x, low, mid - 1);
	    }
	    else{
	        return mid;
	    }
	}