# PAT基础知识——数学问题

# 最大公约数
a与b中公约数中最大的为最大公约数
辗转相除法的递归式：gcd(a,b)=gcd(b,a%b)
```cpp
int gcd(int a,int b){
    if(b==0)return a;
    else return gcd(b,a%b);
}
```
# 最小公倍数
最小公倍数为两数的乘积除以最大公约数。
```cpp
int lcm(int a, int b) {
    return a * b / gcd(a, b);
}
```

# 分数的表示和化简
## 分数的表示
写成假分数的形式，可以使用结构体或者pair来存储只有分子或者分母的情况
```cpp
struct Fraction{
    int up,down;
};
pair<int,int>fraction;
```

## 分数的化简
三项规定

- 分母down为负数则分子up和分母down都变为相反数
- 分子up为0，则down为1
- 约分：分子、分母同时除以分子、分母的_绝对值_最大公约数
```cpp
Fraction reduction(Fraction result){
    if(result.down<0){
        result.up=-result.up;
        result.down=-result.down;
    }
    if(result.up==0){
        result.down=1;
    }else{
        int d=gcd(abs(result.up),abs(result.down));
        result.up/=d;
        result.down/=d;
     }
     return result;
}
```
## 分数的加减乘除
![image.png](https://cdn.nlark.com/yuque/0/2020/png/705461/1580617293477-0d7e6b44-ce34-42d9-9db7-c88216bd3f56.png#align=left&display=inline&height=72&name=image.png&originHeight=144&originWidth=580&size=70324&status=done&style=none&width=290)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/705461/1580617353874-807436c3-2c0a-4f01-995a-e540ea24222c.png#align=left&display=inline&height=640&name=image.png&originHeight=1280&originWidth=960&size=957327&status=done&style=none&width=480)
乘除过程容易导致分子或者分母超int，需要使用long long存储

## 分数的输出
首先化简，然后分三类情况：

- 整数：分母为1，down==1
- 假分数：分子比分母大
- 真分数：分子比分母小
```cpp
void showResult(Fraction r){
    r=reduction(r);
    if(r.down==1)cout<<r.up<<endl;
    else if(r.up>r.down)printf("%d %d/%d",r.up/r.down,abs(r.up)%r.down,r.down);//注意abs
    else printf("%d/%d",r.up,r.down);
}
```
# 素数
## 素数判断
1不是素数

```cpp
bool isPrime(int n){
    if(n<=1)return false;
    else{
     	for(int i=2;i*i<=n;i++){
            if(n%i)return false;
        }
        return true;
    }
}
```
## 埃氏筛法
整数如果不是素数一定可以拆成几个素数的乘积。
埃氏筛法就是每遇到一个素数将其倍数全部表为合数。
求100内素数

```cpp
#include <bits/stdc++.h>
using namespace std;
int prime[100],cnt=0;
bool isPrime[110]={false};
void findPrime(int n){
    for(int i=2;i<=n;i++){
        if(isPrime[i]==false){
            prime[cnt++]=i;
            for(int j=i+i;j<=n;j+=i){
                isPrime[j]=true;
            }
        }
    }
}
int main(){
    findPrime(100);
    for(int i=0;i<cnt;i++){
        cout<<prime[i]<<" ";
    }
 	return 0;   
}
```
## [204. 计数质数](https://leetcode-cn.com/problems/count-primes/)
```cpp
class Solution {
public:
    int countPrimes(int n) {
        for(int i=2;i<n;i++){
            if(isPrime[i]==false){
                cnt++;
                for(int j=i+i;j<n;j+=i){
                    isPrime[j]=true;
                }
            }
        }
        return cnt;
    }
private:
    int cnt=0;
    bool isPrime[10000000]={false};
};
```
# 进制转换
## 1~36进制转为10进制
long int strtol(const char* nptr,char *endptr,int base)
返回long int型值，传入需要转换的字符串、该字符串中不能转换的部分可以存储的位置， **需要转换的字符串的进制** 
可能溢出，这种方法不行。
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    char num[20]="45671hello";
    char *rubbish;
    long int n=strtol(num,&rubbish,8);
    cout<<n;
}
```
注意&rubbish
不会溢出的16进制转化为10进制方法：
```cpp
#include <bits/stdc++.h>
using namespace std;
int num[100][100];
int main(){
	string s;
	cin>>s;
	int len=s.size();
	long long sum=0;
	for(int i=0;i<len;i++){
		if(s[i]>='A'&&s[i]<='F')
			sum=sum*16+s[i]-'A'+10;
		else sum=sum*16+s[i]-'0';
	}
	cout<<sum;
	return 0;	
}
```
## 10进制转换为1~36进制
```cpp
string intToA(int n,int radix)    //n是待转数字，radix是指定的进制
{
	string ans="";
	do{
		int t=n%radix;
		if(t>=0&&t<=9)	ans+=t+'0';
		else ans+=t-10+'a';
		n/=radix;
	}while(n!=0);	//使用do{}while（）以防止输入为0的情况
	reverse(ans.begin(),ans.end());
	return ans;	
}
```
使用do while是为了防止输入为0
## [504. 七进制数](https://leetcode-cn.com/problems/base-7/)
```cpp
class Solution {
public:
    string convertToBase7(int num) {
        bool isNegtive=false;
        if(num<0){
            num*=-1;
            isNegtive=true;
        }
        string ans="";
        do{
            int tmp=num%7;
            ans+='0'+tmp;
            num/=7;
        }while(num!=0);
        if(isNegtive)ans+='-';
        reverse(ans.begin(),ans.end());
        return ans;
    }
};
```
注意可能为负
## 十六进制转八进制
[http://lx.lanqiao.cn/problem.page?gpid=T51](http://lx.lanqiao.cn/problem.page?gpid=T51)
题解：[https://www.liuchuo.net/archives/1278](https://www.liuchuo.net/archives/1278)
先把16进制转化我2进制，再转8进制
```cpp
#include <iostream>
#include <map>
using namespace std;
int main() {
    string b, s, a;
    int n;
    cin >> n;
    string arr[16] = {"0000", "0001", "0010", "0011", "0100", "0101", "0110", "0111", "1000",
        "1001", "1010", "1011", "1100", "1101", "1110", "1111"};
    map<string, string> m;
    m["000"] = "0"; m["001"] = "1"; m["010"] = "2"; m["011"] = "3"; m["100"] = "4";
    m["101"] = "5"; m["110"] = "6"; m["111"] = "7";
    for(int i = 0; i < n; i++) {
        cin >> s;
        int lens = s.length();
        for(int j = 0; j < lens; j++) {
            if(s[j] > '9') {
                b += arr[s[j] - 'A' + 10];
            } else {
                b += arr[s[j] - '0'];
            }
        }
        int lenb = b.length();
        if(lenb % 3 == 1) {
            b = "00" + b;
        } else if(lenb % 3 == 2) {
            b = "0" + b;
        }
        int flag = 0;
        for(int j = 0; j < lenb; j += 3) {
            string temp = b.substr(j, 3);
            string t = m[temp];
            if(j == 0 && t == "0") {
                flag = 1;
                continue;
            }
            if(flag == 1 && j == 3 && t == "0") {
                continue;
            }
            cout << t;
        }
        cout << endl;
        b = "";
    }
    return 0;
}
```

# 统计阶乘尾部0的个数
## [172. 阶乘后的零](https://leetcode-cn.com/problems/factorial-trailing-zeroes/)
尾部的0是由2*5产生的，因子中2和5的个数的较大值为最终结果，而2的个数远多于5，所以只用计算5即可
但是计算1～n的这n个数会超时。
可以观察到5是没5个数出现一次，每5^2会出现两次
所以5的个数为n/5+n/(5^2)+n/(5^3)+……
[https://leetcode-cn.com/problems/factorial-trailing-zeroes/solution/xiang-xi-tong-su-de-si-lu-fen-xi-by-windliang-3/](https://leetcode-cn.com/problems/factorial-trailing-zeroes/solution/xiang-xi-tong-su-de-si-lu-fen-xi-by-windliang-3/)
```cpp
class Solution {
public:
    int trailingZeroes(int n) {
        return n==0?0:n/5+trailingZeroes(n/5);
    }
};
```
## 统计N!二进制表示中最低位1的位置
就是统计因子中2的个数
n/2+n/(2^2)+n/(2^3)+......
# [462. 最少移动次数使数组元素相等 II](https://leetcode-cn.com/problems/minimum-moves-to-equal-array-elements-ii/)
这题不用想什么中位数：设 a <= x <= b，将 a 和 b 都变化成 x 为最终目的，则需要步数为 x-a+b-x = b-a，即两个数最后相等的话步数一定是他们的差，x 在 a 和 b 间任意取；
所以最后剩的其实就是中位数；
```cpp
class Solution {
public:
    int minMoves2(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int move = 0;
        int l = 0, h = nums.size() - 1;
        while (l < h) {
            move += nums[h] - nums[l];
            l++;
            h--;
        }
        return move;
    }
};
```
# 多数投票问题
## [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)
```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int,int>m;
        int max=0,ans=-1;
        for(int num:nums){
            m[num]++;
            int tmp=m[num];
            if(tmp>max){
                max=tmp;
                ans=num;
            }
        }
        return ans;
    }
};
```
# 大整数运算
[https://blog.csdn.net/hacker00011000/article/details/51298294](https://blog.csdn.net/hacker00011000/article/details/51298294)
[https://blog.csdn.net/nk_test/article/details/48912763](https://blog.csdn.net/nk_test/article/details/48912763)
## 大整数存储
```cpp
struct bign{
 	int d[1000];
    int len;
}
```
**整数的高位存储在数组的高位，整数的低位存储在数组的低位** 
### 读入
需要反转存储
```cpp
bign read(char str[]){
 	bign b;
    b.len=strlen(str);
    for(int i=0;i<b.len;i--){
     	b.d[i]=str[b.len-i-1]-'0';   
    }
    return b;
}
```
### 比较
先比较长度，再从后往前（从高位到低位）比较
```cpp
int compare(bign a,bign b){
 	if(a.len>b.len)return 1;
    else if(a.len<b.len)return -1;
    else{
     	for(int i=0;i<a.len;i++){
         	if(a.d[i]>b.d[i])return 1;
            else if(a.d[i]<b.d[i])return -1;
        }
        return 0;
    }
}
```

## 高精度加法
```cpp
bign add(bign a,bign b){
 	bign c;
    int carry=0;//进位
    for(int i=0;i<a.len||i<b.len;i++){
     	int tmp=a[i]+b[i]+carry;
        c.d[c.len++]=tmp%10;
        carry=tmp/10;
    }
    if(carry!=0){
     c.d[c.len++]=carry;   
    }
    return c;
}
```
## 高精度减法
```cpp
bign sub(bign a,bign b){
 	bign c;
    int carry=0;
    for(int i=0;i<a.len||i<b.len;i++){
     	if(a.d[i]<b.d[i]){
         	a.d[i+1]--;
            a.d[i]+=10;
        }
        c.d[c.len++]=a.d[i]-b.d[i];
    }
    while(c.len-1>=1&&c.len[c.len-1]==0){//去除最高位的0，但至少保留一位
     	c.len--;   
    }
    return 0; 
}
```

