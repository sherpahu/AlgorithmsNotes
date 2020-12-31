# PAT基础知识——分治

# 归并排序
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=1e5+10;
int tmp[N];
void merge_sort(int q[],int l,int r){
	if(l>=r) return;
	int mid=l+r>>1;
	merge_sort(q,l,mid);//递归排序
	merge_sort(q,mid+1,r);//递归排序
	int i=l,j=mid+1,k=0;
	while(i<=mid&&j<=r){//merge
		if(q[i]<=q[j]){
			tmp[k++]=q[i++];
		}else{
			tmp[k++]=q[j++];
		}
	}
    //扫尾
	while(i<=mid) tmp[k++]=q[i++];
	while(j<=r) tmp[k++]=q[j++];
    //获得结果
	for(int i=l,j=0;i<=r;i++,j++){
		q[i]=tmp[j];
	}
}
int main(int argc, char** argv) {
	int q[20]={1,3,5,6,2,10,18,2,4,9};
	merge_sort(q,0,9);
	for(int i=0;i<10;i++){
		cout<<q[i]<<" ";
	}
	return 0;
}
```

## 逆序对
```cpp
#include<iostream>
using namespace std;
typedef long long LL;

const int N = 1e5 + 10;
int n;
int q[N], tmp[N];

LL merge_sort(int l, int r){
    if(l >= r) return 0;

    int mid = l + r >> 1;

    LL res = merge_sort(l, mid) + merge_sort(mid + 1, r);//左边内部逆序对数量+右半边内部逆序对数量 

    int i = l, j = mid + 1, k = 0;
    while(i <= mid && j <= r){
        if(q[i] <= q[j]) tmp[k++] = q[i++];
        else{
            res += mid - i + 1;//跨越左右两半的逆序对 ，q[i]左侧即索引小于i的元素不存在逆序对 
            tmp[k++] = q[j++];
        }
    }
    while(i <= mid) tmp[k++] = q[i++];
    while(j <= r) tmp[k++] = q[j++];
    for(int i = l, j = 0; i <= r; i++, j++) q[i] = tmp[j];
    return res;
}

int main(){
    cin >> n;
    for(int i = 0; i < n; i++) scanf("%d", &q[i]);
    cout << merge_sort(0, n - 1) << endl;
    return 0;
}
```
# 分治
将原问题划分成多个规模较小而与原问题相同或相似的子问题，最后合并子问题的解就得到原问题的解
## leetcode241
算术表达式加括号的所有不同结果

```cpp
class Solution {
public:
    string substring(string str,int start,int end){//不包含end处的元素
        if(end!=-1)
            return str.substr(start,end-start);
        else
            return str.substr(start);
    }
    vector<int> diffWaysToCompute(string input) {
        vector<int> ans;
        int len=input.length();
        for(int i=0;i<len;i++){
            char c=input[i];
            if(c=='+'||c=='-'||c=='*'){
                vector<int> left=diffWaysToCompute(substring(input,0,i));
                vector<int> right=diffWaysToCompute(substring(input,i+1,-1));
                for(int l:left){
                    for(int r:right){
                        switch(c){
                            case '+':
                                ans.push_back(l+r);
                                break;
                            case '-':
                                ans.push_back(l-r);
                                break;
                            case '*':
                                ans.push_back(l*r);
                                break;
                        }
                    }
                }
            }
        }
        if(ans.size()==0){//没有运算符时这个字符串就是一个数字
            ans.push_back(stoi(input));
        }
        return ans;
    }
};
```

