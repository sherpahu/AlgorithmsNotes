# PAT基础知识——STL之algorithm

# binary_search()
binary_search()  确定已经**排序**后的容器中是否存在某个元素
```cpp
#include <bits/stdc++.h> 
using namespace std;
int main(){
	vector<int> v = {10,1,5,2,4,11,19,3};
	sort(v.begin(),v.end());
	int i=binary_search(v.begin(),v.end(),10);
	cout<<i;
	int a[]={10,1,5,2,4,11,19,3};
	sort(a,a+8);
	i=binary_search(a,a+8,10);
	cout<<i;
	return 0;
}
```
# lower_bound()&upper_bound()
lower_bound() 从头到尾，查找第一个大于或者等于所列元素的值的位置
用法，参考下面的min_element()；
upper_bound()  从头到尾，查找第一个大于所列元素的值的位置
# make_heap()&pop_heap()&push_heap()&sort_heap()
make_heap( ) 创建一个堆并以序列的形式输出
pop_heap()   从一个堆中移除一个最大的元素
push_heap()   添加一个元素至堆
sort_heap()  将堆转变为有序序列
# max()&min()
max() 返回两个元素间的较大者
min() 返回两个元素中的较小者
# max_element()&min_element()
max_element() 返回序列中的最大值的索引(数组)或迭代器(容器)
min_element()  返回序列中的最小值的索引(数组)或迭代器(容器)
```cpp
#include <bits/stdc++.h> 
using namespace std;
int main(){
	int a[]={1,4,66,43,2,56,-1,443,23,234,4};
	int b= min_element(a,a+7)-a;
	cout<<a[b]<<endl;
	vector<int>v={1,4,66,43,2,56,-11,443,23,234,4};
	auto it=min_element(v.begin(),v.end());
	cout<<*it;
	return 0;
}
```
# sort()
升序排列
# swap()
交换两个元素
# unique()
unique()  移除连续的重复元素
注意连续两个字，也就是说明了要先进性排序操作
为什么呢？
因为unique函数并没有把重复的元素删掉，他只是把那些重复的元素移动到了本序列的末尾，返回值是不重复序列的位置指针。
所以如何显示不重复序列呢？
1.一种方法是直接将不重复序列指针 it 到 vector.end()
对此阶段进行 删除用erase()函数 erase(it,vector.end())
2.直接将vector.begin 到不重复序列尾的 位置指针 it 将元素输出来
即，
```cpp
vector<int>::iterator it=a.begin();

while(it!=  unique(a.begin(),a.end())) {
  cout<<*it<<" ";
  it++;
}

int main() {
	VECTOR_STRING::iterator iNameTor;
	iNameTor = unique(vecNames.begin(), vecNames.end());
	cout << "after unique(), contents are:" << endl;
	printVec(vecNames);
	
	cout << "unique return a iterator, point to the first Duplicate element " << endl;
	cout << iNameTor - vecNames.begin() << endl << endl;  	
	vecNames.erase(iNameTor, vecNames.end()); //删除重复元素
	cout << "after erase(), contents are:" << endl;
}
```

