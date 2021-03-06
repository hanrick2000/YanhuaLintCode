## 943. Range Sum Query - Immutable

Given an integer array nums, find the sum of the elements between indices`i`and`j(i ≤ j)`, inclusive.

### Example

```
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -
>
 1
sumRange(2, 5) -
>
 -1
sumRange(0, 5) -
>
 -3
```

[https://www.lintcode.com/problem/range-sum-query-immutable/description](https://www.lintcode.com/problem/range-sum-query-immutable/description)

### 解题分析:

感觉没啥技巧，硬上就好了

### 代码：

```cpp
class NumArray {
public:

    NumArray(vector<int> nums) {
        prefixsum.push_back(0);
        for (int i = 0; i < nums.size(); i++)
            prefixsum.push_back(nums[i]+prefixsum[i]);

    }

    int sumRange(int i, int j) {
        if (i < 0) i = 0;
        if (j+1 > prefixsum.size()-1) j = prefixsum.size()-2;
        return prefixsum[j+1] - prefixsum[i];
    }
    vector<int> prefixsum;    
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * int param_1 = obj.sumRange(i,j);
 */
```

## 548. Intersection of Two Arrays II

Given two arrays, write a function to compute their intersection.

### Example

Given_nums1_=`[1, 2, 2, 1]`,_nums2_=`[2, 2]`, return`[2, 2]`.

### Challenge

* What if the given array is already sorted? How would you optimize your algorithm?
* What if nums1's size is small compared to num2's size? Which algorithm is better?
* What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

[https://www.lintcode.com/problem/intersection-of-two-arrays-ii/description](https://www.lintcode.com/problem/intersection-of-two-arrays-ii/description)

### 解题分析:

感觉没啥技巧，硬上就好了

### 代码：

```cpp
class Solution {
public:

    /*
     * @param nums1: an integer array
     * @param nums2: an integer array
     * @return: an integer array
     */
    vector<int> intersection(vector<int> nums1, vector<int> nums2) {
        // write your code here
        vector<int> res;
        sort(nums1.begin(), nums1.end());
        sort(nums2.begin(), nums2.end());
        auto it1 = nums1.begin();
        auto it2 = nums2.begin();
        while(it1!= nums1.end() && it2!=nums2.end())
        {
            if (*it1 < *it2)
                it1++;
            else if (*it1 > *it2)
                it2++;
            else
            {
                res.push_back(*it1);
                it1++;it2++;
            }
        }
        return res;
    }
};

class Solution {
public:

    /*
     * @param nums1: an integer array
     * @param nums2: an integer array
     * @return: an integer array
     */
    vector<int> intersection(vector<int> nums1, vector<int> nums2) {
        // write your code here
        vector<int> res;
        if (nums1.size() < nums2.size())
            swap(nums1, nums2);
        unordered_map<int, int> counter;
        for (auto each : nums1)
            counter[each]++;
        for (auto each : nums2)
        {
            if (counter.count(each) )
            {
                res.push_back(each);
                if (--counter[each] == 0)
                    counter.erase(each);
            }
        }
        return res;
    }
};
```

## 405. Submatrix Sum

Given an integer matrix, find a submatrix where the sum of numbers is zero. Your code should return the coordinate of the left-up and right-down number.

### Example

Given matrix

```
[
  [1 ,5 ,7],
  [3 ,7 ,-8],
  [4 ,-8 ,9],
]
```

return`[(1,1), (2,2)]`

### Challenge

O\(n3\) time.

[https://www.lintcode.com/problem/submatrix-sum/description](https://www.lintcode.com/problem/submatrix-sum/description)

### 解题分析:

和2D求最大sum一样

### 代码：

```cpp
class Solution {
public:
    /*
     * @param matrix: an integer matrix
     * @return: the coordinate of the left-up and right-down number
     */
    vector<vector<int>> submatrixSum(vector<vector<int>> &matrix) {
        // write your code here
        vector<vector<int>> res;
        int m = matrix.size();
        if (m == 0)
            return res;
        int n = matrix[0].size();
        if (n == 0)
            return res;
        vector<int> buffer(n, 0);

        for (int i =0; i < m; i++)
        {
            for (int j = i; j <m; j++)
            {
                compress(i,j, buffer, matrix, n);
                auto ret = eval(buffer);
                if (ret.first !=-1)
                {
                    res.push_back(vector<int>{i, ret.first});
                    res.push_back(vector<int>{j, ret.second});
                    return res;
                }
            }
        }
        return res;
    }

    void compress(int i, int j, vector<int>& buffer, vector<vector<int>>& matrix, int n)
    {
        fill(buffer.begin(), buffer.end(), 0);
        for (int k = i; k <= j; k++)
        {
            for (int l = 0; l < n; l++)
                buffer[l] += matrix[k][l];
        }
    }

    pair<int, int> eval(const vector<int>& buffer)
    {
      unordered_map<int,int> dict{{0,-1}};
      int sum = 0;
      for (int i = 0; i < buffer.size(); i++)
      {
          sum += buffer[i];
          if (dict.count(sum))
            return make_pair(dict[sum]+1, i);
          else
            dict[sum] = i;
      }
      return make_pair(-1,-1);
    }
};
```

## 665. Range Sum Query 2D - Immutable

Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner`(row1, col1)`and lower right corner`(row2, col2)`.

### Example

Given matrix =

```
[
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]
```

sumRegion\(2, 1, 4, 3\) -&gt;`8`  
sumRegion\(1, 1, 2, 2\) -&gt;`11`  
sumRegion\(1, 2, 2, 4\) -&gt;`12`

[https://www.lintcode.com/problem/range-sum-query-2d-immutable/description](https://www.lintcode.com/problem/range-sum-query-2d-immutable/description)

### 解题分析:

画格子

### 代码：

```cpp
class NumMatrix {
public:
    NumMatrix(vector<vector<int>> matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        dp = vector<vector<int>>(m+1,vector<int>(n+1, 0));
        for (int i =0; i < m; i++)
        {
            for (int j = 0; j < n; j++)
            {
                dp[i+1][j+1] = dp[i+1][j]+dp[i][j+1]+matrix[i][j] - dp[i][j];
            }
        }
    }

    int sumRegion(int row1, int col1, int row2, int col2) {
        return dp[row2+1][col2+1]-dp[row1][col2+1] - dp[row2+1][col1]+dp[row1][col1];
    }

    vector<vector<int>> dp;

};

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * int param_1 = obj.sumRegion(row1,col1,row2,col2);
 */
```

## 793. Intersection of Arrays

Give a number of arrays, find their intersection, and output their intersection size.

### Example

Given`[[1,2,3],[3,4,5],[3,9,10]]`, return`1`

```
explanation:
Only element 3 appears in all arrays, the intersection is [3], and the size is 1.
```

Given`[[1,2,3,4],[1,2,5,6,7][9,10,1,5,2,3]]`, return`2`

```
explanation:
Only element 1,2 appear in all arrays, the intersection is [1,2], the size is 2.
```

[https://www.lintcode.com/problem/intersection-of-arrays/description](https://www.lintcode.com/problem/intersection-of-arrays/description)

### 解题分析:

pq

### 代码：

```cpp
class Solution {
public:
    /**
     * @param arrs: the arrays
     * @return: the number of the intersection of the arrays
     */
    struct Node{
        Node(int r, int c)
            : row(r), col(c)
        {}
        int row, col;
    };
    int intersectionOfArrays(vector<vector<int>> &arrs) {
        // write your code here

        for (int i = 0; i < arrs.size(); i++)
        {
            sort(arrs[i].begin(), arrs[i].end());
        }
        auto cmp = [&arrs](const Node& left, const Node& right)
        {
            return arrs[left.row][left.col] > arrs[right.row][right.col];
        };

        priority_queue<Node,vector<Node>, decltype(cmp)> pq(cmp);
        for (int i = 0; i < arrs.size(); i++)
        {
            if (!arrs[i].empty())
                pq.push(Node(i,0));
            else
                return 0;
        }

        int res = 0, count = 0, last = 0;

        while(!pq.empty())
        {
            auto node = pq.top();
            pq.pop();
            if (arrs[node.row][node.col] != last || count == 0)
            {
                if (count == arrs.size())
                    res++;
                count = 1;
                last = arrs[node.row][node.col];
            }
            else
            {
                count++;
            }
            node.col++;
            if (node.col < arrs[node.row].size())
                pq.push(node);
        }
        if (count == arrs.size())
            res++;
        return res;
    }
};
```

## 150. Best Time to Buy and Sell Stock II

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like \(ie, buy one and sell one share of the stock multiple times\). However, you may not engage in multiple transactions at the same time \(ie, you must sell the stock before you buy again\).

### Example

Given an example\[2,1,2,0,1\], return 2

[https://www.lintcode.com/problem/best-time-to-buy-and-sell-stock-ii/description](https://www.lintcode.com/problem/best-time-to-buy-and-sell-stock-ii/description)

### 解题分析:

market making

### 代码：

```cpp
class Solution {
public:
    /**
     * @param prices: Given an integer array
     * @return: Maximum profit
     */
    int maxProfit(vector<int> &prices) {
        // write your code here
        int res = 0;
        for (int i = 1; i<prices.size(); i++)
        {
            int delta = prices[i]-prices[i-1];
            if(delta > 0)
                res+=delta;
        }
        return res;
    }
};
```

## 151. Best Time to Buy and Sell Stock III

Say you have an array for which the_i\_thelement is the price of a given stock on day\_i_.

Design an algorithm to find the**maximum**profit. You may complete at most\_two\_transactions.

### Example

Given an example`[4,4,6,1,1,4,2,5]`, return`6`.

[https://www.lintcode.com/problem/best-time-to-buy-and-sell-stock-iii/description](https://www.lintcode.com/problem/best-time-to-buy-and-sell-stock-iii/description)

### 解题分析:

DP

### 代码：

```cpp
class Solution {
public:
    /**
     * @param prices: Given an integer array
     * @return: Maximum profit
     */
    int maxProfit(vector<int> &prices) {
        // write your code here

        if (prices.size()<=1)
            return 0;
        int prevMin = prices[0];
        vector<int> left{0};
        for (int i = 1; i<prices.size(); i++)
        {
            left.push_back(max(left.back(), prices[i] - prevMin));
            prevMin = min(prices[i], prevMin);
        }
        vector<int> right(prices.size(), 0);
        int prevMax = prices.back();
        int n = prices.size();
        for (int i = n-2; i>=0; i--)
        {
            right[i] = max(right[i+1], prevMax - prices[i] );
            prevMax = max(prevMax, prices[i]);
        }
        int res = 0;
        for (int i = 0; i < n-1; i++)
            res = max(res, left[i]+right[i]);
        return res;
    }
};
```



