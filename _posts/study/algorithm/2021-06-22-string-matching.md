---
# title: "[코딩테스트] String Matching"
# layout: post
# subtitle: sort
# # categories: study
# # tags: algorithm
# comments: false
---

# 목차 
1. [기초-String Matching(KMP 알고리즘)](#1장._기초-String_Matching(KMP_알고리즘))
2. [기초-Palindrome](#2장._기초-Palindrome)
3. [초급-Add Strings](#3장._초급-Add_Strings)
~~4. [중급-Group Anagrams](#4장._중급-Group_Anagrams)~~ 
~~5. [중급-Longest substring w/o Repeats](#5장._중급-Longest_substring_w/o_Repeats)~~  
  
## 1장. 기초-String Matching(KMP 알고리즘)
### 문제
28번, 796번

<hr>
## 2장. 기초-Palindrome
문자열을 앞에서 읽을 때와 뒤에서 읽을 때가 같은 경우 -> two pointer index로 주어진 문자열이 palindrome인지 확인가능
### 문제
125번, 680번
### time complexity
- ? O(n)

<hr>
## 3장. 초급-Add String
### 문제
leetcode 415

### binary search 핵심
인덱스 3개 이용

### time complexity
일반 배열에서 search의 time complexity는 O(n)이지만 binary search의 경우 O(logn)  
? best, worst  
  
<U>binary search 구현</U> <https://github.com/keltion/algorithm_study/blob/main/datastructure/binarySearch.cpp>
<hr>
## 4장. 중급-Group Anagrams
#### 문제
leetcode 49
#### Key idea
- hash map 공부하고 재수강
<hr>

## 5장. 중급-Longest substring w/o Repeats
#### 문제
leetcode 3

#### Key idea
- brute force ->O(n<sp>3</sp>)인 이유?
- hash map 공부하고 재수강
