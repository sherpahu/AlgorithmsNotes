# PAT基础知识——搜索(BFS,DFS,回溯)

# 广度优先搜索(BFS)
关键词为“广度”，优先依次访问该岔道口能够直接一步到达的所有结点，然后按照这些结点到达的顺序依照上述规则去访问他们能够直接到达的结点。
使用队列实现
> ![tmp.jpg](https://cdn.nlark.com/yuque/0/2020/jpeg/705461/1579997595747-e0aaebc6-cf11-431a-b72e-234dc5d80fed.jpeg#align=left&display=inline&height=258&name=tmp.jpg&originHeight=258&originWidth=238&size=13312&status=done&style=none&width=238)
> 广度优先搜索一层一层地进行遍历，每层遍历都以上一层遍历的结果作为起点，遍历一个距离能访问到的所有节点。需要注意的是，遍历过的节点不能再次被遍历。
> 第一层：
> - 0 -> {6,2,1,5}
> 
第二层：
> - 6 -> {4}
> - 2 -> {}
> - 1 -> {}
> - 5 -> {3}
> 
第三层：
> - 4 -> {}
> - 3 -> {}
> 
每一层遍历的节点都与根节点距离相同。设 d 表示第 i 个节点与根节点的距离，推导出一个结论：对于先遍历的节点 i 与后遍历的节点 j，有 d <= d。利用这个结论，可以求解最短路径等   **最优解**   问题：第一次遍历到目的节点，其所经过的路径为最短路径。应该注意的是，使用 BFS 只能求解无权图的最短路径，无权图是指从一个节点到另一个节点的代价都记为 1。
> 在程序实现 BFS 时需要考虑以下问题：
> - 队列：用来存储每一轮遍历得到的节点；
> - 标记：对于遍历过的节点，应该将它标记，防止重复遍历。

## 用途

- 计算连通块个数
- 最短路径等**最优解**问题（如：计算从起点到终点走出迷宫所需的最小步数）
## 计算连通块个数
用inq[x][y]表示(x,y)是否入队

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=1e3+10;
int X[]={0,0,1,-1};//增量数组，注意有两个为0
int Y[]={1,-1,0,0};
int matrix[N][N];
bool inq[N][N];
int n;
bool judge(int x,int y){
	if(x<0||x>=n||y<0||y>=n)return false;
	if(matrix[x][y]==0||inq[x][y]==true)return false;
	return true;
}
void bfs(int x,int y){
	queue<pair<int,int> >q;
	q.push({x,y});
	while(!q.empty()){
		pair<int,int> p=q.front();
		q.pop();
		for(int i=0;i<4;i++){
			int newx=p.first+X[i];
			int newy=p.second+Y[i];
			if(judge(newx,newy)){
				q.push({newx,newy});
				inq[newx][newy]=true;
			}
		}
	}
}
int main(int argc, char** argv) {
	ios::sync_with_stdio(false);
	cin>>n;
	for(int i=0;i<n;i++){
		for(int j=0;j<n;j++){
			cin>>matrix[i][j];
		}
	}
	int ans=0;
	for(int i=0;i<n;i++){
		for(int j=0;j<n;j++){
			if(matrix[i][j]==1&&inq[i][j]==false){
				ans++;
				bfs(i,j);
			}
		}
	}
	cout<<ans;
	return 0;
}
```
输入
```
5
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
0 0 0 0 0
```
结果
```
3
```
## 计算在网格中最短路径长度(leetcode1091)
[https://leetcode-cn.com/problems/shortest-path-in-binary-matrix/](https://leetcode-cn.com/problems/shortest-path-in-binary-matrix/)
```cpp
int X[]={1,-1,0,0,1,-1,1,-1};
int Y[]={0,0,1,-1,1,1,-1,-1};
int ttl=0;
struct node{
    int x,y,step;
    node(int _x, int _y, int _s):x(_x),y(_y),step(_s){}//构造函数
};//不能忘记分号
class Solution {
public:
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        int lenx=grid.size(),leny=grid[0].size();
        if(grid[0][0] || grid[grid.size()-1][grid.size()-1]) return -1;
        int x=0,y=0;
        queue<shared_ptr<node> >q;
        q.push(make_shared<node>(0,0,1));
        
        while(!q.empty()){
            auto now=q.front();
            if(now->x == lenx-1 && now->y == leny-1) return now->step;
            q.pop();
            grid[now->x][now->y]=1;
            for(int i=0;i<8;i++){
                int newx=now->x+X[i];
                int newy=now->y+Y[i];
                if(newx<0||newx>=lenx||newy<0||newy>=leny||grid[newx][newy])
                    continue;
                grid[newx][newy]=1;
                q.push(make_shared<node>(newx,newy,now->step+1));
            }
        }
        return -1;
    }
};
```
使用tuple
```cpp
int X[]={1,-1,0,0,1,-1,1,-1};
int Y[]={0,0,1,-1,1,1,-1,-1};
int ttl=0;
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
## 最短路径变式：将一个数分解为最少的完全平方数的个数
leetcode279
[https://leetcode-cn.com/problems/perfect-squares/](https://leetcode-cn.com/problems/perfect-squares/)
若两数之差为一个完全平方数，则二者相当于连通可达，有一条“边”。将一个数分解为完全平方数相当于从n到0的最少边数。
从n开始，每次减去一个完全平方数，所得的值next若为0则得到了结果，否则继续减
```cpp
class Solution {
public:
    int numSquares(int n) {
        bool flag[n+10];
        fill(flag,flag+n+10,false);//局域变量初始化
        vector<int> squares;
        for(int i=1;i*i<=n;i++){
            squares.push_back(i*i);//获得完全平方数
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
## 最短单词路径
一次只改变单词中的一个字母，求变化的最少次数

用map存单词的string

```cpp
class Solution {
public:
    bool isConnected(string a,string b){
        int len=a.size(),cnt=0;
        for(int i=0;i<len;i++){
            if(a[i]!=b[i])cnt++;
        }
        return cnt==1&&cnt!=0;
    }
    void construct_graph(string &beginWord, vector<string>& wordList, map<string,vector<string>>& graph){
        wordList.push_back(beginWord); //这是为了防止wordList中不存在这个beginWord这个单词导致无法构建图
        for(int i = 0; i < wordList.size(); i++){
            graph[wordList[i]] = vector<string>();//构建一个空的基本图，定义key和value
        }
        for(int i = 0; i < wordList.size(); i++){
            for(int j = i+1; j <wordList.size(); j++){//这里注意下一定是从j=i+1开始，因为前面已经考虑过了
                if(isConnected(wordList[i],wordList[j])){
                    graph[wordList[i]].push_back(wordList[j]);
                    graph[wordList[j]].push_back(wordList[i]);
                }
            }
        }
    }
    int BFS_graph(string &beginWord, string &endWord, map<string,vector<string>> &graph){
        queue<pair<string, int>> Q;//定义队列《顶点，步数》
        set<string> visit;//记录已访问的顶点
        Q.push(make_pair(beginWord,1));//初始化起点
        visit.insert(beginWord);//标记起点已经访问
        while(!Q.empty()){
            string node = Q.front().first;
            int step = Q.front().second;
            Q.pop();
            if(node == endWord){//找到就返回，否则继续BFS
                return step;
            }
            const vector<string> &neighbors = graph[node];//取出全部临街点遍历
            for(int i = 0; i < neighbors.size(); i++){
                if(visit.find(neighbors[i]) == visit.end()){
                    Q.push(make_pair(neighbors[i],step+1));
                    visit.insert(neighbors[i]);//标记neighbors已经进入队列了
                }
            }
        }
        return 0;//否则返回没找到
    }
    void getGraph(vector<string>&wordList){
        int n=wordList.size();
        for(int i = 0; i < n; i++){
            graph[wordList[i]] = vector<string>();//构建一个空的基本图，定义key和value
        }
        for(int i=0;i<n;i++){
            for(int j=i+1;j<n;j++){
                if(isConnected(wordList[i],wordList[j])){
                    graph[wordList[i]].push_back(wordList[j]);
                    graph[wordList[j]].push_back(wordList[i]);
                }
            }
        }
    }
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        construct_graph(beginWord,wordList,graph);
        return BFS_graph(beginWord,endWord,graph);
    }
private:
map<string,vector<string> >graph;
};
```
存单词在wordlist中的索引

```cpp
class Solution {
public:
    bool isConnected(string a,string b){
        int len=a.size(),cnt=0;
        for(int i=0;i<len;i++){
            if(a[i]!=b[i])cnt++;
            if(cnt>2)break;
        }
        return cnt==1;
    }
    void getGraph(vector<string>&wordList){
        int n=wordList.size();
        for(int i=0;i<n;i++){
            for(int j=i+1;j<n;j++){
                if(isConnected(wordList[i],wordList[j])){
                    graph[i].push_back(j);
                    graph[j].push_back(i);
                }
            }
        }
    }
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        if(find(wordList.begin(),wordList.end(),beginWord)==wordList.end())
            wordList.push_back(beginWord);
        bool flag[wordList.size()+10]={false};
        //fill(flag,flag+wordList.size()+10,false);//局域变量初始化
        getGraph(wordList);
        queue<pair<int,int> >q;
        int begin=find(wordList.begin(),wordList.end(),beginWord)-wordList.begin();
        int end=find(wordList.begin(),wordList.end(),endWord)-wordList.begin();
        q.push({begin,1});
        while(!q.empty()){
            pair<int,int>p=q.front();
            flag[p.first]=true;
            q.pop();
            for(auto next:graph[p.first]){
                if(next==end){
                    return p.second+1;
                }
                if(flag[next]==true){
                    continue;
                }
                q.push({next,p.second+1});
                flag[next]=true;
            }
        }
        return 0;
    }
