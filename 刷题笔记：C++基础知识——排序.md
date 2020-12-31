# PAT基础知识——排序

# 基于比较的排序
## 冒泡排序O(n^2)
每次通过交换的方式把当前剩余元素的最大/小值移至一端
第一层循环是当前将要排序的第几个元素，第二层循环是从剩余元素中寻找最大/小值
if的判断>是从大到小，<是从小大大
```cpp
#include <bits/stdc++.h>
using namespace std;

int main()
{
   int a[20]={1,4,2,7,4,5,9,3,7,0};
	for(int i=1;i<10;i++){
		for(int j=0;j<10-i;j++){
			if(a[j]>a[j+1]){
				swap(a[j],a[j+1]);	
			}
		}
	}
	for(int i=0;i<10;i++){
		cout<<a[i]<<" ";
	}
   return 0;
}
```
结果
```cpp
0 1 2 3 4 4 5 7 7 9 
```
## 选择排序O(n^2)
每次在剩余预算中选择最大的与最靠前的元素交换

```cpp
#include <bits/stdc++.h>
using namespace std;

int main()
{
   int a[20]={1,4,2,7,4,5,9,3,7,0};
	for(int i=0;i<10;i++){
		int k=i;
		for(int j=i;j<10;j++){
			if(a[k]>a[j]){
				k=j;	
			}
		}
		swap(a[i],a[k]);
	}
	for(int i=0;i<10;i++){
		cout<<a[i]<<" ";
	}
   return 0;
}
```
结果

```cpp
0 1 2 3 4 4 5 7 7 9 
```
## 快速排序O(nlogn)
sort()默认从小到大
传入cmp可以从大到小排序，或者对结构体排序。
起始与终点可以传入数组地址或迭代器
### 数组地址
```cpp
#include <bits/stdc++.h>
using namespace std;
bool cmp(const int &a,const int &b){//传入引用是为了加速
	return a>b;	
}
int main()
{
   int a[20]={1,4,2,7,4,5,9,3,7,0};
	sort(a,a+10);//从小到大
	for(int i=0;i<10;i++){
		cout<<a[i]<<" ";
	}
	cout<<endl;
	sort(a,a+10,cmp);//从大到小
	for(int i=0;i<10;i++){
		cout<<a[i]<<" ";
	}
   return 0;
}
```
### 迭代器
```cpp
#include <bits/stdc++.h>
using namespace std;
bool cmp(const int &a,const int &b){
	return a>b;	
}
int main()
{
   	int a[20]={1,4,2,7,4,5,9,3,7,0};
	vector<int> v(a,a+10);
	sort(v.begin(),v.end());
	for(int i=0;i<10;i++){
		cout<<v[i]<<" ";	
	}
	cout<<endl;
   return 0;
}
```
### cmp结构体
cmp函数中>从大到小，<从小到大
```cpp
#include <bits/stdc++.h>
using namespace std;
struct student{
	int num;
	string name;
}stu[100];
bool cmp(student &a,student&b){
	return a.name!=b.name?a.name>b.name:a.num<b.num;	
}
int main()
{
   	stu[0].num=123;
	stu[0].name="Lucy";
	stu[1].num=23;
	stu[1].name="Anna";
	stu[2].num=321;
	stu[2].name="Jack";
	stu[3].num=4;
	stu[3].name="Jack";
	stu[4].num=4;
	stu[4].name="Tom";
	sort(stu,stu+5,cmp);
	for(int i=0;i<5;i++){
		cout<<stu[i].num<<" "<<stu[i].name<<endl;
	}
   return 0;
}
```
### 手写快排
```cpp
void quick_sort(int q[], int l, int r){
    if (l >= r) return;

    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j){
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    quick_sort(q, l, j), quick_sort(q, j + 1, r);
}
```
# 归并排序

```cpp
void merge_sort(int q[], int l, int r)
{
    if (l >= r) return;

    int mid = l + r >> 1;
    merge_sort(q, l, mid);
    merge_sort(q, mid + 1, r);

    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r)
        if (q[i] < q[j]) tmp[k ++ ] = q[i ++ ];
        else tmp[k ++ ] = q[j ++ ];

    while (i <= mid) tmp[k ++ ] = q[i ++ ];
    while (j <= r) tmp[k ++ ] = q[j ++ ];

    for (i = l, j = 0; i <= r; i ++, j ++ ) q[i] = tmp[j];
}
```

