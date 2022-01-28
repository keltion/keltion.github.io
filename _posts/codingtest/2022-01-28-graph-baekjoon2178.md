---
title: "[graph] 미로 탐색"
layout: post
subtitle: graph
categories: codingtest
tags: graph
comments: false
---
# baekjoon 2178
좌표와 출발지점, 도착지점이 주어지고 출발지점에서 도착지점까지의 최단거리를 구하는 문제입니다.
좌표 이동간의 cost는 모두 동일하므로 BFS를 이용하여 풀면 되는 문제입니다.

BFS는 루트에서 시작하여 인접한 노드들을 우선적으로 탐색하는 방법입니다. 따라서 방문여부를 표시하는
seen에 출발점으로부터 cost를 기록하면서 BFS를 진행한다면, 출발 노드에서부터 목표 노드까지의
 최소 cost를 구할 수 있습니다.

```cpp
#include <bits/stdc++.h>
   
int map_y, map_x;
std::array<std::array<int, 100>, 100> map;
std::array<std::array<int, 100>, 100> seen;
// 2178번 - 미로 탐색
std::queue<std::pair<int, int>> que;

std::array<int, 4> dx{0, 1, 0, -1};
std::array<int, 4> dy{-1, 0, 1, 0};

// 유효한 vertex인지 확인
bool verify(int y, int x) {
    return x < 0 | x > 99 | y < 0 | y > 99; 
}

void bfs() {
    que.emplace(std::make_pair(0, 0));
    seen[0][0] = 1;
    while(!que.empty()) {
        int x, y;
        std::tie(y,x) = que.front();
        que.pop();
        for(int i=0; i< 4; i++) {
            int ny = y + dy[i];
            int nx = x + dx[i];
            if((map[ny][nx] == 1) && !seen[ny][nx] && !verify(ny, nx)) {
                que.emplace(std::make_pair(ny, nx));
                seen[ny][nx] = seen[y][x]+1;
            }
        }

    }
    std::cout << seen[map_y-1][map_x-1] << "\n";
}

int main() {
    std::cin >> map_y >> map_x;
    for(int i=0; i<map_y; i++) {
        for(int j=0;j<map_x; j++) {
            scanf("%1d",&map[i][j]);
        }
    }

    bfs();
}
```

## 회고
1. bfs를 구현하는데 시간이 너무 오래걸렸다. 시간을 단축시켜보자.
2. std::tie 사용에 익숙해질 것.
3. 붙어있는 정수를 하나씩 받는 방법 숙지.
4. 노드 탐색시 탐색 가능한 노드인지에 대한 조건 빠뜨리지 말기
    - map[y][x] == 1
    - seen[y][x] == 0
    - 유효한 좌표값