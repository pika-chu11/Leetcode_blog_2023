# [704.Binary Search](https://leetcode.com/problems/binary-search/)

```
class Solution {
public:
    int search(vector<int>& nums, int target) {
    int head_ind = 0, tail_ind = nums.size() - 1;
    int mid_ind;
        while (head_ind <= tail_ind) {
            mid_ind = head_ind + (tail_ind - head_ind)/2;
            
            if (nums[mid_ind] == target) {
                return mid_ind;
            }
            else if (nums[mid_ind] > target) {
                tail_ind = mid_ind - 1;
            }
            else {
                head_ind = mid_ind + 1;
            }
        }
        return -1;
    };
};
```
