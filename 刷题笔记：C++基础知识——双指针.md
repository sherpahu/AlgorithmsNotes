# PAT基础知识——双指针

# 双指针总结
[https://zhuanlan.zhihu.com/p/95747836](https://zhuanlan.zhihu.com/p/95747836)
## 快慢指针
类似于龟兔赛跑，两个链表上的指针从同一节点出发，其中一个指针前进速度是另一个指针的两倍。利用快慢指针可以用来解决某些算法问题，比如

1. 计算链表的中点：快慢指针从头节点出发，每轮迭代中，快指针向前移动两个节点，慢指针向前移动一个节点，最终当快指针到达终点的时候，慢指针刚好在中间的节点。
1. 判断链表是否有环：如果链表中存在环，则在链表上不断前进的指针会一直在环里绕圈子，且不能知道链表是否有环。使用快慢指针，当链表中存在环时，两个指针最终会在环中相遇。
1. 判断链表中环的起点：当我们判断出链表中存在环，并且知道了两个指针相遇的节点，我们可以让其中任一个指针指向头节点，然后让它俩以相同速度前进，再次相遇时所在的节点位置就是环开始的位置。
1. 求链表中环的长度：只要相遇后一个不动，另一个前进直到相遇算一下走了多少步就好了
1. 求链表倒数第k个元素：先让其中一个指针向前走k步，接着两个指针以同样的速度一起向前进，直到前面的指针走到尽头了，则后面的指针即为倒数第k个元素。（严格来说应该叫先后指针而非快慢指针）

如果有其它的想法，可以评论区评论一下~
## 碰撞指针
一般都是**排好序的数组或链表**，否则无序的话这两个指针的位置也没有什么意义。特别注意两个指针的循环条件在循环体中的变化，小心右指针跑到左指针左边去了。常用来解决的问题有

1. 二分查找问题
1. n数之和问题：比如两数之和问题，先对数组排序然后左右指针找到满足条件的两个数。如果是三数问题就转化为一个数和另外两个数的两数问题。以此类推。
## 滑动窗口法
两个指针，一前一后组成滑动窗口，并计算滑动窗口中的元素的问题。
这类问题一般包括

1. 字符串匹配问题
1. 子数组问题
# leetcode167
碰撞指针（n数之和问题）
[https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)
题目描述：在有序数组中找出两个数，使它们的和为 target。
使用双指针，一个指针指向值较小的元素，一个指针指向值较大的元素。指向较小元素的指针从头向尾遍历，指向较大元素的指针从尾向头遍历。

- 如果两个指针指向元素的和 sum == target，那么得到要求的结果；
- 如果 sum > target，移动较大的元素，使 sum 变小一些；
- 如果 sum < target，移动较小的元素，使 sum 变大一些。
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        vector<int> ans;
        int i=0,j=numbers.size()-1;
        while(i<j){
            if(numbers[i]+numbers[j]<target){
                i++;
            }else if(numbers[i]+numbers[j]>target){
                j--;
            }else{
                ans.push_back(i+1);
                ans.push_back(j+1);
                return ans;
            }
        }
        return ans;
    }
};
```
# leetcode633
[https://leetcode-cn.com/problems/sum-of-square-numbers/](https://leetcode-cn.com/problems/sum-of-square-numbers/)
与上题类似，换汤不换药，n数之和的变种，将“和”变为了“平方和”。

```cpp
class Solution {
public:
    bool judgeSquareSum(int c) {
        long long i=0,j=(long long)(sqrt(c));
        while(i<=j){
            long long sum=i*i+j*j;
            if(sum==c){
                return true;
            }else if(sum>c){
                j--;
            }else{
                i++;
            }
        }
        return false;
    }
};
```
只能使用long long，否则会溢出
# leetcode345
[https://leetcode-cn.com/problems/reverse-vowels-of-a-string/description/](https://leetcode-cn.com/problems/reverse-vowels-of-a-string/description/)
典型碰撞指针

```cpp
class Solution {
public:
    string reverseVowels(string s) {
        bool isYuanyin[200];
        isYuanyin['a']=true;
        isYuanyin['e']=true;
        isYuanyin['i']=true;
        isYuanyin['o']=true;
        isYuanyin['u']=true;
        isYuanyin['A']=true;
        isYuanyin['E']=true;
        isYuanyin['I']=true;
        isYuanyin['O']=true;
        isYuanyin['U']=true;
        int i=0,j=s.size()-1;
        char str[1000000];
        strcpy(str,s.c_str());
        while(i<j){
            if(isYuanyin[str[i]]==true&&isYuanyin[str[j]]==true){
                char tmp=str[i];
                str[i]=str[j];
                str[j]=tmp;
                i++;
                j--;
            }else if(isYuanyin[str[i]]==true&&isYuanyin[str[j]]!=true){
                j--;
            }else if(isYuanyin[str[i]]!=true&&isYuanyin[str[j]]==true){
                i++;
            }else if(isYuanyin[str[i]]!=true&&isYuanyin[str[j]]!=true){
                i++;
                j--;
            }
        }
        s=str;
        return s;
    }
};
```
# leetcode680
对撞指针
将判断某一段是否为回文串作为函数单独提出来，删除某一字符即左区间起始右移，右区间终点左移。
```cpp
class Solution {
public:
    bool isPalindrome(string s,int start,int end){
        int i=start,j=end;
        while(i<j){
            if(s[i]==s[j]){
                i++;
                j--;
            }else{
                return false;
            }
        }
        return true;
    }
    bool validPalindrome(string s) {
        int i=0,j=s.size()-1;
        while(i<j){
            if(s[i]==s[j]){
                i++;
                j--;
            }else{
                return isPalindrome(s,i+1,j)|isPalindrome(s,i,j-1);
            }
        }
        return true;
    }
};
```
# leetcode88
快慢指针，都指向数组的开始。但要注意当某一个数组遍历完成后还需要将剩余的元素添加进去
[https://leetcode-cn.com/problems/merge-sorted-array/](https://leetcode-cn.com/problems/merge-sorted-array/)
## 需要额外开辟空间的做法（从头到尾）
```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        vector<int> ans;
        int i=0,j=0;
        while(i<m&&j<n){
            if(nums1[i]<nums2[j]){
                ans.push_back(nums1[i]);
                i++;
            }else{
                ans.push_back(nums2[j]);
                j++;
            }
        }
        if(i!=m){
            while(i<m){
                ans.push_back(nums1[i]);
                i++;
            }
        }
        if(j!=n){
            while(j<n){
                ans.push_back(nums2[j]);
                j++;
            }
        }
        nums1=ans;
    }
};
```
## 不需要额外开辟空间（从尾到头）
```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int ttl=m+n-1;//合并完成后最后一个元素的索引
        m--;//num1最后一个元素索引是m-1
        n--;
        while(m>=0||n>=0){
            if(n<0){
                nums1[ttl--]=nums1[m];
                m--;
            }else if(m<0){
                nums1[ttl--]=nums2[n];
                n--;
            }else if(nums1[m]<nums2[n]){
                nums1[ttl--]=nums2[n];
                n--;
            }else if(nums1[m]>=nums2[n]){
                nums1[ttl--]=nums1[m];
                m--;
            }
        }

    }
};
```
# leetcode141
快慢指针，慢指针走一步，快指针走两步，当有环时两个指针一定会变得相等。
其实不一定是两步，一个一直走得快，一个一直走得慢，两者必然有一时刻会在环上相遇
[https://leetcode-cn.com/problems/linked-list-cycle/](https://leetcode-cn.com/problems/linked-list-cycle/)
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if(head==NULL){
            return false;
        }
        ListNode *l1=head,*l2=head->next;
        while(l2!=NULL){
            if(l1==l2){
                return true;
            }else{
                l1=l1->next;
                if(l2->next!=NULL)
                    l2=l2->next->next;
                else
                    return false;
                //l2->next为NULL时l2->next没有next所以l2=l2->next->next;会报错
            }
        }
        return false;
    }
};
```

