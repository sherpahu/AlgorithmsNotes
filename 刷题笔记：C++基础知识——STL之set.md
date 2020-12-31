# PAT基础知识——STL之set

# 定义
set<typename> name;
# 遍历
还是直接用迭代器或者范围for循环遍历，得到的顺序是升序的（存在红黑树中）

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
	set<int> s;
	s.insert(1);
	s.insert(2);
	s.insert(3);
	s.insert(4);
	for(set<int>::iterator it=s.begin();it!=s.end();it++){
		cout<<*it<<" ";
	}
	cout<<endl<<"------------------"<<endl;
	for(auto it=s.begin();it!=s.end();it++){//C++11
		cout<<*it<<" ";
	}
	cout<<endl<<"------------------"<<endl;
	for(auto it:s){//C++11
		cout<<it<<" ";
	}
    cout<<endl<<"------------------"<<endl;
	for(int it:s){
		cout<<it<<" ";
	}
}
```
结果
```
1 2 3 4
------------------
1 2 3 4
------------------
1 2 3 4
```

# 用法
## 增

- insert()

由于set是使用红黑树实现的，有序，所以不使用类似于vector的push_back()来插入，而是使用insert。（同理map也可以使用insert一个pair或者value_type来增加元素）
时间复杂度O(logn)
```cpp
set<int> s;
s.insert(1);
s.insert(2);
s.insert(3);
s.insert(4);
```
## 删
### erase()

- 通过迭代器删除单个元素,时间复杂度O(1)

也可以结合find()来用(这样的话与下面直接删除value的方法一致)
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
	set<int> s;
	s.insert(1);
	s.insert(2);
	s.insert(3);
	s.insert(4);
	for(set<int>::iterator it=s.begin();it!=s.end();it++){
		cout<<*it<<endl;
	}
	cout<<"----------------------"<<endl;
	s.erase(s.begin());
	for(set<int>::iterator it=s.begin();it!=s.end();it++){
		cout<<*it<<endl;
	}
	cout<<"----------------------"<<endl;
	s.erase(s.find(3));
	for(set<int>::iterator it=s.begin();it!=s.end();it++){
		cout<<*it<<endl;
	}
	cout<<"----------------------"<<endl;
}
```
结果
```
1
2
3
4
----------------------
2
3
4
----------------------
2
4
----------------------
```

- 通过value删除单个元素

时间复杂度O(logn)
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
	set<int> s;
	s.insert(1);
	s.insert(2);
	s.insert(3);
	s.insert(4);
	for(set<int>::iterator it=s.begin();it!=s.end();it++){
		cout<<*it<<endl;
	}
	cout<<"----------------------"<<endl;
	s.erase(2);
	for(set<int>::iterator it=s.begin();it!=s.end();it++){
		cout<<*it<<endl;
	}
	cout<<"----------------------"<<endl;
}
```
结果
```
1
2
3
4
----------------------
1
3
4
----------------------
```

- 通过迭代器删除一个区间

s.erase(it_begin,it_end)删除[it_begin,it_end)的元素
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
	set<int> s;
	s.insert(1);
	s.insert(5);
	s.insert(3);
	s.insert(4);
	for(set<int>::iterator it=s.begin();it!=s.end();it++){
		cout<<*it<<endl;
	}
	cout<<"----------------------"<<endl;
	set<int>::iterator it3=s.find(3);
	s.erase(it3,s.end());
	for(set<int>::iterator it=s.begin();it!=s.end();it++){
		cout<<*it<<endl;
	}
	cout<<"----------------------"<<endl;
}
```
结果

```
1
3
4
5
----------------------
1
----------------------
```
由于set是有序的通过find(3)可以找到3的迭代器，erase(it3,s.end())可以删除3以及比3大的值

### clear()
清空
s.clear()
## 改
直接erase删除重新insert添加就行了
## 查
### find()
s.find(value)返回值等于value的元素的迭代器，如果没找到就返回s.end()
时间复杂度O(logN)
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
	set<int> s;
	s.insert(1);
	s.insert(5);
	s.insert(3);
	s.insert(4);
	for(set<int>::iterator it=s.begin();it!=s.end();it++){
		cout<<*it<<endl;
	}
	cout<<"----------------------"<<endl;
	set<int>::iterator it3=s.find(3);
	s.erase(it3,s.end());
	cout<<*it3<<endl;
	cout<<"----------------------"<<endl;
}
```
结果

```
1
3
4
5
----------------------
3
----------------------
```
### size()
查看set中元素个数,时间复杂度O(1)
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
	set<int> s;
	s.insert(1);
	s.insert(5);
	s.insert(3);
	s.insert(4);
	for(set<int>::iterator it=s.begin();it!=s.end();it++){
		cout<<*it<<endl;
	}
	cout<<"----------------------"<<endl;
	cout<<s.size()<<endl;
	cout<<"----------------------"<<endl;
}
```
结果

```
1
3
4
5
----------------------
4
----------------------
```
### empty()
判断set是否为空，空返回1，非空返回0
```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
	set<int> s;
	cout<<s.empty()<<endl;
	cout<<"------"<<endl;
	s.insert(1);
	cout<<s.empty()<<endl;
}
```
结果

```
1
------
0
```

# 一言以蔽之
```cpp
#include <iostream>
#include <set>
using namespace std;
int main(){
	set<int>s;//定义集合s
	s.insert(1);//用insert插入元素
	cout<<*(s.begin())<<endl;//输出第一个元素，其中*为取值
	for(int i=0;i<6;i++){
		s.insert(i);//向集合中插入i
	}
	for(auto it=s.begin();it!=s.end();it++){ //用迭代器遍历
		cout <<*it<<" ";
	}
	cout<<endl<<"----------------"<<endl;
	for(auto it:s){ //用迭代器遍历
		cout <<it<<" ";
	}
	cout<<endl<<(s.find(2)!=s.end())<<endl;//find查找s中的值，找到返回对应迭代器，没找到返回s.end()
	cout<<(s.find(10)!=s.end())<< endl;//s.find(10)!=s.end()表示能够找到
	s.erase(1);//删除集合s中的元素1
	cout<<(s.find(1)!=s.end())<<endl;//此时元素1就无法找到了
	return 0;
}
```

```
1
0 1 2 3 4 5
----------------
0 1 2 3 4 5
1
0
0
```

