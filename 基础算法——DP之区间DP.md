[https://leetcode-cn.com/problems/remove-boxes/solution/yi-wen-tuan-mie-qu-jian-dp-by-bnrzzvnepe-m2si/](https://leetcode-cn.com/problems/remove-boxes/solution/yi-wen-tuan-mie-qu-jian-dp-by-bnrzzvnepe-m2si/)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/705461/1615426221262-59904972-8601-4071-81a1-d37029d50d2c.png#align=left&display=inline&height=307&margin=%5Bobject%20Object%5D&name=image.png&originHeight=513&originWidth=1218&size=171556&status=done&style=none&width=729)
#### [312. 戳气球](https://leetcode-cn.com/problems/burst-balloons/)
dp[st][ed]表示[st,ed]区间的最大硬币值
注意：每戳破一个气球得到的硬币值等于该气球与[st,ed]外的st-1和ed+1的乘积
dp[st][ed]=max(dp[st][i-1]+dp[i+1][ed]+nums[st-1]*nums[i]*nums[ed+1]);
```cpp
    int dp[510][510];
    int maxCoins(vector<int>& nums) {
        return dfs(nums,0,nums.size()-1);
    }
    int dfs(vector<int>& nums,int st,int ed){
        if(st>ed)return 0;
        if(dp[st][ed])return dp[st][ed];
        int res=0;
        for(int i=st;i<=ed;++i){
            res=max(res,dfs(nums,st,i-1)+dfs(nums,i+1,ed)+get(nums,st-1)*get(nums,i)*get(nums,ed+1));
        }
        return dp[st][ed]=res;
    }
    int get(vector<int>& nums,int i){
        if(i==-1||i==nums.size())return 1;
        else return nums[i];
    }
```
#### [516. 最长回文子序列](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)
dp[i][j]=dp[i+1][j-1]+2 if s[i]==s[j]
dp[i][j]=max(dp[i+1][j],dp[i][j-1])
```cpp
int longestPalindromeSubseq(string s) {
        vector<vector<int>>dp(s.size()+1,vector<int>(s.size()+1,1));
        for(int j=1;j<s.size();++j){
            for(int i=j-1;i>=0;--i){
                if(s[i]==s[j]){
                    if(i+1==j)dp[i][j]=2;
                    else dp[i][j]=dp[i+1][j-1]+2;
                }else{
                    dp[i][j]=max(dp[i+1][j],dp[i][j-1]);
                }
            }
        }
        return dp[0][s.size()-1];
    }
```
变式：[1216. 验证回文字符串 III](https://leetcode-cn.com/problems/valid-palindrome-iii/)
```cpp
    bool isValidPalindrome(string s, int k) {
        vector<vector<int>>dp(s.size()+1,vector<int>(s.size()+1,1));
        int ans=0;
        for(int j=1;j<s.size();++j){
            for(int i=j-1;i>=0;--i){
                if(s[i]==s[j]){
                    if(i+1==j)dp[i][j]=2;
                    else dp[i][j]=dp[i+1][j-1]+2;
                }else{
                    dp[i][j]=max(dp[i+1][j],dp[i][j-1]);
                }
            }
        }
        //return dp[0][s.size()-1];
        if(s.size()-dp[0][s.size()-1]<=k){
            return true;
        }else return false;
    }
```
#### [813. 最大平均值和的分组](https://leetcode-cn.com/problems/largest-sum-of-averages/)
dp[i][k]表示以A[i]结尾的k个分组的最大平均值和
dp[i][k]=max(dp[i][k],dp[j][k-1]+sum(A[j...i])/(i-j)); j:i-1 => 0
```cpp
    double dp[110][110],sum[110];
    double largestSumOfAverages(vector<int>& A, int K) {
        for(int i=1;i<=A.size();++i){
            sum[i]=sum[i-1]+A[i-1];
        }
        for(int i=1;i<=A.size();++i){
            dp[i][1]=(sum[i]-sum[0])/i;
            for(int k=2;k<=K && k<=i;++k){
                for(int j=i-1;j>=0;--j){
                    dp[i][k]=max(dp[i][k],dp[j][k-1]+(sum[i]-sum[j])/(i-j));
                }
            }
        }
        return dp[A.size()][K];
    }
```
#### [1039. 多边形三角剖分的最低得分](https://leetcode-cn.com/problems/minimum-score-triangulation-of-polygon/)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/705461/1615428606001-319938e8-0d27-4274-ba26-2b04a07c6a80.jpeg#align=left&display=inline&height=394&margin=%5Bobject%20Object%5D&originHeight=394&originWidth=460&size=0&status=done&style=none&width=460)
dp[i][j]=min(dp[i][m]+A[i]*A[j]*A[m]+dp[m][j]),for m in range [i+1,j-1]
```cpp
    int dp[55][55];
    int minScoreTriangulation(vector<int>& A) {
        return dfs(A,0,A.size()-1);
    }
    int dfs(vector<int>& A,int st,int ed){
        if(ed-st<2)return 0;
        if(dp[st][ed])return dp[st][ed];
        int res=INT_MAX;
        for(int i=st+1;i<ed;++i){
            int left=dfs(A,st,i),right=dfs(A,i,ed);
            res=min(res,left+right+A[st]*A[i]*A[ed]);
        }
        return dp[st][ed]=res;
    }
```
#### [1547. 切棍子的最小成本](https://leetcode-cn.com/problems/minimum-cost-to-cut-a-stick/)
切[left,right]的成本为right-left
dp[left][right]=min(res,right-left+dfs(left,x,cuts)+dfs(x,right,cuts));
```cpp
    unordered_map<int,unordered_map<int,int>> dp;
    int dfs(int left,int right,vector<int>& cuts){
        if(dp[left][right]!=0)return dp[left][right];
        bool flag=true;
        int res=INT_MAX;
        for(int x:cuts){
            if(left<x&&x<right){
                res=min(res,right-left+dfs(left,x,cuts)+dfs(x,right,cuts));
            }else if(x>=right)break;
        }
        return dp[left][right]=(res==INT_MAX?0:res);
    }
    int minCost(int n, vector<int>& cuts) {
        sort(cuts.begin(),cuts.end());
        return dfs(0,n,cuts);
    }
```


