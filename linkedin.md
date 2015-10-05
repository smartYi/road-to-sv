# Linkedin

> search similar people的API

大致说一下前端，后端，不用太细

> 给一排房子，用RGB三种颜色染色，相邻不能染成同色，每个房子染对应颜色会有对应的weight（W[N][3]），求最大的weight和

follow up，N种颜色

> 两个字符串S，T，求最短的S的子串，使其包含T中所有的character，character可重复

sliding window

> nested array，etc [[1,2], 2, [[3], [4]]]，input是nested array的iterator，实现next element的iterator

版上高频 

> 罗马转十进制

反过来

> design，tiny url 

高频

> Maximum SubArray  

leetcode 只要return max value 然后过一遍他的input

> 比较长  

    /**
     * Find threevalues that can be the lengths of the sides of a triangle.
     * Three segmentsof lengths A, B, C can form a triangle if and only if:
     *
     *      A + B > C
     *      B + C > A
     *      A + C > B
     *
     * e.g.
     *  6, 4, 5 can form a triangle
     * 10, 2, 7 can't
     *
     * @paramsegmentLengths the lengths of segments that might form a triangle.
     * @return threevalues that can be the lengths of the sides of a triangle,
     *         or an empty array if there are no suchvalues in segmentLengths.
     * sample input:segmentLengths = [10, 5, 7, 4, 3]
     */
     
第一反应brute force 因为没见过 同时手极快的google了 然后google到了这个（没权限发链接 大家自行google geeksforgeeks/find-number-of-triangles-possible）

但是面试官不要求count 有多少个triangle 只要return 一组 就行了 此题的重点是要sort 好像背后还有什么数学原因

我当时就扫了遍帖子 说要sort 然后后面写了个iteration match 第一组符合的pair