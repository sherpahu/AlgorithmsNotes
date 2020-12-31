# PAT基础知识——STL之list

使用list来表示链表，就不用写指针了。
l.insert(pos,value)在任意位置插入，l.erase(pos)删除，l.push_back(value)尾插，l.push_front(value)头插，l.pop_back(value)尾删，l.pop_front(value)头删
```cpp
#include<iostream>
#include<list>
using namespace std;


void TestList1()
{
    //构造空的list
    list<int>L1;

    //构造10个值为1的list
    list<int>L2(10, 1);

    //构造用一段区间中的元素构造list
    int arr[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 0 };
    list<int> L3(arr, arr + sizeof(arr) / sizeof(arr[0]));

    //用L3取构造L4
    list<int>L4(L3);

    cout << "L1元素个数:" << L1.size() << endl;
    cout << "L2元素个数:" << L2.size() << endl;
    cout << "L3元素个数:" << L3.size() << endl;
    cout << "L4元素个数:" << L4.size() << endl;

    //遍历list1
    list<int>::iterator it1 = L1.begin();
    while (it1 != L1.end())
    {
        cout << *it1 << " ";
        ++it1;
    }
    cout << endl;

    //遍历list2
    list<int>::iterator it2 = L2.begin();
    while (it2 != L2.end())
    {
        cout << *it2 << " ";
        ++it2;
    }
    cout << endl;

    //遍历list3
    list<int>::iterator it3 = L3.begin();
    while (it3 != L3.end())
    {
        cout << *it3 << " ";
        ++it3;
    }
    cout << endl;

    //遍历list4
    list<int>::iterator it4 = L4.begin();
    while (it4 != L4.end())
    {
        cout << *it4 << " ";
        ++it4;
    }
    cout << endl;

    //逆向遍历list4
    list<int>::reverse_iterator it5 = L4.rbegin();
    while (it5 != L4.rend())
    {
        cout << *it5 << " ";
        ++it5;
    }
    cout << endl;
}
void TestList2()
{
    list<int>L;

    //尾插
    L.push_back(1);
    L.push_back(2);
    L.push_back(3);
    L.push_back(4);
    L.push_back(5);
    L.push_back(6);

    list<int>::iterator it = L.begin();
    while (it != L.end())
    {
        cout << *it << " ";
        ++it;
    }
    cout << endl;

    //尾删
    L.pop_back();
    L.pop_back();
    it = L.begin();
    while (it != L.end())
    {
        cout << *it << " ";
        ++it;
    }
    cout << endl;

    //头插
    L.push_front(0);
    it = L.begin();
    while (it != L.end())
    {
        cout << *it << " ";
        ++it;
    }
    cout << endl;

    L.pop_front();
    it = L.begin();
    while (it != L.end())
    {
        cout << *it << " ";
        ++it;
    }
    cout << endl;

    ////任意位置的插入
    //list<int>::iterator pos = find(L.begin(), L.end(), 2);
    //if (pos != L.end())
    //  pos = L.insert(pos, 9);
    //it = L.begin();
    //while (it != L.end());
    //{
    //  cout << *it << " ";
    //  ++it;
    //}
    //cout << endl;

    ////任意位置删除、
    //L.erase(pos);
    //it = L.begin();
    //while (it != L.end())
    //{
    //  cout << *it << " ";
    //  ++it;
    //}
    //cout << endl;

    //给list重新赋值
    int arr[] = { 1,2,3, 4, 5, 6, 7, 8, 9 };
    L.assign(arr, arr + sizeof(arr) / sizeof(arr[0]));
    it = L.begin();
    while (it != L.end())
    {
        cout << *it << " ";
        ++it;
    }
    cout << endl;
    cout << "第一个元素" << L.front() << endl;
    cout << "第一个元素" << L.back() << endl;


}

void TestList3()
{
    int arr[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
    list<int>L(arr, arr + sizeof(arr) / sizeof(arr[0]));
    list<int>::iterator it = L.begin();
    while (it != L.end())
    {
        cout << *it << " ";
        ++it;
    }
    cout << endl;

    L.remove(3); //按值删除
    it = L.begin();
    while (it != L.end())
    {
        cout << *it << " ";
        ++it;
    }
    cout << endl;

    class Odd
    {
    public:
        bool operator()(int value)
        {
            return value & 0x01;
        };
    };

    L.remove_if(Odd());
    it = L.begin();
    while (it != L.end())
    {
        cout << *it << " ";
        ++it;
    }
    cout << endl;

}

void TestList4()
{
    int arr[] = { 1, 2,  5, 3,1, 2, 3, 4, 5,7,6, 8, 9 };
    list<int>L(arr, arr + sizeof(arr) / sizeof(arr[0]));
    L.unique();

    list<int>::iterator it = L.begin();
    while (it != L.end())
    {
        cout << *it << " ";
        ++it;
    }
    cout << endl; 

    L.sort();
    it = L.begin();
    while (it != L.end())
    {
        cout << *it << " ";
        ++it;
    }
    cout << endl;

    L.unique();
    it = L.begin();
    while (it != L.end())
    {
        cout << *it << " ";
        ++it;
    }
    cout << endl;

    //class Three
    //{
    //public:
    //  bool operator()(int left, int right)
    //  {
    //      return 0 == (left + right) % 3;
    //  }
    //};
    //L.unique(Three());
    //it = L.begin();
    //while (it != L.end());
    //{
    //  cout << *it << " ";
    //  ++it;
    //}
    //cout << endl;
}

void TestList5()
{
    list<int>L;
    L.push_back(1);
    L.push_back(4);
    L.push_back(6);

    list<int>L2;
    L2.push_back(2);
    L2.push_back(3);
    L2.push_back(8);

    //将两个已序链表合并成为一个链表，合并后依然有序
    L.merge(L2);
    list<int>::iterator it = L.begin();
    while (it != L.end())
    {
        cout << *it << " ";
        ++it;
    }
    cout << endl;

    //反转链表
    L.reverse();
    it = L.begin();
    while (it != L.end())
    {
        cout << *it << " ";
        ++it;
    }
    cout << endl;
}
int main()
{

    //TestList1(); 构造及其遍历
    //TestList2();
    //TestList3();
    //TestList4();
    //TestList5();
    return 0;
}
```