# 桶排序
用数组或者map<typename,int>即可

# 求第K大的值
## 排序O(NlogN)
sort()后第K个就是
## 堆O(NlogK)
### 数组求第k小
```cpp
#include <bits/stdc++.h>
using namespace std;
int findKth(int nums[],int l,int k){
		priority_queue<int> q;
		for(int i=0;i<l;i++){
			q.push(nums[i]);
			if(q.size()>k){
				q.pop();
			}
		}
		return q.top();
}
int main()
{
   	int a[20]={1,4,2,7,4,5,9,3,7,0};
	cout<<findKth(a,10,3);
	sort(a,a+10);
	cout<<endl<<a[3-1]<<endl;
   return 0;
}
```
结果

```
2
2
```
### vector求第K大
数组变成最大堆比较麻烦，vector使用priority_queue<int,vector<int>,greater<int> > q;即可
```cpp
#include <bits/stdc++.h>
using namespace std;
int findKth(vector<int>nums,int l,int k){
		priority_queue<int,vector<int>,greater<int> > q;
		for(int i=0;i<l;i++){
			q.push(nums[i]);
			if(q.size()>k){
				q.pop();
			}
		}
		return q.top();
}
int main()
{
   	int a[20]={1,4,2,7,4,5,9,3,7,0};
	vector<int> v(a,a+10);
	cout<<findKth(v,10,3);
	sort(a,a+10,greater<int>() );
	cout<<endl<<a[3-1]<<endl;
   return 0;
}
```
结果
```
7
7
```

## 快速选择O(N)
基于快排
快排代码：
```cpp
void quick_sort(int q[], int l, int r){
    if (l >= r) return;

    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j){
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    quick_sort(q, l, j), quick_sort(q, j + 1, r);
}
```
快速选择：
### 算法一
```cpp
#include<iostream>
using namespace std;
const int N = 1e5 + 10;
int n, k;
int q[N];

int quick_sort(int l, int r, int k){
    if(l == r) return q[l];
    
    int i = l - 1, j = r + 1,x = q[l];
    while(i < j){
        while(q[++i] < x);
        while(q[--j] > x);
        if(i < j) swap(q[i], q[j]);
    }
    int s1 = j - l + 1;
    if(k <= s1) quick_sort(l, j, k);
    else quick_sort(j + 1, r, k- s1);
}
int main(){
    cin >> n >> k;
    for(int i = 0; i < n; i++) scanf("%d", &q[i]);
    cout << quick_sort(0, n - 1, k) << endl;
    return 0;
}
```
### 算法二(更易懂)
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=1e5+10;
int A[N];//={1,5,3,6,2,9,10,1,10,4};
int quick_sort(int q[], int l, int r,int k){
    if (l >= r) return q[k];

    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j){
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    if(k>j) return quick_sort(q, j + 1, r,k);
    else quick_sort(q, l, j,k);
}
int main(int argc, char** argv) {
	int n,k;
	ios::sync_with_stdio(false);
	cin>>n>>k;
	for(int i=0;i<n;i++){
		cin>>A[i];
	}
	cout<<quick_sort(A,0,n-1,k-1);
	return 0;
}
```

## leetcode215
堆只能排到一半的名次，还凑合吧，写快速选择太麻烦了

```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int,vector<int>,greater<int> > q;
        for(auto num:nums)   {
            q.push(num);
            if(q.size()>k){
                q.pop();
            }
        }
        return q.top();
    }
};
```
## leetcode451
[https://leetcode-cn.com/problems/sort-characters-by-frequency/](https://leetcode-cn.com/problems/sort-characters-by-frequency/)
使用priority_queue
```cpp
class Solution {
public:
    struct cmp{
        bool operator()(pair<char,int>&a,pair<char,int>&b){
            return a.second<b.second;
        }
    };
    string frequencySort(string s) {
        unordered_map<char,int>m;
        int l=s.length();
        for(int i=0;i<l;i++){
            m[s[i]]++;
        }
        priority_queue<pair<char,int>,vector<pair<char,int> >,cmp>q;
        for(auto it=m.begin();it!=m.end();it++){
            q.push(*it);
        }
        string ans;
        while(!q.empty()){
            pair<char,int>p=q.top();
            q.pop();
            for(int i=0;i<p.second;i++){
                ans+=p.first;
            }
        }
        return ans;
    }
};
```

