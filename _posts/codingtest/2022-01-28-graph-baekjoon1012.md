---
title: "[graph] 미로 탐색"
layout: post
subtitle: graph
categories: codingtest
tags: graph
comments: false
---
# baekjoon 1012
connected component의 개수를 세는 문제입니다. 이 경우 BFS, DFS 모두 사용할 수 있습니다.
이번에는 iterative한 DFS를 구현하여 문제를 풀었습니다. 하나의 connected component를 순회할 때마다
count값을 올려주어 개수를 계산하였습니다.

```cpp
#include <bits/stdc++.h>

int loop;
int y, x, n;

std::array<std::array<int, 51>, 51> map;
std::array<std::array<int, 51>, 51> seen;
std::stack<std::pair<int, int>> st;
std::array<int, 4> dx = {0, 1, 0, -1};
std::array<int, 4> dy = {1, 0, -1, 0};

bool verify(int ny, int nx) {
    return ny < 0 || ny > y || nx < 0 || nx > x;
}

void dfs(int y, int x) {
    st.emplace(std::make_pair(y,x));
    seen[y][x] = 1;
    while(!st.empty()) {
        int ny, nx;
        std::tie(y, x)=st.top();
        st.pop();
        for(int i =0; i<4; i++) {
            ny = y + dy[i];
            nx = x + dx[i];
            if(!verify(ny, nx) && map[ny][nx] && !seen[ny][nx]) {
                st.emplace(std::make_pair(ny, nx));
                seen[ny][nx] =1;
            }
        }
    }
}
int main() {
    std::cin.tie(NULL);
    std::cout.tie(NULL);
    std::cin >> loop;
    while(loop--) {
        std::fill(map.begin(), map.end(), std::array<int,51>());
        std::fill(seen.begin(), seen.end(), std::array<int, 51>());
        int count =0;
        std::cin >> x >> y >> n;
        for(int i=0; i<n; i++) {
            int map_x, map_y;
            std::cin >> map_x >> map_y;
            map[map_y][map_x] = 1;
        }

        for(int i = 0; i < y; i++) {
            for(int j = 0; j < x; j++) {
                if(map[i][j] && !seen[i][j]) {
                    dfs(i, j);
                    count++;
                }
            }
        }
        std::cout << count << "\n";
    }
}
```

## 회고
1. 이미 순회를 마친 connected component의 노드인지를 확인하는 조건문에 노드의 유무를 확인하지 않아 이상한 값이 나왔다.
2. 노드 방문하고 seen값을 표시하지 않아 무한루프가 돌아갔다.