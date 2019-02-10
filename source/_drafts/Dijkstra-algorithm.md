title: Dijkstra algorithm
author: Willy Wang
tags: []
categories: []
date: 2018-05-19 10:00:00
---
```cpp
Graph{
    set<int> V
    map<int, map<int, int>> E;
}
Graph g;
Graph sp;
bool check[MAXN];
int cost[MAXN];
Node {
    int id;
    int value;
    bool operator<( const Node r ) {
        return value<r.value;
    }
}
void dijkstra (int s, int e) {
    priority_queue<Node, vecotr<Node>, greater<Node>> pq; Node start; start.id = s; start.value = 0; pq.push_back(start);
    int last = -1;
    for(int i = 0; i < MAXN; i++) {
        cost[i] = INT_MAX;
    }
    while(!pq.empty()) {
        Node tmp = pq.top(); pq.pop();
        if(check[tmp.id]) continus;
        check[tmp.id] = true;
        if(last!=-1) {
            sp.V.push_back(tmp.id);
            sp.V.push_back(last);
            sp.V.E[tmp.id][last] = g.V.E[tmp.id][last];
            sp.V.E[last][tmp.id] = g.V.E[last][tmp.id];
        }
        if(tmp.id==e) break;
        for(auto it: g[tmp.id]) {
            if(!chcek[it.first]) {
                if( tmp+it.second<cost[it.first] ) {
                    Node tmpi = {it.first, tmp+it.second};
                    cost[it.first] = tmp+it.second;
                    pq.push_back(tmpi);
                }
            }
        }
        last = it.first;
    }
}
```

