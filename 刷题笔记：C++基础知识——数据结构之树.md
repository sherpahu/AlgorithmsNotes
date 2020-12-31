# PAT基础知识——数据结构之树

# 基本概念
- 满二叉树：每一层的节点个数都达到了该层的最大节点个数
- 完全二叉树：出了最下面一层外，所有层的节点个数达到了该层的最大节点个数，而最下面一层只从左到右 **连续** 存在若干节点，这些节点右侧没有节点
# 静态存储
```cpp
const int maxn=110;
int n,m,Max=INT_MIN,l,index=0;
int level[maxn];
struct Node{
    int data,level,lchild,rchild;
} node[maxn];
int newNode(int value){
    node[index].data=value;
    node[index].lchild=node[index].rchild=-1;
    return index++;
}
void insert(int &root,int value){
    if(root==-1){
        root=newNode(value);
        return;
    }
    if(应该插入左子树){
        insert(node[root].lchild,value);
    } else{
        insert(node[root].rchild,value);
    }
}
int value[maxn];
int create(int n){
    int root=-1;
    for (int i = 0; i < n; ++i) {
        insert(root,value[i]);
    }
}
void search(int root,int value,int newvalue){
    if(root==-1)return;
    if(node[root].data==value)node[root].data=newvalue;
    search(node[root].lchild,value,newvalue);
    search(node[root].rchild,value,newvalue);
}
void preorder(int root){
    if(root==-1)return;
    printf("%d",node[root].data);
    preorder(node[root].lchild);
    preorder(node[root].rchild);
}
void inorder(int root){
    if(root==-1)return;
    inorder(node[root].lchild);
    printf("%d",node[root].data);
    inorder(node[root].rchild);
}
void postorder(int root){
    if(root==-1)return;
    postorder(node[root].lchild);
    postorder(node[root].rchild);
    printf("%d",node[root].data);
}
void leverlorder(int root){
    queue<int>q;
    q.push(root);
    while(!q.empty()){
        int now=q.front();
        q.pop();
        printf("%d",node[now].data);
        if(node[now].lchild!=-1)q.push(node[now].lchild);
        if(node[now].rchild!=-1)q.push(node[now].rchild);
    }
}
int pre[maxn],in[maxn],post[maxn];
//根据先序和中序建树
int create(int prel,int prer,int il,int ir){
    if(prel>prer)return -1;
    int root=newNode(pre[prel]);
    int k;
    for (k = il; k <= ir; ++k) {
        if(in[k]==pre[prel])break;
    }
    int numleft=k-il;//左子树节点个数
    node[root].lchild=create(prel+1,prel+numleft,il,k-1);
    node[root].rchild=create(prel+numleft+1,prer,k+1,ir);
    //post.push_back(pre[prel]);//可以顺便后序遍历
    return root;
}
//根据中序和后序建树
int create(int postl,int postr,int il,int ir){
    if(postl>postr)return -1;
    int root=newnode(post[postr]);
    int k;
    for (k = il; k <= ir; ++k) {
        if(in[k]==post[postr])break;
    }
    int numleft=k-il;
    //in.push_back(post[postr]);//可以顺便先序遍历
    node[root].lchild=create(postl,postl+numleft-1,il,k-1);
    node[root].rchild=create(postl+numleft,postr-1,k+1,ir);
    return root;
}
//根据先序和中序建树&顺便层次遍历
vector<int>level[maxn];
int MAXL=-1;
int create(int prel,int prer,int il,int ir,int l){
    if(prel>prer)return -1;
    int root=newNode(pre[prel]);
    int k;
    for (k = il; k <= ir; ++k) {
        if(in[k]==pre[prel])break;
    }
    int numleft=k-il;//左子树节点个数
    level[l].push_back(pre[prel]);//用level记录每层节点
    if(l>MAXL)MAXL=l;//用MAXL记录最大层数
    node[root].lchild=create(prel+1,prel+numleft,il,k-1,l+1);
    node[root].rchild=create(prel+numleft+1,prer,k+1,ir,l+1);
    return root;
}
for(int i=0;i<=MAXL;++i){//输出层次遍历结果
    for(int x:level[i]){
        cout<<x<<" ";
    }
}
//另一种方式根据中序、后序建树
void create(int root, int l, int r) {
    if(l > r) return ;
    int i = l;
    while(i < r && in[i] != post[root]) i++;
    printf("%d ", post[root]);
    pre(root - 1 - r + i, l, i - 1);
    pre(root - 1, i + 1, r);
}
//另一种方式根据中序、前序建树
void create(int root, int l, int r) {
    if(l > r) return ;
    int i = l;
    while(i < r && in[i] != pre[root]) i++;
    post(root + 1, l, i - 1);
    post(root + 1 + i - l, i + 1, r);
    printf("%d ", pre[root]);
}
```
# 二叉树的存储与基本操作
## 二叉树的存储
二叉树采用递归定义
```cpp
struct node{
 	typename data;//数据
    node* lchild;//指向左子树的根节点的指针
    node* rchild;//指向右子树的根节点的指针
};
```
初始时根节点不存在
```cpp
node* root=NULL;
```
## 新建节点
```cpp
node* newNode(int v){
 	node* Node=new node;
    Node->data=v;
    Node->lchild=Node->rchild=NULL;
    return Node;
}
```
## 二叉树的查找和修改
递归进行查找
```cpp
void search(node* root,int x,int newdata){
 	if(root==NULL)return;//递归边界
    if(root->data==x)root->data=newdata;//修改,修改之后仍需要查找其余左右子树节点
    search(root->lchild,x,newdata);
    search(root->rchild,x,newdata);
}
```
## 二叉树的插入
在当前root为NULL时进行节点赋值
由于需要改变root的指针内容所以传入引用&
```cpp
void insert(node* &root,int x){
 	if(root==NULL){
        root=newNode(x);
        return;
    }
    if(依据二叉树的性质插在左子树){
        insert(root->lchild,x);
    }else{
        insert(root->rchild,x);
    }
}
```
## 二叉树的创建
```cpp
node* create(int data[],int n){
 	node* root=NULL;
    for(int i=0;i<n;i++){
        insert(root,data[i]);
    }
    return root;
}
```
# 二叉树的遍历
二叉树的有四种遍历方式，先序遍历、中序遍历、后序遍历、层次遍历。
前三种使用dfs实现，所谓先中后是指 **根节点** 的遍历顺序，而 **左子树一定先于右子树被遍历** 
先序遍历：根左右
中序遍历：左根右
后序遍历：左右根
层次遍历使用bfs实现
## 先序遍历
先序遍历：根左右
性质：第一个节点一定是根节点
```cpp
void preorder(node* root){
 	if(root==NULL)return;
    cout<<root->data<<endl;//先访问根节点
    preorder(root->lchild);//左
    preorder(root->rchild);//右
}
```
## 中序遍历
中序遍历：左根右
性质：根节点将左右子树分开
```cpp
void preorder(node* root){
 	if(root==NULL)return;
    preorder(root->lchild);//左
    cout<<root->data<<endl;//访问根节点
    preorder(root->rchild);//右
}
```
## 后序遍历
后序遍历：左右根
性质：最后一个节点一定是根节点
```cpp
void preorder(node* root){
 	if(root==NULL)return;
    preorder(root->lchild);//左
    preorder(root->rchild);//右
    cout<<root->data<<endl;//最后访问根节点
}
```
## 层序遍历
类似于bfs，bfs的广度对应二叉树中的层次
基本步骤：

