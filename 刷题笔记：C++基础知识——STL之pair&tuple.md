# PAT基础知识——STL之pair&amp;tuple

# pair
## 定义
pair<typename1,typename2> name;
## 用法
### 初始化：

- 使用大括号
```cpp
#include <bits/stdc++.h>
using namespace std;

int main()
{
   pair<int,int> p;
	p={1,2};
	cout<<p.first<<" "<<p.second<<endl;
   return 0;
}
```

- 直接对first，second赋值
```cpp
#include <bits/stdc++.h>
using namespace std;

int main()
{
   pair<int,int> p;
	p.first=1;
	p.second=1;
	cout<<p.first<<" "<<p.second<<endl;
   return 0;
}
```
### 查询
p.first,p.second
## 用途

- 替换二元结构体，初始化、获取元素值更方便
- 用于map的insert
```cpp
#include <bits/stdc++.h>
using namespace std;

int main()
{
   map<string,int> m;
	m.insert(make_pair("hello",0));
	m.insert(pair<string,int>("world",1));
	for(map<string,int>::iterator it=m.begin();it!=m.end();it++){
		cout<<it->first<<" "<<it->second<<endl;	
	}
   return 0;
}
```
# tuple
C++11
## 定义
pair<int,int,int> p;
## 用法
### 初始化
括号，类似于对象的初始化
```cpp
std::tuple mytuple(10, 'a', 3.14);
```
大括号
```cpp
queue<tuple<int,int,int> >q;
q.push({0,0,1});
```
### 查询
get<0>(myTuple);get<1>(myTuple);get<2>(myTuple);
## 用途
充当三元结构体
## 一言以蔽之
leetcode1091
```cpp
int X[]={1,-1,0,0,1,-1,1,-1};
int Y[]={0,0,1,-1,1,1,-1,-1};
class Solution {
public:
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        int lenx=grid.size(),leny=grid[0].size();
        if(grid[0][0] || grid[grid.size()-1][grid.size()-1]) return -1;
        int x=0,y=0;
        queue<tuple<int,int,int> >q;//x,y,step
        q.push({0,0,1});
        
        while(!q.empty()){
            auto now=q.front();
            if(get<0>(now)==lenx-1&&get<1>(now)==leny-1)return get<2>(now);
            q.pop();
            grid[get<0>(now)][get<1>(now)]=1;
            for(int i=0;i<8;i++){
                int newx=get<0>(now)+X[i];
                int newy=get<1>(now)+Y[i];
                if(newx<0||newx>=lenx||newy<0||newy>=leny||grid[newx][newy])
                    continue;
                grid[newx][newy]=1;
                q.push({newx,newy,get<2>(now)+1});
            }
        }
        return -1;
    }
};
```

