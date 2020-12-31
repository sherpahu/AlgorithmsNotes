# PAT基础知识——STL之map

# 定义
map<typename1,typename2> m;
```cpp
map<int,string> m;
```
char数组无法作为key，所以想用字符串作为key只能使用string

为避免超时应该尽量使用unordered_map代替map
```cpp
#include<tr1/unordered_map>//注意写法
using namespace std;
using namespace std::tr1;//注意写法
```
# 访问
## 通过key访问
可以通过m[key]的方式直接获得key对应的value值

```cpp
map<char,int> m;
m['c']=10;
m['c']=20;//原本的值被覆盖
```
## 迭代器遍历
it->first访问key,it->second访问value

```cpp
for(map<char,int>::it=m.begin();it!=m.end();it++){
 	cout<<it->first<<" " <<it->second<<endl;
}
```
## 范围for循环遍历
不是指针，it.first得到key,it.second得到value
```cpp
for(auto it:m){
 	cout<<it.first<<" "<<it.second<<endl;   
}
```
# 用法
## 增

- 通过key索引直接插入

直接新增key和value，若key已经存在则覆盖掉
```cpp
m['d']=10000;
```

- insert()插入
   - 一种方式通过pair
   - 另外一种方式通过value_type
```cpp
m.insert(pair<char,int>('e',2000));
m.insert(map<char,int>::value_type('f',3000'));
```
汇总

```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
	map<char,int> m;
	m['a']=1000;
	cout<<m['a']<<endl;
	cout<<"------------------"<<endl;
	m['a']=2000;
	cout<<m['a']<<endl;
	cout<<"------------------"<<endl;
	m['b']=3000;
	for(map<char,int>::iterator it=m.begin();it!=m.end();it++){
		cout<<it->first<<" "<<it->second<<endl;
	}
	cout<<"------------------"<<endl;
	m.insert(pair<char,int>('c',4000));
	m.insert(map<char,int>::value_type('d',5000));
	for(map<char,int>::iterator it=m.begin();it!=m.end();it++){
		cout<<it->first<<" "<<it->second<<endl;
	}
	cout<<"------------------"<<endl;
}
```
## 删
### erase()

- 删除单个元素——key或者迭代器
   - 通过key删除

```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
 	map<char,int> m;
    m['a']=1;
    m['b']=2;
    m['c']=3;
    m['d']=4;
    for(map<char,int>::iterator it=m.begin();it!=m.end();it++){
		cout<<it->first<<" "<<it->second<<endl;
	}
	cout<<"------------------"<<endl;
    m.erase('c');
    for(map<char,int>::iterator it=m.begin();it!=m.end();it++){
		cout<<it->first<<" "<<it->second<<endl;
	}
	cout<<"------------------"<<endl;
}
```
结果

```
a 1
b 2
c 3
d 4
------------------
a 1
b 2
d 4
------------------
```

   - 通过迭代器
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    map<char,int> m;
    m['a']=1;
    m['b']=2;
    m['c']=3;
    m['d']=4;
    for(map<char,int>::iterator it=m.begin();it!=m.end();it++){
		cout<<it->first<<" "<<it->second<<endl;
	}
	cout<<"------------------"<<endl;
    map<char,int>::iterator it=m.find('c');
    m.erase(it);
    for(map<char,int>::iterator it=m.begin();it!=m.end();it++){
		cout<<it->first<<" "<<it->second<<endl;
	}
	cout<<"------------------"<<endl;
}
```
结果

```
a 1
b 2
c 3
d 4
------------------
a 1
b 2
d 4
------------------
```

- 删除区间

利用迭代器，map.erase(it_begin,it_end)删除[it_begin,it_end)的内容
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    map<char,int> m;
    m['a']=1;
    m['b']=2;
    m['c']=3;
    m['d']=4;
    for(map<char,int>::iterator it=m.begin();it!=m.end();it++){
		cout<<it->first<<" "<<it->second<<endl;
	}
	cout<<"------------------"<<endl;
    map<char,int>::iterator it=m.find('c');
    m.erase(it,m.end());
    for(map<char,int>::iterator it=m.begin();it!=m.end();it++){
		cout<<it->first<<" "<<it->second<<endl;
	}
	cout<<"------------------"<<endl;
}
```
结果

