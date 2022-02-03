---
title: "[graph] Course Schedule"
layout: post
subtitle: graph
categories: codingtest
tags: graph
comments: false
---
# leetcode 207
방향그래프에서 사이클의 존재여부를 묻는 문제입니다. 하나의 track(어떤 vertex에서 시작되어 DFS가 끝날때 까지)에서 어떤 노드(사이클이 발생할 수 있는 노드)에
중복방문을 한다면 이는 사이클이 존재함을 의미합니다. 하지만 방향그래프에서 노드 방문 표시를 위해 seen만 이용하면
중복방문을 할 수 없기에 사이클의 존재유무도 알 수 없게됩니다. 따라서 seen으로 사이클이 발생하지 않는 노드의 방문을 체크하여 사이클 상황 이외의 노드 중복방문을 막고,
track_loop를 이용하여 노드의 방문을 체크하여 사이클 상황에서 노드에 중복방문을 할 수 있게 하였습니다.  
```cpp
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> vertexs(numCourses);
        std::unordered_set<int> seen;

        for(auto& vertex: prerequisites) {
            vertexs[vertex[1]].emplace_back(vertex[0]);
        }

        for(int idx=0; idx<vertexs.size(); idx++) {
            std::unordered_set<int> track_loop;
            bool ret = dfs(idx, vertexs, track_loop, seen);
            if(ret == true) {
                return false;
            } 
        }
        return true;
    }
    bool dfs(int idx, vector<vector<int>>& vertexs, std::unordered_set<int>& track_loop, std::unordered_set<int>& seen) {
        if(seen.find(idx) != seen.end()) {
            return false;
        }
        if(track_loop.find(idx) != seen.end()) {
            return true;
        }
        
        for(auto& element : vertexs[idx]) {
            track_loop.emplace(idx);
            bool ret = dfs(element, vertexs, track_loop, seen);
            if(ret == true) {
                return true;
            }
        }
        seen.emplace(idx);
        return false;   
    }
};
```

## 회고
- DFS를 iterative하게도 구현해보자.
- 주어진 조건을 어떻게 활용할 것인지, 반복문의 횟수가 정확하게 맞는지 정교하게 생각할 것.
    - vertexs 초기화시 numCourses가 아닌 numCourses의 최댓값인 10000으로 해줬다.
    - 트랙을 vertexs.size()번이 아닌 prerequisites.size()번으로 돌리는 실수를 함.