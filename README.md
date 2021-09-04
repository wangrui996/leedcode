
STL学习

vector


注意：如果初始化时没有指定数组大小，如vector<int> v; 后面没有使用resize，那数组大小就是0，没法直接用v[i]向数组添加元素，只能用push_back(val)，此时由于size是0，容量也是0，会先扩充容




<algorithm>库  

1.reverse 反转容器中的内容  
函数原型如下，使用std::iter_swap()交换两个元素，从实现可以看出，反转的范围是[first， last)；时间复杂度是线性的。
 ```cpp
template <class BidirectionalIterator>
  void reverse (BidirectionalIterator first, BidirectionalIterator last)
{
  while ((first!=last)&&(first!=--last)) {
    std::iter_swap (first,last);
    ++first;
  }
}
```



# leedcode  
* [1数组](#1数组)
  * [](#)
  * [](#)
  * [](#)
  * [](#)
* [2链表](#2链表)
  * [1](#1)
  * [2剑指offer06_从尾到头打印链表](#2剑指offer06_从尾到头打印链表)

## 1数组  

## 2链表    

### 2剑指offer06_从尾到头打印链表  
[2剑指offer06_从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)  
输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。  
法一：考虑只遍历一次链表的话，栈具有后进先出的特点因此先放入栈，获取栈的size，创建一个大小相同的数组，循环弹出栈的元素到数组。
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
    vector<int> reversePrint(ListNode* head) {
        stack<int> s;
        vector<int> print;
        ListNode* curNode = head;
        if(!head)
        {
            return print;
        }
        while(curNode != nullptr)
        {
            s.push(curNode->val);
            curNode = curNode->next;
        }
        int size = s.size();
        for(int i = 0; i< size; i++)
        {
            print.push_back(s.top());
            s.pop();
        }
        return print;
    }
};
```

法二：直接使用数组vector存放链表元素。首先遍历一次链表，记录元素个数n，之后创建一个size为n的数组，重新遍历链表并从数组后到前存放元素(或者遍历链表同时按顺序拿出元素到数组，最后利用reverse反转数组内容)。
















