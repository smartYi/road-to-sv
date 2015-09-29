+ Two's Complement - 负数可以看作是最高位的 1 为负，其他位为正，相加得到最后的值
	+ 例如 -1 = (1111) 最高位的 1 表示 -8， 剩下三位等于 7，相加后等于 -1
+ logical right shift - put a `0` in the most significant bit - `>>>`
+ arithmetic right shift - put a `1` in the most significant bit - `>>`

# Get Bit

Shifts 1 over by `i` bits, creating a value that looks like `00010000`. AND operation

	boolean getBit(int num, int i){
		return ((num & (1 << i)) != 0);
	}

# Set Bit

Shifts 1 over by `i` bits, creating a value like `00010000`. OR operation

	int setBit(int num, int i){
		return num | (1 << i);
	}

# Clear Bit

Create a number like `11101111` by creating the reverse of it (`00010000`). AND operation.

	int clearBit(int num, int i){
		int mask = ~(1 << i);
		return num & mask;
	}

To clear all bits from the most significant bit through `i` (inclusive), we create a mask with a `1` at the ith bit(1 << i). Then we subtract 1 from it, giving us a sequence of 0s followed by i 1s. AND operation.

	int clearBitsMSBthroughI(int num, int i){
		int mask = (1 << i) - 1;
		return num & mask;
	}

To clear bits from i through 0 (inclusive), we take a sequence of 1s (which is -1) and shift it over by 31 - i bits.

	int clearBitsIthrough0(int num, int i){
		int mask = ~(-1 >>> (31 - i));
		return num & mask;
	}

# Update Bit

Set the ith bit to a value `v`

	int updateBit(int num, int i, boolean bitIs1){
		int value = bitIs1 ? 1 : 0;
		int mask = ~(1 << i);
		return (num & mask) | (value << i);
	}