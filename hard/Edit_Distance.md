### 题目
[编辑距离](https://leetcode-cn.com/problems/edit-distance/)

### 思路
无，仅仅觉得转变为dp从后面“插入”“修改”进行转移很巧妙

### 代码
```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        // dp[i][j] 代表 word1 中前 i 个字符，变换到 word2 中前 j 个字符，最短需要操作的次数
        vector<vector<int>> dp(word1.size() + 1, vector<int>(word2.size() + 1, 1000));

        for (int i = 0; i < dp.size(); i++) {
            dp[i][0] = i;
        }
        for (int j = 0; j < dp[0].size(); j++) {
            dp[0][j] = j;
        }
        //添加的时候为什么从dp[i][j - 1]转移过来
        //考虑：
        //word1:          abcfxxx
        //i:                 ↑
        //word2:          abdxxx
        //j:                ↑
        //如果选择添加一个字符，由于dp的状态，只能添加到word1的末尾与word2形成匹配，为什么？
        //因为由最优子结构可知：word1中前i个字符与到word2中前j个字符必须匹配，且dp状态存储的
        //是合法匹配序列的最少操作数，因此，如果我选择添加字符，就必须添加至末尾，
        //不能够影响前面序列(0..i-1)的状态(他们已经有自己的变动了嘛)，然后又由于word1中前i个
        //字符与到word2中前j个字符必须匹配，所以添加的这个字符要和word2[j]相同，然后就可以把
        //他俩都忽视掉。这样一来，就可以由dp[i][j - 1]转移过来了。实际上这道题的dp都是基于末
        //尾的“添加”“修改”进行转移的。。。
        for (int i = 1; i < dp.size(); i++) {
            for (int j = 1; j < dp[i].size(); j++) {
                dp[i][j] = min( {dp[i - 1][j - 1], dp[i - 1][j], dp[i][j - 1]} ) + 1;
                if (word1[i - 1] == word2[j - 1]) {
                    dp[i][j] = min(dp[i][j], dp[i - 1][j - 1]);
                }                    
            }
        }
        return dp.back().back();
    }
};
```
