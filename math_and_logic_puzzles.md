# Prime Numbers

Every positive integer can be decomposed into a product of primes

# Divisibility

    Let x = 2^(j0) * 3^(j1) * 5 ^(j2) * .....
    Let y = 2^(k0) * 3^(k1) * 5 ^(k2) * .....

If mod(y, x) == 0, for all i, ji < ki

    gcd(x,y) = 2^min(j0,k0) * 3^min(j1,k1) * 5^min(j2,k2) * ...
    lcm(x,y) = 2^max(j0,k0) * 3^max(j1,k1) * 5^max(j2,k2) * ...
    gcd * lcm = xy

# Check for Primality

	boolean prime(int n){
		if (n < 2){
			return false;
		}
		int sqrt = (int) Math.sqrt(n);
		for (int = 2; i <= sqrt; i++){
			if (n % i == 0) return false;
		}
		return true;
	}

# Probability

P(A and B) = P(B given A) P(A)

P(A or B) = P(A) + P(B) - P(A and B)

# Start Talking

Don't panic when you get a brainteaser. Like algorithm questions, interviewers want to see how you tackle a problem; they don't expect you to immediately know the anser. Start talking, and show the interviewer how you approach a problem.