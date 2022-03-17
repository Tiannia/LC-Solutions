### 题目
[接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

### 思路
**DP**：最简单，开数组记录左右最大值高度即可
**双指针**：下标i处能接到的雨水量有leftmax[i]和rightmax[i]的最小值决定，于是就可以使用双指针，left从左往右，right从右往左，根据柱子高度比较并更新leftmax和rightmax即可。记住：如果height[left]<height[right]，则必有leftmax<rightmax，为什么呢？因为left和right会交替指向当前的max(leftmax,rightmax)中较大值那一个，指向较大值那个是不能动的，只能另一边移动到有比其更大的值出现。
**单调栈**：高到低再到高即可。维护单调递减栈。

### 代码
```c++
class Solution {
public:
    int trap(vector<int>& height) {
        int ans = 0;
        //单调递减栈
        stack<int> stk;
        int n = height.size();
        for (int i = 0; i < n; ++i) {
            while (!stk.empty() && height[i] > height[stk.top()]) {
                int top = stk.top();
                stk.pop();
                if (stk.empty()) { //我们在当前需要进行计算的柱子左边找不到“屏障”（即比其高度更高的，因为这是单调递减栈）
                    break;
                }
                int left = stk.top();
                int currWidth = i - left - 1;                                //计算宽度
                int currHeight = min(height[left], height[i]) - height[top]; //计算高度
                ans += currWidth * currHeight;
            }
            stk.push(i);
        }
        return ans;
    }
};
```