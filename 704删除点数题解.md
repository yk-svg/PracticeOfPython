### 删除并获得点数

#### 给你一个整数数组 nums ，你可以对它进行一些操作。每次操作中，选择任意一个 nums[i] ，删除它并获得 nums[i] 的点数。之后，你必须删除 所有 等于 nums[i] - 1 和 nums[i] + 1 的元素。开始你拥有 0 个点数。返回你能通过这些操作获得的最大点数

#### 题目思路主要是数字处理和动态规划。具体步骤就是：首先归类数字，将所有相同的数字相加，得到相同数字和，然后使用动态规划，为什么这么做？因为题目说选择nums[i]，那么就要删除nums[i]和nums[i]-1,nums[i]+1。我的想法是首先排序，如：{2,3,4,4,7},如果那么其实如果出现了不连续数组，也就是连续数字之间差值大于1,则原来数组变成多个子数组，那么根据题目要求，各个子数组是互不相扰的，因为差值大于一，所以使用动态规划求解各个子数组的最大值，然后相加求解，就是最大值。但是我看题解，其实没必要排序，为什么?题解只是寻求相同数字的和，然后使用动态规划，仔细想想其实题目本身给出的条件就是本身就暗示了数组不连续，所以没必要人为对数组排序，反而只需要根据最大数值创立数组，然后和桶排序一样，将各自数字存入数组就可以了，然后使用动态规划。
#### 感悟：一是cpp语法：vector<int>sum(maxVal + 1),
#### 创建长度(maxVal+1)的数组,sum.back()+= val; 用于将相同数值累加.sum.push_back(val); 用于处理新的不连续的数值，并将其添加到 sum中.二是觉得是不是#### 题目核心是动态规划，但前面处理方式决定了题目难度,进一步推广所有题目都是这样的？不知道，一个猜测

```cpp
class Solution {
private:
    int rob(vector<int> &nums) {
        int size = nums.size();
        if (size == 1)
            return nums[0];
        int first = nums[0], second = max(nums[0], nums[1]);
        for (int i = 2; i < size; i++) {
            int temp = second;
            second = max(first + nums[i], second);
            first = temp;
        }
        return second;
    }

public:
    int deleteAndEarn(vector<int>& nums) {
        /* 
        // 手动排序（已替换为sort函数）
        for(int i = 0; i < nums.size(); i++) {
            for(int j = i + 1; j < nums.size(); j++) {
                int t;
                if (nums[i] > nums[j]) {
                    t = nums[j];
                    nums[j] = nums[i];
                    nums[i] = t;
                }
            }
        }
        */
        sort(nums.begin(), nums.end());
        vector<int> sum = {nums[0]};
        int count = 0;

        for (int i = 1; i < nums.size(); i++) {
            int val = nums[i];
            if (val == nums[i - 1]) {
                sum.back() += val;  // 累加相同的数值
            } 
            else if (val == nums[i - 1] + 1) {
                sum.push_back(val);  // 处理相邻的数值
            }  
            else {
                count += rob(sum);  // 处理完一段数值后调用 rob 函数
                sum = {val};        // 重置 sum 数组为当前数值
            }
        }

        count += rob(sum);  // 处理最后一段数值
        return count;
    }
};
