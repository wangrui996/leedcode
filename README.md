
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
  
* [4队列](#4队列)
  * [232用栈实现队列](#232用栈实现队列)

* [二分法](#二分法)





执行用时：
0 ms
, 在所有 C++ 提交中击败了
100.00%
的用户
内存消耗：
6.8 MB
, 在所有 C++ 提交中击败了
48.36%
的用户
	
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
遍历字符串，用一个栈存储还没有产生匹配的括号字符，栈顶永远存储的是最"内层"的括号字符；具体的，当每遍历一个字符的时候，先看下栈中元素是否为空，为空说明之前的字符都被“抵消”了，可以直接将当前字符放进去；如果不为空，则判断是否需要将当前字符入栈，如果当前字符不是右括号类型的字符或者当前字符与栈顶字符不成匹配关系，则将字符入栈，否则就将栈顶元素弹出  

需要注意的是匹配关系中，key和value的顺序不能反过来；因为"()"是匹配的但是“)(”不可以，而每种括号字符与前面的括号字符匹配时，一定是右括号为key来查找是否与前面的左括号value匹配；    

时间复杂度：O(N),正确匹配情况或全是左括号情况下需要遍历完字符串s  
空间复杂度：O(N),栈和哈希表使用线性空间大小  

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
            //如果栈为空，则直接入栈（后面可以看到这样做不太合理，因为如果是右括号且栈为空的话，可以直接返回false了）
			if(stk.empty())
            {
                stk.push(ch);
            }
			//栈不为空
			//判断  当前字符如果是左括号或者是右括号但不匹配，则入栈 (也不太合理因为如果是右括号，且与栈顶不匹配，那整个字符串肯定不合要求)
            else if(!pairs.count(ch) || pairs[ch] != stk.top())
            {
                stk.push(ch);
            }
            else    //如果成功匹配则让栈顶元素出栈
            {
                stk.pop();
            }
        }
        return stk.empty();
    }
};
```
该方法没有考虑可以提前结束遍历的情况，下面方法2对此进行优化，也是力扣官方的题解方法    

方法二:优化一下方法一  
与上面方法思路一致，但开始时先根据字符串长度决定是否需要遍历，因为如果是奇数直接返回false就好了(注意提示中说了最少含有一个字符，不需要把空字符串作为特殊情况处理)  
另外，如果当前字符是一个右括号，那么它必须在栈顶能够找到一个匹配的字符，否则说明该字符在它前面的字串中是没有匹配字符的，可以直接返回false而不需要将它入栈了，而且因为入栈也会导致后面不会再出现匹配的情况，最后栈一定不为空，和直接返回false是一样的；    

方法一虽然解决了问题但是进一步想下面的思路才更合理，栈中只会存放左括号字符，且栈顶一定是最内层的左括号，一旦遍历到右括号字符，在它前面一定要有与之匹配的左括号，且“从左到右”遍历到的右括号也一定是最内层的，只看它是否与栈顶字符匹配就好了    

时间复杂度：O(N),正确匹配情况或全是左括号情况下需要遍历完字符串s  
空间复杂度：O(N),栈和哈希表使用线性空间大小  

```cpp
class Solution {
public:
    bool isValid(string s) {
        int len = s.size();
        if(len % 2 == 1)
            return false;
        unordered_map<char, char> pairs = {
            {')', '('},
            {'}', '{'},
            {']', '['},
        };
        stack<char> stk;
        for(char ch : s)
        {
            if(pairs.count(ch))
            {
                if(stk.empty() || pairs[ch] != stk.top())
                    return false;
                else 
                    stk.pop();
            }
            else
            {
                stk.push(ch);
            }
        }
        return stk.empty();
    }
};
```
方法三:  
不用哈希表  

还是左括号就入栈，右括号就先判断栈中是否有元素，有的话按照匹配关系直接列举所有情况判断  

时间复杂度：O(N),正确匹配情况或全是左括号情况下需要遍历完字符串s  
空间复杂度：O(N),栈需要线性空间大小  

/*
执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户
内存消耗：6 MB, 在所有 C++ 提交中击败了94.12%的用户
*/
```cpp
class Solution {
public:
    bool isValid(string s) {
        int len = s.size();
        if(len % 2 == 1)
            return false;
        stack<char> stk;
        for(char ch : s)
        {
            //左括号直接入栈
            if(ch == '(' || ch == '{' || ch == '[')
                stk.push(ch);
            else
            {
                //当前字符为右括号的情况
                //如果栈为空直接false
                if(stk.empty())
                    return false;
                else if( (ch == ')' && stk.top() == '(') || (ch == '}' && stk.top() == '{') || (ch == ']' && stk.top() == '[') )
                    stk.pop();
                else
                    return false;
            }
        }
        return stk.empty();
    }
};
```

## 4队列  

### 232用栈实现队列    
[leedcode链接](https://leetcode-cn.com/problems/implement-queue-using-stacks/) 
题目描述：
请你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作（push、pop、peek、empty）：

实现 MyQueue 类：  

	void push(int x) 将元素 x 推到队列的末尾
	int pop() 从队列的开头移除并返回元素
	int peek() 返回队列开头的元素
	boolean empty() 如果队列为空，返回 true ；否则，返回 false  

说明：  

	你只能使用标准的栈操作 —— 也就是只有 push to top, peek/pop from top, size, 和 is empty 操作是合法的。
	你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。  

进阶： 

	你能否实现每个操作均摊时间复杂度为 O(1) 的队列？换句话说，执行 n 个操作的总时间复杂度为 O(n) ，即使其中一个操作可能花费较长时间。  

示例：  

	输入：
	["MyQueue", "push", "push", "peek", "pop", "empty"]
	[[], [1], [2], [], [], []]
	输出：
	[null, null, null, 1, 1, false]

	解释：
	MyQueue myQueue = new MyQueue();
	myQueue.push(1); // queue is: [1]
	myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
	myQueue.peek(); // return 1
	myQueue.pop(); // return 1, queue is [2]
	myQueue.empty(); // return false  

提示：  

	1 <= x <= 9
	最多调用 100 次 push、pop、peek 和 empty
	假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）  


思路分析：  
	用栈实现题目要求的队列，那么主要问题就是要考虑队列的队首和队尾分别在哪，并且由于队列和栈操作数据的方式不同仅用一个栈无法实现该功能因此用题目要求的两个栈表示队列。

这里记录下我思考的过程：
最开始考虑将一个栈作为队列的数据区，另一个栈在某些操作时临时存放队列元素;   
这样需要考虑队首是在栈顶还是栈底:  

方法一：  
队首在栈1的栈顶：  
出队pop()、取队首元素peak()、判断是否为空empty()的时间和空间复杂度是O(1);  
入队push(x)的时候需要先将栈1元素全部弹出到栈2，然后把新元素压入栈1，再把栈2中所有元素弹出并压入栈1；时间复杂度是O(n),空间复杂度O(n);  

时间复杂度：O(n)
空间复杂度：O(n),需要额外的一个栈来协助完成入队操作  


方法二：  
也是我的第一版答案  
当时还不知道摊还的概念，想的就是每次执行完入队或者出队后，如果紧接着再执行相同的操作就不要重复操作所有元素了。  
于是把队列myqueue设置成一个可变的，可能是栈1，也可能是栈2
栈1表示队列时，栈顶作为队首，栈2表示队列时，栈顶作为队尾；
初始化队列myqueue为stk1，每次执行相关操作时先根据myqueue是stk1或stk2来判断是否要操作所有元素，具体可看代码注释

但该方法在无法保证每次的操作是什么时，无法保证时间复杂度，我们主要看入队和出队：   
入队时元素可能在stk1也可能在stk2，所以最坏时间复杂度是O(n)，出队时同理

```cpp

