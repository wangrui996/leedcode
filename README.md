
STL学习

[Cplusplus.com](https://www.cplusplus.com/)

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
  * [206反转链表](#206反转链表)

* [3栈](#3栈)
  * [20有效的括号](#20有效的括号)
  

	
* [3剑指offer](#3剑指offer)
  * [03数组中重复的数字](#03数组中重复的数字)
  * [06从尾到头打印链表](#06从尾到头打印链表)
  * [24反转链表](#24反转链表)
  
 
 
	

## 1数组  

## 2链表    
### 206反转链表  
同剑指offer24


## 3栈   

### 20有效的括号  
[leedcode链接](https://leetcode-cn.com/problems/valid-parentheses) 

题目描述：
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。  

有效字符串需满足：  

左括号必须用相同类型的右括号闭合。  
左括号必须以正确的顺序闭合。  

示例 1：

	输入：s = "()"
	输出：true
示例 2：

	输入：s = "()[]{}"
	输出：true
示例 3：

	输入：s = "(]"
	输出：false
示例 4：

	输入：s = "([)]"
	输出：false
示例 5：

	输入：s = "{[]}"
	输出：true

提示：

	1 <= s.length <= 104
	s 仅由括号 '()[]{}' 组成

方法一:
利用栈后进先出的特点
遍历字符串，用一个栈存储还没有产生匹配的括号字符，栈顶永远存储的是最"内层"的括号字符；具体的，当每遍历一个字符的时候，先看下栈中元素是否为空，为空说明之前的字符都被“抵消”了，可以直接将当前字符放进去；如果不为空，则判断它与栈顶的括号字符是否是匹配关系，匹配的话就弹出栈顶元素，不匹配的话就将当前字符入栈；
需要注意的是匹配关系中，key和value的顺序不能反过来；因为"()"是匹配的但是“)(”不可以，而每种括号字符与前面的括号字符匹配时，一定是右括号为key来查找是否与前面的左括号value匹配；

```cpp
class Solution {
public:
    bool isValid(string s) {
        unordered_map<char, char> pairs = {
            {')', '('},
            {'}', '{'},
            {']', '['},
        };
        if(s.empty())
            return true;
        stack<char> stk;
        for(char ch : s)
        {
            if(stk.empty())
            {
                stk.push(ch);
            }
            else if(pairs[ch] == stk.top())
            {
                stk.pop();
            }
            else
            {
                stk.push(ch);
            }
        }
        if(stk.empty())
            return true;
        else
            return false;
    }
};
```

## 3剑指offer


### 03数组中重复的数字
[leedcode链接](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof) 
题目描述：
找出数组中重复的数字。  

在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。 

示例 1：
    
    输入：
    [2, 3, 1, 0, 2, 5, 3]
    输出：2 或 3   



限制：  

    2 <= n <= 100000  


方法一:
对数组进行排序后，重复数字是紧挨着的，遍历数组即可
但是该方法需要对原数组进行排序
时间复杂度：
空间复杂度：
```cpp
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        //数组排序后重复数字一定是在一起的
        sort(nums.begin(), nums.end());
        for(int i = 1; i < nums.size(); i++)
        {
            if(nums[i] == nums[i-1])
            {
                return nums[i];
            }
        }
        return -1;
    }
};
```





### 06从尾到头打印链表  
[06从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)  
输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。  
方法一：考虑只遍历一次链表的话，栈具有后进先出的特点因此先放入栈，获取栈的size，创建一个大小相同的数组，循环弹出栈的元素到数组。  
时间复杂度：遍历一遍链表O(n),再遍历一次栈放到数组，O(n),所以时间复杂度是O(n)  
空间复杂度：O(n) ,要额外用一个栈保存每个链表每个节点的值  
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

方法二：直接使用数组vector存放链表元素。首先遍历一次链表，记录元素个数n，之后创建一个size为n的数组，重新遍历链表并从数组后到前存放元素(或者遍历链表同时按顺序拿出元素到数组，最后利用reverse反转数组内容)。
时间复杂度：O(n)    
空间复杂度：O(1)  一般来说输出结果数组不算再额外空间占用里面

法三：递归  	                           
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
        //递归解法
        if(!head)
        {
            vector<int> v;
            return v;
        }
        vector<int> v = reversePrint(head->next);
        v.push_back(head->val);
        return v;
    }
};
```							   
					
### 24反转链表							   
  本题同leedcode206
  [视频求解教程](https://leetcode-cn.com/problems/reverse-linked-list/solution/shi-pin-jiang-jie-die-dai-he-di-gui-hen-hswxy/)
方法一：迭代解法  
将当前节点的next指向前一个节点，因此需要一个prev和cur指针分别指向当前和前一个节点，cur初始值为头节点，prev初始值为nullptr，当cur为nullptr时，返回prev即可； 另外需要注意在修改cur的next指向时先保存一下，否则迭代时就找不到下一个节点了。
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
    ListNode* reverseList(ListNode* head) {
        //迭代
        //将当前节点的next指向前一个节点，因此需要有个prev节点，和一个cur节点
        ListNode *prev = nullptr;
        ListNode *cur = head;
        while(cur)
        {
            ListNode * next = cur->next;
            cur->next = prev;
            prev = cur;
            cur = next;
        }
        return prev;
    }
};
```  
方法二：递归解法  
递归解法需要符合三个条件  
1.一个大问题能否拆解成两个子问题  
2.单独看子问题，其解决方式与大问题相同  
3.存在最小子问题  

对于反转一个单链表，可以拆解成两个子问题，一个是解决头节点对应的问题(反转头节点)；一个是剩余节点组成的问题(将剩余节点反转)；我们看到，头节点是一个节点不需要反转，解决第二个子问题以后，"归"的过程中，整个问题就变成了反转当前头节点和剩余节点组成的整体，其实是反转两个节点；  
对于子问题，求解方式可以与大问题一致(具有相同结构)；拆到最后，要解决的子问题只剩一个节点时，发现它不需要反转，就找到了最小最问题  

解决思路：  
"递"的过程不断拆解问题规模，到最后只有一个节点时，执行"归"的过程：两个节点的反转，head->next->next = head; head = nullptr;解决完两个节点子问题后继续往回到3个节点的子问题，此时后两个节点已经反转完成，只需要继续执行head->next->next = head; head = nullptr;反转当前头节点和下一个节点  
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
    ListNode* reverseList(ListNode* head) {
        //递归
        //终止条件：空节点或者"递"到了最后一个节点(只有一个节点不需要反转，或者说反转完成了已经)
        if(head == nullptr || head->next == nullptr)
        {
            return head;
        }
        //“归”的时候需要返回一个已经反转后的链表
        ListNode *reverse = reverseList(head->next); //不断调用该函数完成“递”的过程
        //“归”的过程
        head->next->next = head;
        head->next = nullptr;
        //执行完上两步，当前子问题即解决，需要返回的是当前已经反转好的链表，也就是将reverse作为头节点返回
        return reverse;
    }
};
```  

