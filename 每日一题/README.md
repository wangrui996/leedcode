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