# KMP算法  

解决经典的字符串匹配问题  

[KMP算法详解](https://blog.csdn.net/v_july_v/article/details/7041827)  
[KMP算法详解](https://www.zhihu.com/question/21923021)


## 问题描述  

假设一个文本串s = "aabaabaaf"   一个模式串 sub =  "aabaaf"  问文本串中含不含模式串  

##　暴力解法　　




## KMP算法  

对于一个模式串sub  
**前缀：** 模式串中包含首字母但不包含尾字母的所有字串 对于这个模式串，就是a  aa  aab  aaba  aabaa 五个
**后缀：** 模式串中包含尾字母但不包含首字母的所有字串，对于这个模式串，是f  af  aaf   baaf  abaaf 五个  

**最长相等前后缀** ：就是求最长 相等的前后缀    

那么，对于模式串sub，它的所有字串的最长相等前后缀长度分别是多少？

* a：     0    它的最长相等前后缀 0  
* aa ：   1    前缀a  后缀a
* aab :   0  
* aaba :  1  
* aabaa : 2
* aabaaf : 0

**模式串的前缀表** ：010120  即使各个子串的最长相等前后缀长度组成的序列    

###　使用前缀表进行匹配　　

1.从文本串s首字符开始与模式串匹配，当遇到某个不匹配字符时，查找该字符之前部分的模式串的最长相等前后缀是多少，如下面情况，当模式串中f与文本串中b不匹配时，模式串前面的字串中(aabaa),拥有的最长相等前后缀长度是2，2意味着，aabaa中前面个2字符前缀和后面的2字符前缀相等(在这里是aa)
aabaabaaf  
aabaaf
     ^
010120

2.找到了最长相等前后缀长度后，我们实际上是可以将模式串向后移动，将最长相等前后缀的前缀移动到后缀位置，同时当前继续从刚才指针停的位置继续比较 ；但注意，我们写代码时，指针是可以移动的，但是字符串没法在内存中像下面那样向后移动来与上面的文本串重新保持有一部分对齐(时间和空间成本大)   
aabaabaaf  
   aabaaf 
     ^

3.因此我们在设置指针的时候，移动的是模式串中的指针  
当得到模式串一个位置与文本串不匹配时，去看它的前一位在前缀表中的值，得到它前面的子串(aabaa)的最长相等前后缀长度2   此时我们根据2的思路可以知道，只需要继续比较模式串中b的字符和刚才文本串中的位置，这个**模式串中要移动到的新的位置对应的索引是2，正好等于最长相等前后缀长度2**  

**next数组** ： **前缀表，它告诉我们在遇到冲突时应该回退到哪里**


4.前缀表怎么求：代码部分  next数组保存  

* next数组保存的是前缀表本身  当遇到冲突时，根据模式串冲突字符的前一个字符对应的前缀表中的值，决定要移动到的新的位置，上面这个例子就是2  

* next数组是前缀表向右移动一位，首位置为-1，遇到冲突时，直接取冲突位置对应的前缀表值  next [-11012]  

* next数组是原前缀表减1  next [-1 0 -1 0 1 -1]  还是看冲突位置的前一位，但是会把前一位值加上1   

总之**next数组解决**的就是**遇到冲突以后回退的位置**


5.next




### 个人理解

kmp算法关键在于：在当前对文本串和模式串检索的过程中，若出现了不匹配，如何充分利用已经匹配的部分。

匹配到不正确的地方以前的子串跟上面的子串相等所以下面的子串的最长前缀肯定跟上面最长子串的后缀有匹配。  




## KMP代码  


### 求解next数组  

对于一个模式串s

1.初始化  

* 定义两个指针i和j， j指向前缀起始位置，初始化为0   i指向后缀起始位置，从i遍历到s.size() - 1;(后缀起始位置范围就是[1, s.size()-1])  

* next数组初始化  next[0] = j;


2.处理前后缀不相同的情况  

也就是 s[i] != s[j] ： 此时j要回退，回退的位置是查询next[j-1]，但是不止回退一次，要一直循环，但是j为0的时候，next[j-1]会越界，因此while循环条件还要加个j > 0    

**就是说：** 遇到不相等情况，j一直回退到0位置或者到他俩相同的地方  

3.处理前后缀相同的情况
上面循环结束后，判断是否相等，因为上面的循环可能因为是s[i] == s[j]停下，也可能是因为j==0停下了，此时s[i]和是s[j]不知道是否相等  

if(s[i] == s[j]) : 说明找到了相同前后缀，同时向后移动i和j，因为i在for循环会移动所以循环体里面只要 j++;  

4.next数组赋值  

处理完上述两种情况后，要对next数组进行赋值  next[i] = j;

```cpp
void getNext(int* next, const string& s) {
    int j = 0;
    next[0] = 0;
    for(int i = 1; i < s.size(); i++) {
        while (j > 0 && s[i] != s[j]) { // j要保证大于0，因为下面有取j-1作为数组下标的操作
            j = next[j - 1]; // 注意这里，是要找前一位的对应的回退位置了
        }
        if (s[i] == s[j]) {
            j++;
        }
        next[i] = j;
    }
}
```  


### 匹配  

1.定义两个下表i，j分别指向文本串s和模式串sub起始位置  

i从0开始循环遍历文本串  

```cpp 
for (int i = 0; i < s.size(); i++)
```

2.循环比较s[i]与sub[j]  直接两者相等或j退回到0处  

* 如果s[i] != sub[j]  **j就要回退寻找下一个匹配的位置，这是KMP算法的核心**，回退位置是从next[j-1]处找，那么仍然是注意j > 0否则j等于0了但是s[i] != sub[j]，导致next[j-1]越界  

3.判断s[i] 是否等于 sub[j]  
while循环结束后:  

* if(s[i] == sub[j])  j++  i也要向后移动只不过在for循环做  

4.判断文本串s中是否出现了模式串sub  

如果j指向了模式串的末尾，说明模式串sub完全匹配了文本串s中的某个子串  

也就是j == sub.size() - 1时，s[i] == sub[j]  因为此时j要++，所以最后判断的是j是否等于sub.size(),等于就说明匹配成功  

此时，我们返回(或者)文本串中匹配子串出现的首位置，即 return (i - sub.size() + 1)  



```cpp
int strStr(string s, string sub) {
    if (sub.size() == 0) {
        return 0;
    }
    int next[sub.size()];
    getNext(next, sub);
    int j = 0;
    for (int i = 0; i < s.size(); i++) {
        while(j > 0 && s[i] != sub[j]) {
            j = next[j - 1];
        }
        if (s[i] == sub[j]) {
            j++;
        }
        if (j == sub.size()) {
            return (i - sub.size() + 1);
        }
    }
    return -1;
}
```


## 总结   

下面是力扣官方提到的KMP算法的三点疑问，检验学习效果

* 1.设查询串的的长度为 nn，模式串的长度为 mm，我们需要判断模式串是否为查询串的子串。那么使用 KMP 算法处理该问题时的时间复杂度是多少？在分析时间复杂度时使用了哪一种分析方法？

时间复杂度为 O(n+m)O(n+m)，用到了均摊分析（摊还分析）的方法。

具体地，无论在预处理过程还是查询过程中，虽然匹配失败时，指针会不断地根据 fail 数组向左回退，看似时间复杂度会很高。但考虑匹配成功时，指针会向右移动一个位置，这一部分对应的时间复杂度为 O(n+m)O(n+m)。又因为向左移动的次数不会超过向右移动的次数，因此总时间复杂度仍然为 O(n+m)O(n+m)。



* 2.如果有多个查询串，平均长度为 nn，数量为 kk，那么总时间复杂度是多少？  
时间复杂度为 O(nk+m)O(nk+m)。模式串只需要预处理一次。

* 3.在 KMP 算法中，对于模式串，我们需要预处理出一个 \textit{fail}fail 数组（有时也称为 next数组、π 数组等）。这个数组到底表示了什么？
fail[i] 等于满足下述要求的 x 的最大值：s[0:i]s[0:i] 具有长度为 x+1 的完全相同的前缀和后缀。这也是 KMP 算法最重要的一部分。





先是暴力解————>时间复杂度太高，想办法优化————>每一次匹配失败，假设发生文本串s的r+1处，那么s[i]到s[r-1]和模式串某个前缀是相等的————>暴力解法是再从s[i+1]处，模式串的j从0处重新依次比较—————>实际上通过举例发现，s[i+1]与sub[0]可能根本不相等，是没有必要从i+1处开始的  

**一个简单的想法是，起码让i向后移动到s[i]和sub[0]相等**  进而考虑，此时s[i+1]和sub[1]就相等吗，不一定     


**问题的关键在于：当发生冲突时，怎样利用已经失败的信息**

***继续分析** ： 

1.如下图  在i=5处发生了匹配失败，此时我们有个**重要信息**是**前面的匹配成功的串**   

![image](https://user-images.githubusercontent.com/58176267/157580463-51574a92-f450-4589-b044-0de270f958c3.png)

2.我们试着移动一下i  i等于0开始匹配，发现在i=5时冲突，那么令i=1，j=0重新匹配:,发现用暴力方法的时候在i=2 j=2时又冲突

![image](https://user-images.githubusercontent.com/58176267/157580599-3b3f8419-62e6-47ca-8350-ce107f31eb47.png)

3.继续用暴力的方法移动i，当i=3的时候，暴力匹配有点不一样了，因为我们发现此时文本串的aa和模式串的aa正好是匹配的，

![image](https://user-images.githubusercontent.com/58176267/157580905-e2a0b757-8131-41b9-96e9-20d87f9546f3.png)

那么此时实际上i可以从下图的位置开始即可对吧，**此时发现，i不就是在第一次匹配冲突的地方吗？**，当第一次i=0时，i在索引5的地方发生冲突，现在折腾了这么久，i又回到了这里，那**以后发生冲突的时候i是不是可以不用动了？**   

![image](https://user-images.githubusercontent.com/58176267/157581052-5c59aff7-4d0a-40cd-a084-b7cbf111b37e.png)

再看j，j一开始在j = 5(字符f)处发生冲突，折腾了这么久以后，j是向前回退了？那么如果以后发生冲突时i不动，那j回退到哪里合适呢？ **这就是KMP算法的核心所在：要利用上已经有的信息来做决定** ，第一次发生冲突时，我们得到了一个已经匹配的子串"aabaa"对吧 ，看下面两张图，一个是移动前，一个是移动后；因为我们**希望尽可能多的利用已经匹配好的子串"aabaa"，那么移动后的位置，就需要模式串中的aabaa的某个前缀(这里是aa)和文本串的子串“aabaa”的某个后缀(这里是aa)相等，又因为文本串和模式串匹配好的子串是一样的(不然怎么叫匹配呢)，所以只分析模式串即可，也就是j=5时，去找模式串中sub[0:4]这个子串的相等的前后缀；

![image](https://user-images.githubusercontent.com/58176267/157581878-318a0295-0ef2-4e2e-9969-6073db32cd40.png)

![image](https://user-images.githubusercontent.com/58176267/157581892-4ef6d2e2-a873-4f3a-89a3-4be4691d55a3.png)

虽然我们希望在发生错误匹配时尽可能快的向后跳，但不希望错过某些匹配，实际上要想跳的快，这个相等的前后缀长度越小越快，但是我们利用KMP算法求的时候，此时要知道的是"aabaa"中，最长相等前后缀；比如这里，子串aabaa中，前后缀a和a也是相等的，但不能直接跳到下面的情况，因为上图的情况也有可能发生匹配，这就是为什么要找最长相等前后缀    

![image](https://user-images.githubusercontent.com/58176267/157582546-0cedaade-71a3-48c7-a037-02c0a49ab10e.png)

找到最长相等前后缀以后，j回退多少呢？ 实际上在j处发生冲突，j应该回退到next[j-1],结合这个例子很容易理解  

![image](https://user-images.githubusercontent.com/58176267/157582887-d82a2672-3e35-422b-87d3-4b624f52ee9c.png)

由于整个匹配过程中，这个最长相等前后缀的长度可能会频繁用到，而且是模式串的任意子串都要知道它的最长相等前后缀，因此才有了KMP算法中经典的next数组，来保存这个序列，可以称为前缀表  


