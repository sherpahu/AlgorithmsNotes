# PAT基础知识——杂项

# int等类型的最大值
整个整数 0x7FFFFFFF 的二进制表示就是除了首位是 0，其余都是1
就是说，这是最大的整型数 int（因为第一位是符号位，0 表示他是正数）

用 INT_MAX 常量可以替代这个值。
所以目测0x好像是表示这是一个十六进制数。
相应的最小值可以表示成0x80000000或INT_MIN，这里注意一个问题就是INT_MAX和INT_MIN都被包含在一个叫<climits>的头文件中，
这个头文件用法如下：
<climits>头文件定义的[符号常量](https://www.baidu.com/s?wd=%E7%AC%A6%E5%8F%B7%E5%B8%B8%E9%87%8F&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1YLuhFhnjf4PjTzrj99nW6s0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EP10knWfsn1n)
CHAR_MIN      　char的最小值
SCHAR_MAX      signed char 最大值
SCHAR_MIN       signed char 最小值

UCHAR_MAX      unsigned char 最大值
SHRT_MAX      　short 最大值
SHRT_MIN       short 最小值
USHRT_MAX      unsigned short 最大值
INT_MAX      　int 最大值
INT_MIN        int 最小值
UINT_MAX      　unsigned int 最大值
UINT_MIN        unsigned int 最小值
LONG_MAX      long最大值
LONG_MIN      　long最小值
ULONG_MAX      unsigned long 最大值
FLT_MANT_DIG    float 类型的尾数
FLT_DIG         float 类型的最少[有效数字](https://www.baidu.com/s?wd=%E6%9C%89%E6%95%88%E6%95%B0%E5%AD%97&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1YLuhFhnjf4PjTzrj99nW6s0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EP10knWfsn1n)位数
FLT_MIN_10_EXP  　带有全部有效数的float类型的负指数的最小值（以10为底）
FLT_MAX_10_EXP    float类型的正指数的最大值（以10为底）
FLT_MIN        保留全部精度的float类型正数最小值
FLT_MAX        float类型正数最大值
# io加速
```cpp
std::ios::sync_with_stdio(false); 
cin.tie(nullptr); 
```

- 用 `sync_with_stdio` 接口，关闭 `std::cin` 和 `std::cout` 与 `scanf` 和 `printf` 的同步，减少了相当的 IO 开销。
- 用 `cin.tie` 接口，完成了 `cin` 和 `cout` 的解耦，减少了大量 flush 调用。

参考：[https://blog.csdn.net/weixin_40539125/article/details/87799796](https://blog.csdn.net/weixin_40539125/article/details/87799796)
# 读取整行
## cin.getline()
在<istream>中的getline()函数有两种重载形式：
istream& getline (char* s, streamsize n );
istream& getline (char* s, streamsize n, char delim );
作用是： 从istream中读取至多n个字符(包含结束标记符)保存在s对应的数组中。即使还没读够n个字符，
如果遇到delim或 字数达到限制，则读取终止，delim都不会被保存进s对应的数组中。
```cpp
#include "stdafx.h"
#include <iostream>
//使用标准输入流和标准输出流。
// std::cin ;  std::cout ;  std::endl
int main(){
char name[256], wolds[256];
std::cout << "Please input your name: ";
std::cin.getline(name, 256);
std::cout << "Please input your wolds: ";
std::cin.getline(wolds, 256);
std::cout << "The result is:   " << name<< ", " << wolds<< std::endl;
std::cout << std::endl;
return 0;
}
```
## getline(cin,string s)
在<string>中的getline函数有四种重载形式：
istream& getline (istream&  is, string& str, char delim);
istream& getline (istream&& is, string& str, char delim);
istream& getline (istream&  is, string& str);
istream& getline (istream&& is, string& str);
用法和上第一种类似，但是读取的istream是作为参数is传进函数的。读取的字符串保存在string类型的str中。
函数的变量：
is    ：表示一个输入流，例如cin。
str   ：string类型的引用，用来存储输入流中的流信息。
delim ：char类型的变量，所设置的截断字符；在不自定义设置的情况下，遇到’\n’，则终止输入。
```cpp
#include "stdafx.h"
#include <iostream>
#include <string>
int main(){
std::string name;
std::cout << "Please input your name: ";
std::getline(std::cin, name);
std::cout << "Welcome to here!" << std::ends<< name << std::endl;
std::cout << std::endl;
return 0;
}
```

