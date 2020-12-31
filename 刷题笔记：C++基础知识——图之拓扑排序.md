# PAT基础知识——图之拓扑排序

拓扑排序可以按照《算法笔记》P416页所示BFS解，但更通用的是DFS
## BFS判断是否可以拓扑排序 [LeetCode207](https://leetcode-cn.com/problems/course-schedule/)
```cpp
class Solution {
public:
    vector<int> inDegree;   //保存每个顶点的入度
    vector<vector<int>> G;
    int n;
    bool topSort(){
        int num=0;
        queue<int>q;
        for(int i=0;i<n;i++){
            if(inDegree[i]==0)q.push(i);
        }
        while(!q.empty()){
            int now=q.front();
            q.pop();
            //for(int x:G[now]){
            for(int i=0;i<G[now].size();++i){
                int x=G[now][i];
                --inDegree[x];
                if(inDegree[x]==0){
                    q.push(x);
                }
            }
            num++;
        }
        if(num==n)return true;
        else return false;
    }
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        G = vector<vector<int>>(numCourses, vector<int>()); //初始化图
        inDegree = vector<int>(numCourses, 0);
        n=numCourses;
        for(auto v:prerequisites){
            G[v[1]].push_back(v[0]);
            inDegree[v[0]]++;
        }
        return topSort();
    }
};
```
## DFS判断是否可以拓扑排序[LeetCode207](https://leetcode-cn.com/problems/course-schedule/)
用int类型的vis数组，0为未访问，1为已访问，2为可以作为拓扑排序的合法前置课程。
DFS遇到vis[i]=1就表明成了环，无法拓扑。
```cpp
class Solution {
private:
    vector<vector<int>>edges;
    vector<int>vis;
    bool invalid;
public:
    void dfs(int root){
        vis[root]=1;
        for(int x:edges[root]){
            if(vis[x]==0){
                dfs(x);
                if(invalid)return;
            }else if(vis[x]==1){
                invalid=true;
                return;
            }
        }
        vis[root]=2;
    }
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        edges.resize(numCourses);
        vis.resize(numCourses);
        for(auto v:prerequisites){
            edges[v[1]].push_back(v[0]);
        }
        for(int i=0;i<numCourses;++i){
            if(vis[i]==0){
                dfs(i);
                if(invalid)return false;
            }
        }
        return true;
    }
};
```
## DFS求拓扑排序序列[LeetCode210](https://leetcode-cn.com/problems/course-schedule-ii/)
每当遍历到一个合法的前置课程就存入栈中（实为vector），最后将vector翻转输出。
个人思考，若当输出不唯一时要求输出最小的序列，可以在读入edge的时候排序一下。
```cpp
class Solution {
private:
    vector<vector<int>>edges;
    vector<int>rst;
    vector<int>vis;
    bool invalid;
public:
    void dfs(int root){
        vis[root]=1;
        for(int x:edges[root]){
            if(vis[x]==0){
                dfs(x);
                if(invalid)return;
            }else if(vis[x]==1){
                invalid=true;
                return;
            }
        }
        vis[root]=2;
        rst.push_back(root);
    }
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vis.resize(numCourses);
        edges.resize(numCourses);
        for(auto x:prerequisites){
            edges[x[1]].push_back(x[0]);
        }
        for(int i=0;i<numCourses;++i){
            if(vis[i]==0&&!invalid){
                dfs(i);
            }
        }
        if(invalid)return {};
        reverse(rst.begin(),rst.end());
        return rst;
    }
};
```
