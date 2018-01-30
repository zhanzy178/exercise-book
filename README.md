# leetcode
The repository is used to store my code for problems in leetcode

## 015 three sum
> 2017-09-11 08:18

算法accpted！但是运行时间特别慢，只击败1%的cpp代码
算法学习改进中......

> 2017-09-13 08:27

通过网上学习代码，发现我的算法有如下不足：
- 排序使用的是优先队列，这里可以改用algorithm里的sort，方便消重，而2sum则不必要进行排序，因为这里排序至少需要nlogn的时间复杂度，而2sum只需要获得对应的一组目标的位置即可，则可以在n的时间复杂度内完成
- 在对第二，第三个数进行查找的时候，为了迎合大小顺序原则，对后面的序列进行了两次遍历，一次用于map存储下标，一次是逐个搜索；改进是需要放弃原来的2sum思想，因为对于3sum我们需要看到不同的是，这里有出现重复序列的可能；由于从小到大拼，所以可以自然的想到对后面的序列进行两端向中间靠拢的搜索，于是可以设置left，right，对于当前sum=nums[left]+nums[right]比较target，根据大小比适当移动left和right；实现这个的同时还需要进行消重的移动
- 外层循环里加入判断，判断当前位置nums[i]如果大于0，则后面的搜索是找不到新的目标序列的（但是不知道为什么加入这个后反而变慢了）





## 006 ZigZag Conversion
> 2017-09-13 12:16
- bug1: zigzag排列形式理解，以及规律探索
- bug2: s="AB" 1; 当转换行数为1时，出现bug，当作特殊情况处理，临界点bug





## 007 int2str
> 2017-09-18 08:35
- 对越界的判别方法
    - 符号变化
    - long long 范围用INT_MAX和INT_MIN判定





## 009 Palindrome Number
> 2017-09-19 13:00
- bug1 忘了考虑负数 x=INT_MIN=-2147483648 无法abs
- bug2 100021 验证中间变成只有一位的数字 不可用x<10判定
- bug3 `k==0` 不可`k==1`
- improve thinking 回文比较方法，数字反向后比较





## 010 Regular Expression Matching
> 2017-09-25 08:03
- 复杂思路：使用stack的思路，从后往前进行匹配，但是对于'\*'和'.'的特例情况无法彻底解决，只能通过“加插件”的方式对其补充特殊情况，这种方法导致自己越补漏洞越出现新的bug。所以需要取得新的思考方向，得到一个可证明的方法。总结，以后当一个思路修修补补了很多后就不要用这个思路了，换一个试试，因为这样的方法不优雅
- 递归思路：`isMatch(ssss, x\*xxx)=isMatch(ssss, xxx) | | ((s==x||x=='.')&&isMatch(sss, x\*xxx))`；以及`isMatch(ssss, xxxx)=isMatch(sss, xxx)；`得到状态转移方程，通过该方程递推判断；其实这是一个深度搜索，对所有情况递归遍历。时间复杂度，可以对最坏情况进行分析，即正则式是这种形式'x\*x\*x\*'，则每次递归都会进入两个子程序，考虑近似于遍历满二叉树，则树的最大深度为T+P/2，则时间复杂度O($2^{T+P/2}$)；达到指数级
- 动态规划思路：
    - Top-Down : 类似于上面的递归思路，但是，递归的思路做了很多重复运算，所以可以用一个矩阵保存已经计算过的结果，传入每一层的递归调用中，减少不必要的重复计算。这里通过递归调用的方法实现
    - Bottom-Up：不需要进行函数自身的递归调用，通过状态转移方程可以知道，一个状态的答案由右边的状态和下一层的状态决定，应该对整个状态矩阵进行从底向上从右到左的计算，最底层的计算可以在上面几层的计算里使用到
    - Bottom-Up：注意点：对于边界值的特殊情况的处理需要仔细斟酌，矩阵的两个维度可以大多一排，存特殊值。




## 169_Majority Element

> 2017-09-27 17:33

- finished
- 答案方法更巧妙，通过一个count，利用了众数过半的特点




## 053_Maximum Subarray

> 2017-09-27 18:08





## 215_Kth Largest Element in an Array

> 2017-09-30 08:52

- 加深理解了partition算法
- 更深一步学会使用three-way partition 算法









## 023_Merge k sorted lists

> 2017-09-30 15:04

- 难点
  - 指针操作
  - 队列递推，两两合并，反向BFS，自底向上





##  218_The Skyline Problem

> 2017-10-01 18:28

- 使用归并融合的思想
  - 分治思想：将建筑物分为两块，分别融合，在子块里面递归分块；
  - 所以递归调用的函数抽象成一个二叉树
  - 改进：通过队列自底向上进行两两归并
    - 每取队列头两个skyline进行归并，放入队列尾
    - 总体而言是在递归二叉树思想基础上一层一层向上归并，从而省去了递归调用函数所耗费的时间
