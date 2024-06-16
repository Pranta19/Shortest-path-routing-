#include<bits/stdc++.h>

using namespace std;


typedef pair<int, int> pii; 
vector<vector<pii>> graph;
unordered_map<char, int> charToIndex;
unordered_map<int, char> indexToChar;


vector<int> dijkstra(int source, int numVertices) {
   
    priority_queue<pii, vector<pii>, greater<pii>> pq;

   
    vector<int> dist(numVertices, INT_MAX);

   
    pq.push({0, source});
    dist[source] = 0;

    
    while (!pq.empty()) {
      
        int u = pq.top().second;
        pq.pop();

        
        for (auto &adj : graph[u]) {
            int weight = adj.first;
            int v = adj.second;

          
            if (dist[u] + weight < dist[v]) {
                dist[v] = dist[u] + weight;
                pq.push({dist[v], v});
            }
        }
    }

    return dist;
}

int main() {
    int numVertices, numEdges;
    cout << "Enter the number of vertices and edges: ";
    cin >> numVertices >> numEdges;

    graph.resize(numVertices);
    charToIndex.reserve(numVertices);
    indexToChar.reserve(numVertices);

    cout << "Enter the vertices: ";
    for (int i = 0; i < numVertices; ++i) {
        char vertex;
        cin >> vertex;
        charToIndex[vertex] = i;
        indexToChar[i] = vertex;
    }

    cout << "Enter the edges (source, destination, weight):" << endl;
    for (int i = 0; i < numEdges; ++i) {
        char u, v;
        int w;
        cin >> u >> v >> w;
        graph[charToIndex[u]].push_back({w, charToIndex[v]});
        graph[charToIndex[v]].push_back({w, charToIndex[u]});
    }

    char sourceChar;
    cout << "Enter the source vertex: ";
    cin >> sourceChar;
    int source = charToIndex[sourceChar];

    vector<int> distances = dijkstra(source, numVertices);

    cout << "Shortest distances from source vertex " << sourceChar << ":" << endl;
    for (int i = 0; i < numVertices; ++i) {
        if (distances[i] == INT_MAX) {
            cout << "Vertex " << indexToChar[i] << " is unreachable" << endl;
        } else {
            cout << "Vertex " << indexToChar[i] << ": " << distances[i] << endl;
        }
    }
    cout<<" author of the code Pranta Dhar id C231219 IIUC "<<endl;

    return 0;
}
