---
layout: page
title: Day2
---

# [977. Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/)

```C++
// 前后双指针
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int front = 0, end = nums.size() - 1;
        vector<int> ans(nums.size());
        int i = end;
        while(front <= end) {
            if(abs(nums[front]) < abs(nums[end])) {
                ans[i] = nums[end] * nums[end];
                end--; // only move the end index since we have to recheck it after front is updated
            }
            else {
                ans[i] = nums[front] * nums[front];
                front++;
            }
            i--;
        }
        return ans;
    }
};
```
> 这里使用了两个指针更新数据，`front` 和 `end` 用来对比前后数据。哪个的平方数值比较大就先放在 `vector` 的后面。 这样可以保证 `vector` 里面的数据是从小到大排列。因为原 `vector` 是按照从小到大排列的，而越小的负数它的平方反而会越大。最大的正数排在最后所以运用两个前后指针去确认和对比大小，以确定正确排序。

> ### **Caution:** 
> - `pow` method的耗时十分的多。如果是想控制 run time 最好，注意一下。

# [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

```C++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int min_length = INT32_MAX;
        int i = 0; // start index
        int length, sum = 0;

        for (int j = 0; j < nums.size(); j++) {
            sum += nums[j];

            while(sum >= target) { // move the start index
                length = j - i + 1;

                // Update mini. length: min(min_length, length)
                min_length = min_length > length? length : min_length;

                // Update start index
                sum -= nums[i];
                i++;
            }

        }
            return min_length == INT32_MAX? 0 : min_length; 
    }
};
```
> - 这一题一开始思路只有暴力解法，是看教程解出来的。
> - 此题也是运用了一个 __双指针__ 去实现 *滑动窗口* 的解法：
>   1. 运用一个指针确定初始 `subarray` 的开头
>   2. 运用第二个指针来实现滑动窗口：
> - 把从指针1到指针2的包含数值（窗口）加在一起查看是否超过或者等于指定的数据。 
> - 如果是，则把当前两个指针之间(窗口 `subarray`)的距离记录下来。
>   - 对比当前记录是比最小的 `length`小，如果是则，最小 `length`换成当前 `length`。
>   - 减去指针1的指数直到小于指定数值，并且每减一次指针1向指针2的方向移动一格。这样可以达到快速检测下一次窗口长度。
> - 如果不是，则继续运行。


# [59. Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/)

```C++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        // A vector of n:
        // each element contains a vector of n with initialization of 0
        // 2D array
        vector<vector<int>> ans(n, vector<int>(n,0));
        // limit the edges for:
        // 1. left to right
        // 2. top to bottom
        int offset = 1; // limit the edges

        // 3. for right to left
        // 4. bottom to top
        int left = 0, top = 0;

        // determine how many complete loop we need to done
        int loop = n/2;

        // determine the middle count if n == ODD
        int mid = n/2;

        // numbers need to fill
        int num = 1; 

        int i = 0;
        int j = 0; // keep track so that find the right pos

        while (loop--) // while current loop > 0
        {
            i = left;
            j = top;
            
            // left to right (左闭右开)
            for (i = left; i < n - offset; i++) {
                ans[top][i] = num++; // i.e. ans[top][i] = num; num++;
            }

            // top to bottom (左闭右开)
            for (j = top; j < n - offset; j++) {
                ans[j][i] = num++;
            }

            // right to left
            for (; i > left; i--) {
                ans[j][i] = num++;
            }

            // bottom to top
            for (; j > top; j--) {
                ans[j][i] = num++;
            }

            top++;
            left++;
            offset++;
        }

        // if n == ODD
        // fill the middle space manually
        if (n % 2) {
            ans[mid][mid] = num;
        }
        
        return ans;
    }
};

```

> ## **Importance：**
> - 这道题刚开始完全没有思路，中途有一点但是抓不住。看了答案之后，恍然大悟。
> - 处理边界很重要！！
> - 需要找到每个循环的规律，然后保持每条边的填入规则（左闭右开）。
> 关于 `loop`部分为什么除以2，大佬的解释是：
>   - 因为搜索一圈之后，下一圈的上边会往下走，下一圈的下边会往上走，高度就少2，下一圈的左边会往右走，下一圈的右边会往左走，相当于宽少2了，每次下一圈都会比上一圈的高度宽度都少2，直到这个没有外圈了，没有外圈就是宽度是和高度都是0的时候（偶数的情况），每次宽，高缩小2，直到宽，高是0。因为宽，高都一样，而且是一起缩2的，那么就当高缩2到0的时候就结束了，要缩多少次，就是圈。高假设是偶数，偶数-2-2-2一直到0，不就是这个偶数除2吗，就是圈数啦
> - LeetCode上另一个大佬Krahets的[解法](https://leetcode.cn/problems/spiral-matrix-ii/solutions/12594/spiral-matrix-ii-mo-ni-fa-she-ding-bian-jie-qing-x/)（比较通俗易懂）


 ```C++
class Solution {
    public int[][] generateMatrix(int n) {
        int l = 0, r = n - 1, t = 0, b = n - 1;
        int[][] mat = new int[n][n];
        int num = 1, tar = n * n;
        while(num <= tar){
            for(int i = l; i <= r; i++) mat[t][i] = num++; // left to right.
            t++;
            for(int i = t; i <= b; i++) mat[i][r] = num++; // top to bottom.
            r--;
            for(int i = r; i >= l; i--) mat[b][i] = num++; // right to left.
            b--;
            for(int i = b; i >= t; i--) mat[i][l] = num++; // bottom to top.
            l++;
        }
        return mat;
    }
}

作者：Krahets
链接：https://leetcode.cn/problems/spiral-matrix-ii/solutions/12594/spiral-matrix-ii-mo-ni-fa-she-ding-bian-jie-qing-x/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```