class MyQueue {
public:
    MyQueue() {
        stk1 = new stack<int>();
        stk2 = new stack<int>();
        myqueue = stk1;
    }
    
    void push(int x) {
        if(myqueue == stk1) //入队操作，如果是队列是stk1(元素全部在stk1中)，先拿到stk2中
        {
            while(!stk1->empty())
            {
                stk2->push(stk1->top());
                stk1->pop();
            }
            myqueue = stk2;
        }
       
        myqueue->push(x); //将新元素压入栈，如果此时全部元素已经在栈2了则不需要动
    }
    
    int pop() {
        if(myqueue == stk2)
        {
            while(!stk2->empty())
            {
                stk1->push(stk2->top());
                stk2->pop();
            }
            myqueue = stk1;
        }
        int front = myqueue->top();
        myqueue->pop();
        return front;
    }
    
    int peek() {
        if(myqueue == stk2)
        {
            while(!stk2->empty())
            {
                stk1->push(stk2->top());
                stk2->pop();
            }
            myqueue = stk1;
        }
        return myqueue->top();
    }
    
    bool empty() {
        if(myqueue->empty())
            return true;
        else
            return false;
    }

private:
    stack<int> *stk1;
    stack<int> *stk2;
    stack<int> *myqueue;
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```


方法三：

把队首设置在栈1的栈尾：  

方法二虽然在执行重复操作时还可以，但是如果入队出队交替进行，需要不断移动全部元素。考虑栈1的栈顶始终作为队尾，入队时一直在栈1进行，出队操作将栈1全部元素放入栈2(栈顶变栈尾)，后面执行出队一直从栈2的栈顶弹出，入队仍然在栈1；当出队时发现栈2元素为0时，再从栈1中全部元素放入栈2；

入队push(x)、判断是否为空empty()的时间复杂度为O(1);  
出队pop()或者取首元素peak()：先把栈1中所有元素弹出并放到栈2，此时栈2的栈顶元素是栈1的栈尾元素，也就是队首元素；但是注意，在执行完一次出队或取首元素操作后，我们不将栈2所有元素再放回去，因为下一次操作如果还是出队或取首元素，那么仍然对全部元素进行上述操作，这样出队或取首元素的时间复杂度就是O(n); 利用摊还的思想，我们执行一次出队或者取首元素操作后，栈2中的元素已经非常适合出队和取首元素的操作了，因此我们考虑不将全部元素再放回去，此时如果出队或取首元素就从栈2的栈顶操作，如果入队直接向栈1的栈顶压入即可；另外，当执行出队或者取首元素时发现栈2为空时，就再将栈1的全部元素放到栈2； 这样的话，最坏时间复杂度是O(n),摊还复杂度O(1);空间复杂度O(1)；
进一步的，如果我们在首次执行入队操作时记录该元素为队首front，在之后执行入队、判断是否为空和取首元素时不改变它；在出队完成后，重新将栈2新的栈顶元素置为front；这样其它操作的复杂度不变，取首元素的时间复杂度为O(1)(不记录的话最坏情况下取首元素时要操作所有元素)


```cpp
class MyQueue {
public:
    MyQueue() {
        stk1 = new stack<int>();
        stk2 = new stack<int>();
    }
    //入队时向stk1压入
    void push(int x) {
		stk1->push(x);	 
    }
    //由于pop()和peek()有重复的操作，这里复用peek()的操作先获取队首元素
    int pop() {		
		int front = peek();
        stk2->pop(); 
        return front;
    }
   
    int peek() {
        if(stk2->empty())
		{
			while(!stk1->empty())
            {
                stk2->push(stk1->top());
                stk1->pop();
            }
		}
        int front = stk2->top();  
        return front;
    }
    
    bool empty() {
        if(stk1->empty() && stk2->empty())
            return true;
        else
            return false;
    }
private:
    stack<int> *stk1;
    stack<int> *stk2;
};
/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
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

## 二分法  

