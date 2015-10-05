# Google

> 给一个先递增后递减的数组，找到最大值。这数组是非严格递增递减，也就是说有重复。

写了个Binary Search之后面试官问我如果输入是1233321怎么办

> Moving Average. 给的是一个stream,求前K个数的平均值

用个dequeue存K个数就行了

> 给你一个URL让你求这个URL访问到的页面的和。面试官给了一个API，输入URL返回URL所连接到的URL

写个DFS然后调用这个API，再用个Set去重就好了。