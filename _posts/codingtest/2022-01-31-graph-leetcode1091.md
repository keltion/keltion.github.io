---
title: "[graph] Shortest Path in Binary Matrix"
layout: post
subtitle: graph
categories: codingtest
tags: graph
comments: false
---
# leetcode 1091
기존의 문제들과는 다르게 인접한 vertex를 좌표상 대각선에 존재하는 vertex를 포함하여 정의하였습니다. 방향을 나타내는 배열을 제외하면 기존 문제들과의 풀이와 큰 차이점이 없습니다.

```cpp
class Solution {
public:
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        if(grid[0][0] == 1) return -1;
        int col = grid.size();
        int row = grid[0].size();
        //BFS
        int dx[] = {0, 1, 0, -1, -1, 1, -1, 1};
        int dy[] = {1, 0, -1, 0, -1, 1, 1, -1};
        queue<pair<int, int>> que;
        int seen[100][100] = {0, };
        que.emplace(make_pair(0,0));
        seen[0][0] = 1;
        
        while(!que.empty()){
            int y, x;
            tie(y, x) = que.front();
            que.pop();
            for(int i=0; i< 8; i++) {
                int ny = y + dy[i];
                int nx = x + dx[i];
                if(!verify(ny, nx, col, row) && !seen[ny][nx] && !grid[ny][nx]) {
                    que.emplace(make_pair(ny, nx));
                    seen[ny][nx] = seen[y][x] + 1;
                }                
            }
        }
        if(seen[col-1][row-1]==0) {
            return -1;
        }
        return seen[col-1][row-1];
    }
    bool verify(int y, int x, int col, int row) {
        return x < 0 || x >= row || y < 0 || y >= col;
    }
};
```

## 회고
문제의 조건을 잘 확인하자. 특히 좌표값의 1이 유효한 값인지 0이 유효한 값인지 확인할 것.