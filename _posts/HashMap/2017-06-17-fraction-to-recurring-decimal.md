---
layout: post
title:  分数转换为小数形式
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 166: [Fraction to Recurring Decimal](https://leetcode.com/problems/fraction-to-recurring-decimal/description/)
## 2. 描述：
给定两个分别代表分子和分母的整数，返回字符串形式的分数。 如果分数的一部分是重复的，则使用括号将重复的部分包围起来。
## 3. 示例：
```
分子为1， 分母为2， 返回“0.5”
分子为2， 分母为1， 返回“2”
分子为2， 分母为3，返回“0.(6)”
```
## 4. 解决方案：
```
string fractionToDecimal(int numerator, int denominator) {
    if(!numerator) return "0";
    string res;
    if(numerator < 0 ^ denominator < 0) res += "-";
    long numer = (long)numerator < 0 ? ((long)numerator * (-1) ): (long)numerator;
    long denom = (long)denominator < 0 ? (long) denominator * (-1) : (long)denominator;
    long inte = numer / denom;
    res += to_string(inte);
    long rmd = numer % denom;
    if(!rmd){
        return res;
    }
    rmd *= 10;
    res += ".";
    unordered_map<long, long> nums_map;
    while(rmd){
        long quo = rmd / denom;
        if(nums_map.find(rmd) != nums_map.end()){
            res.insert(nums_map[rmd],1,'(');
            res+=")";
            break;
        }
        nums_map[rmd] = res.size();
        res += to_string(quo);
        rmd = (rmd % denom) * 10;
    }
    return res;
}
```
## 5. 分析：
解决的重点：
1. 结果的符号
2. 处理会造成溢出的情况，所以应该采用long
3. 处理（1）没有小数的情况；（2）处理小数部分不重复的情况；（3）处理小数部分重复的情况
