# Binary Search and log\(n\) problems

## 458 Last Position of Target

Find the last position of a target number in a sorted array. Return -1 if target does not exist.

Have you met this question in a real interview?

Yes

**Example**

Given`[1, 2, 2, 4, 5, 5]`.

For target =`2`, return 2.

For target =`5`, return 5.

For target =`6`, return -1.

[http://www.lintcode.com/en/problem/longest-palindrome/](http://www.lintcode.com/en/problem/last-position-of-target/#)

### 解题分析:

关键词：sorted array, last position, -1

典型的二分查找加线性搜索， 直接写代码就行了

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: An integer array sorted in ascending order
     * @param target: An integer
     * @return: An integer
     */
    int lastPosition(vector<int> &nums, int target) {
        // write your code here
        if (nums.empty())
            return -1;
        size_t beg = 0, end = nums.size() - 1, mid = 0;
        while(beg + 1 < end)
        {
            mid = beg + (end - beg) / 2;
            if (nums[mid] >= target)
                end = mid;
            else
                beg = mid;
        }
        int ind = -1;
        if (nums[end] == target)
            ind = end;
        else if (nums[beg] == target)
            ind = beg;
        else 
            return -1;
        while(ind + 1 < nums.size() && nums[ind] == nums[ind+1])
            ind++;
        return ind;
    }
};
```

### 复杂度分析:

O\(log\(n\)\), n = nums.size\(\), 最坏的情况是o\(n\)

## 585 Maximum Number in Mountain Sequence

Given a mountain sequence of`n`integers which increase firstly and then decrease, find the mountain top.

Have you met this question in a real interview?

Yes

**Example**

Given`nums`=`[1, 2, 4, 8, 6, 3]`return`8`  
Given`nums`=`[10, 9, 8, 7]`, return`10`

[http://www.lintcode.com/en/problem/maximum-number-in-mountain-sequence/\#](http://www.lintcode.com/en/problem/maximum-number-in-mountain-sequence/#)

### 解题分析:

二分法的第二种应用OOOXXX， 找分界点问题。

任何一类问题，当左边是O， 右边是X的时候找最后一个O，或者第一个X的时候可以用二分法的思想来解决。

找第一个X伪码如下：

for \( beg : end\)

mid = beg + \(end - beg\) /2

if  isX\(mid\):

```
   beg = mid
```

else

```
   end = mid
```

if \(isX\(end\)\)

```
  return end
```

return beg

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: a mountain sequence which increase firstly and then decrease
     * @return: then mountain top
     */
    int mountainSequence(vector<int> &nums) {
        // write your code here
        int n = nums.size();
        if ( 0 == n)
            return INT_MIN;
        if ( 1 == n)
            return nums[0];

        int beg = 0, end = n-1, mid = 0;
        while( beg + 1 < end)
        {
            mid = beg + (end-beg)/2;
            if (nums[mid] > nums[mid+1]) // decreasing
                end = mid;
            else // increasing
                beg = mid;
        }
        if (nums[beg] > nums[beg+1])
            return nums[beg];
        return nums[end];
    }
};
```

### 复杂度分析:

O\(log\(n\)\)

## 460 Find K Closest Elements

Given a target number, a non-negative integer`target`and an integer array A sorted in ascending order, find the`k`closest numbers to target in A, sorted in ascending order by the difference between the number and target. Otherwise, sorted in ascending order by number if the difference is same.

Have you met this question in a real interview?

Yes

**Example**

Given A =`[1, 2, 3]`, target =`2`and k =`3`, return`[2, 1, 3]`.

Given A =`[1, 4, 6, 8]`, target =`3`and k =`3`, return`[4, 1, 6]`.

