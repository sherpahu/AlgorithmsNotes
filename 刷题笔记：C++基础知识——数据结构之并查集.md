# PAT基础知识——数据结构之并查集

# 并查集
并查集有两个操作：


- 合并：将不同集合的根节点中一个节点的父节点赋值为另一个节点
- 查找：判断两个元素是否在同一个集合中（查找根节点是否相同）4
## 模板
```cpp
(1)朴素并查集：

    int p[N]; //存储每个点的祖宗节点

    // 返回x的祖宗节点
    int find(int x)
    {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i ++ ) p[i] = i;

    // 合并a和b所在的两个集合：
    p[find(a)] = find(b);


(2)维护size的并查集：

    int p[N], size[N];
    //p[]存储每个点的祖宗节点, size[]只有祖宗节点的有意义，表示祖宗节点所在集合中的点的数量

    // 返回x的祖宗节点
    int find(int x)
    {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i ++ )
    {
        p[i] = i;
        size[i] = 1;
    }

    // 合并a和b所在的两个集合：
    size[find(b)] += size[find(a)];
    p[find(a)] = find(b);


(3)维护到祖宗节点距离的并查集：

    int p[N], d[N];
    //p[]存储每个点的祖宗节点, d[x]存储x到p[x]的距离

    // 返回x的祖宗节点
    int find(int x)
    {
        if (p[x] != x)
        {
            int u = find(p[x]);
            d[x] += d[p[x]];
            p[x] = u;
        }
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i ++ )
    {
        p[i] = i;
        d[I] = 0;
    }

    // 合并a和b所在的两个集合：
    p[find(a)] = find(b);
    d[find(a)] = distance; // 根据具体问题，初始化find(a)的偏移量
```

## 初始化
```cpp
void init(int x){
 	for(int x:X){//遍历所有x
     	father[x]=x;
        isRoot[x]=false;
    }
}
```
## 查找根节点
```cpp
//普通
int findFather(int x){
	while(x!=father[x]){
		x=father[x];
	}
	return x;
}
//路径压缩,均摊为O(1)
int findFather(int x){
    int a=x;
	while(x!=father[x]){
		x=father[x];
	}
    //此时x为根节点，下面将所有节点的父节点都改为
    while(a!=father[a]){
        int tmp=a;//防止a被覆盖
        a=father[a];
        father[tmp]=x;
    }
	return x;
}
```
## 合并
```cpp
void Union(int x,int y){
    int fx=findFather(x);
    int fy=findFather(y);
    father[fx]=fy;
}
```
# 并查集计算互不关联集合的个数
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=110;
int father[N];
bool isRoot[N];
int findFather(int x){
	while(x!=father[x]){
		x=father[x];
	}
	return x;
}
void Union(int x,int y){
	int fx=findFather(x);
	int fy=findFather(y);
	if(fx!=fy){
		father[fx]=fy;
	}
}
void init(int n){
	for(int i=1;i<=n;i++){
		father[i]=i;
		isRoot[i]=false;
	}
}
int main(){
	int n,m,a,b;
	cin>>n>>m;
	init(n);
	for(int i=0;i<m;i++){
		cin>>a>>b;
		Union(a,b);
	}
	for(int i=1;i<=n;i++){
		isRoot[findFather(i)]=true;
	}
	int ans=0;
	for(int i=1;i<=n;i++){
		ans+=isRoot[i];
	}
	cout<<ans;
	return 0;
}
```
# 并查集计算互不关联集合的个数及集合中元素个数
使用维护size的并查集

设置一个Size[father]将集合中元素存储在以根节点为索引的数组中。
[https://www.acwing.com/problem/content/839/](https://www.acwing.com/problem/content/839/)
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=100010;
int father[N],isRoot[N],Size[N];
int n,m;
int findFather(int x){
    int tmp=x;
    while(x!=father[x]){
        x=father[x];
    }
    while(tmp!=father[tmp]){
        int original=tmp;
        tmp=father[tmp];
        father[original]=x;
    }
    return x;
}
void init(){
    for(int i=1;i<=n;i++){
        father[i]=i;
        isRoot[i]=0;
        Size[i]=1;
    }
}
void Union(int x,int y){
    int fx=findFather(x);
    int fy=findFather(y);
    if(fx!=fy){
        father[fx]=fy;
        Size[fy]+=Size[fx];
    }
}
int main(){
    cin>>n>>m;
    init();
    string s;
    int x,y;
    bool flag=true;
    for(int j=0;j<m;j++){
        cin>>s>>x;
        if(s=="C"){
            flag=true;
            cin>>y;
            Union(x,y);
        }else if(s=="Q1"){
            cin>>y;
            int fx=findFather(x);
            int fy=findFather(y);
            if(fx!=fy)cout<<"No"<<endl;
            else cout<<"Yes"<<endl;
        }else{
            cout<<Size[findFather(x)]<<endl;
        }
    }
    return 0;
}
```
# 食物链
需要把同类域，天敌域，捕食域映射为三个并查集
[https://www.acwing.com/problem/content/242/](https://www.acwing.com/problem/content/242/)
[https://www.acwing.com/solution/AcWing/content/1007/](https://www.acwing.com/solution/AcWing/content/1007/)
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=50010*3;
int father[N],eat[N];
int n,k;
int get(int x){
    int tmp=x;
    while(x!=father[x]){
        x=father[x];
    }
    while(tmp!=father[tmp]){
        int a=tmp;
        tmp=father[tmp];
        father[a]=x;
    }
    return x;
}
void Union(int x,int y){
    int fx=get(x);
    int fy=get(y);
    if(fx!=fy){
        father[fx]=fy;
    }
}
void init(){
    for(int i=1;i<=3*n;i++){
        father[i]=i;
    }
}
int main(){
    cin>>n>>k;
    init();
    int a,x,y,ans=0;
    while(k--){
        cin>>a>>x>>y;
        if(x>n||y>n){
            ans++;
            continue;
        }
        if(a==1){
            if(get(x)==get(y+n)||get(x)==get(y+n+n)){
                ans++;
                continue;
            }
            Union(x,y);
            Union(x+n,y+n);
            Union(x+n+n,y+n+n);
        }else{
            if(x==y||get(x)==get(y)||get(x)==get(y+n)){
                ans++;
                continue;
            }
            Union(x,y+n+n);
            Union(x+n,y);
            Union(x+n+n,y+n);
        }
    }
    cout<<ans;
    return 0;
}
```

