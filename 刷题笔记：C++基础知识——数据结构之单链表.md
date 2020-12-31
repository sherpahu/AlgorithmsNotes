# PAT基础知识——数据结构之单链表

# 单链表模板
```cpp
// head存储链表头，e[]存储节点的值，ne[]存储节点的next指针，idx表示当前用到了哪个节点
int head, e[N], ne[N], idx;
// 初始化
void init(){
    head = -1;
    idx = 0;
}
// 在链表头插入一个数a
void insert(int a){
    e[idx] = a, ne[idx] = head, head = idx ++ ;
}
// 将头结点删除，需要保证头结点存在
void remove(){
    head = ne[head];
}
```
# acwing826 单链表
[https://www.acwing.com/problem/content/828/](https://www.acwing.com/problem/content/828/)
```cpp
#include<iostream>
using namespace std;
const int N=100010;
int idx,head,n[N],ne[N];
int a;
void add_head(int x){
    n[idx]=x;
    ne[idx]=head;
    head=idx++;
}
void add(int k,int x){
    n[idx]=x;
    ne[idx]=ne[k];
    ne[k]=idx++;
}
void remove(int k){
    ne[k]=ne[ne[k]];
}

int main(){
    head=-1;idx=0;
    cin>>a;
    while(a--){
        string op;
        int k,x;
        cin>>op;
        if(op=="D"){
            cin>>k;
            if(!k)head=ne[head];
            remove(k-1);
        }
        else if(op=="H"){
            cin>>x;
            add_head(x);
        }
        else if(op=="I"){
            int k,x;
            cin>>k>>x;
            add(k-1,x);
        }
    }
    for(int i=head;i!=-1;i=ne[i])
      cout<<n[i]<<" ";
    return 0;

}
```

