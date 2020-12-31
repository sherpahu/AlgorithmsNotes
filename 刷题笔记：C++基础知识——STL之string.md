# PAT基础知识——STL之string

append,insert,erase,replace,size,compare,empty,at,substr,find
# 定义
string str = “content”; equals string str (“content”);
# 访问
## 通过下标访问
类似与数组
```cpp
string str="abc";
for(int i=0;i<str.size();i++){
	cout<<str[i]<<endl;   
}
```
## 通过迭代器访问
```cpp
for(string<int>::iterator it=str.begin();it!=str.end();it++){
 	cout<<*it<<endl;   
}
```
类似于vector，string可以对迭代器直接加减某数字
# 转化
## 转化为char[]数组
string可以转化为char数组
用于atoi()或者printf等位置

```cpp
str.c_str();
```
## 转化为int,long,float
**stoi(int), stol(long), stof(float), stod(double)**
```cpp
string str="123";
int tmp=stoi(str);
```
## 转化为小写或大写
### 字符
tolower('A');得到ascii码
(char)tolower('A');得到字符
toupper类似
### char字符串
strlwr strupr 函数
strlwr
函数原型: char *strlwr（char *a）
函数功能：将字符串a转换为小写形式
函数返回: 返回指向s参数的指针
所属文件：#include <string.h>
strupr
函数原型：char *strupr（char *a）
函数功能：把字符串a中的串转换成大写
函数返回: 返回指向s参数的指针
所属文件：#include <string.h>
```cpp
#include <iostream>
#include <string.h>
using namespace std;
int main(){ 
    char s[]="jfWxs"; 
    char *p= strupr(s);
    cout<<p<<endl;//JFWXS
    p = strlwr(s);
    cout<<p<<endl;//jfwxs 
}
```
### string
transform和tolower及toupper进行结合
transform(first1,last1,first2,result,binary_op);//first1是第一个容器的首迭代器，last1为第一个容器的末迭代器，first2为第二个容器的首迭代器，result为存放结果的容器，binary_op为要进行操作的二元函数对象或sturct、class
```cpp
#include <iostream>
#include <algorithm>
using namespace std;
int main(){
    string str = "JFwxs";
    transform(str.begin(),str.end(),str.begin(),::toupper);
    cout<<str<<endl;//JFWXS
    transform(str.begin(),str.end(),str.begin(),::tolower);
    cout<<str<<endl;//jfwxs
}
```
# 复制
string str2(str1);
# 用法
## 增

- 尾部添加：string.append(""); .append(a,index0,string::npos);//把字符串a的首字母到尾字母加上
- operator+=：直接拼接str3=str1+str2;str3+=str1;
## 删
时间复杂度均为O(n)

- 删除单个元素：string.erase(it);//删除元素位置的迭代器。时间复杂度为O(n)

如果写索引的话会讲后面全部截断。
此处与vector不同，vector不能传入下标进行删除

- 删除区间：时间复杂度均为O(n)
   - string.erase(it_start,it_end)//删除从[it_start,it_end)的元素，迭代器
   - string.erase(pos,length)//pos起始索引，length删除的长度省略就是全删
- 清空：string.clear();时间复杂度均为O(1)
## 改
通过下标进行修改时务必保证此下标对应的位置已经初始化，否则没有分配空间就无法赋值。
```cpp
string s="";
s[2]='a';
```

- 中间插入：
   - string.insert(pos,string);
```cpp
str.insert(2,"content");
```

   - string.insert(it,it2,it3)l//it为原字符串想要插入的位置，it2,it3为待插入字符串首尾迭代器的位置(it3为待插入字符串的下一个地址)
- replace：
   - str.replace(pos,len,str2)//将str从pos位置开始长度为len的子串替换为str2
   - str.replace(it1,it2,str2)//将str从it1迭代器到it2迭代器的子串替换为str2
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    string str1="Maybe you will fail.";
    string str2="will not";
    string str3="Surely";
    cout<<str1.replace(10,4,str2)<<endl;
    cout<<str1.replace(str1.begin(),str1.begin()+5,str3)<<endl;;
 	return 0;   
}
```
结果
```
Maybe you will not fail.
Surely you will not fail.
```
## 查
### find
str.find(str2)当str2为str的子串时返回其在str中第一次出现的位置，如果str2不是str的子串，返回string::npos

find_first_of      查找包含子串中的任何字符，返回第一个位置
find_first_not_of      查找不包含子串中的任何字符，返回第一个位置
find_last_of      查找包含子串中的任何字符，返回最后一个位置
find_last_not_of      查找不包含子串中的任何字符，返回最后一个位置

   - 用法1：str.find(str2)

若str2是str的子串，则返回str2在str中第一次出现的位置
若str2不是str的子串，则返回string::npos

   - 用法2：str.find(str2,pos)，从str的pos位置处开始匹配str2
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
	string str="Thank you for your smile";
    string str2="you";
    string str3="me";
    if(str.find(str2)!=string::npos){
     	cout<<str.find(str2)<<endl;   
    }
    if(str.find(str2,7)!=string::npos){
     	cout<<str.find(str2,7)<<endl;   
    }
    if(str.find(str3)==string::npos){
     	cout<<str3<<" is not the substr of "<<str<<endl; 
    }
}
```
结果
```
6
14
me is not the substr of Thank you for your smile
```
### string的判空 empty()
string.empty();
### string的比较
#### compare()
string.compare(pos,len,str2,pos2,len2);
#### 直接使用>、<、==、>=、<=、!=
```cpp
#include <iostream>
#include <string>
using namespace std;
void TrueOrFalse (int x)
{
    cout << (x?"True":"False")<<endl;
}

int main ()
{
    string S1 = "DEF";
    string CP1 = "ABC";
    string CP2 = "DEF";
    string CP3 = "DEFG";
    string CP4 ="def";
    cout << "S1= " << S1 << endl;
    cout << "CP1 = " << CP1 <<endl;
    cout << "CP2 = " << CP2 <<endl;
    cout << "CP3 = " << CP3 <<endl;
    cout << "CP4 = " << CP4 <<endl;
    cout << "S1 <= CP1 returned ";
    TrueOrFalse (S1 <=CP1);
    cout << "S1 <= CP2 returned ";
    TrueOrFalse (S1 <= CP2);
    cout << "S1 <= CP3 returned ";
    TrueOrFalse (S1 <= CP3);
    cout << "CP1 <= S1 returned ";
    TrueOrFalse (CP1 <= S1);
    cout << "CP2 <= S1 returned ";
    TrueOrFalse (CP2 <= S1);
    cout << "CP4 <= S1 returned ";
    TrueOrFalse (CP4 <= S1);
    cin.get();
    return 0;
}
```
### string字符个数/长度 size(),length()
size()或者length()，O(1)
string.size() equals string.length(); //常用于循环
for(int 1=0; i<string.size(); i++){}
### string的提取 substr()
string[index] equals string.at(index); 单个提取
string.substr(); 返回全部内容
string.substr(pos,len);//很重要
**由索引直接得到子串**
**string.substr(start,end-start+1);**