[http://www.lintcode.com/en/problem/find-k-closest-elements/\#](http://www.lintcode.com/en/problem/find-k-closest-elements/#)

### 解题分析:

算是一道二分法的变种

关键词： sorted, closest, ascending

二分法的时候不但可以用来找给定数字，而且可以用来找最近。因为本质上二分是用的夹逼， 同微积分里面算极限类似。如果用基本二分法的通用模板的话，我们可以发现，最后beg和end的index所代表的值是最逼近所需要查找的值的。

那么从beg 或者 end开始， 向周围扩散k个数字则是我们期望的答案。  以\[ 1, 4, 6, 8\] ， target = 3, k =3 为例子， beg = 0, end = 1, 因为a\[1\] = 4 最接近3，从end 开始，两边用双指针法逐步更新答案 \[4, 1, 6\] 为所得。

### 代码：

```cpp
class Solution {
public:
    /**
     * @param A: an integer array
     * @param target: An integer
     * @param k: An integer
     * @return: an integer array
     */
    vector<int> kClosestNumbers(vector<int> &A, int target, int k) {
        // write your code here
        vector<int> result;
        if (A.empty() || k == 0)
            return result;
        // what if k > A.size()?

        int beg = 0, end = A.size() - 1, mid = 0;
        while (beg + 1 < end)
        {
            mid = beg + (end - beg) / 2;
            if (A[mid] >= target)
                end = mid;
            else
                beg = mid;
        }

        int left = beg, right = end;
        for (int i = 0; i < k ; i++)
        {
            if (left < 0 && right >= A.size())
                break;

            if (left < 0)
            {
                result.push_back(A[right++]);
                continue;
            }
            if (right >= A.size())
            {
                result.push_back(A[left--]);
                continue;
            }
            if ( abs(A[left] - target) <= abs(A[right] - target) )
            {
                result.push_back(A[left--]);
            }
            else
            {
                result.push_back(A[right++]);
            }
        }
        return result;
    }
};
```

### 复杂度分析:

O\(log\(n\) + k\), n = A.size\(\)

## 447 Search in a Big Sorted Array

Given a big sorted array with positive integers sorted by ascending order. The array is so big so that you can not get the length of the whole array directly, and you can only access the kth number by`ArrayReader.get(k)`\(or ArrayReader-&gt;get\(k\) for C++\). Find the first index of a target number. Your algorithm should be in O\(log k\), where k is the first index of the target number.

Return -1, if the number doesn't exist in the array.

##### Notice

If you accessed an inaccessible index \(outside of the array\), ArrayReader.get will return`2,147,483,647`.

Have you met this question in a real interview?

Yes

**Example**

Given`[1, 3, 6, 9, 21, ...]`, and target =`3`, return`1`.

Given`[1, 3, 6, 9, 21, ...]`, and target =`4`, return`-1`.

### 解题分析:

Keywords: sorted array, search

明显还是二分法， 但是与一般的二分不一样的地方是这道题目不知道终点在哪里，所以就要二分的办法找到第一个大于target的位置，在进行二分搜索。 越界条件很有意思， 是INT\_MAX，所以肯定不是target， 否则无法搜索。

1. 找到终点
2. 二分搜索

### 代码：

```cpp
class Solution {
public:
    /*
     * @param reader: An instance of ArrayReader.
     * @param target: An integer
     * @return: An integer which is the first index of target.
     */
    int searchBigSortedArray(ArrayReader * reader, int target) {
        // write your code here
        if (!reader)
            return -1;
        int beg = 0, end = 1;

        while(reader->get(end) < target)
            end *= 2;

        while(beg + 1 < end)
        {
            int mid = beg + (end-beg)/2;
            int val = reader->get(mid);
            if (target <= val)
                end = mid;
            else
                beg = mid;
        }
        if (reader->get(beg) == target)
            return beg;
        else if (reader->get(end) == target)
            return end;
        return -1;
    }
};
```

### 复杂度分析:

O\(log\(k\) \)

## 428 Pow\(x, n\)

Implement pow\(x, n\).

##### Notice

You don't need to care about the precision of your answer, it's acceptable if the expected answer and your answer 's difference is smaller than`1e-3`.

Have you met this question in a real interview?

Yes

**Example**

```
Pow(2.1, 3) = 9.261
Pow(0, 1) = 0
Pow(1, 0) = 1
```

[http://www.lintcode.com/en/problem/powx-n/](http://www.lintcode.com/en/problem/powx-n/)

### 解题分析:

求power肯定就是二分了 ， 关键是有几个corner case

x= 0

n = 0

n是负数

考虑清楚这几个边界条件后，直接写代码

### 代码：

递归版本：

```cpp
class Solution {
public:
    /*
     * @param x: the base number
     * @param n: the power number
     * @return: the result
     */
    double myPow(double x, int n) {
        // write your code here
        if (n == 0)
            return 1;
        if (n == 1)
            return x;
        if (n == -1)
            return 1./x;
        double val = myPow(x, n/2);
        if (n%2)
            return val*val* (n<0? 1./x:x);
        return val*val;
    }
};
```

非递归版本：非递归版本看的答案，我觉得很巧妙。 有一个地方要想到： 就是如果要以log\(n\)的复杂度来解决问题需要从n收敛到n=0,在收敛过程中不行的n/=2,同时不停的ans\*= tmp, tmp的初始值为x， 这样在幂指数收敛过程中答案也在以log\(n\)的速度给出， 我觉得很巧妙, 可惜我没看懂lol中间的一个trick, 放弃非递归的解法

### 复杂度分析:

O\(log\(n\) \)



## 159 Find Minimum in Rotated Sorted Array

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

\(i.e.,`0 1 2 4 5 6 7`might become`4 5 6 7 0 1 2`\).

Find the minimum element.

##### Notice

You may assume no duplicate exists in the array.

Have you met this question in a real interview?

Yes

**Example**

Given`[4, 5, 6, 7, 0, 1, 2]`return`0`

### 解题分析:

因为是rotated array, 那么最小的值就是rotated的位置符合前面描述的OOXX的标准：

左面大于A\[0\], 右面小于A\[0\]

如图![](/assets/159.png)

### 代码：



```cpp
class Solution {
public:
    /**
     * @param nums: a rotated sorted array
     * @return: the minimum number in the array
     */
    int findMin(vector<int> &nums) {
        // write your code here
        int n = nums.size();
        if (0 == n)
            return INT_MAX;
        if (1 == n)
            return nums[0];
        if (nums[0] < nums[n-1])
            return nums[0];
        int beg = 0, end = n-1, mid = 0;
        while(beg + 1 < end)
        {
            mid = beg + (end - beg)/2;
            if (nums[mid] < nums[0])
                end = mid;
            else
                beg = mid;
        }
        return nums[beg] < nums[end]? nums[beg] : nums[end];
    }
};
```

### 复杂度分析:

O\(log\(n\) \)

### 笔记:

这个题目在升序的情况下出错了， \[1, 2, 3\]， 后来临时加了一个判断来判定是不是升序。 原因是OOXX假设了先升序后降序，这样代码就不work, 所以OOXX的套路下， 做好先判断是不是符合再OOXX