private:
vector<int>graph[100000];
};
```

# 深度优先搜索(DFS)
![tmp.png](https://cdn.nlark.com/yuque/0/2020/png/705461/1579997461747-f61eb8cf-0f41-4ff5-9a86-5ac2eefc2e91.png#align=left&display=inline&height=198&name=tmp.png&originHeight=198&originWidth=331&size=14336&status=done&style=none&width=331)
深度优先搜索在得到一个新节点时立即对新节点进行遍历：从节点 0 出发开始遍历，得到到新节点 6 时，立马对新节点 6 进行遍历，得到新节点
 4；如此反复以这种方式遍历新节点，直到没有新节点了，此时返回。返回到根节点 0 的情况是，继续对根节点 0 进行遍历，得到新节点 
2，然后继续以上步骤。
从一个节点出发，使用 DFS 对一个图进行遍历时，能够遍历到的节点都是从初始节点可达的，DFS 常用来求解这种   **可达性**   问题。
在程序实现 DFS 时需要考虑以下问题：

- 栈：用栈来保存当前节点信息，当遍历新节点返回时能够继续遍历当前节点。可以使用递归栈。
- 标记：和 BFS 一样同样需要对已经遍历过的节点进行标记。
## 用途

- 计算连通块个数
## 计算连通块个数
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=1e3+10;
int X[]={0,0,1,-1};
int Y[]={1,-1,0,0};
int matrix[N][N];
bool inq[N][N];
int n;
bool judge(int x,int y){
	if(x<0||x>=n||y<0||y>=n)return false;
	if(matrix[x][y]==0||inq[x][y]==true)return false;
	return true;
}
void bfs(int x,int y){
	queue<pair<int,int> >q;
	q.push({x,y});
	while(!q.empty()){
		pair<int,int> p=q.front();
		q.pop();
		for(int i=0;i<4;i++){
			int newx=p.first+X[i];
			int newy=p.second+Y[i];
			if(judge(newx,newy)){
				q.push({newx,newy});
				inq[newx][newy]=true;
			}
		}
	}
}
void dfs(int x,int y){
	inq[x][y]=true;
	for(int i=0;i<4;i++){
		int newx=x+X[i];
		int newy=y+Y[i];
		if(judge(newx,newy)){
			dfs(newx,newy);
		}
	}
}
int main(int argc, char** argv) {
	ios::sync_with_stdio(false);
	cin>>n;
	for(int i=0;i<n;i++){
		for(int j=0;j<n;j++){
			cin>>matrix[i][j];
		}
	}
	int ans=0;
	for(int i=0;i<n;i++){
		for(int j=0;j<n;j++){
			if(matrix[i][j]==1&&inq[i][j]==false){
				ans++;
				//bfs(i,j);
				dfs(i,j);
			}
		}
	}
	cout<<ans;
	return 0;
}
```
输入及结果同bfs计算连通块个数
### leetcode200
[https://leetcode-cn.com/problems/number-of-islands/](https://leetcode-cn.com/problems/number-of-islands/)
类似迷宫等题不用另外设置isVisited[][]数组，在访问完后将grid[i][j]修改为0即可
```cpp
class Solution {
public:
    void bfs(vector<vector<char>>& grid,int x,int y){
        if (x >= 0 && x < grid.size() && y >= 0 && y < grid[0].size() && grid[x][y] == '1') {
		    grid[x][y] = '0';
		    dfs(grid, x - 1, y);//只能走向四个方向，直接这么写就行了
		    dfs(grid, x + 1, y);
		    dfs(grid, x, y - 1);
		    dfs(grid, x, y + 1);
	    }
    }
    int numIslands(vector<vector<char>>& grid) {
        int sum = 0;
	    for (int i = 0; i < grid.size(); i++) {
		    for (int j = 0; j < grid[0].size(); j++) {
			    if (grid[i][j] == '1') {
			    	sum++;
			    	dfs(grid, i, j);
			    }
		    }
	    }
	    return sum;
    }
};
```
## 变式:计算连通块最大面积(leetcode695)
[https://leetcode-cn.com/problems/max-area-of-island/](https://leetcode-cn.com/problems/max-area-of-island/)
```cpp
class Solution {
public:
    int dfs(vector<vector<int>>& grid,int x,int y){
        grid[x][y]=0;
        int area=1;
        for(int i=0;i<4;i++){
            int newx=x+X[i];
            int newy=y+Y[i];
            if(newx>=0&&newx<lx&&newy>=0&&newy<ly&&grid[newx][newy]==1){
                area+=dfs(grid,newx,newy);
            }
        }
        return area;
    }
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        lx=grid.size();
        ly=grid[0].size();
        int ans=0;
        for(int i=0;i<lx;i++){
            for(int j=0;j<ly;j++){
                if(grid[i][j]==1)
                    ans=max(ans,dfs(grid,i,j));
            }
        }
        return ans;
    }
private:
    int X[5]={1,-1,0,0};
    int Y[5]={0,0,1,-1};
    int lx,ly;
};
```

## 改变dfs的方向
leetcode547
[https://leetcode-cn.com/problems/friend-circles/](https://leetcode-cn.com/problems/friend-circles/)
实际上人数只有一维，只不过人与人的关系构成了二维矩阵M。dfs的方向是找i的所有朋友j并再找j的所有朋友
```cpp
class Solution {
public:
    void dfs(vector<vector<int>>& M,int i){
        hasVisited[i]=true;
        for(int j=0;j<n;j++){
            if(M[i][j]==1&&hasVisited[j]==false){
                dfs(M,j);
            }
        }
    }
    int findCircleNum(vector<vector<int>>& M) {
        n=M.size();
        int ans=0;
        for(int i=0;i<n;i++){
            if(hasVisited[i]==false){
                ans++;
                dfs(M,i);
            }
        }
        return ans;
    }
private:
    int n;
    bool hasVisited[1010];
};
```
# 回溯Backtracking
Backtracking（回溯）属于 DFS。

- 普通 DFS 主要用在   **可达性问题**  ，这种问题只需要执行到特点的位置然后返回即可。
- 而 Backtracking 主要用于求解   **排列组合**   问题，例如有 { 'a','b','c' } 三个字符，求解所有由这三个字符排列得到的字符串，这种问题在执行到特定的位置返回之后还会继续执行求解过程。

因为 Backtracking 不是立即返回，而要继续求解，因此在程序实现时，需要注意对元素的标记问题：

- 在访问一个新元素进入新的递归调用时，需要将新元素标记为已经访问，这样才能在继续递归调用时不用重复访问该元素；
- 但是在递归返回时，需要将元素标记为未访问，因为只需要保证在一个递归链中不同时访问一个元素，可以访问已经访问过但是不在当前递归链中的元素。
## N皇后
[https://www.acwing.com/problem/content/845/](https://www.acwing.com/problem/content/845/)
```cpp
#include <bits/stdc++.h>
using namespace std;
bool slash[20],reslash[20],row[20],col[20];
char grid[20][20];
int n;
void dfs(int x,int y,int step){
    if (y == n) y = 0, x ++ ;
    if(x==n&&step>=n){
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                cout<<grid[i][j];
            }
            cout<<endl;
        }
        cout<<endl;
    }
    if(x==n)return;
    //不放皇后
    dfs(x,y+1,step);
    //放皇后
    if(!row[x]&&!col[y]&&!slash[x+y]&&!reslash[x-y+n]){
        grid[x][y]='Q';
        row[x]=col[y]=slash[x+y]=reslash[x-y+n]=true;
        dfs(x,y+1,step+1);
        //回溯，恢复现场
        row[x]=col[y]=slash[x+y]=reslash[x-y+n]=false;
        grid[x][y]='.';
    }
}
int main(){
    cin>>n;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            grid[i][j] = '.';
    dfs(0,0,0);
    return 0;
}
```

