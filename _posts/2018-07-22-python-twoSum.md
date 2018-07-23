---
layout: post
title: 两数之和
tags:
  - python,leetcode
lang: zh-Hans
---

在leetcode上做的第一个题目

<!--more-->
题目[[传送门]](https://leetcode-cn.com/problems/two-sum/description/)：

给定一个整数数组和一个目标值，找出数组中和为目标值的两个数。

你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。

示例:  
```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

这是最简单最慢的写法，还以为能侥幸通过，没想到就被一个极其变态的测试用例拦下来了，大致就是 [1,3,5,7,9,...,1609,1611] QAQ

```python
class Solution:
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """

        for i in range(len(nums)):
            for j in range(len(nums)):
                if nums[i]+nums[j]==target:
                    if i!=j:
                        return [i,j]
```

于是我又把这个方法优化了一丢丢，酱紫：

```python
class Solution:
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        list2=nums.copy()
        n=0
        for i in range(len(nums)):
            list2.pop(0)
            n+=1
            for j in range(len(list2)):
                if nums[i]+list2[j]==target:
                    if i!=j+n:
                        return [i,j+n]
```

好吧我老老实实的去看解法，我凑! 还可以使用字典这么玩：

# 方法一：暴力法

> 暴力法很简单。遍历每个元素 xx，并查找是否存在一个值与 target - xtarget−x 相等的目标元素。  
复杂度分析：

# 方法二：两遍哈希表
> 为了对运行时间复杂度进行优化，我们需要一种更有效的方法来检查数组中是否存在目标元素。如果存在，我们需要找出它的索引。保持数组中的每个元素与其索引相互对应的最好方法是什么？哈希表。  
通过以空间换取速度的方式，我们可以将查找时间从 O(n)O(n) 降低到 O(1)O(1)。哈希表正是为此目的而构建的，它支持以 近似 恒定的时间进行快速查找。我用“近似”来描述，是因为一旦出现冲突，查找用时可能会退化到 O(n)O(n)。但只要你仔细地挑选哈希函数，在哈希表中进行查找的用时应当被摊销为 O(1)O(1)。  
一个简单的实现使用了两次迭代。在第一次迭代中，我们将每个元素的值和它的索引添加到表中。然后，在第二次迭代中，我们将检查每个元素所对应的目标元素（target - nums[i]target−nums[i]）是否存在于表中。注意，该目标元素不能是 nums[i]nums[i] 本身！ 

# 方法三：一遍哈希表  
> 事实证明，我们可以一次完成。在进行迭代并将元素插入到表中的同时，我们还会回过头来检查表中是否已经存在当前元素所对应的目标元素。如果它存在，那我们已经找到了对应解，并立即将其返回。

python实现：  
```python
class Solution:
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        dict1={}
        for i in range(len(nums)):
            if target-nums[i] in dict1:
                return [dict1[target-nums[i]],i]
            if nums[i] not in dict1:
                dict1[nums[i]]=i
```

> 路还很长，我还年轻，真好