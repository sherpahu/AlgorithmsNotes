# 划分型DP
### [132. 分割回文串 II](https://leetcode-cn.com/problems/palindrome-partitioning-ii/)
一个整串最少分割为几个回文串
采用中心扩散法，先求出s[i...j]是否为回文串用bool isPal[i][j]表示
```cpp
vector<vector<bool>>isPal(s.size()+1,vector<bool>(s.size()+1,false));
for(int i=0;i<s.size();++i){
    //只有奇数个元素的串，中心只有一个元素
    for(int l=i,r=i;l>=0&&r<s.size();--l,++r){
        if(s[l]==s[r])isPal[l][r]=true;
        else break;
    }
    //有偶数个元素的串，中心为两个相等的元素
    if(s[i]==s[i+1])
    for(int l=i,r=i+1;l>=0&&r<s.size();--l,++r){
        if(s[l]==s[r])isPal[l][r]=true;
        else break;
    }
}
```
最后状态转移方程为dp[i]=dp[j-1]+1, if isPal[j-1][i-1] 0<=j<=i
```cpp
    int minCut(string s) {
        vector<vector<bool>>isPal(s.size()+1,vector<bool>(s.size()+1,false));
        vector<int>dp(s.size()+1,s.size()+1);
        for(int i=0;i<s.size();++i){
            for(int l=i,r=i;l>=0&&r<s.size();--l,++r){
                if(s[l]==s[r])isPal[l][r]=true;
                else break;
            }
            if(s[i]==s[i+1])
            for(int l=i,r=i+1;l>=0&&r<s.size();--l,++r){
                if(s[l]==s[r])isPal[l][r]=true;
                else break;
            }
        }
        dp[0]=0;
        for(int i=1;i<=s.size();++i){
            for(int j=1;j<=i;++j){
                if(isPal[j-1][i-1])dp[i]=min(dp[i],dp[j-1]+1);
                /*if(i==s.size()){
                    cout<<j<<' '<<isPal[j-1][i-1]<<' '<<dp[i]<<endl;
                }*/
            }
            
        }
        return dp[s.size()]-1;
    }
```
# 双序列DP
### 最长公共子序列
```cpp
int longestCommonSubsequence(string &A, string &B) {
        // write your code here
        //if(A.size()==0||B.size()==0)return 0;
        vector<vector<int>>dp(A.size()+1,vector<int>(B.size()+1,0));
        for(int i=1;i<=A.size();++i){
            for(int j=1;j<=B.size();++j){
                if(A[i-1]==B[j-1]){
                    dp[i][j]=dp[i-1][j-1]+1;
                }else{
                    dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
                }
            }
        }
        return dp[A.size()][B.size()];
    }
```
### [交叉字符串](https://www.lintcode.com/problem/interleaving-string/description)
判断s1,s2拆分后是否可以组成s3
![image.png](https://cdn.nlark.com/yuque/0/2021/png/705461/1615855879671-91492b7a-19ab-4b82-a444-38878b6a3592.png#align=left&display=inline&height=194&margin=%5Bobject%20Object%5D&name=image.png&originHeight=388&originWidth=1161&size=548908&status=done&style=none&width=580.5)
需要初始化
要特别注意，循环从0开始，在状态转移时需要判断是否索引大于1，为的就是从dp[1][0]开始转移起
```cpp
bool isInterleave(string &s1, string &s2, string &s3) {
        // write your code here
        vector<vector<bool>>dp(s1.size()+1,vector<bool>(s2.size()+1,false));
        dp[0][0]=true;
        for(int i=0;i<=s1.size();++i){
            for(int j=0;j<=s2.size();++j){
                int k=i+j;
                if(i>=1&&s1[i-1]==s3[k-1])dp[i][j]=dp[i][j]||dp[i-1][j];
                if(j>=1&&s2[j-1]==s3[k-1])dp[i][j]=dp[i][j]||dp[i][j-1];
            }
        }
        return dp[s1.size()][s2.size()];
    }
```
### [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)
需要初始化
if(i==0)dp[i][j]=j;
if(j==0)dp[i][j]=i;
```cpp
    int minDistance(string &word1, string &word2) {
        // write your code here
        vector<vector<int>>dp(word1.size()+1,vector<int>(word2.size()+1,word1.size()+word2.size()+2));
        dp[0][0]=0;
        for(int i=0;i<=word1.size();++i){
            for(int j=0;j<=word2.size();++j){
                if(i==0)dp[i][j]=j;
                if(j==0)dp[i][j]=i;
                if(i>0&&j>0&&word1[i-1]==word2[j-1]){
                    dp[i][j]=dp[i-1][j-1];
                }else if(i>0&&j>0){
                    dp[i][j]=min(dp[i-1][j]+1,min(dp[i][j-1]+1,dp[i-1][j-1]+1));
                }
            }
        }
        return dp[word1.size()][word2.size()];
    }
```
### 删除word1中字符有多少种方法可以得到word2
需要初始化
word1长度为0时不能得到word2，dp[0][j]=0;
word2长度为0时只有一种方法:删除word1中所有字符，dp[i][0]=1;
i>0,j>0时，dp[i][j]表示word1[0...i-1]得到word2[0...j-1]的方法数
首先，dp[i][j]=dp[i-1][j]即删去word1当前字符可以得到word2的方法数
其次，当word1[i-1]==word2[j-1]时，dp[i][j]还等于word1不删当前字符的方法数，是word1,word2当前字符之前字符串的方法数
```cpp
    int numDistinct(string &word1, string &word2) {
        // write your code here
        vector<vector<int>>dp(word1.size()+1,vector<int>(word2.size()+1,word1.size()+word2.size()+2));
        dp[0][0]=0;
        for(int i=0;i<=word1.size();++i){
            for(int j=0;j<=word2.size();++j){
                if(i==0)dp[i][j]=0;
                if(j==0)dp[i][j]=1;
                if(i>0)dp[i][j]=dp[i-1][j];
                if(i>0&&j>0&&word1[i-1]==word2[j-1]){
                    dp[i][j]=dp[i][j]+dp[i-1][j-1];
                }
            }
        }
        return dp[word1.size()][word2.size()];
    }
```
### [10. 正则表达式匹配](https://leetcode-cn.com/problems/regular-expression-matching/)

![](https://cdn.nlark.com/yuque/0/2021/jpeg/705461/1615881692011-584cf8c5-77a1-4a5a-b5aa-90203505f654.jpeg#align=left&display=inline&height=402&margin=%5Bobject%20Object%5D&originHeight=402&originWidth=1009&size=0&status=done&style=none&width=1009)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/705461/1615881698281-8d6469e5-f7e9-44f1-a1a9-8c5ec681399e.jpeg#align=left&display=inline&height=362&margin=%5Bobject%20Object%5D&originHeight=362&originWidth=997&size=0&status=done&style=none&width=997)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/705461/1615881706969-f0b8a00d-7054-4901-83fa-4ce3969fc803.jpeg#align=left&display=inline&height=378&margin=%5Bobject%20Object%5D&originHeight=378&originWidth=989&size=0&status=done&style=none&width=989)


```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size();
        int n = p.size();

        auto matches = [&](int i, int j) {
            if (i == 0) {
                return false;
            }
            if (p[j - 1] == '.') {
                return true;
            }
            return s[i - 1] == p[j - 1];
        };

        vector<vector<int>> f(m + 1, vector<int>(n + 1));
        f[0][0] = true;
        for (int i = 0; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (p[j - 1] == '*') {
                    f[i][j] |= f[i][j - 2];//表示*只代表出现0次
                    if (matches(i, j - 1)) {
                        f[i][j] |= f[i - 1][j];//表示*只代表出现多次，所以考虑s[0...i-2]与p匹配
                    }
                }
                else {
                    if (matches(i, j)) {
                        f[i][j] |= f[i - 1][j - 1];
                    }
                }
            }
        }
        return f[m][n];
    }
};
```


# LIS
#### [300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)
```cpp
    int lengthOfLIS(vector<int>& nums) {
        vector<int>dp;
        dp.push_back(nums[0]);
        for(int i=1;i<nums.size();++i){
            if(nums[i]>dp[dp.size()-1])dp.push_back(nums[i]);
            else{
                auto it=lower_bound(dp.begin(),dp.end(),nums[i]);
                *it=nums[i];
            }
        }
        return dp.size();
    }
```
最长递增子序列求整个序列而不只是长度
![image.png](https://cdn.nlark.com/yuque/0/2021/png/705461/1615539549486-bed7b557-70dd-48fc-9cf3-56c76e46c20b.png#align=left&display=inline&height=448&margin=%5Bobject%20Object%5D&name=image.png&originHeight=568&originWidth=946&size=97605&status=done&style=none&width=746)
```cpp
#include <bits/stdc++.h>
using namespace std;
int LIS1(vector<int>&nums,vector<int>&pre,vector<int>&lis){//O(n^2)
    vector<int>dp(nums.size(),1);
    pre.resize(nums.size());
    fill(pre.begin(),pre.end(),-1);
    //递推
    for(int i=0;i<nums.size();++i){
        for(int j=0;j<i;++j){
            if(nums[i]>nums[j]){
                dp[i]=max(dp[i],dp[j]+1);
                pre[i]=j;
            }
        }
    }
    //获取LIS末尾元素
    int m=-1;
    for(int i=0;i<nums.size();++i){
        if(m==-1||dp[i]>dp[m]||dp[i]==dp[m]&&nums[m]>nums[i])m=i;
    }
    //获取LIS长度
    int len=dp[m];
    //获取LIS
    lis.resize(len);
    int idx=len;
    while(m!=-1){
        lis[--idx]=nums[m];
        m=pre[m];
    }
    return len;
}
int LIS2(vector<int>&nums,vector<int>&lis){//O(nlogn)
    vector<int>ends,dp(nums.size(),0);
    int maxIdx=0;
    for(int i=0;i<nums.size();++i){
        int idx=lower_bound(ends.begin(),ends.end(),nums[i])-ends.begin();
        if(idx==ends.size()){
            ends.push_back(nums[i]);
            dp[i]=ends.size();
            maxIdx=i;
        }else{
            ends[idx]=nums[i];
            dp[i]=idx+1;
            if(idx+1==ends.size())maxIdx=i;
        }
    }
    lis.resize(ends.size());
    int idx=ends.size()-1;
    lis[idx]=nums[maxIdx];
    for(int i=maxIdx;i>=0;--i){
        if(nums[i]<nums[maxIdx]&&dp[i]==dp[maxIdx]-1){
            lis[--idx]=nums[i];
            maxIdx=i;
        }
    }
    return ends.size();
}
int main() {
    int n;
    vector<int>nums,pre,lis;
    cin>>n;
    for(int i=0,x;i<n;++i){
        cin>>x;
        nums.push_back(x);
    }
    int len=LIS2(nums,lis);
    for(int i=0;i<len;++i){
        cout<<lis[i]<<' ';
    }
    return 0;
}
```
#### [673. 最长递增子序列的个数](https://leetcode-cn.com/problems/number-of-longest-increasing-subsequence/)
注意是求个数
```cpp
    int findNumberOfLIS(vector<int>& nums) {
        vector<int>dp(nums.size()+1,1),cnt(nums.size()+1,1);
        int maxVal=0;
        for(int i=0;i<nums.size();++i){
            for(int j=0;j<i;++j){
                if(nums[i]>nums[j]){
                    if (dp[j] + 1 > dp[i]) {
                        dp[i] = dp[j] + 1;
                        cnt[i] = cnt[j];
                    } else if (dp[j] + 1 == dp[i]) {
                        cnt[i] += cnt[j];
                    }
                }
            }
            if(dp[i]>maxVal){
                maxVal=dp[i];
            }
        }
        int ans=0;
        for(int i=0;i<nums.size();++i){
            if(dp[i]==maxVal)
            ans+=cnt[i];
        }
        return ans;
    }
```
#### [1713. 得到子序列的最少操作次数](https://leetcode-cn.com/problems/minimum-operations-to-make-a-subsequence/)
计算索引序列的最大递增子序列
```cpp
    unordered_map<int,int>n2i;
    int minOperations(vector<int>& target, vector<int>& arr) {
        for(int i=0;i<target.size();++i){
            n2i[target[i]]=i;
        }
        return target.size()-lengthOfLIS(arr);
    }
    int lengthOfLIS(vector<int>& nums) {
        vector<int>dp;
        int st=0;
        while(st<nums.size()&&!n2i.count(nums[st]))st++;
        if(st<nums.size()){
            dp.push_back(n2i[nums[st]]);
        }
        for(int i=st;i<nums.size();++i){
            int x;
            if(!n2i.count(nums[i]))continue;
            else x=n2i[nums[i]];
            if(dp[dp.size()-1]<x)dp.push_back(x);
            else{
                auto it=lower_bound(dp.begin(),dp.end(),x);
                *it=x;
            }
        }
        return dp.size();
    }
```
#### [674. 最长连续递增序列](https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence/)
```cpp
    int findLengthOfLCIS(vector<int>& nums) {
        if(nums.size()==0)return 0;
        int ans=1,pre=nums[0],st=0;
        for(int i=1;i<nums.size();++i){
            if(nums[i]>pre){
                ans=max(ans,i-st+1);
            }else{
                st=i;
            }
            pre=nums[i];
        }
        return ans;
    }
```
#### [字典序最小的最长递增子序列](https://www.nowcoder.com/questionTerminal/30fb9b3cab9742ecae9acda1c75bf927)
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    int n;
    cin>>n;
    vector<int>nums(n,0),ends,dp(n,0);
    for(int i=0;i<n;++i){
        cin>>nums[i];
    }
    ends.push_back(nums[0]);
    dp[0]=1;
    int maxIdx=0;
    for(int i=1;i<n;++i){
        if(ends[ends.size()-1]<nums[i]){
            ends.push_back(nums[i]);
            dp[i]=ends.size();
            maxIdx=i;
        }else{
            int idx=lower_bound(ends.begin(),ends.end(),nums[i])-ends.begin();
            ends[idx]=nums[i];
            dp[i]=idx+1;
            if(dp[i]==dp[maxIdx]){
                maxIdx=i;
            }
        }
    }
    vector<int>res(ends.size());
    int len=ends.size()-1;
    res[len--]=nums[maxIdx];
    for(int i=maxIdx;i>=0;--i){
        if(dp[i]+1==dp[maxIdx]){
            maxIdx=i;
            res[len--]=nums[maxIdx];
        }
    }
    for(int x:res){
        cout<<x<<' ';
    }
    return 0;
}

```
#### [最大递增子序列和](https://www.nowcoder.com/questionTerminal/dcb97b18715141599b64dbdb8cdea3bd)
将最长上升子系列O(n^2)的方法转移方式改为dp[i]=dp[j]+num[i];
```cpp
#include <bits/stdc++.h>
using namespace std;
int num[1010],dp[1010];
int main(){
    int n;
    cin>>n;
    for(int i=0;i<n;++i){
        cin>>num[i];
    }
    int ans=num[0];
    for(int i=0;i<n;++i){
        dp[i]=num[i];
    }
    for(int i=0;i<n;++i){
        for(int j=0;j<i;++j){
            if(num[j]<num[i]){
                dp[i]=max(dp[i],dp[j]+num[i]);
            }
        }
        ans=max(ans,dp[i]);
    }
    cout<<ans;
    return 0;
}
```
# 爬楼梯及其变形
#### [91. 解码方法](https://leetcode-cn.com/problems/decode-ways/)
字母被解码为1~26
s[i]为0时只有s[i-1]为'1'或'2'才能解码，其他时候不能解码
s[i]为1~6时，若前面为'1'或'2'有两种解码方法：与前一位共同解码，单独解码，其他时候只能单独解码
s[i]为7~9时，若前面为'1'有两种解码方法：与前一位共同解码，单独解码，其他时候只能单独解码
```cpp
    int numDecodings(string s) {
        vector<int>dp(s.size()+1,0);
        dp[0]=1;
        for(int i=1;i<=s.size();++i){
            char c=s[i-1];
            if(s[i-1]=='0'){
                if(i>1&&(s[i-2]=='1'||s[i-2]=='2'))dp[i]=dp[i-2];
                else return 0;
            }else if('1'<=s[i-1]&&s[i-1]<='6'){
                if(i>1&&(s[i-2]=='1'||s[i-2]=='2'))dp[i]=dp[i-1]+dp[i-2];
                else dp[i]=dp[i-1];
            }else{
                if(i>1&&s[i-2]=='1')dp[i]=dp[i-1]+dp[i-2];
                else dp[i]=dp[i-1];
            }
        }
        return dp[s.size()];
    }
```
#### [152. 乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/)
```cpp
    int maxProduct(vector<int>& nums) {
        int maxAns=nums[0],minAns=nums[0],ans=nums[0];
        for(int i=1;i<nums.size();++i){
            int mx = maxAns, mn = minAns;
            maxAns=max(mx*nums[i],max(mn*nums[i],nums[i]));
            minAns=min(mx*nums[i],min(mn*nums[i],nums[i]));
            ans=max(maxAns,ans);
        }
        return ans;
    }
```