```
a 1
b 2
c 3
d 4
------------------
a 1
b 2
------------------
```
### clear()
清空所有元素，复杂度为O(n)
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    map<char,int> m;
    m['a']=1;
    m['b']=2;
    cout<<m.size()<<endl;
    m.clear();
    cout<<m.size()<<endl;
 	return 0;   
}
```
## 改

- 直接使用key修改
```cpp
m['a']=1234;
```
若value用于计算key出现次数，需要递增则直接使用m['a']++，不用先查找是否存在。


- 使用迭代器修改

修改it->second

由于find只能根据key来查找，所有这个例子是绕了一大圈，本身没有意义。
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    map<char,int> m;
    m['a']=1;
    m['b']=2;
    m['c']=3;
    m['d']=4;
    for(map<char,int>::iterator it=m.begin();it!=m.end();it++){
		cout<<it->first<<" "<<it->second<<endl;
	}
	cout<<"------------------"<<endl;
    map<char,int>::iterator it=m.find('c');
    it->second=1000;
    for(map<char,int>::iterator it=m.begin();it!=m.end();it++){
		cout<<it->first<<" "<<it->second<<endl;
	}
	cout<<"------------------"<<endl;
}
```
## 查

- find()通过key查找对应的迭代器, O(logN)

```cpp
map<char,int>::iterator it=m.find('a');
cout<<it->first<<" "<<it->second<<endl;
```

- size()查看map的大小
```cpp
m.size();
```
# 用途
模拟题常用
# 另外
map的键值一一对应，而一个key对应多个value可以使用multimap
C++11中有unordered_map，用散列代替了map中的红黑树，可以只映射不排序，速度比map快，当map超时时可以换用unordered_map
尽量使用unordered_map代替map
蓝桥杯中的用法：
```cpp
#include<tr1/unordered_map>//注意写法
using namespace std;
using namespace std::tr1;//注意写法
```
# 典型题
[PAT甲级1053](https://pintia.cn/problem-sets/994805342720868352/problems/1071785190929788928)
```cpp
#include <bits/stdc++.h>
using namespace std;
struct node{
	string t;
	int score;
};
bool cmp(const node &a,const node &b){
	return a.score!=b.score?a.score>b.score:a.t<b.t;
}
int main(){
	ios::sync_with_stdio(false);
	int n,k,num;
	string s;
	cin>>n>>k;
	vector<node> v(n);
	for(int i=0;i<n;i++){
		cin>>v[i].t>>v[i].score;
	}
	for(int i=1;i<=k;i++){
		cin>>num>>s;
		printf("Case %d: %d %s\n",i,num,s.c_str());
		vector<node> ans;
		int cnt=0,sum=0;
		if(num==1){
			for(int j=0;j<n;j++){
				if(v[j].t[0]==s[0])
					ans.push_back(v[j]);
			}
		}else if(num==2){
			for(int j=0;j<n;j++){
				if(v[j].t.substr(1,3)==s){
					cnt++;
					sum+=v[j].score;
				}
			}
			if(cnt!=0){
				printf("%d %d\n",cnt,sum);
			}
		}else if(num==3){
			unordered_map<string,int> m;
			for(int j=0;j<n;j++){
				if(v[j].t.substr(4,6)==s)
					m[v[j].t.substr(1,3)]++;
			}
			for(auto it:m)
				ans.push_back({it.first,it.second});
		}
		sort(ans.begin(),ans.end(),cmp);
		for(int j=0;j<ans.size();j++){
			printf("%s %d\n",ans[j].t.c_str(),ans[j].score);
		}
		if (((num == 1 || num == 3) && ans.size() == 0) || (num == 2 && cnt ==0)) 
			printf("NA\n");
	}
	return 0;
}
```

[PAT乙级1069](https://pintia.cn/problem-sets/994805260223102976/problems/994805265159798784)

# 一言以蔽之
```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;
int main() {
	map<string, int> m; // 定义⼀一个空的map m，键是string类型的，值是int类型的
	m["hello"] = 2; // 将key为"hello", value为2的键值对(key-value)存⼊入map中
	cout << m["hello"] << endl; // 访问map中key为"hello"的value, 如果key不不存在，则返回0
	cout << m["world"] << endl;
	m["world"] = 3; // 将"world"键对应的值修改为3
	m[","] = 1; // 设⽴立⼀一组键值对，键为"," 值为1
	// ⽤用迭代器器遍历，输出map中所有的元素，键⽤用it->first获取，值⽤用it->second获取
	for (auto it = m.begin(); it != m.end(); it++) {
		cout << it->first << " " << it->second << endl;
	}
	// 访问map的第⼀一个元素，输出它的键和值
	cout << m.begin()->first << " " << m.begin()->second << endl;
	// 访问map的最后⼀一个元素，输出它的键和值
	cout << m.rbegin()->first << " " << m.rbegin()->second << endl;
	// 输出map的元素个数
	cout << m.size() << endl;
	return 0;
}
```
结果
```
2
0
, 1
hello 2
world 3
, 1
world 3
3
```

