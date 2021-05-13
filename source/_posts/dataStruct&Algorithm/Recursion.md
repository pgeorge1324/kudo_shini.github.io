---
date: 2021-05-13 14:26:52
title: 递归（Recursion）
tags:
- dataStruct&Algorithm
categories:
- [dataStruct&Algorithm]
---

# 递归（Recursion）

### <u>递归需要满足的三个条件</u>

- **一个问题的解可以分解为几个子问题的解**
- **这个问题与分解之后的子问题，除了数据规模不同，求解思路完全一样**
- **存在递归终止条件**

```
1.爬楼梯
2.斐波那契数列 //小心整形溢出
3.各位相加（数根）//(num-1)%9+1
```

***编写递归代码的关键是，只要遇到递归，我们就把它抽象成一个递推公式，不用想一层层的调用关系，不要试图用人脑去分解递归的每个步骤。***

### 递归代码要警惕堆栈溢出

### 递归代码要警惕重复计算

### 将递归代码改写为非递归代码

```
int f(int n) 
{ 
    if (n == 1) return 1; 
    if (n == 2) return 2;
    int ret = 0;
    int pre = 2;
    int prepre = 1;
    for (int i = 3; i <= n; ++i) { 
    ret = pre + prepre; prepre = pre; pre = ret; 
    }
    return ret;
}
```
