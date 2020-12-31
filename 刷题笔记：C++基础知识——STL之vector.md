# PAT基础知识——STL之vector

vector为变长数组
# vector的定义
## 一维
```cpp
vector<int> tmp1;
typedef pair<int,int> PII;
vector<PII> tmp2;
vector<node> tmp3;
```
初始化：

- 只传如初始化size,但每个元素值为默认值
```cpp
vector<int> tmp1(10);//初始化10个元素,但每个元素值为默认值0
```

- 初始化size,并且设置初始值
```cpp
vector<int> tmp1(10,-1);//初始化10个元素，并且值为-1
```

- 借助数组初始化

vector<int> v(a,a+10);
```cpp
#include <bits/stdc++.h>
using namespace std;
bool cmp(const int &a,const int &b){
	return a>b;	
}
int main()
{
   int a[20]={1,4,2,7,4,5,9,3,7,0};
	vector<int> v(a,a+10);
	//sort(a.begin(),a.end());
	for(int i=0;i<10;i++){
		cout<<v[i]<<" ";	
	}
	cout<<endl;
   return 0;
}
```
不能直接使用vector<int> v={1,2,3};来初始化vector

- 借助迭代器初始化

map<int,int> m;

vector<pair<int,int> >v(m.begin(),m.end());
这样初始化就不用单独使用for循环再赋值了

## 二维
```cpp
vector<vector<int> > tmp4;//方法一，二维都是不定长的
vector<int> tmp5[arraySize];//方法二，一维的长度已经固定，另一维才是变长的
```
二维数组的赋值
```cpp
vector<vector<string> > v;
for(int i = 0; i < 3; i++) {
    string s;
    getline(cin, s);
    vector<string> row;
    int j = 0, k = 0;
    while(j < s.length()) {
        if(s[j] == '[') {
           while(k++ < s.length()) {
               if(s[k] == ']') {
                   row.push_back(s.substr(j+1, k-j-1));
                   break;
               }
           }
        }
        j++;
    }
    v.push_back(row);
}
```


## 提前占位的方法
v.resize(n);调整容器的大小，使其包含 n（参数）个元素
```cpp
vector<int> v;
cin>>n;
v.resize(n);
```



不提前占位的话，直接赋值会报错。

# vector的遍历
## 下标访问
```cpp
for(int i=0;i<v.size();i++){
    cout<<v[i]<<endl;
}
```
## 迭代器访问
迭代器可以理解为类似于指针的东西，通过*it来访问元素的值

```cpp
for(vector<int>::iterator it=v.begin();it!=v.end();it++){
 	cout<<*it;   
}
```
v[i]与*(v.begin()+i)是等价的
## 范围for循环遍历
auto得到的是值，不是指针，不需要*

```cpp
for(auto v:V){
 	cout<<v<<endl;   
}
```


# 用法
## 增
### push_back()尾部添加
```cpp
for(int i=0;i<10;i++){
 	v.push_back(i);
}
```
### insert()向任意迭代器处插入值
insert(it,value)
```cpp
v.insert(v.begin()+2,-1);//向v[2]处插入值-1
```
## 删
### erase()
传入一个迭代器就删除单个元素erase(v.begin()+2)删除v[2]
传入两个迭代器就删除一个区间[first,second)的元素erase(v.begin()+2,v.begin()+5);删除[2,5)的元素v2],v[3],
v[4]
```cpp
v.erase(v.begin()+2);
v.erase(v.begin()+2,v.begin()+5);
```
### clear()
清空所有元素
v.clear();
## 改

- 下标访问，直接修改
```cpp
v[5]=1234;
```

- 迭代器访问，修改
```cpp
*(v.begin()+5)=10000000;
```
## 查

- 遍历获得所有元素：见之前的
### size()
size()查得数组长度，O(1)
```cpp
cout<<v.size()<<endl;
printf("%d",v.size());
```
### find()
find()查找数组元素的位置，没有找到就返回v.end()
```cpp
vector<int>::iterator it=find(v.begin(),v.end(),val);
if(it!=v.end()){
    *(it)=1234;
 	cout<<"found"<<endl;   
}else{
    cout<<"not found"<<endl;
}
```
# 排序sort()
默认升序,由小到大
```cpp
#include <algorithm>
sort(vector.begin(),vector.end());
```
通过自定义排序函数可以改变排序方式


```cpp
bool cmp(const int &a,const int &b){
 	return a>b;//>是由大到小排列   
}
sort(vector.begin(),vector.end(),cmp);
```
# 翻转reverse()
reverse()

```cpp
reverse(v.begin(),v.end());
```


