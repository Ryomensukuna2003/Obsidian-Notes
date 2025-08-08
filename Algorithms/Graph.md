Nodes/Vertexes - where 2 lines meet
Edge - Lines in graph that connect 2 nodes

Directed and Undirected graph
	In directed graph edges have direction and can be one way or two way
	In undirected graph edges can go both the direction

Degrees - Number of edges connected to a node
		- In Directed graph there are 2 terms
			- Indegree of a graph
			- Outdegree of a graph


Ways to represent graph
1. Adjacency matrix (takes upto n^2) 
2. Adjacency List `(2*Edge)`

Directed and Undirected Graph 
```cpp
int main(){
	int n,m;cin>>n>>m;
	vector<vector<int>> vec1(n + 1);
	rep(i,0,m){
		int x,y;cin>>x>>y;
		vec1[x].push_back(y);
		// vec1[y].push_back(x); // For undirected graph
	}
}
```

Weighted Graph
```cpp
int main(){
	int n,m;cin>>n>>m;
	vector<vector<pair<int,int>>>adj(n+1);
	rep(i,0,m){
		int x,y,w;cin>>x>>y>>w;
		adj[x].push_back({y, w});
		// adj[y].push_back({x, w}); // For undirected graph
	}
}
```


BFS traversal
```cpp
vector<int>bfs(int n,vector<int>&adj){
	vector<bool>visited(adj.size(),0);
	queue<int>q1;
	q1.push(0);
	vector<int>bfs;
	while(!q1.empty()){
		int curr=q1.front();
		q1.pop();
		bfs.push_back(curr);
		visited[curr]=1;
		for(auto x:adj[curr]){
			if(!visited[x]){
				visited[x]=1;
				q1.push(x);
			}
		}
	}
	return bfs;
}
```

DFS traversal
```cpp
vector<vector<int>> adj;
vector<bool> visited;

void dfs(int node) {
    visited[node] = true;
    cout << node << " ";

    for (int neighbor : adj[node]) {
        if (!visited[neighbor]) {
            dfs(neighbor);
        }
    }
}

```


BFS traversal in matrix 
```cpp
 void bfs(vector<vector<int>>& grid){
        int n = grid.size(), m = grid[0].size();
        queue<pair<int,int>> q1;
        vector<vector<int>> vis(n, vector<int>(m, false));
        int dx[] = {-1, 1, 0, 0};
        int dy[] = {0, 0, -1, 1};
        // BFS
        while(!q1.empty()){
            int x = q1.front().first.first;
            int y = q1.front().first.second;
            int level = q1.front().second;
            q1.pop();
            for(int dir = 0; dir < 4; dir++){
                int nx = x + dx[dir];
                int ny = y + dy[dir];
                if(nx >= 0 && ny >= 0 && nx < n && ny < m && !vis[nx][ny] && grid[nx][ny] == 1){
                    vis[nx][ny] = true;
                    q1.push({{nx, ny}, level + 1});
                }
            }
        }
}
```



Topological Sorting
Applied in directed acyclic graph which states that if u and v has edge going through u to v then u should appear before v