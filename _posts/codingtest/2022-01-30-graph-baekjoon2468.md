---
title: "[graph] 안전 영역"
layout: post
subtitle: graph
categories: codingtest
tags: graph
comments: false
---
# baekjoon 2468
어떤 특정 값(0~100) 이상의 값을 가진 좌표들만을 vertex로 생각하여 connected components의 최대 개수를 구하는 문제입니다.

```cpp
#include <bits/stdc++.h>

int n;
std::array<std::array<int, 100>, 100> map;
std::array<std::array<int, 100>, 100> seen;
std::stack<std::pair<int, int>> st;

std::array<int, 4> dy{0, 1, 0, -1};
std::array<int, 4> dx{-1, 0, 1, 0};

bool verify(int y, int x) {
    return x < 0 || x >= n || y < 0 || y >= n;
}

void dfs(int y, int x, int n) {
    st.push(std::make_pair(y,x));
    seen[y][x] =1;
    while(!st.empty()) {
        int cy, cx;
        std::tie(cy, cx) =st.top();
        st.pop();
        for(int i=0; i< 4; i++) {
            int ny = cy + dy[i];
            int nx = cx + dx[i];
            if(!verify(ny, nx) && !seen[ny][nx] && map[ny][nx] > n) {
                st.push(std::make_pair(ny, nx));
                seen[ny][nx] = 1;
            }
        }
    }
} 

int main() {
    std::cin >> n;
    for(int i=0; i<n; i++) {
        for(int j=0; j<n; j++) {
            std::cin >> map[i][j];
        }
    }
    int count = 0;
    int max = 0;
    int loop = n;
    for(int k = 0; k < 101; k++) {
        count = 0;
        seen = {0,};
        for(int i=0; i<n; i++) {
            for(int j=0; j<n; j++) {
                if(map[i][j] > k && !seen[i][j]) {
                    dfs(i, j, k);
                    count++;
                }
            }
        }
        if(max <= count) max = count;
    }
    std::cout << max << "\n";
}
```

## 회고
없음