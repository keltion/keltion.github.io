---
title: "[backtracking] Letter Combinations of a Phone Number"
layout: post
subtitle: backtracking
categories: codingtest
tags: backtracking
comments: false
---
# leetcode 17
2~9사이의 숫자만 포함된 문자열이 주어집니다. 숫자가 표현할 수 있는 가능한 문자조합을 리턴하면 되는 문제입니다. 
![phone.jpg](./phone.jpg)  

예를들어 "26"이 입력으로 들어왔다면 ["am", "an", "ao", "bm", "bn", "bo", "cm", "cn", "co"]를 리턴해주면 됩니다.
   
## 풀이
입력된 문자열을 digits = "26" 이라고 해보면 digits[0]으로 만들 수 있는 문자(ex "a"), digits[1]로 만들 수 있는 문자(ex "m")를 조합하고 그 이후에는 문자가 존재하지 않으므로 다시 "a"와 조합할 수 있는 문자를 찾아야합니다. 즉 발생할 수 있는 모든 경우의 수를 따지고 더 이상 진행할 필요가 없으면 다른 조합 가능한 문자열을 찾아야 합니다. 이는 Back tracking을 이용하여 구현할 수 있습니다.


## complexity
- time complexity : O(n4<sup>n</sup>)
- space complexity : O(n)


```cpp
class Solution {
public:
    void MakeLetter(int idx, string& combination, string& digits) {
        if (idx >= digits.length()) {
            sol.emplace_back(combination);
            return;
        }
        for (auto& ch : map[digits[idx]]) {
            combination.push_back(ch);
            MakeLetter(idx + 1, combination, digits);
            combination.pop_back();
        }    
    }
    
    void Mapping() {
        map['2'] = "abc";
        map['3'] = "def";
        map['4'] = "ghi";
        map['5'] = "jkl";
        map['6'] = "mno";
        map['7'] = "pqrs";
        map['8'] = "tuv";
        map['9'] = "wxyz";
    }
    
    vector<string> letterCombinations(string digits) {
        if (digits.length() == 0) {
            return sol;
        }
        string combination = "";
        Mapping();
        MakeLetter(0, combination, digits);
        return sol;
    }
private:
    unordered_map<char, string> map;
    vector<string> sol;
};
```

## 회고
- vector와 같은 컨테이너는 참조를 가질 수 없다.
- 참조여부를 잘 판단해서 문자열을 다루자.