- skyline归并方法———“影子叠加算法”
  - 当总体的算法过程确定后，算法的核心问题就变成了skyline的归并问题

  - skyline的归并方法是通过观察skyline的合并规律的总结，但是没有具体的证明过程

  - 方法：

    - 设两个已经按x坐标排序的skyline坐标的数组的当前高度h1和h2，初始化为0
    - 每次比较两个数组头的坐标的x值（比如pos1，pos2）
      - 假设 pos1.x<pos2.x
        - 在对应的数组skyline1头弹出pos1
        - 同时更新pos1对应skyline1高度值h1
        - `h1=pos1.y`
        - 将pos1插入合并后的数组newskyline前，比较h1，h2，取其中较大的数更新为pos1的新的高度，将pos1插入数组，**（注意：插入前要消重，防止插入连续y值相等的数）**
        - `newskyline.push_back(pair<int, inr>(pos1.x, max(h1, h2)));`
      - 当pos1.x==pos2.x
        - 同时弹出skyline1，skyline2的头元素pos1， pos2
        - 更新h1， h2
        - 再一次插入更新高度为`max(h1, h2)`新的坐标值到新合并后的数组中**（注意：插入前要消重，防止插入连续y值相等的数）**





##  310_Minimum Height Trees

> 2017-10-16 20:16

- 寻找最小高度树
- 一开始直观思路是：通过深度优先搜索寻找一条树中的叶子到叶子的最长路径，取路径中间节点
- 通过思考发现办法不实际，有特例使得仅通过深度优先搜索无法找寻到最长路径
- 简单查看答案发现一种更为简单的办法，类似于拓扑排序，简单的说就是每轮去掉树的叶子节点，直到最后仅剩下一片或者两片叶子
- 实现过程略复杂，考虑到一种可能出现的问题，即在一轮去除叶子节点的过程里会对树倒数第二层的节点实时更新，使得原本非叶子节点的节点在一轮为结束的时候变为叶子节点，导致误删
  - 设置标记数组，标记删除一个叶子节点后受其影响而更新的节点
  - 设置标记数组，标记已经被删除的节点，以便最后查询得到最终删剩的节点



## 133_Clone_Graph

> 2017-10-19 14:51

- 图的遍历
- 指针的使用
- map的使用



## 207_Course Schedule

> 2017-10-27 00:03

- 拓扑排序



## 032_Longest Valid Parentheses

> 2017-10-28 10:14

- 难度：hard


- stack，括号匹配
- DP，动态规划




## 174_Dungeon Game

> 2017-11-03 18.09

- 难度：hard
- 初始值问题
- DP，动态规划



## 044_Wildcard Matching

> 2017-11-16 23:47

- 难度：medium
- DP问题
- 边界值

## 091_Decode Ways

> 2017-11-15 15:11

- 难度：medium
- DP
- 两位和一位情况的仔细考虑

## 639_Decode Ways II

> 2017-11-15 17:47

- 难度： hard
- DP
- 分类讨论情况多，需要思维清晰



## 132_Palindrome Partitioning II

> 2017-11-28 14:24

- 难度：hard
- DP
- 将两个dp合并，节省运行时间，需要考虑末端特殊情况





## 140_Word Break II

> 2017-11-30 20:52

- 难度：hard
- DP+DFS



## 198\_House Robber & 303\_Range Sum Query-Immutable

> 2017-12-10 21:39

- 难度：easy
- DP



## 494_Target Sum

>  2017-12-10 21:39

- 难度：medium
- DP
- 考虑如何将原问题转换，通过数学公式简化为一个更简单的等价问题
- 对复杂的dp问题的思考，可以尝试转换，例如数组下标存在负数等



## 741_Cherry Pickup

> 2017-12-11 14:45

- 难度：hard
- DP
- 两条路径的最大路讨论，需要复杂的动态规划设计，考虑k长度的路径，同时计算两条路和的最大值



## 70_Climbing Stairs

> 2017-12-11 16:13

- 难度：easy
- DP、矩阵快速幂

## 669_Trim a Binary Search Tree
> 2017-12-15 10:59
- 难度：easy
- DFS，不能用delete释放内存

## 687_Longest Univalue Path
> 2017-12-15 10:59
>
- 难度：easy
- DFS



## 652_Find Duplicate Subtrees

> 2017-12-18 11:22

- 难度：medium
- DFS+Hash

## 662_Maximum Width of Binary Tree

> 2017-12-21 19:52

- 难度：medium
- DFS


## 671_Second Minimum Node In a Binary Tree

> 2018-01-26 16:54

- 难度：easy


## 501_Find Mode in Binary Search Tree

> 2018-01-30 07:38

- 难度: easy
