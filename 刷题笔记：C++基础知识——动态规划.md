# PAT基础知识——动态规划

基本流程：

- 找到状态和选择
- 明确dp数组/函数的定义
- 寻找状态之间的关系

这类问题存在**重叠子问题**，并且具备**最优子结构**。其核心思想就是枚举，只不过通过**状态转移方程**自底向上地没有重复地枚举。
**暴力的递归解法 -> 带备忘录的递归解法 -> 迭代的动态规划解法**
# 动态规划要点及单序列双序列的处理
[https://algorithm.yuanbin.me/zh-hans/dynamic_programming/](https://algorithm.yuanbin.me/zh-hans/dynamic_programming/)
动态规划是一种「分治」的思想，通俗一点来说就是「大事化小，小事化无」的艺术。在将大问题化解为小问题的「分治」过程中，保存对这些小问题已经处理好的结果，并供后面处理更大规模的问题时直接使用这些结果。嗯，感觉讲了和没讲一样，还是不会使用动规的思想解题...
下面看看知乎上的熊大大对动规比较「正经」的描述。
> 动态规划是通过拆分问题，定义问题状态和状态之间的关系，使得问题能够以递推（或者说分治）的方式去解决。

以上定义言简意赅，可直接用于实战指导，不愧是参加过NOI的。
动规的思想虽然好理解，但是要真正活用起来就需要下点功夫了。建议看看下面知乎上的回答。
动态规划最重要的两个要点：

1. 状态(状态不太好找，可先从转化方程入手分析)
1. 状态间的转化方程(从题目的隐含条件出发寻找递推关系)

其他的要点则是如初始化状态的确定(由状态和转化方程得知)，需要的结果(状态转移的终点)
动态规划问题中一般从以下四个角度考虑：

1. 状态(State)
1. 状态间的转移方程(Function)
1. 状态的初始化(Initialization)
1. 返回结果(Answer)

动规适用的情形：

1. 最大值/最小值, leetcode64
1. 有无可行解
1. 求方案个数, leetcode62

(如果需要列出所有方案，则一定不是动规，因为全部方案为指数级别复杂度，所有方案需要列出时往往用递归)

4. 给出的数据不可随便调整位置
## 单序列(DP_Sequence)
单序列动态规划的状态通常定义为：数组前 i 个位置, 数字, 字母 或者 以第i个为... 返回结果通常为数组的最后一个元素。
按照动态规划的四要素，此类题可从以下四个角度分析。

1. State: f[i] 前i个位置/数字/字母...
1. Function: f[i] = f[i-1]... 找递推关系
1. Initialization: 根据题意进行必要的初始化
1. Answer: f[n-1]
## 双序列(DP_Two_Sequence)
一般有两个数组或者两个字符串，计算其匹配关系。双序列中常用二维数组表示状态转移关系，但往往可以使用滚动数组的方式对空间复杂度进行优化。举个例子，以题 [Distinct Subsequences](http://algorithm.yuanbin.me/zh-hans/dynamic_programming/distinct_subsequences.html) 为例，状态转移方程如下：
```cpp
f[i+1][j+1] = f[i][j+1] + f[i][j] (if S[i] == T[j])
f[i+1][j+1] = f[i][j+1] (if S[i] != T[j])
```
从以上转移方程可以看出 `f[i+1][*]` 只与其前一个状态 `f[i][*]` 有关，而对于 `f[*][j]` 来说则基于当前索引又与前一个索引有关，故我们以递推的方式省略第一维的空间，并以第一维作为外循环，内循环为 j, 由递推关系可知在使用滚动数组时，若内循环 j 仍然从小到大遍历，那么对于 `f[j+1]` 来说它得到的 `f[j]` 则是当前一轮(`f[i+1][j]`)的值，并不是需要的 `f[i][j]` 的值。所以若想得到上一轮的结果，必须在内循环使用逆推的方式进行。文字表述比较模糊，可以自行画一个二维矩阵的转移矩阵来分析，认识到这一点非常重要。
小结一下，使用滚动数组的核心在于：

1. 状态转移矩阵中只能取 `f[i+1][*]` 和 `f[i][*]`, 这是使用滚动数组的前提。
1. 外循环使用 i, 内循环使用 j 并同时使用逆推，这是滚动数组使用的具体实践。

代码如下：
```cpp
public class Solution {
    /**
     * @param S, T: Two string.
     * @return: Count the number of distinct subsequences
     */
    public int numDistinct(String S, String T) {
        if (S == null || T == null) return 0;
        if (S.length() < T.length()) return 0;
        if (T.length() == 0) return 1;
        int[] f = new int[T.length() + 1];
        f[0] = 1;
        for (int i = 0; i < S.length(); i++) {
            for (int j = T.length() - 1; j >= 0; j--) {
                if (S.charAt(i) == T.charAt(j)) {
                        f[j + 1] += f[j];
                }
            }
        }
        return f[T.length()];
    }
}
```
# **动态规划的三大步骤**
[https://zhuanlan.zhihu.com/p/91582909](https://zhuanlan.zhihu.com/p/91582909)
动态规划，无非就是利用**历史记录**，来避免我们的重复计算。而这些**历史记录**，我们得需要一些**变量**来保存，一般是用**一维数组**或者**二维数组**来保存。下面我们先来讲下做动态规划题很重要的三个步骤，
如果你听不懂，也没关系，下面会有很多例题讲解，估计你就懂了。之所以不配合例题来讲这些步骤，也是为了怕你们脑袋乱了
**第一步骤**：定义**数组元素的含义**，上面说了，我们会用一个数组，来保存历史数组，假设用一维数组 dp[] 吧。这个时候有一个非常非常重要的点，就是规定你这个数组元素的含义，例如你的 dp[i] 是代表什么意思？
**第二步骤**：找出**数组元素之间的关系式**，我觉得动态规划，还是有一点类似于我们高中学习时的**归纳法**的，当我们要计算 dp[n] 时，是可以利用 dp[n-1]，dp[n-2].....dp[1]，来推出 dp[n] 的，也就是可以利用**历史数据**来推出新的元素值，所以我们要找出数组元素之间的关系式，例如 dp[n] = dp[n-1] + dp[n-2]，这个就是他们的关系式了。而这一步，也是最难的一步，后面我会讲几种类型的题来说。
学过动态规划的可能都经常听到> **最优子结构**，把大的问题拆分成小的问题，说时候，最开始的时候，我是对> **最优子结构**一梦懵逼的。估计你们也听多了，所以这一次，我将> **换一种形式来讲，不再是各种子问题，各种最优子结构**。所以大佬可别喷我再乱讲，因为我说了，这是我自己平时做题的套路。
**第三步骤**：找出**初始值**。学过**数学归纳法**的都知道，虽然我们知道了数组元素之间的关系式，例如
 dp[n] = dp[n-1] + dp[n-2]，我们可以通过 dp[n-1] 和 dp[n-2] 来计算 
dp[n]，但是，我们得知道初始值啊，例如一直推下去的话，会由 dp[3] = dp[2] + dp[1]。而 dp[2] 和 dp[1] 
是不能再分解的了，所以我们必须要能够直接获得 dp[2] 和 dp[1] 的值，而这，就是**所谓的初始值**。
由了**初始值**，并且有了**数组元素之间的关系式**，那么我们就可以得到 dp[n] 的值了，而 dp[n] 的含义是由你来定义的，你想**求什么，就定义它是什么**，这样，这道题也就解出来了。
# 动态规划详解
[https://labuladong.gitbook.io/algo/dong-tai-gui-hua-xi-lie/dong-tai-gui-hua-xiang-jie-jin-jie](https://labuladong.gitbook.io/algo/dong-tai-gui-hua-xi-lie/dong-tai-gui-hua-xiang-jie-jin-jie)
**动态规划问题的一般形式就是求最值**。动态规划其实是运筹学的一种最优化方法，只不过在计算机问题上应用比较多，比如说让你求**最长**递增子序列呀，**最小**编辑距离呀等等。
既然是要求最值，核心问题是什么呢？**求解动态规划的核心问题是穷举**。因为要求最值，肯定要把所有可行的答案穷举出来，然后在其中找最值呗。
动态规划就这么简单，就是穷举就完事了？我看到的动态规划问题都很难啊！
首先，动态规划的穷举有点特别，因为这类问题**存在「重叠子问题」**，如果暴力穷举的话效率会极其低下，所以需要「备忘录」或者「DP table」来优化穷举过程，避免不必要的计算。
而且，动态规划问题一定会**具备「最优子结构」**，才能通过子问题的最值得到原问题的最值。
另外，虽然动态规划的核心思想就是穷举求最值，但是问题可以千变万化，穷举所有可行解其实并不是一件容易的事，只有列出**正确的「状态转移方程**」才能正确地穷举。
以上提到的重叠子问题、最优子结构、状态转移方程就是动态规划三要素。具体什么意思等会会举例详解，但是在实际的算法问题中，**写出状态转移方程是最困难的**，这也就是为什么很多朋友觉得动态规划问题困难的原因，我来提供我研究出来的一个思维框架，辅助你思考状态转移方程：
明确「状态」 -> 定义 dp 数组/函数的含义 -> 明确「选择」-> 明确 base case。
下面通过斐波那契数列问题和凑零钱问题来详解动态规划的基本原理。前者主要是让你明白什么是重叠子问题（斐波那契数列严格来说不是动态规划问题），后者主要举集中于如何列出状态转移方程。
请读者不要嫌弃这个例子简单，**只有简单的例子才能让你把精力充分集中在算法背后的通用思想和技巧上，而不会被那些隐晦的细节问题搞的莫名其妙**。想要困难的例子，历史文章里有的是。
斐波那契数列
[https://labuladong.gitbook.io/algo/dong-tai-gui-hua-xi-lie/dong-tai-gui-hua-xiang-jie-jin-jie#yi-fei-bo-na-qi-shu-lie](https://labuladong.gitbook.io/algo/dong-tai-gui-hua-xi-lie/dong-tai-gui-hua-xiang-jie-jin-jie#yi-fei-bo-na-qi-shu-lie)
凑零钱问题
[https://labuladong.gitbook.io/algo/dong-tai-gui-hua-xi-lie/dong-tai-gui-hua-xiang-jie-jin-jie#er-cou-ling-qian-wen-ti](https://labuladong.gitbook.io/algo/dong-tai-gui-hua-xi-lie/dong-tai-gui-hua-xiang-jie-jin-jie#er-cou-ling-qian-wen-ti)
# 最优子结构详解
「最优子结构」是某些问题的一种特定性质，并不是动态规划问题专有的。也就是说，很多问题其实都具有最优子结构，只是其中大部分不具有重叠子问题，所以我们不把它们归为动态规划系列问题而已。
我先举个很容易理解的例子：假设你们学校有 10 个班，你已经计算出了每个班的最高考试成绩。那么现在我要求你计算全校最高的成绩，你会不会算？当然会，而且你不用重新遍历全校学生的分数进行比较，而是只要在这 10 个最高成绩中取最大的就是全校的最高成绩。
我给你提出的这个问题就**符合最优子结构**：可以从子问题的最优结果推出更大规模问题的最优结果。让你算**每个班**的最优成绩就是子问题，你知道所有子问题的答案后，就可以借此推出**全校**学生的最优成绩这个规模更大的问题的答案。
你看，这么简单的问题都有最优子结构性质，只是因为显然没有重叠子问题，所以我们简单地求最值肯定用不出动态规划。
再举个例子：假设你们学校有 10 个班，你已知每个班的最大分数差（最高分和最低分的差值）。那么现在我让你计算全校学生中的最大分数差，你会不会算？可以想办法算，但是肯定不能通过已知的这 10 个班的最大分数差推到出来。因为这 10 个班的最大分数差不一定就包含全校学生的最大分数差，比如全校的最大分数差可能是 3 班的最高分和 6 班的最低分之差。
这次我给你提出的问题就**不符合最优子结构**，因为你没办通过每个班的最优值推出全校的最优值，没办法通过子问题的最优值推出规模更大的问题的最优值。前文「动态规划详解」说过，想满足最优子结，子问题之间必须互相独立。全校的最大分数差可能出现在两个班之间，显然子问题不独立，所以这个问题本身不符合最优子结构。
**那么遇到这种最优子结构失效情况，怎么办？策略是：改造问题**。对于最大分数差这个问题，我们不是没办法利用已知的每个班的分数差吗，那我只能这样写一段暴力代码：
```cpp
int result = 0;
for (Student a : school) {
    for (Student b : school) {
        if (a is b) continue;
        result = max(result, |a.score - b.score|);
    }
}
return result;
```
改造问题，也就是把问题等价转化：最大分数差，不就等价于最高分数和最低分数的差么，那不就是要求最高和最低分数么，不就是我们讨论的第一个问题么，不就具有最优子结构了么？那现在改变思路，借助最优子结构解决最值问题，再回过头解决最大分数差问题，是不是就高效多了？
当然，上面这个例子太简单了，不过请读者回顾一下，我们做动态规划问题，是不是一直在求各种最值，本质跟我们举的例子没啥区别，无非需要处理一下重叠子问题。
前文「不同定义不同解法」和「高楼扔鸡蛋进阶」就展示了如何改造问题，不同的最优子结构，可能导致不同的解法和效率。
再举个常见但也十分简单的例子，求一棵二叉树的最大值，不难吧（简单起见，假设节点中的值都是非负数）：你看这个问题也符合最优子结构，以 `root` 为根的树的最大值，可以通过两边子树（子问题）的最大值推导出来，结合刚才学校和班级的例子，很容易理解吧。
```cpp
int maxVal(TreeNode root) {
    if (root == null)
        return -1;
    int left = maxVal(root.left);
    int right = maxVal(root.right);
    return max(root.val, left, right);
}
```
当然这也不是动态规划问题，旨在说明，最优子结构并不是动态规划独有的一种性质，能求最值的问题大部分都具有这个性质；**但反过来，最优子结构性质作为动态规划问题的必要条件，一定是让你求最值的**，以后碰到那种恶心人的最值题，思路往动态规划想就对了，这就是套路。
动态规划不就是从最简单的 base case 往后推导吗，可以想象成一个链式反应，以小博大。但只有符合最优子结构的问题，才有发生这种链式反应的性质。
找最优子结构的过程，其实就是证明状态转移方程正确性的过程，方程符合最优子结构就可以写暴力解了，写出暴力解就可以看出有没有重叠子问题了，有则优化，无则 OK。这也是套路，经常刷题的朋友应该能体会。
这里就不举那些正宗动态规划的例子了，读者可以翻翻历史文章，看看状态转移是如何遵循最优子结构的，这个话题就聊到这，下面再来看另外个动态规划迷惑行为。
# dp 数组的遍历方向
我相信读者做动态规问题时，肯定会对 `dp` 数组的遍历顺序有些头疼。我们拿二维 `dp` 数组来举例，有时候我们是正向遍历：
```cpp
int[][] dp = new int[m][n];
for (int i = 0; i < m; i++)
    for (int j = 0; j < n; j++)
        // 计算 dp[i][j]
```
有时候我们反向遍历：
```cpp
for (int i = m - 1; i >= 0; i--)
    for (int j = n - 1; j >= 0; j--)
        // 计算 dp[i][j]
```
有时候可能会斜向遍历：
```cpp
// 斜着遍历数组
for (int l = 2; l <= n; l++) {
    for (int i = 0; i <= n - l; i++) {
        int j = l + i - 1;
        // 计算 dp[i][j]
    }
}
```
甚至更让人迷惑的是，有时候发现正向反向遍历都可以得到正确答案，比如我们在「团灭股票问题」中有的地方就正反皆可。
那么，如果仔细观察的话可以发现其中的原因的。你只要把住两点就行了：
**1、遍历的过程中，所需的状态必须是已经计算出来的**。
**2、遍历的终点必须是存储结果的那个位置**。
下面来距离解释上面两个原则是什么意思。
比如编辑距离这个经典的问题，详解见前文「编辑距离详解」，我们通过对 `dp` 数组的定义，确定了 base case 是 `dp[..][0]` 和 `dp[0][..]`，最终答案是 `dp[m][n]`；而且我们通过状态转移方程知道 `dp[i][j]` 需要从 `dp[i-1][j]`, `dp[i][j-1]`, `dp[i-1][j-1]` 转移而来，如下图：
![](https://cdn.nlark.com/yuque/0/2020/jpeg/705461/1580262403399-02be41d5-e7c4-4fb9-afcb-bee98313ec08.jpeg#align=left&display=inline&height=720&originHeight=720&originWidth=1280&size=0&status=done&style=none&width=1280)
那么，参考刚才说的两条原则，你该怎么遍历 `dp` 数组？肯定是正向遍历：
```cpp
for (int i = 1; i < m; i++)
    for (int j = 1; j < n; j++)
        // 通过 dp[i-1][j], dp[i][j - 1], dp[i-1][j-1]
        // 计算 dp[i][j]
```
因为，这样每一步迭代的左边、上边、左上边的位置都是 base case 或者之前计算过的，而且最终结束在我们想要的答案 `dp[m][n]`。
再举一例，回文子序列问题，详见前文「子序列问题模板」，我们通过过对 `dp` 数组的定义，确定了 base case 处在中间的对角线，`dp[i][j]` 需要从 `dp[i+1][j]`, `dp[i][j-1]`, `dp[i+1][j-1]` 转移而来，想要求的最终答案是 `dp[0][n-1]`，如下图：
![](https://cdn.nlark.com/yuque/0/2020/jpeg/705461/1580262440619-51b4e4e2-22c3-4ac1-9822-c7e36e6d01b5.jpeg#align=left&display=inline&height=720&originHeight=720&originWidth=1280&size=0&status=done&style=none&width=1280)
这种情况根据刚才的两个原则，就可以有两种正确的遍历方式：
![](https://cdn.nlark.com/yuque/0/2020/jpeg/705461/1580262479743-f26ad7a1-f244-4527-9368-d5dc32b18d3c.jpeg#align=left&display=inline&height=720&originHeight=720&originWidth=1280&size=0&status=done&style=none&width=1280)
要么从左至右斜着遍历，要么从下向上从左到右遍历，这样才能保证每次 `dp[i][j]` 的左边、下边、左下边已经计算完毕，得到正确结果。
现在，你应该理解了这两个原则，主要就是看 base case 和最终结果的存储位置，保证遍历过程中使用的数据都是计算完毕的就行，有时候确实存在多种方法可以得到正确答案，可根据个人口味自行选择。
在使用滚动数组优化空间复杂度的时候，要是可能使用倒序遍历防止覆盖，如01背包
# 斐波那契数列类型
递归可解，不存在最优的问题。通过备忘录实现自顶向下，通过动态规划实现自底向上两种优化。
## [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)
### 暴力解法
```cpp
class Solution {
public:
    int climbStairs(int n) {
       if(n==1) return 1;
       else if(n==2) return 2;
       else return climbStairs(n-1)+climbStairs(n-2);
    }
};
```
### 备忘录解法
```cpp
class Solution {
public:
    int climbStairs(int n) {
       if(n==1){ 
           memo[1]=1;
           return 1;
        }
       else if(n==2){ 
           memo[2]=2;
           return 2;
       }
       else{ 
           if(memo[n]!=0)return memo[n];
           else{
                int ans=climbStairs(n-1)+climbStairs(n-2);
                memo[n]=ans;
                return ans;
           }
       }
    }
private:
    int memo[1000]={0};
};
```
### 动态规划解法
```cpp
class Solution {
public:
    int climbStairs(int n) {
        for(int i=1;i<=n;i++){
            if(i==1)dp[1]=1;
            else if(i==2)dp[2]=2;
            else dp[i]=dp[i-1]+dp[i-2];
        }
        return dp[n];
    }
private:
    int dp[1000];
};
```
优化存储空间
```cpp
class Solution {
public:
    int climbStairs(int n) {
        if(n==1)return 1;
        else if(n==2)return 2;
        int curr,prev1=2,prev2=1;
        for(int i=3;i<=n;i++){
            curr=prev1+prev2;
            prev2=prev1;
            prev1=curr;
        }
        return curr;
    }
};
```
## [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)
![图片.png](https://cdn.nlark.com/yuque/0/2020/png/705461/1580099109596-dbf1bad2-7504-4c06-b439-e47c314bb8d0.png#align=left&display=inline&height=339&name=%E5%9B%BE%E7%89%87.png&originHeight=442&originWidth=942&size=70785&status=done&style=none&width=723)
dp[i]=max(dp[i-2]+nums[i],dp[i-1]);
dp[i]表示到了i处时的最大收益，i处可以抢也可以不抢
由于不能抢临近的，所以决定抢i处时dp[i]=dp[i-2]+nums[i]，不抢时dp[i]=dp[i-1]
```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int pre1=0,pre2=0,cur=0;
        int len=nums.size();
        for(int i=0;i<len;i++){
            cur=max(pre2+nums[i],pre1);
            pre2=pre1;
            pre1=cur;
        }
        return cur;
    }
};
```
## [213. 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)
环状排列意味着第一个房子和最后一个房子中只能选择一个偷窃，因此可以把此环状排列房间问题约化为两个单排排列房间子问题：

- 在不偷窃第一个房子的情况下（即 nums[1:]），最大金额是 p1；
- 在不偷窃最后一个房子的情况下（即 nums[:n−1]），最大金额是 p2。

   综合偷窃最大金额： 为以上两种情况的较大值，即 max(p1,p2)max(p1,p2)max(p1,p2) 。
```cpp
class Solution {
public:
    int rob(vector<int>& nums,int start,int end) {
        int pre1=0,pre2=0,cur=0;
        int len=nums.size();
        for(int i=start;i<end;i++){
            cur=max(pre2+nums[i],pre1);
            pre2=pre1;
            pre1=cur;
        }
        return cur;
    }
    int rob(vector<int>& nums) {
        if(nums.size()==1)return nums[0];
        if(nums.size()==2)return max(nums[0],nums[1]);
        int n=nums.size();
        return max(rob(nums,0,n-1),rob(nums,1,n));
    }
};
```
# 带权矩阵路径的最优值
不带权的最短路径可以使用bfs求解（此时相当于权重为1）
## [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)
由于只能向右或者向左走，所以grid[i][j]处的最小路径和在从 `grid[i-1][j]` , `grid[i][j-1]` 处分别向右和向下走的过程中获得，故 `dp[i][j]=min(dp[i-1][j],dp[i][j-1])+grid[i][j];` 
```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        dp[0][0]=grid[0][0];
        int lx=grid.size(),ly=grid[0].size();
        for(int i=0;i<lx;i++){
            for(int j=0;j<ly;j++){
                if(i==0&&j==0)continue;
                if(i==0)dp[i][j]=dp[i][j-1]+grid[i][j];
                else if(j==0) dp[i][j]=dp[i-1][j]+grid[i][j];
                else{
                    dp[i][j]=min(dp[i-1][j],dp[i][j-1])+grid[i][j];
                }
            }
        }
        return dp[lx-1][ly-1];
    }
private:
    int dp[1010][1010]={0};
};
```
优化存储空间
```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        dp[0]=grid[0][0];
        int lx=grid.size(),ly=grid[0].size();
        for(int i=0;i<lx;i++){
            for(int j=0;j<ly;j++){
                if(i==0&&j==0)continue;
                if(i==0) dp[j]=dp[j-1]+grid[i][j];
                else if(j==0) dp[j]=dp[j]+grid[i][j];
                else{
                    dp[j]=min(dp[j],dp[j-1])+grid[i][j];
                }
            }
        }
        return dp[ly-1];
    }
private:
    int dp[1010]={0};
};
```
## 62矩阵中起点到终点的不同路径
[https://leetcode-cn.com/problems/unique-paths/description/](https://leetcode-cn.com/problems/unique-paths/description/)
一般而言要求不能重复访问某处则使用dfs，但此题只要两条路径有一个点不同即为不同路径，此时去掉isVisited[][] 矩阵时用dfs仍可以得到正确结果，但是TLE。
用动态规划排除重叠子问题解决TLE
### DFS（TLE）
```cpp
class Solution {
public:
    void dfs(int x,int y,int m,int n){
        if(x==m-1&&y==n-1){
            ttl++;
        }
        for(int i=0;i<2;i++){
            int newx=x+X[i];
            int newy=y+Y[i];
            if(newx>=0&&newx<m&&newy>=0&&newy<n){
                dfs(newx,newy,m,n);
            }
        }
    }
    int uniquePaths(int m, int n) {
        dfs(0,0,m,n);
        return ttl;
    }
private:
    int ttl=0;
    int X[5]={1,0};
    int Y[5]={0,1};
};
```
### 动态规划
从 `grid[i-1][j]` , `grid[i][j-1]` 处分别向右和向下走可以到达grid[i][j]处
```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        fill(dp,dp+110,1);
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                dp[j]=dp[j]+dp[j-1];
            }
        }
        return dp[n-1];
    }
private:
    int dp[110];
};
```
### 数学方法求解
> 也可以直接用数学公式求解，这是一个组合问题。机器人总共移动的次数 S=m+n-2，向下移动的次数 D=m-1，那么问题可以看成从 S 中取出 D 个位置的组合数量，这个问题的解为 C(S, D)。

```cpp
public int uniquePaths(int m, int n) {
    int S = m + n - 2;  // 总共的移动次数
    int D = m - 1;      // 向下的移动次数
    long ret = 1;
    for (int i = 1; i <= D; i++) {
        ret = ret * (S - D + i) / i;
    }
    return (int) ret;
}
```
# 数组区间
## [303. 区域和检索 - 数组不可变](https://leetcode-cn.com/problems/range-sum-query-immutable/)
```cpp
class NumArray {
public:
    NumArray(vector<int>& nums) {
        int len=nums.size();
        for(int i=0;i<len;i++){
            sum[i+1]=sum[i]+nums[i];
        }
    }
    
    int sumRange(int i, int j) {
        return sum[j+1]-sum[i];
    }
private:
    int sum[100000]={0};
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(i,j);
 */
```
## [413. 等差数列划分](https://leetcode-cn.com/problems/arithmetic-slices/)
令dp[i]为以i结尾的等差数列的个数
由于相邻三个数之差相等才构成等差数列，所以当 `A[i]-A[i-1]==A[i-1]-A[i-2]` 时 `A[i],A[i-1],A[i-2]` 构成等差数列， `dp[i]=dp[i-1]+1` ;
```cpp
class Solution {
public:
    int numberOfArithmeticSlices(vector<int>& A) {
        int len=A.size();
        dp[0]=0;
        dp[1]=0;
        for(int i=2;i<len;i++){
            if(A[i]-A[i-1]==A[i-1]-A[i-2]){
                dp[i]=dp[i-1]+1;
            }
        }
        int cnt=0;
        for(int num:dp){
            cnt+=num;
        }
        return cnt;
    }
private:
    int dp[100000];
};
```
# 分割整数的最优方案
## [343. 整数拆分](https://leetcode-cn.com/problems/integer-break/)
当某个数j作为要拆分的数的时候因子乘积的结果是取不到j值本身的，但是作为因子的时候是可以取到j值本身的
所以dp[i] = max(dp[i], max(dp[j],j)*k);
```cpp
class Solution {
public:
    int integerBreak(int n) {
        dp[1]=1;
        for(int i=2;i<=n;i++){
            for(int j=1;j<i;j++){
                int k=i-j;
                dp[i] = max(dp[i], max(dp[j],j)*k);
            }
            dp[i]=max(dp[i],i-1);
        }
        return dp[n];
    }
private:
    int dp[10000];
};
```
## [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)
### bfs
```cpp
class Solution {
public:
    int numSquares(int n) {
        bool flag[n+10];
        fill(flag,flag+n+10,false);//局域变量初始化
        vector<int> squares;
        for(int i=1;i*i<=n;i++){
            squares.push_back(i*i);
        }
        queue<pair<int,int>>q;//n,level
        q.push({n,0});
        flag[n]=true;
        while(!q.empty()){
            pair<int,int>p=q.front();
            q.pop();
            for(int s:squares){
                int next=p.first-s;
                if(next<0)break;//后面的都比这个大，直接退出for循环
                if(next==0)return p.second+1;
                if(flag[next]==true)continue;//走过了
                q.push({next,p.second+1});
                flag[next]=true;
            }
        }
        return n;
    }
};
```
### 动态规划
TLE
![](https://cdn.nlark.com/yuque/__latex/9b52c570907149e3d386f8aa8279345e.svg#card=math&code=dp%5Bi%5D%3D%5Cbegin%7Bcases%7D%0A1%2Ci%3D1%E6%88%96%E5%AE%8C%E5%85%A8%E5%B9%B3%E6%96%B9%E6%95%B0%5C%5C%0Amin%28dp%5Bj%5D%2Bdp%5Bi-j%5D%29%2Ci%3E1%E4%B8%94%E4%B8%8D%E6%98%AF%E5%AE%8C%E5%85%A8%E5%B9%B3%E6%96%B9%E6%95%B0%281%3C%3Dj%3Ci%29%0A%5Cend%7Bcases%7D&height=68&width=614)
```cpp
class Solution {
public:
    int numSquares(int n) {
        for(int i=1;i*i<=n;i++){
            isSquare[i*i]=true;
        }
        if(isSquare[n])return 1;
        dp[1]=1;
        for(int i=2;i<=n;i++){
            if(isSquare[i])dp[i]=1;
            else{
                int tmp=INT_MAX;
                for(int j=1;j<i;j++){
                    tmp=min(tmp,dp[j]+dp[i-j]);
                }
                dp[i]=tmp;
            }
        }
        return dp[n];
    }
private:
    bool isSquare[10000]={false};
    int dp[10000];
};
```
不超时的方法
由于n要分解为完全平方数，所以上述代码每次求dp[i]的时候都要从1遍历到i-1是有所浪费的，应该遍历小于i的完全平方数
![](https://cdn.nlark.com/yuque/__latex/4f3b60bb48f2359ad05f100708483b25.svg#card=math&code=dp%5Bi%5D%3D%5Cbegin%7Bcases%7D%0A1%2Ci%3D1%E6%88%96%E5%AE%8C%E5%85%A8%E5%B9%B3%E6%96%B9%E6%95%B0%5C%5C%0Amin%281%2Bdp%5Bi-square%5D%29%2Ci%3E1%E4%B8%94%E4%B8%8D%E6%98%AF%E5%AE%8C%E5%85%A8%E5%B9%B3%E6%96%B9%E6%95%B0%28square%3Ci%29%0A%5Cend%7Bcases%7D&height=68&width=634)
```cpp
class Solution {
public:
    int numSquares(int n) {
        int cnt=0;
        for(int i=1;i*i<=n;i++){
            isSquare[i*i]=true;
            squares[cnt++]=i*i;
        }
        if(isSquare[n])return 1;
        dp[1]=1;
        for(int i=2;i<=n;i++){
            if(isSquare[i])dp[i]=1;
            else{
                int tmp=INT_MAX;
                for(int j=0;j<cnt;j++){
                    if(squares[j]>i)break;
                    tmp=min(tmp,1+dp[i-squares[j]]);
                }
                dp[i]=tmp;
            }
        }
        return dp[n];
    }
private:
    bool isSquare[10000]={false};
    int squares[10000]={0};
    int dp[10000];
};
```
# 最长递增子序列
已知一个序列 {S, S,...,S}，取出若干数组成新的序列 {S, S,..., S}，其中 i1、i2 ... im 保持递增，即新序列中各个数仍然保持原数列中的先后顺序，称新序列为原序列的一个  **子序列**  。
如果在子序列中，当下标 ix > iy 时，S > S，称子序列为原序列的一个  **递增子序列**  。
定义一个数组 dp 存储最长递增子序列的长度，dp[n] 表示以 S 结尾的序列的最长递增子序列长度。对于一个递增子序列 {S, S,...,S}，如果 im < n 并且 S < S，此时 {S, S,..., S, S} 为一个递增子序列，递增子序列的长度增加 1。满足上述条件的递增子序列中，长度最长的那个递增子序列就是要找的，在长度最长的递增子序列上加上 S 就构成了以 S 为结尾的最长递增子序列。因此 dp[n] = max{ dp[i]+1 | S < S && i < n} 。
因为在求 dp[n] 时可能无法找到一个满足条件的递增子序列，此时 {S} 就构成了递增子序列，需要对前面的求解方程做修改，令 dp[n] 最小为 1，即：
![](https://camo.githubusercontent.com/c6b76b65c1880a507bf1cb0250456121db57a1ab/68747470733a2f2f63732d6e6f7465732d313235363130393739362e636f732e61702d6775616e677a686f752e6d7971636c6f75642e636f6d2f65653939346461342d306663372d343433642d616335362d6330386361663030613230342e6a7067#align=left&display=inline&height=45&originHeight=45&originWidth=472&status=done&style=none&width=472)

对于一个长度为 N 的序列，最长递增子序列并不一定会以 S 为结尾，因此 dp[N] 不是序列的最长递增子序列的长度，需要遍历 dp 数组找出最大值才是所要的结果，max{ dp[i] | 1 <= i <= N} 即为所求。
## [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)
### O(n^2)解法
```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        dp[0]=1;
        int len=nums.size();
        for(int i=1;i<len;i++){
            for(int j=0;j<i;j++){
                if(nums[j]<nums[i]){
                    dp[i]=max(dp[i],dp[j]);
                }
            }
            dp[i]++;
        }
        int ans=0;
        for(int i=0;i<len;i++){
            if(dp[i]>ans){
                ans=dp[i];
            }
        }
        return ans;
    }
private:
    int dp[10000]={0};
};
```
### 二分查找O(nlogn)
使用 `tail[i]` 存储长度为i的上升子序列的尾部的最小值。最终tail数组的长度为最长递增子序列的长度
构建tail的方法是用nums中的元素替换tail中恰好不大于它的元素，即对nums中每一个元素num，查找恰好不小于num的索引的位置index，令tail[index]=num;
“恰好不大于”的原因在于增加len是根据index是否等于len决定的，即使在tail中不替换相等的两个元素也可以保证tail的性质，但是len无法及时更新。
具体查看
[https://leetcode-cn.com/problems/longest-increasing-subsequence/solution/zui-chang-shang-sheng-zi-xu-lie-dong-tai-gui-hua-2/](https://leetcode-cn.com/problems/longest-increasing-subsequence/solution/zui-chang-shang-sheng-zi-xu-lie-dong-tai-gui-hua-2/)
回顾二分查找：
二分查找元素在数组中的位置：
```cpp
int binarySearch(int nums,int len,int target){
 	int l=0,r=len-1;
    while(l<=r){
     	int m=l+(r-l)/2;
        if(nums[m]==target){
            return m;
        }else if(nums[m]>target){
            r=m-1;//当while条件为l<=r时此处必须为r=m-1;否则若为r=m则导致死循环
        }else{
            l=m+1;
        }
    }
    return -1;//没找到
}
```
二分查找恰好符合条件的元素的最小索引值
```cpp
int binarySearch(int nums,int len,int target){
 	int l=0,r=len-1;
    while(l<=r){
        int m=l+(r-l)/2;
        if(isQuanlified(nums[m]){
            r=m-1;
        }else{
            l=m+1;
        }
    }
    return l;//恰好符合条件的元素的最小索引值
}
```
故最终代码为
```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int len=0;
        for(auto num:nums){
            int index=binarySearch(tail,len,num);
            tail[index]=num;
            if(index==len){
                len++;
            }
        }
        return len;
    }
    int binarySearch(int tail[],int len,int num){
        int l=0,r=len-1;
        while(l<=r){
            int mid=l+r>>1;
            if(tail[mid]>=num){
                r=mid-1;
            }else{
                l=mid+1;
            }
        }
        return l;
    }
private:
    int tail[10000]={0};
};
```
## [646. 最长数对链](https://leetcode-cn.com/problems/maximum-length-of-pair-chain/)
与上题类似，只不过需要**事先排序**
```cpp
class Solution {
public:
    int findLongestChain(vector<vector<int>>& pairs) {
        dp[0]=1;
        int len=pairs.size();
        sort(pairs.begin(),pairs.end(),[](const auto& a,const auto& b){
            return (a[0]<b[0])||(a[0]==b[0]&&a[1]<b[1]);
        });
        for(int i=1;i<len;i++){
            for(int j=0;j<i;j++){
                if(pairs[j][1]<pairs[i][0]){
                    dp[i]=max(dp[i],dp[j]);
                }
            }
            dp[i]++;
        }
        int ans=0;
        for(int i=0;i<len;i++){
            if(dp[i]>ans){
                ans=dp[i];
            }
        }
        return ans;
    }
private:
    int dp[10000]={0};
};
```
## [376. 摆动序列](https://leetcode-cn.com/problems/wiggle-subsequence/)
**线性动规**
整个序列就像山地一样，有上坡和下坡，而摆动序列求的是子序列，上坡、下坡只能算一个，故当 `nums[i]>nums[i-1]` 时 `up=down+1;` ， `nums[i]<nums[i-1]` 时 `down=up+1` 。
```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int down=1,up=1;
        int len=nums.size();
        if(len==0)return 0;
        for(int i=1;i<len;i++){
            if(nums[i]>nums[i-1])up=down+1;
            if(nums[i]<nums[i-1])down=up+1;
        }
        return max(up,down);
    }
};
```
# 最长公共子序列
对于两个子序列 S1 和 S2，找出它们最长的公共子序列。
定义一个二维数组 dp 用来存储最长公共子序列的长度，其中 dp[i][j] 表示 S1 的前 i 个字符与 S2 的前 j 个字符最长公共子序列的长度。考虑 S1 与 S2 值是否相等，分为两种情况：

- 当 S1==S2 时，那么就能在 S1 的前 i-1 个字符与 S2 的前 j-1 个字符最长公共子序列的基础上再加上 S1 这个值，最长公共子序列长度加 1，即 dp[i][j] = dp[i-1][j-1] + 1。
- 当 S1 != S2 时，此时最长公共子序列为 S1 的前 i-1 个字符和 S2 的前
 j 个字符最长公共子序列，或者 S1 的前 i 个字符和 S2 的前 j-1 个字符最长公共子序列，取它们的最大者，即 dp[i][j] = 
max{ dp[i-1][j], dp[i][j-1] }。

综上，最长公共子序列的状态转移方程为：
![](https://camo.githubusercontent.com/d30a7c81b039f6f97d6369fc9b34666d67c4a1f8/68747470733a2f2f63732d6e6f7465732d313235363130393739362e636f732e61702d6775616e677a686f752e6d7971636c6f75642e636f6d2f65636438396132322d633037352d343731362d383432332d6530626138393233306539612e6a7067#align=left&display=inline&height=85&originHeight=85&originWidth=577&status=done&style=none&width=577)

对于长度为 N 的序列 S 和长度为 M 的序列 S，dp[N][M] 就是序列 S 和序列 S 的最长公共子序列长度。
与最长递增子序列相比，最长公共子序列有以下不同点：

- 针对的是两个序列，求它们的最长公共子序列。
- 在最长递增子序列中，dp[i] 表示以 S 为结尾的最长递增子序列长度，子序列必须包含 S ；在最长公共子序列中，dp[i][j] 表示 S1 中前 i 个字符与 S2 中前 j 个字符的最长公共子序列长度，不一定包含 S1 和 S2。
- 在求最终解时，最长公共子序列中 dp[N][M] 就是最终解，而最长递增子序列中 dp[N] 不是最终解，因为以 S 为结尾的最长递增子序列不一定是整个序列最长递增子序列，需要遍历一遍 dp 数组找到最大者。
## [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)
`dp[i+1][j+1]` 表示 `text1[:i+1]` 与 `text2[:j+1]` 的最大公共子序列
为了避免出现索引为负，所以这么设置
```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int l1=text1.size(),l2=text2.size();
        for(int i=0;i<l1;i++){
            for(int j=0;j<l2;j++){
                if(text1[i]==text2[j]){
                    dp[i+1][j+1]=dp[i][j]+1;
                }else{
                    dp[i+1][j+1]=max(dp[i][j+1],dp[i+1][j]);
                }
            }
        }
        return dp[l1][l2];
    }
private:
    int dp[1000][1000]={0};
};
```
# 编辑数组以拼凑回文数组

[https://www.nowcoder.com/questionTerminal/00fccaa8e30d414ab86b9bb229bd8e68?f=discussion](https://www.nowcoder.com/questionTerminal/00fccaa8e30d414ab86b9bb229bd8e68?f=discussion)
注意遍历的方向，由于在计算i处时需要i+1所以i倒着遍历，而j正着遍历
```cpp
#include <bits/stdc++.h>
using namespace std;
int dp[10010][10010],a[10010];
int main(){
    std::ios::sync_with_stdio(false);
    int n;
    cin>>n;
    for(int i=0;i<n;i++){
        cin>>a[i];
        dp[i][i]=a[i];
    }
    for(int j=1;j<n;j++){
        for(int i=j-1;i>=0;i--){
            if(a[i]==a[j]){
                dp[i][j]=dp[i+1][j-1]+2*a[i];
            }else{
                dp[i][j]=min(dp[i+1][j]+2*a[i],dp[i][j-1]+2*a[j]);
            }
        }
    }
    cout<<dp[0][n-1];
    return 0;
}
```

# 0-1 背包
有一个容量为 N 的背包，要用这个背包装下物品的价值最大，这些物品有两个属性：体积 w 和价值 v。
定义一个二维数组 dp 存储最大价值，其中 dp[i][j] 表示前 i 件物品体积不超过 j 的情况下能达到的最大价值。设第 i 件物品体积为 w，价值为 v，根据第 i 件物品是否添加到背包中，可以分两种情况讨论：

- 第 i 件物品没添加到背包，总体积不超过 j 的前 i 件物品的最大价值就是总体积不超过 j 的前 i-1 件物品的最大价值，dp[i][j] = dp[i-1][j]。
- 第 i 件物品添加到背包中，dp[i][j] = dp[i-1][j-w] + v。

第 i 件物品可添加也可以不添加，取决于哪种情况下最大价值更大。因此，0-1 背包的状态转移方程为：
![](https://camo.githubusercontent.com/40d008d495f18865d4563ad6510f808382970794/68747470733a2f2f63732d6e6f7465732d313235363130393739362e636f732e61702d6775616e677a686f752e6d7971636c6f75642e636f6d2f38636232626536362d336434372d343162612d623535622d3331396663363839343064342e706e67#align=left&display=inline&height=44&originHeight=44&originWidth=474&status=done&style=none&width=474)
由于状态转移方程中有i-1,j-w所以从1开始遍历，保证第一个元素为0且索引合法
```cpp
// W 为背包总体积
// N 为物品数量
// weights 数组存储 N 个物品的重量
// values 数组存储 N 个物品的价值
//dp[i][j]为前i件物体，体积不超过j时的最大价值
int knapack(int W,int N,int weights[],int values[]){
 	for(int i=1;i<=N;i++){
        int v=values[i-1],w=weights[j-1];
		for(int j=1;j<=W;j++){
            if(j<w){
                dp[i][j]=dp[i-1][j];
            }else{
                dp[i][j]=max(dp[i-1][j],dp[i-1][j-w]+v);
            }
        }
    }
    return dp[N][W];
}
```
**空间优化**
在程序实现时可以对 0-1 背包做优化。观察状态转移方程可以知道，前 i 件物品的状态仅与前 i-1 件物品的状态有关，因此可以将 dp 定义为一维数组，其中 dp[j] 既可以表示 dp[i-1][j] 也可以表示 dp[i][j]。此时，
![](https://camo.githubusercontent.com/8b6d2a40c8b41e3b5917f64c52d5b64ce491829c/68747470733a2f2f63732d6e6f7465732d313235363130393739362e636f732e61702d6775616e677a686f752e6d7971636c6f75642e636f6d2f39616538396631362d373930352d346136662d383861322d3837346234636163393166342e6a7067#align=left&display=inline&height=52&originHeight=52&originWidth=391&status=done&style=none&width=391)
因为 dp[j-w] 表示 dp[i-1][j-w]，因此不能先求 dp[i][j-w]，防止将 dp[i-1][j-w] 覆盖。也就是说要先计算 dp[i][j] 再计算 dp[i][j-w]，在程序实现时需要按倒序来循环求解。
```cpp
// W 为背包总体积
// N 为物品数量
// weights 数组存储 N 个物品的重量
// values 数组存储 N 个物品的价值
//dp[i][j]为前i件物体，体积不超过j时的最大价值
int knapack(int W,int N,int weights[],int values[]){
 	for(int i=1;i<=N;i++){
        int v=values[i-1],w=weights[j-1];
		for(int j=w;j>=1;j--){
            if(j>=w){
                dp[j]=max(dp[j],dp[j-w]+v);
            }
        }
    }
    return dp[W];
}
```
**无法使用贪心算法的解释**
0-1 
背包问题无法使用贪心算法来求解，也就是说不能按照先添加性价比最高的物品来达到最优，这是因为这种方式可能造成背包空间的浪费，从而无法达到最优。考虑下面的物品和一个容量为
 5 的背包，如果先添加物品 0 再添加物品 1，那么只能存放的价值为 16，浪费了大小为 2 的空间。最优的方式是存放物品 1 和物品 
2，价值为 22.

| id | w | v | v/w |
| --- | --- | --- | --- |
| 0 | 1 | 6 | 6 |
| 1 | 2 | 10 | 5 |
| 2 | 3 | 12 | 4 |

**变种**

- 完全背包：物品数量为无限个

- 多重背包：物品数量有限制

- 多维费用背包：物品不仅有重量，还有体积，同时考虑这两种限制

- 其它：物品之间相互约束或者依赖
## [416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum=0;
        for(int num:nums){
            sum+=num;
        }
        if(sum%2!=0){
            return false;
        }
        int W=sum>>1;
        int N=nums.size();
        dp[0] = true;
        for(int num:nums){
            for(int j=W;j>=num;j--){
                dp[j]=dp[j] || dp[j - num];
            }
        }
        return dp[W];
    }
private:
    int dp[10000];
};
```

