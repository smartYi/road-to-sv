# XOR 异或

> 异或：相同为0，不同为1。也可用「不进位加法」来理解。

异或操作的一些特点：

    x ^ 0 = x
    x ^ 1s = ~x // 1s = ~0
    x ^ (~x) = 1s
    x ^ x = 0 // interesting and important!
    a ^ b = c => a ^ c = b, b ^ c = a // swap
    a ^ b ^ c = a ^ (b ^ c) = (a ^ b) ^ c // associative

# 移位操作

移位操作可近似为乘以/除以2的幂。0b0010 * 0b0110等价于0b0110 << 2. 下面是一些常见的移位组合操作。

1. 将x最右边的n位清零 `x & (~0 << n)`
2. 获取x的第n位值(0或者1) `x & (1 << n)`
3. 获取x的第n位的幂值 `(x >> n) & 1`
4. 仅将第n位置为1 `x | (1 << n)`
5. 仅将第n位置为0 `x & (~(1 << n))`
6. 将x最高位至第n位(含)清零 `x & ((1 << n) - 1)`
7. 将第n位至第0位(含)清零 `x & (~((1 << (n + 1)) - 1))`
8. 仅更新第n位，写入值为v; v为1则更新为1，否则为0 `mask = ~(1 << n); x = (x & mask) | (v << i)`

---

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