
Longest Increasing Subsequence.
O(n^2) is good, but O(nlogn) is better

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        if (n <= 1) {
            return n;
        }
        
        //dp[i] -> length is i, and the tail number is nums[i];
        vector<int> dp(n, 0);
        int len = 0;
        dp[0] = nums[0];
        
        for (int i = 1; i < n; i++) {
            if (nums[i] > dp[len]) {
                dp[++len] = nums[i];
            } else {
                int j = lower_bound(dp.begin(), dp.begin()+len, nums[i]) - dp.begin();
                dp[j] = nums[i];
            }
        }
        return len+1;
    }
    
private:
    int binarySearch(vector<int>& dp, int l, int r, int target) {
        while (l < r) {
            int m = (r - l) / 2 + l;
            if (dp[m] < target) {
                l = m + 1;
            } else {
                r = m;
            }
        }
        
        dp[l] = target;
        return l;
    }
};
```