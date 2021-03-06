## 第一部分

当给定一个**一维数组**，求**子序**列**满足条件**的**最**大最小最短中最长的问题， 使用这个方法需要证明j不需要往回重置到i.

一般跑几个例子就可以看出来

![](/assets/slidewindowtemplate.png)

## 

## 406. Minimum Size Subarray Sum

Given an array of n positive integers and a positive integer s, find the minimal length of a subarray of which the sum ≥ s. If there isn't one, return -1 instead.

### Example

Given the array`[2,3,1,2,4,3]`and s =`7`, the subarray`[4,3]`has the minimal length under the problem constraint.

### Challenge

If you have figured out the O\(n\) solution, try coding another solution of which the time complexity is O\(n log n\).

**Note:**  
跑一个暴力的例子

for i-n

for j -n

```
   if sum &gt;= target

      break
```

i = 0:  2, 5, 6, 8 j = 3

i = 1:  3, 4, 6 j = 3 j不需要动， 因为j以前的已经被上次遍历证明小于target了。  那么就可以用滑动窗口了。

```cpp
class Solution {
public:
    /**
     * @param nums: an array of integers
     * @param s: An integer
     * @return: an integer representing the minimum size of subarray
     */
    int minimumSize(vector<int> &nums, int s) {
        // write your code here
        int sum = 0;
        int len = INT_MAX;
        int j = 0;
        int n = nums.size();
        for (int i = 0; i < n; i++) // start of window
        {
            while(j < n) // no need to go back 
            {
                if (sum >= s) // if condition break
                    break;
                else // update local window
                    sum += nums[j++];
            }
            // update state of i; 
            if (sum >= s) 
                len = min(len, j-i);
            sum -= nums[i];
        }
        return len == INT_MAX? -1:len;
    }
};
```

## 384. Longest Substring Without Repeating Characters

Given a string, find the length of the longest substring without repeating characters.

### Example

For example, the longest substring without repeating letters for`"abcabcbb"`is`"abc"`, which the length is`3`.

For`"bbbbb"`the longest substring is`"b"`, with the length of`1`.

### Challenge

O\(n\) time

还是j不需要再回头，且满足，substring, condition :no repeating, longest

```cpp
class Solution {
public:
    /**
     * @param s: a string
     * @return: an integer
     */
    int lengthOfLongestSubstring(string &s) {
        // write your code here
        // substring , without repeating, longest
        unordered_set<char> windowCounter;
        int res = 0;
        int j = 0;
        for (int i = 0; i < s.size(); i++)
        {
            while(j < s.size())
            {
                if (windowCounter.count(s[j]))
                    break;
                else
                    windowCounter.insert(s[j++]);
            }
            // update state i 
            res = max(res, j - i);
            windowCounter.erase(s[i]);
        }
        return res;
    }
};
```

## 32. Minimum Window Substring

Given a string source and a string target, find the minimum window in source which will contain all the characters in target.

### Example

For source =`"ADOBECODEBANC"`, target =`"ABC"`, the minimum window is`"BANC"`

### Challenge

Can you do it in time complexity O\(n\) ?

```cpp
class Solution {
public:
    /**
     * @param source : A string
     * @param target: A string
     * @return: A string denote the minimum window, return "" if there is no such a string
     */


    bool isAll(vector<int>& sourceHash, vector<int>& targetHash)
    {
        for (int i = 0; i < 256; i++)
        {
            if (targetHash[i] !=0 && sourceHash[i] < targetHash[i])
                return false;
        }
        return true;
    }
    string minWindow(string &source , string &target) {
        // write your code here
        vector<int> targetHash(256, 0);
        for (auto each : target)
            targetHash[each]++;
        vector<int> sourceHash(256, 0);
        int j = 0;
        int len = INT_MAX;
        string res;

        for (int i = 0; i < source.size(); i++)
        {
            while(j < source.size())
            {
                if (isAll(sourceHash, targetHash))
                    break;
                else
                {
                    sourceHash[source[j++]]++;
                }
            }
            if ( isAll(sourceHash, targetHash) && j-i < len) // this place, the j might be the end, we cannot control
            {
                len = j-i;
                res = source.substr(i, j-i);
            }
            sourceHash[source[i]]--;
        }
        return res;
    }
};
```

## 386. Longest Substring with At Most K Distinct Characters

Given a string_s_, find the length of the longest substring T that contains at most k distinct characters.

### Example

For example, Given s =`"eceba"`,`k = 3`,

T is`"eceb"`which its length is`4`.

### Challenge

O\(n\), n is the size of the string_s_.

这里面有个大坑.用字典来维护size k, 但是注意不能用一旦字典size == k就退出， 因为cccc可以连续，如果c已经在字典里，则可以继续。排除这种情况后dict.size\(\) == k退出， 否则继续更新。

