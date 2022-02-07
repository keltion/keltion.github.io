---
title: "[graph] 줄 세우기"
layout: post
subtitle: graph
categories: codingtest
tags: graph
comments: false
---
# baekjoon 2252
https://www.acmicpc.net/problem/2252  
Topological Sort 문제입니다. 주어진 데이터로 그래프를 만들고 vertex를 순회하며 indegree를 표시하는 배열을 완성하였습니다. 이후에 indegree값이 0인 vertex들을 queue에 넣고 pop할 때 해당 vertex와 인접한 vertex들의 indegree값을 1씩 빼주었습니다. queue에 값이 존재하지 않을 때까지 반복적으로 위 작업을 진행하며 pop을 할 때 해당 vertex를 출력해주어 문제를 풀었습니다.

## complexity
- Time complexity : O(v+e)
- Space complexity : O(v)
  
```cpp
#include <bits/stdc++.h>

int main() {
    int N, M;
    std::cin >> N >> M;
    std::vector<std::vector<int>> map(N+1);
    std::vector<int> indegree(N+1);
    std::queue<int> que;
    for(int i = 0; i < M; i++) {
        int s1, s2;
        std::cin >> s1 >> s2;
        map[s1].emplace_back(s2);
    }

    for(auto& vertex : map) {
        for(auto& nei : vertex) {
            indegree[nei]++;
        }
    }
    for(int i = 1; i <= N; i++) {
        if(indegree[i] == 0) {
            que.emplace(i);
        }
    }
    while(!que.empty()) {
        int curr = que.front();
        que.pop();
        std::cout << curr << ' ';
        for(auto& nei : map[curr]) {
            indegree[nei]--;
            if(indegree[nei] == 0) {
                que.emplace(nei);
            }
        }
    }
    std::cout << "\n";
}
```

## 회고
없음