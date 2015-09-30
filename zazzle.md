第一个给我当面面试的公司，希望一切顺利

# 矩阵行走问题(On Campus)

一个 n*m 矩阵，从左上角走到右下角的所有路径，只能往右或者往下。需要说出时间复杂度。

## 题解

**思路一：数学法**

1. 使用排列组合。因为只能向右走或者向下走，在(m-1)+(n-1)次行走后，才能到达终点，也就是右下角。而在这m+n-2次行走中，有m-1次是向下，n-1次向右，所以是一个选择问题 C(m+n-2,m-1) = C(m+n-2,n-1)
2. 水平行走记作0，竖直行走记作1。每一种行走足迹可以作为一个0,1串，其中n-1个0，m-1个1。可以看做0000000000000（n-1个0）1111111111111（m-1个1）的重排列，也就是：(m+n-2)!/((m-1)!(n-1)!)

所以构造一个01串即可

**思路二：深搜**

如果矩阵有的格子可以走，有的格子不可以走，输出所有路径。(a[i][j]==1表示可以走，a[i][j]==0表示不可以走）

那么加入一个判断条件即可。

深搜的时间复杂度分析：每次递归有两个状态，一共需要递归 `m+n-2` 次。所以是 O(2^(m+n-2)) = O(2^n)

```cpp
//
//  main.cpp
//  InterviewTest
//
//  Created by WangDa on 9/29/15.
//  Copyright © 2015 DaWang. All rights reserved.
//

#include <iostream>
#include <vector>
using namespace std;

int pathCount = 0;

struct Point
{
    int x;
    int y;
    Point(int i, int j) : x(i), y(j)
    {}
};

//问题1  x y是起始点，m n是终止点 向量中坐标记录路径
void Path1(int x, int y, int m, int n, vector<Point>& vec, int len)
{
    if (x == m || y == n)
        return;
    Point p(x, y);
    vec[len++] = p;
    if (x == m - 1 && y == n - 1)
    {
        pathCount++;
        cout << "Path" << pathCount << ": ";
        for (int i = 0; i < vec.size(); ++i)
            cout << "(" << vec[i].x << ',' << vec[i].y << ") ";
        cout << endl;
    }
    else
    {
        Path1(x, y+1, m, n, vec, len);
        Path1(x+1, y, m, n, vec, len);
    }
}

//问题2
void Path2(int x, int y, int m, int n, vector<Point>& vec, int len, int safe[][4])
{
    if (x == m || y == n || safe[x][y] == 0)
        return;
    Point p(x, y);
    vec[len++] = p;
    if (x == m - 1 && y == n - 1)
    {
        for (int i = 0; i < vec.size(); ++i)
            cout << vec[i].x << ' ' << vec[i].y << endl;
    }
    else
    {
        Path2(x, y+1, m, n, vec, len, safe);
        Path2(x+1, y, m, n, vec, len, safe);
    }
}

int main(int argc, const char * argv[]) {
    int m = 3, n = 4;
    int x = 0, y = 0;
    int len = 0;
    Point p(0, 0);
    vector<Point> vec(m+n-1, p);
    Path1(x, y, m, n, vec, len);
    
    //int safe[][4] = { {1, 1, 1, 0},{0, 1, 1, 1}, {0, 0, 1, 1} };
    //Path2(x, y, m, n, vec, len, safe);
    return 0;
}

```

# 最长子回文串(phone screen)

参考 