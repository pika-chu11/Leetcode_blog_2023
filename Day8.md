---
layout: page
title: Day8
---

# [344. Reverse String](https://leetcode.com/problems/reverse-string/)

```C++
class Solution {
public:
    void reverseString(vector<char>& s) {
        int i = 0, j = s.size() - 1;

        while(i < j){
            // Swap the letters
            char temp = s[i]; 
            s[i] = s[j];
            s[j] = temp;
            i++;
            j--;
        }
    }
};

```
> Time Complexity: O(n) <br>
> Space Complexity: O(1)


> - We used **two pointer** in this problem. The *first* pointer point to the first letter of the string and the *second* pointer point to the last letter of the string. Both pointer swap the pointed value as they move toward the middle of the string.
> <br>
> - *Additional:* There are different ways to swap letters :  
> 
```C++ 
// The first approach
s[i] ^= s[j];
s[j] ^= s[i];
s[i] ^= s[j];
```
```C++
// The second approach
swap(s[i],s[j]);
```

___
# [541. Reverse String II](https://leetcode.com/problems/reverse-string-ii/)

```C++
class Solution {

void reverse(string &s, int start, int end) {
    for (int i = start, j = end; i < j; i++, j--) {
        swap(s[i], s[j]);
    }
}
public:
    string reverseStr(string s, int k) {
        // index i will reverse the first k letters 
        // for every 2k letter of the string.
        for (int i = 0; i < s.size(); i += (2*k)) {
            // There is more then k letters
            // reverse the first k letters
            if (i + k <= s.size()) {
                reverse(s, i, i + k - 1);
            }
            // there is less than k letters
            // reverse all the letters.
            else {
                //reverse the remaining string
                reverse(s, i, s.size()-1); 
            }
        }
        return s;

    }
};
```
> 一开始这一题写的是用左闭右闭，写得一般：
```C++
class Solution {
public:
    string reverseStr(string s, int k) {
        int i = 0; 
        int j = i + k - 1;

        int right = s.size()-1;
        while (i < right) {
            if (j > right){
                j = s.size()-1;
            }
            int m = j;
            while(i < j) {
                swap(s[i], s[j]);
                i++;
                j--;
            }

            i = m + k + 1;
            j = i + k - 1;
        }

        return s;
    }
};

```
> 时间复杂度: O(n) <br>
> 空间复杂度: O(1)


> 其实这题就是处理两个cases:
> 
> 1. 当有少于k个字母的时候的边界
> 2. 当有大于等于k个字母的时候的边界<br>
> 
> 在这两个cases里判断要反转的区间之后，这一题就跟上面一样了都是反转string。

___
# [剑指 Offer 05. 替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)

```C++
class Solution {
public:
    string replaceSpace(string s) {

        /* Approach 1 */
        string ans;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] != ' '){
                ans += s[i];
            }
            else{
                ans += "%20";
            }
        }
        return ans;

         /* Approach 2 */
        int spaces = 0;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == ' '){
                spaces++;
            }
        }

        int oldSize = s.size();
        s.resize(s.size() + spaces * 2);  // Add double space for each space occur
        int newSize = s.size();

        for (int i = newSize - 1, j = oldSize - 1 ; j < i ; i--, j--) {
            if (s[j] != ' '){
                s[i] = s[j];
            }
            else {
                s[i] = '0';
                s[i-1] = '2';
                s[i-2] = '%';
                i -= 2;         // total move three times but the loop would help onc. 
            }
        }
        return s;
    }
};
```

> 上面有两个写法：<br>
> * 方法1：
>   1.  运用了额外的字符串作为辅助。
>   2.  复制所有字符，遇到空格是改为%20。<br>
> 
> 此方法提高空间复杂度。<br>
> * 方法2：
>   1. 从后向前替换空格，**双指针法**。
>   2. 先算一遍字符串里的空格总数。
>   3. 根据空格数在字符串后用 `s.resize` 增加字符串空间，方便我们从后往前面操作。
>   4. 在新增的最后位置里开始倒是从原来字符串的最后开始抄。
>   5. 遇到空格手动加上三个字符 `0`, `2`, `%`, 此处要注意 指针`i` 要移动多两格。


___
# [151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

```C++
class Solution {
void reverseWord(string &ans, string &s,  int start, int end) {
    for (int i = start + 1; i <= end; i++) {
        ans += s[i];
    }
    if (start != -1) {
        ans += ' ';
    }
}
public:
    string reverseWords(string s) {
        string ans;
        int i = s.size() - 1;
        int j, k;
        while(i >= 0){
            while (s[i] == ' ' && i > 0) i--;
            j = i;
            while (i >= 0 && s[i] != ' ') i--;
            if (i == j) break;
            reverseWord(ans,s,i,j);
        }
        if (ans[ans.size()-1] == ' ') {
            ans.pop_back();
        }
        return ans;
    }
};
```
> 以上解法运用了额外辅助空间。<br>
> 解法思路：**双指针**
> - 移动指针，使其跳过空格和字符，找到一个word的正确区间。（这里要注意先运行跳过空间的命令，因为这样才能正确的找到一个字的尾端。然后再向前移动到前端。）
> - 给这个区间的字加再辅助字符串里。
> - 因为是从字符串的后面开始找词，所以最后面的词在辅助字符的最前面，实现反转。
>
> ***TODO:***  另外不使用额外辅助空间的写法还未完成，有时间会尝试一下
> [Link](https://programmercarl.com/0151.%E7%BF%BB%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.html#%E6%80%9D%E8%B7%AF) 

___
# [剑指 Offer 58 - II. 左旋转字符串](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

```C++
class Solution {
    void reverseWord(string &s, int start, int end) {
        for(int i = start, j = end; i < j; i++, j--) {
            swap(s[i], s[j]);
        }
    }
public:
    string reverseLeftWords(string s, int n) {
        reverseWord(s, 0, 0 + n - 1 );
        reverseWord(s, 0 + n, s.size()-1);
        reverseWord(s, 0, s.size() -1);
        return s;
    }
};
```

> 本题思路（仅在原字符串里操作）：
> - 分区域反转（两个）
>   - `n` 个字母之前 和 `n`个字母之后的部分
> - 最后反转全部。
>
> 如下：
>
> - 输入：s = "abcdefg", k = 2
>   1. 先反转ab -> ba
>   2. 再反转cdefg -> gfedc
>   3. 这样得到: ba | gfedc
>   4. 将上面反转得到: cdefgab