```cpp
class Solution {
public:
    /**
     * @param s: A string
     * @param k: An integer
     * @return: An integer
     */
    int lengthOfLongestSubstringKDistinct(string &s, int k) {
        // write your code here
        int n = s.size();
        if (k == 0)
            return 0;
        int res = 0;
        int j = 0;
        unordered_map<char,int> dict;
        for (int i = 0; i < n; i++)
        {
            while(j < n)
            {
                /* this is wrong, why ? because once dict.size == k, and the next is also 
                in the dict, it does not change it and we can still increase the value
                if (dict.size() >= k)
                    break;
                else
                    dict[s[j++]]++;
                */
                if (dict.count(s[j]))
                    dict[s[j++]]++;
                else 
                {
                    if (dict.size() == k)
                        break;
                    dict[s[j++]]++;
                }

            }
            res = max(res, j-i);
            if (--dict[s[i]] == 0)
                dict.erase(s[i]);
        }
        return res;
    }
};
```



## ![](/assets/heapfortopk.png)

## 401. Kth Smallest Number in Sorted Matrix

Find the\_k\_th smallest number in at row and column sorted matrix.

### Example

Given k =`4`and a matrix:

```
[
  [1 ,5 ,7],
  [3 ,7 ,8],
  [4 ,8 ,9],
]
```

return`5`

### Challenge

Solve it in O\(k log n\) time where n is the bigger one between row size and column size.

这也算一类题目了， 用的思想是top k 就上pq, 碰到不确定的备选答案就丢进去，因为他始终给你维护一个最优的答案。 另外记住pq default是他妈的最大heap,日。

用pq做BFS的辅助找第k大，是多选情况下的应用

```cpp
class Solution {
public:
    /**
     * @param matrix: a matrix of integers
     * @param k: An integer
     * @return: the kth smallest number in the matrix
     */
    int kthSmallest(vector<vector<int>> &matrix, int k) {
        // write your code here
        int m = matrix.size();
        int n = matrix[0].size();

        auto cmp = [&matrix](const pair<int,int>& left, const pair<int,int>& right){
            return matrix[left.first][left.second] > matrix[right.first][right.second];
        };
        priority_queue<pair<int,int>, vector<pair<int,int>>, decltype(cmp)> pq(cmp);
        pq.emplace(0, 0);
        int v = 0;
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        visited[0][0] = true;
        static vector<int> dx{0, 1};
        static vector<int> dy{1, 0};
        for (int i = 0; i < k; i++)
        {
            auto p = pq.top();
            pq.pop();
            v = matrix[p.first][p.second];
            for (int j = 0; j < 2; j++)
            {
                int nx = p.first + dx[j];
                int ny = p.second + dy[j];
                if (nx < 0 || nx >= m || ny<0 || ny >=n || visited[nx][ny])
                    continue;
                visited[nx][ny] = true;
                pq.emplace(nx,ny);
            }
        }
        return v; 
    }
};
```

## 373. Find K Pairs with Smallest Sums

You are given two integer arrays**nums1**and**nums2**sorted in ascending order and an integer**k**.

Define a pair**\(u,v\)**which consists of one element from the first array and one element from the second array.

Find the k pairs**\(u1,v1\),\(u2,v2\) ...\(uk,vk\)**with the smallest sums.

**Example 1:**

```
Given nums1 = [1,7,11], nums2 = [2,4,6],  k = 3

Return: [1,2],[1,4],[1,6]

The first 3 pairs are returned from the sequence:
[1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
```

**Example 2:**

```
Given nums1 = [1,1,2], nums2 = [1,2,3],  k = 2

Return: [1,1],[1,1]

The first 2 pairs are returned from the sequence:
[1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]
```

**Example 3:**

```
Given nums1 = [1,2], nums2 = [3],  k = 3 

Return: [1,3],[2,3]

All possible pairs are returned from the sequence:
[1,3],[2,3]
```

return`5`

[https://leetcode.com/problems/find-k-pairs-with-smallest-sums/description/](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/description/)

### Challenge

第K x 的问题， 还是上来想pq,面临多选就网上push,找到为止。

```cpp
class Solution {
public:
    vector<pair<int, int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
        vector<pair<int,int>> result;

        auto cmp = [&nums1,&nums2](const pair<int,int>& left, const pair<int,int>& right){
            return nums1[left.first]+nums2[left.second] > nums1[right.first]+nums2[right.second];  
        };


        int m = nums1.size();
        int n = nums2.size();
        if (0 == m || 0 == n)
            return result;
        vector<vector<bool>> visited(m, vector<bool>(n, false));

        priority_queue<pair<int,int>, vector<pair<int,int>>,decltype(cmp)> pq(cmp);

        pq.emplace(0, 0);
        visited[0][0] = true;
        for (int i = 0; i < k && !pq.empty(); i++)
        {
            auto p = pq.top();
            pq.pop();
            result.emplace_back(nums1[p.first], nums2[p.second]);
            int next_num1 = p.first+1;
            int next_num2 = p.second+1;

            if (next_num1 < m && !visited[next_num1][p.second])
            {
                pq.emplace(next_num1, p.second);
                visited[next_num1][p.second] = true;
            }

            if (next_num2 < n && !visited[p.first][next_num2])
            {
                pq.emplace(p.first,next_num2);
                visited[p.first][next_num2] = true;
            }
        }
        return result;
    }
};
```



