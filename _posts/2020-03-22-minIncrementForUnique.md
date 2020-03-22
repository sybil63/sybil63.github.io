---
published: true
layout: post
title: 945. 使数组唯一的最小增量，贪心 O(max(A_i))
date: 2020-03-22 11:17
comments: true
readingtime: 2
tags: [leetcode, 中等]
---
### 解题思路
观察可发现，此题的`A[i]`范围极小（`0 <= A[i] < 40000`），
所以可以用一个`Hash`记录数组里每个元素出现的次数，如果有重复的元素，那么就是需要处理的。

由于只能进行递增操作，需要使每个元素增加的次数最小，那么就让它**增加为最近的不重复元素即可**。
比如 `[1, 2, 2, 2, 3, 3]`， 在`hash`里为`[0, 1, 3, 2]`（0出现0次，1出现1次，2出现3次，3出现2次），
 * 发现`2`不唯一，就需要把多余的`2`变为与它相邻的元素`3`， hash=[0, 1, 1, 5]，minInc = 2；
 * 发现`3`不唯一，就需要把多余的`3`变为与它相邻的元素`4`， hash=[0, 1, 1, 1, 4]， minInc = 2 + 4 = 6；
 * 以此类推，直到`hash`的边界`max(A[i])`，假设`hash[max(A[i])] - 1 = n`，用求和公式算出**边界答案**, $$minInc = minInc + \frac{n(n+1)}{2}$$。因为只需要把多余的元素依次变为
 $$max(A[i])+1, max(A[i])+2, ..., + max(A[i]) + n$$

**复杂度分析：**
* 时间复杂度: $$O(max(A_{i}))$$
* 空间复杂度: $$O(max(A_{i}))$$

### 代码

```java
class Solution {
    public int minIncrementForUnique(int[] A) {
        int[] times = new int[40001];
        int maxValue = 0;
        for (int a: A) {
            times[a] ++;
            if (a > maxValue) maxValue = a;
        }
        int minInc = 0;
        for (int i = 0; i <= maxValue; ++i) {
            if (times[i] > 0) {
                times[i+1] += times[i]-1;
                minInc += times[i]-1;
            }
        }
        if (times[maxValue+1] > 0) {
            int n = times[maxValue+1] - 1;
            minInc += n*(n+1)/2;
        }
        return minInc;
    }
}
```