- 将根节点加入队列q
- 取出队首元素，访问
- 若该节点有左孩子（lchild!=NULL），则将左孩子加入队列
- 若该节点有右孩子（lchild!=NULL），则将右孩子加入队列
- 回到第二步直至队列为空
### 普通
```cpp
void layerorder(node* root){
 	queue<node*> q;
    q.push(root);
    while(!q.empty()){
     	node* now=q.front();
        q.pop();
        cout<<now->data<<endl;
        if(now->lchild!=NULL)q.push(now->lchild);
        if(now->rchild!=NULL)q.push(now->rchild);
    }
}
```
### 增加layer
```cpp
struct node{
  	int data;
    int layer;
    node* lchild;
    node* rchild;
};
void layerorder(node* root){
 	queue<node*> q;
    root->layer=1;
    q.push(root);
    while(!q.empty()){
     	node* now=q.front();
        q.pop();
        cout<<now->data<<endl;
        if(now->lchild!=NULL){
            now->lchild->layer=now->layer+1;
            q.push(now->lchild);
        }
        if(now->rchild!=NULL){
            now->rchild->layer=now->layer+1;
            q.push(now->rchild);
        }
    }
}
```
# 二叉树的重建


# 递归求解
树是一种递归的结构，与深度、子树数目等相关的量都可以用递归的方法求解
## [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root==NULL)return 0;
        else return max(maxDepth(root->left),maxDepth(root->right))+1;
    }
};
```
## [110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)
```cpp
class Solution {
public:
    int maxDepth(TreeNode* root){
        if(flag){
            if(root==NULL)return 0;
            else{
                int l=maxDepth(root->left);
                int r=maxDepth(root->right);
                if(abs(l-r)>1)flag=false;
                return max(l,r)+1;
            }
        }else{return 0;}//剪枝
    }
    bool isBalanced(TreeNode* root) {
        maxDepth(root);
        return flag;
    }
private:
    bool flag=true;
};
```
## [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)
```cpp
class Solution {
public:
    int maxDepth(TreeNode* root){
        if(root==NULL)return 0;
        else{
            int l=maxDepth(root->left);
            int r=maxDepth(root->right);
            if(l+r>m)m=l+r;
            return max(l,r)+1;
        }
    }
    int diameterOfBinaryTree(TreeNode* root) {
        maxDepth(root);
        return m;
    }
private:
    int m=0;
};
```
## [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)
```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == NULL) return NULL;
        TreeNode* left=root->left;// 后面的操作会改变 left 指针，因此先保存下来
        root->left=invertTree(root->right);
        root->right=invertTree(left);
        return root;
    }
};
```
## [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)
```cpp
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
        if(t1==NULL&&t2==NULL)return NULL;
        else if(t1==NULL)return t2;
        else if(t2==NULL)return t1;
        else{
            TreeNode* node=new TreeNode(t1->val+t2->val);
            node->left=mergeTrees(t1->left,t2->left);
            node->right=mergeTrees(t1->right,t2->right);
            return node;
        }
    }
};
```
## [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)
```cpp
class Solution {
public:
    void recursion(TreeNode* root,int now,int sum){
        if(root==NULL)return;
        if(root->right==NULL&&root->left==NULL){
            if(root->val+now==sum)flag=true;
        }
        if(root->right!=NULL){
            recursion(root->right,root->val+now,sum);
        }
        if(root->left!=NULL){
            recursion(root->left,root->val+now,sum);
        }
    }
    bool hasPathSum(TreeNode* root, int sum) {
        recursion(root,0,sum);
        return flag;
    }
private:
    bool flag=false;
};
```
## [437. 路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)
### 简单递归方法
```cpp
class Solution {
public:
    int pathSumStartWithRoot(TreeNode* root, int sum) {
        if(root==NULL)return 0;
        int ret=0;
        if(root->val==sum)ret++;
        ret+=pathSumStartWithRoot(root->right,sum-root->val)+pathSumStartWithRoot(root->left,sum-root->val);
        return ret;
    }
    int pathSum(TreeNode* root, int sum) {
        if(root==NULL)return 0;
        return pathSumStartWithRoot(root,sum)+pathSum(root->right,sum)+pathSum(root->left,sum);
    }
};
```
### DFS+回溯？？？？
不是很懂
```cpp
class Solution {
public:
    int helper(TreeNode* root,int sum,int pathSum){
        int res=0;
        if(root==NULL)return 0;
        pathSum+=root->val;
        res+=m[pathSum-sum];
        m[pathSum]++;
        res+=helper(root->left,sum,pathSum)+helper(root->right,sum,pathSum);
        m[pathSum]--;
        return res;
    }
    int pathSum(TreeNode* root, int sum) {
        m[0]=1;
        return helper(root,sum,0);
    }
private:
    unordered_map<int,int>m;
};
```


