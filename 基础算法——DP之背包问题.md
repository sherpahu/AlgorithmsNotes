# 01背包
代码随想录：[https://mp.weixin.qq.com/s/FwIiPPmR18_AJO5eiidT6w](https://mp.weixin.qq.com/s/FwIiPPmR18_AJO5eiidT6w)
dp[i][j]=dp[i-1][j-1];
dp[i][j]=min(dp[i][j],dp[i-1][j-coins[i-1]]+1);
#### [416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)
判断能否分割
```cpp
	bool canPartition(vector<int>& nums) {
        int tot=0;
        for(int x:nums){
            tot+=x;
        }
        if(tot%2)return false;
        int half=tot/2;
        vector<vector<bool>>dp(nums.size()+1,vector<bool>(half+1,false));
        dp[0][0]=true;
        for(int i=1;i<=nums.size();++i){
            for(int j=0;j<=half;++j){
                dp[i][j]=dp[i-1][j];
                if(j>=nums[i-1]&&dp[i-1][j-nums[i-1]]){
                    dp[i][j]=true;
                }
            }
        }
        return dp[nums.size()][half];
    }
```
#### [474. 一和零](https://leetcode-cn.com/problems/ones-and-zeroes/) 二维的01背包
```cpp
    int findMaxForm(vector<string>& strs, int m, int n) {
        int dp[610][110][110];
        memset(dp,0,sizeof(dp));
        for(int i=1;i<=strs.size();++i){
            int zero=0,one=0;
            for(char c:strs[i-1]){
                if(c=='0')zero++;
                else one++;
            }
            for(int j=0;j<=m;++j){
                for(int k=0;k<=n;++k){
                    dp[i][j][k]=dp[i-1][j][k];
                    if(j>=zero&&k>=one){
                        dp[i][j][k]=max(dp[i-1][j][k],dp[i-1][j-zero][k-one]+1);
                    }
                }
            }
        }
        return dp[strs.size()][m][n];
    }
```
#### [494. 目标和](https://leetcode-cn.com/problems/target-sum/) 将集合划分为两个和相等的子集
把所有符号为正的数总和设为一个背包的容量，容量为x；把所有符号为负的数总和设为一个背包的容量，容量为y。在给定的数组中，有多少种选择方法让背包装满。令sum为数组的总和，则x+y = sum。而两个背包的差为S,则x-y=S。从而求得x=(S+sum)/2。
[https://leetcode-cn.com/problems/target-sum/solution/huan-yi-xia-jiao-du-ke-yi-zhuan-huan-wei-dian-xing/](https://leetcode-cn.com/problems/target-sum/solution/huan-yi-xia-jiao-du-ke-yi-zhuan-huan-wei-dian-xing/)
```cpp
	int findTargetSumWays(vector<int>& nums, int S) {
        int sum=0;
        for(int x:nums){
            sum+=x;
        }
        if(sum<S||(sum+S)%2)return 0;
        unordered_map<int,int>dp[110];
        dp[0][0]=1;
        for(int i=1;i<=nums.size();++i){
            for(int j=0;j<=(sum+S)/2;++j){
                dp[i][j]=dp[i-1][j];
                if(j>=nums[i-1])
                dp[i][j]+=dp[i-1][j-nums[i-1]];
            }
        }
        return dp[nums.size()][(sum+S)/2];
    }
```
#### [1049. 最后一块石头的重量 II](https://leetcode-cn.com/problems/last-stone-weight-ii/) 将集合划分为和的差最小的子集
两个石头合并后剩余二者差的绝对值，目标是将合并到剩余石头最小
问题转化为将集合划分为和之差最小的两个集合
```cpp
    int lastStoneWeightII(vector<int>& stones) {
        int tot=0;
        for(int x:stones){
            tot+=x;
        }
        int sum=tot;
        tot/=2;
        vector<vector<int>>dp(stones.size()+1,vector<int>(tot+1,0));
        for(int i=1;i<=stones.size();++i){
            for(int j=0;j<=tot;++j){
                dp[i][j]=dp[i-1][j];
                if(j>=stones[i-1]){
                    dp[i][j]=max(dp[i-1][j],dp[i-1][j-stones[i-1]]+stones[i-1]);
                }
            }
        }
        return sum-dp[stones.size()][tot]*2;
    }
```


# 完全背包
代码随想录：[https://mp.weixin.qq.com/s/akwyxlJ4TLvKcw26KB9uJw](https://mp.weixin.qq.com/s/akwyxlJ4TLvKcw26KB9uJw)
如果求组合数就是外层for循环遍历物品，内层for遍历背包。
如果求排列数就是外层for遍历背包，内层for循环遍历物品。


dp[i][j]=dp[i-1][j];
dp[i][j]=min(dp[i][j],dp[i][j-coins[i-1]]+1);
与01背包的区别就是红色的i而不是i-1
#### [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/) 最少元素个数
最透彻的讲解：[https://mp.weixin.qq.com/s/dyk-xNilHzNtVdPPLObSeQ](https://mp.weixin.qq.com/s/dyk-xNilHzNtVdPPLObSeQ)
```cpp
    int coinChange(vector<int>& coins, int amount) {
        vector<vector<int>>dp(coins.size()+1,vector<int>(amount+1,amount+1));
        dp[0][0]=0;
        for(int i=1;i<=coins.size();++i){
            for(int j=0;j<=amount;++j){
                dp[i][j]=dp[i-1][j];
                if(j>=coins[i-1]&&dp[i][j-coins[i-1]]!=amount+1){
                    dp[i][j]=min(dp[i][j],dp[i][j-coins[i-1]]+1);
                }
            }
        }
        return dp[coins.size()][amount]==amount+1?-1:dp[coins.size()][amount];
    }
```
#### [518. 零钱兑换 II](https://leetcode-cn.com/problems/coin-change-2/) 求完全背包的方案数
```cpp
    int change(int amount, vector<int>& coins) {
        vector<vector<int>>dp(coins.size()+1,vector<int>(amount+1,0));
        dp[0][0]=1;//没有零钱，也没有需要兑换的钱，所以方案只有一种
        for(int i=1;i<=coins.size();++i){
            for(int j=0;j<=amount;++j){
                dp[i][j]=dp[i-1][j];
                if(j>=coins[i-1]){
                    dp[i][j]+=dp[i][j-coins[i-1]];
                }
            }
        }
        return dp[coins.size()][amount];
    }
```
# 多重背包
dp[i][j]=dp[i][j-1];
dp[i][j]=min(dp[i][j],dp[i-1][j-coins[i-1]*k]+w[i-1]*k);
#### [AcWing 4. 多重背包问题](https://www.acwing.com/problem/content/4/)
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=110;
int n,V;
int v[N],w[N],s[N],dp[N][N];
int main(){
    cin>>n>>V;
    for(int i=0;i<n;++i){
        cin>>v[i]>>w[i]>>s[i];
    }
    for(int i=1;i<=n;++i){
        for(int j=0;j<=V;++j){
            dp[i][j]=dp[i-1][j];
            for(int k=1;k<=s[i-1]&&k*v[i-1]<=j;++k){
                dp[i][j]=max(dp[i][j],dp[i-1][j-k*v[i-1]]+k*w[i-1]);
            }
        }
    }
    cout<<dp[n][V];
}
```


