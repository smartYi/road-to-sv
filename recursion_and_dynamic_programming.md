+ Recursive solutions, by definition, are built off of solutions to subproblems.
+ The bottom-upa approach is ofent the most intuitive. Start with simple case.
+ The top-down approach can be more complex since it's less concrete. We think about how we can divide the problem for case N into subproblems.
+ Half-and-Half Approach -> Binary Search

# Recursive vs. Iterative Solutions

+ Recursive algorithms can be very space inefficient.
+ Each recursive call adds a new layer to the stack, which means that if your algorithm recurses to a depth of n, it uses at lest O(n) memory.
+ **All** recursive algorithms can be implemented iteratively, although sometimes the code to do so is much more complex.

# Dynamic Programming & Memorization

+ Taking a recursive algorithm and finding the overlapping subproblems.
+ Cache those results for future recursive calls.

一个例子: Fibonacci Numbers

Recursive

	int fibonacci(int i){
		if (i == 0) return 0;
		if (i == 1) return 1;
		return fibonacci(i-1) + fibonacci(i-2);
	}

Cache the results of fibonacci(i) between calls

	int fibonacci(int n){
		return fibonacci(n, new int[n+1]);
	}

	int fibonacci(int i, int[] memo){
		if (i == 0 || i == 1) return i;

		if (memo[i] == 0){
			memo[i] = fibonacci(i-1, memo) + fibonacci(i-2, memo);
		}
		return memo[i];
	}

Bottom-Up Dynamic Programming

	int fibonacci(int n){
		if (n == 0) return 0;
		else if (n == 1) return 1;

		int[] memo = new int[n];
		memo[0] = 0;
		memo[1] = 1;
		for (int i = 2; i < n; i++){
			memo[i] = memo[i-1] + memo[i-2];
		}
		return memo[n-1] + memo[n-2];
	}

Just store afew variables

	int fibonacci(int n){
		if (n == 0) return 0;
		int a = 0;
		int b = 1;
		for (int i = 2; i < n; i++){
			int c = a + b;
			a = b;
			b = c;
		}
		return a + b;
	}