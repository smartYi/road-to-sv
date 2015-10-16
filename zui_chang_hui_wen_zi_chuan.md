+ 难度：中等

给出一个字符串（假设长度最长为1000），求出它的最长回文子串，你可以假定只有一个满足条件的最长回文串。

样例

    给出字符串 "abcdzdcab"，它的最长回文子串为 "cdzdc"。

挑战

    O(n2) 时间复杂度的算法是可以接受的，如果你能用 O(n) 的算法那自然更好。

## 题解

这个方法其实很直观，就是从头扫描到尾部，每一个字符以它为中心向2边扩展，扩展到不能扩展为止（有不同的字符），返回以每一个字符为中心的回文，然后不断更新最大回文并返回之。

算法简单，而且复杂度为O(n^2),空间复杂度为O(1)


```java
public class Solution {
    /**
     * @param s input string
     * @return the longest palindromic substring
     */
    public String longestPalindrome(String s) {
        if (s == null) {
            return null;
        }

        String ret = null;

        int len = s.length();
        int max = 0;
        for (int i = 0; i < len; i++) {
            String s1 = getLongest(s, i, i);
            String s2 = getLongest(s, i, i + 1);

            if (s1.length() > max) {
                max = Math.max(max, s1.length());
                ret = s1;
            }

            if (s2.length() > max) {
                max = Math.max(max, s2.length());
                ret = s2;
            }
        }

        return ret;
    }

    public String getLongest(String s, int left, int right) {
        int len = s.length();
        while (left >= 0 && right < len) {
            // when i is in the center.
            if (s.charAt(left) != s.charAt(right)) {
                break;
            }

            left--;
            right++;
        }

        return s.substring(left + 1, right);
    }
}

```

DP 方法

DP 因为是二维动态规划  Time:O(n^2), Space:O(n^2)

```java
public String longestPalindrome(String s) {
        if (s == null) {
            return null;
        }
        
        String ret = null;
        
        int len = s.length();
        int max = 0;
        
        boolean[][] D = new boolean[len][len];
        
        for (int j = 0; j < len; j++) {
            for (int i = 0; i <= j; i++) {
                D[i][j] = s.charAt(i) == s.charAt(j) && (j - i <= 2 || D[i + 1][j - 1]);
                if (D[i][j]) {
                    if (j - i + 1 > max) {
                        max = j - i + 1;
                        ret = s.substring(i, j + 1);
                    }
                }
            }
        }
        
        return ret;
    }
```

解说：
状态表达式：D[i][j] 表示i,j这2个索引之间的字符串是不是回文。
递推公式： D[i][j] = if ( char i == char j) && （D[i + 1][j - 1]  ||  j - i <= 2）） 这个很好理解，跟递归是一个意思，只不过 动规的好处就是我们可以重复利用这些结果。

初始化：
D[i][i] = true;实际上也不用特别初始化，都可以套到递推公式里头。 所以主页君的代码会看起来很简单。

注意：用max记录回文长度，回文找到时，更新一下max,及结果的起始地址，结束地址。

现在重点来了，我们怎么设计这个动规才可以重复利用呢？

从这里可以看出D[i + 1][j - 1]， 我们推i,j的时候用到了i+1, j-1，其实意思就是在计算i,j时，关于同一个j-1的所有的i必须要计算过。

画图如下：
    1. 00
    2. 00 01
          11
    3. 00 01 02
          11 12
             22
    3. 00 01 02 03                       
          11 12 13
             22 23  
                33

看到上面的递推关系了吗？只要我们一列一列计算，就能够成功地利用这个动规公式。这也是动态规划的关键性设计所在。
如果你不知道如何设计，就和主页群一样，画一个图来看我们计算某值的时候，需要哪些已经有的值。如上图所示，我们需要的是i+1, j - 1，实际上就是左下角的值，这样的话我们只要一列一列计算，就能成功动态规划。
注意：一行一行计算就会失败！

所以我们的循环的设计是这样的：

    for (int j = 0; j < len; j++) 
      { for (int i = 0; i <= j; i++) {

具体请参见代码，慢慢感受一下。这个才是动规的精髓咯。