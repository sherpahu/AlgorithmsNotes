# PAT基础知识——STL之queue&amp;priority_queue

# 定义
queue<typename> name;
先进先出的容器
# 遍历
没有begin(),end()迭代器，无法遍历。
只能通过front(),back()查看队首、队尾元素
# 用法
## 增
### push()
q.push(value);将value插入队尾
## 删
### pop()
q.pop();队首元素出队
## 查
### front()
s.front()获得队首元素值，即最早入队元素的值
使用前需要使用empty()判断队列是否为空，为空时会报错
### back()
s.back()获得队尾元素值，即最晚入队元素的值。
使用前需要使用empty()判断队列是否为空，为空时会报错
### empty()
判断队列是否为空
### size()
查询队列大小
# 用途
BFS常用
```cpp
void BFS(int s){
 	queue<Node> q;
    Node start;
    start.v=s;
    start.layer=0;
    q.push(start);
    inq[start.v]=true;
    while(!q.empty()){
     	Node topNode=q.front();
        q.pop();
        int u=topNode.v;
        for(int i=0;i<Adj[u].size();i++){
         	Node next=Adj[u][i];
            next.layer=topNode.layer+1;
            if(inq[next.v]==false){//尚未访问加入队列
             	q.push(next);
                inq[next.v]=true;//标记为已访问
            }
        }
    }
}
```

# 一言以蔽之
```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
	queue<int> q;
	q.push(1);
	q.push(2);
	q.push(3);
	q.push(4);
	cout<<"size " <<q.size()<<endl;
	cout<<"head "<<q.front()<<endl;
	cout<<"tail "<<q.back()<<endl;
	q.pop();
	cout<<"head "<<q.front()<<endl;
	cout<<"tail "<<q.back()<<endl;
	cout<<q.empty()<<endl;
	while(!q.empty()){
		cout<<"head "<<q.front()<<endl;
		cout<<"size "<<q.size()<<endl;
		q.pop();
	}
}
```
结果
```
size 4
head 1
tail 4
head 2
tail 4
0
head 2
size 3
head 3
size 2
head 4
size 1
```
# priority_queue()
与queue的操作函数。
默认为最大堆,即越大的越先pop
需要改变排序方式时，记住与sort()相反即可，greater<int>为小的优先，return a>b为从小到大
## 元素为int
使用greater<int>改为最小堆：
```cpp
priority_queue<int,vector<int>,greater<int> >q;
```
less<int>仍为最大堆
```cpp
priority_queue<int,vector<int>,less<int> >q;
```
## 其他数据结构
改写
```cpp
struct cmp()(const pair<int,int>&a,const pair<int,int>&b){
	return a.second<b.second;//从大到小
};//注意分号
priority<pair<int,int>,vector<pair<int,int> >,cmp>q;
```
## 一言以蔽之
## leetcode347
```cpp
struct cmp{
    bool operator()(const pair<int,int>&a,const pair<int,int>&b){
        return a.second<b.second;
    }
};
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int,int> m;
        for(auto num:nums){
            ++m[num];
        }
        priority_queue<pair<int,int>,vector<pair<int,int>>,cmp> q;
        for(auto iter = m.begin(); iter != m.end(); ++iter){
            q.push(*iter);
        }
        vector<int>ans;
        while(!q.empty()){
            if(ans.size()<k){
                ans.push_back(q.top().first);
                q.pop();
            }else{
                break;
            }
        }
        return ans;
    }
};
```

