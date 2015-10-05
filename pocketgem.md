# PocketGem

> strStr()

待搜索

> 传入两个个数组，A = [1, 3,3,3,3, 6,6,6,6,6, 8,8]，  B = [5, 2, 4, 9];
写一个函数返回数组C = [6, 8, 3, -1];

A中每个数字都有重复，B中的数字代表不同的重复次数，返回这些重复次数分别对应的数字。
当然，如果A中有些不同的数组重复相同的次数，答案不唯一，any correct answer is accepted

> 题目如下，给一个科技树

          a     e
        /   \  /
       b    c
        \   /
          d

写一个function print出需要研究的科技

example： 
    
    If c is completed, 
        then techpath(d) => [a, b, d]

难点有两个：

1. how to check cycle?
2. how to print the path in right order?