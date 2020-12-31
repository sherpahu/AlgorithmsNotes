# PAT基础知识——STL之stack

# 定义
stack<typename> name;即可
# 用法
## 增
### push()
入栈
## 删
### pop()
弹出栈顶元素
## 改
## 查
### top()
取栈顶元素，实现stack内元素的访问
### empty()
判断栈是否为空
### size()
栈内元素总的个数
# 一言以蔽之
```cpp
#include <bits/stdc++.h>
using namespace std;

int main()
{
   stack<int> s;
	for(int i=1;i<5;i++){
		s.push(i);	
	}
	while(s.empty()==false){
		cout<<s.top()<<endl;
		s.pop();
	}
   return 0;
}
```

