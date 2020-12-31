# PAT基础知识——STL之其他(fill,sort)

# fill(start,end,value)
fill可能会导致超时
## 填充一维数组
```cpp
int a[5]={1,2,3,4,5};
fill(a,a+5,-1);
```
## 填充二维数组
```cpp
const int m=1e5,n=1e5;
int a[m][n];
fill(a[0],a[0]+m*n;
```
# sort
## lambda表达式(C++11)
不用额外写cmp函数的方法

```cpp
sort(pairs.begin(),pairs.end(),[](const auto& a,const auto& b){
	return (a[0]<b[0])||(a[0]==b[0]&&a[1]<b[1]);
});
```
```cpp
sort( a, a+n, [](const auto& a,const auto& b){return a>b;} );//降序排序：不依赖a和b的具体类型
```
C++中，一个lambda表达式表示一个可调用的代码单元。**我们可以将其理解为一个未命名的内联函数**。它与普通函数不同的是，lambda必须使用尾置返回来指定返回类型。
参考：[https://blog.csdn.net/weixin_40539125/article/details/87799796](https://blog.csdn.net/weixin_40539125/article/details/87799796)
## C++89写法
```cpp
bool compare(int& a,int& b){
    return a>b;
}
sort(a, a+n, compare);
```

# reverse
`reverse(a,a+len);` 可以将长为len的数组a反转
`reverse(start,end);` 可以将数组指针在[start,end)间的元素反转
`reverse(it1,it2);` 可以将迭代器在it1到it2间的元素反转


