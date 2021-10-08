# 每日一题  

## 0482. 密钥格式化
[leecode链接](https://leetcode-cn.com/problems/license-key-formatting/)
```cpp
class Solution {
public:
    string licenseKeyFormatting(string s, int k) {    
        string ss, ans;
        //将原字符串替换为没有'-'且都为大写字母的字符串
        for(char c : s)
        {
            if(c == '-')
                continue;
            if(c <= 'z' && c >= 'a')
                c += 'A' - 'a';
            ss += c;       
        }
        //计算第一部分有多少个字符串 
        int first = ss.size() % k;
        //
        if(first != 0)
        {
            ans += ss.substr(0, first);
            ans += '-';
        }
        //处理ss中剩余的部分，每k个一组取出来作为子串拼接到ans后，并添加一个'-'
        for(int i = first; i < ss.size(); i+=k)
        {
            ans += ss.substr(i, k);
            ans += '-';
        }
    //如果只有first部分，那么ans最后一个字符为'-'，如果有后面的部分，最后一个字符也为'-',因此最后只取前n-1个字符
        return ans.substr(0, ans.size() - 1);
    }
};
```

### 





### 0414.第三大的数  

#### 方法1：set集合  
题目要求了重复的元素只算一次，想法是同时维护一个保存有三个元素的有序序列，标准库的set集合能自动排序，且能够去除重复元素，符合我们的要求，每次向set集合插入一个元素，如果元素个数大于3，就把最小的删除掉,最后根据集合的大小决定返回第三大的数还是最大的数    

时间复杂度:O(n)
空间复杂度:O(1)
```cpp
class Solution {
public:
    int thirdMax(vector<int>& nums){ 
        int n = nums.size();
        set<int> s;
        for(int i = 0; i < n; i++)
        {
            s.insert(nums[i]);
            if(s.size() > 3)
            {
                s.erase(*s.begin());
            }
        }
        if(s.size() == 3)
            return *s.begin();
        else
            return *s.rbegin();
    }
};
```


#### 方法2：设置3个观察变量分别记录最大，次大，第三大的数字 

在一个序列中找最大和第二大的数的经典思路是，用两个变量分别代表最大和次大的数，遍历数组根据数组元素与这两个变量的比较结果决定是否修改两个变量。  

注意点：注意题目中是否将重复元素算上，即1,2,2在该题中是无第三大的值的  

这里也是一样的思路：设置变量first，second，third分别代表最大，第二大和第三大的数字，遍历nums数组
初始值设置：题目说了数组元素的范围是[-2^31, 2^31-1],这三个变量的初始值应该是一个足够小的数，至少要比数组可能的最小值还要小
1.若nums[i] > first, 则  

    third = second;
    second = first;
    first = nums[i];

2.若  second < nums[i] < first  (因为不算重复元素，因此次大的值不光要大于second，还要小于first，排除等于first的情况),则  

    thrid = second；
    second = nums[i];

3.若 third < nums[i] < second;则  

    third = nums[i];
遍历完成后，根据third的值是否为"无穷小"判断返回third还是first。

```cpp
class Solution {
public:
    int thirdMax(vector<int>& nums){ 
        const long long INF = LONG_MIN;
        long first = INF, second = INF, third = INF;
        for(int i = 0; i < nums.size(); i++)
        {
            if(nums[i] > first)
            {
                third = second;
                second = first;
                first = nums[i];
            }
            else if(nums[i] > second && nums[i] < first)
            {
                third = second;
                second = nums[i];              
            }
            else if(nums[i] > third && nums[i] < second)
            {
                third = nums[i];
            }
        }
        if(third != INF)
            return third;
        else
            return first;
    }
};
```



