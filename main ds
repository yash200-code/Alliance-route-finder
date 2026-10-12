#include <iostream>
#include <vector>
#include <queue>
#include <string>
#include <algorithm>
#include <limits>
#include <functional>

using namespace std;

const int N = 13;

const string places[N] = {
    "Security Office",
    "Parking",
    "Administrative Block",
    "Library",
    "Learning Centre 2",
    "Halls of Residence",
    "Sports Grounds",
    "Learning Centre 1",
    "Temple",
    "Play Courts",
    "Food Court",
    "University Parking",
    "Student Activity Centre"
};

struct Edge {
    int destination;
    int distance;
};

vector<Edge> graph[N];

// Add an undirected road between two locations.
void addRoad(int u, int v, int distance) {
    graph[u].push_back({v, distance});
    graph[v].push_back({u, distance});
}

// Illustrative connections and distances.
// Replace these with verified campus roads and measurements.
void buildGraph() {
    addRoad(0, 1, 100);
    addRoad(1, 2, 180);
    addRoad(2, 3, 120);
    addRoad(2, 4, 150);
    addRoad(3, 7, 140);
    addRoad(7, 4, 100);
    addRoad(3, 10, 130);
    addRoad(10, 9, 110);
    addRoad(9, 6, 160);
    addRoad(6, 8, 200);
    addRoad(8, 11, 90);
    addRoad(11, 5, 170);
    addRoad(10, 12, 80);
    addRoad(12, 4, 120);
    addRoad(5, 6, 140);
}

void showLocations() {
    cout << "\n========== CAMPUS LOCATIONS ==========\n";

    for (int i = 0; i < N; i++) {
        cout << i + 1 << ". " << places[i] << '\n';
    }

    cout << "======================================\n";
}

bool validLocation(int location) {
    return location >= 1 && location <= N;
}

void printRoute(const vector<int>& parent, int destination) {
    vector<int> route;

    for (int current = destination;
         current != -1;
         current = parent[current]) {
        route.push_back(current);
    }

    reverse(route.begin(), route.end());

    cout << "\nRoute:\n";

    for (int i = 0; i < (int)route.size(); i++) {
        cout << places[route[i]];

        if (i + 1 < (int)route.size()) {
            cout << " -> ";
        }
    }

    cout << '\n';
}

// BFS: Finds a route with the fewest edges,
// not necessarily the shortest physical distance.
void runBFS(int start, int destination) {
    vector<bool> visited(N, false);
    vector<int> parent(N, -1);
    queue<int> q;

    visited[start] = true;
    q.push(start);

    while (!q.empty()) {
        int current = q.front();
        q.pop();

        if (current == destination) {
            break;
        }

        for (const Edge& edge : graph[current]) {
            int next = edge.destination;

            if (!visited[next]) {
                visited[next] = true;
                parent[next] = current;
                q.push(next);
            }
        }
    }

    if (!visited[destination]) {
        cout << "\nNo route exists between these locations.\n";
        return;
    }

    cout << "\nBFS Result";
    printRoute(parent, destination);
    cout << "BFS finds a route with the fewest road connections.\n";
}

// DFS: Finds a route by exploring connected locations.
bool dfsSearch(int current, int destination,
               vector<bool>& visited,
               vector<int>& parent) {
    visited[current] = true;

    if (current == destination) {
        return true;
    }

    for (const Edge& edge : graph[current]) {
        int next = edge.destination;

        if (!visited[next]) {
            parent[next] = current;

            if (dfsSearch(next, destination, visited, parent)) {
                return true;
            }
        }
    }

    return false;
}

void runDFS(int start, int destination) {
    vector<bool> visited(N, false);
    vector<int> parent(N, -1);

    if (!dfsSearch(start, destination, visited, parent)) {
        cout << "\nNo route exists between these locations.\n";
        return;
    }

    cout << "\nDFS Result";
    printRoute(parent, destination);
    cout << "DFS finds a route using depth-first exploration.\n";
}

// Dijkstra: Finds the minimum-distance route
// when all road distances are non-negative.
void runDijkstra(int start, int destination) {
    const long long INF = numeric_limits<long long>::max();

    vector<long long> dist(N, INF);
    vector<int> parent(N, -1);

    using State = pair<long long, int>;
    priority_queue<State, vector<State>, greater<State>> pq;

    dist[start] = 0;
    pq.push({0, start});

    while (!pq.empty()) {
        long long currentDistance = pq.top().first;
        int current = pq.top().second;
        pq.pop();

        if (currentDistance != dist[current]) {
            continue;
        }

        if (current == destination) {
            break;
        }

        for (const Edge& edge : graph[current]) {
            int next = edge.destination;
            long long newDistance =
                currentDistance + edge.distance;

            if (newDistance < dist[next]) {
                dist[next] = newDistance;
                parent[next] = current;
                pq.push({newDistance, next});
            }
        }
    }

    if (dist[destination] == INF) {
        cout << "\nNo route exists between these locations.\n";
        return;
    }

    cout << "\nDijkstra Result";
    printRoute(parent, destination);

    cout << "Total distance: " << dist[destination]
         << " metres (if edge weights are measured in metres).\n";
}

int main() {
    buildGraph();

    int choice;

    cout << "====================================\n";
    cout << "       CAMPUS ROUTE FINDER\n";
    cout << "====================================\n";

    while (true) {
        cout << "\n1. Display Campus Locations\n";
        cout << "2. Find Route Using BFS\n";
        cout << "3. Find Route Using DFS\n";
        cout << "4. Find Shortest Route Using Dijkstra\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";

        if (!(cin >> choice)) {
            cout << "Invalid input. Please enter a number.\n";
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            continue;
        }

        if (choice == 5) {
            cout << "\nThank you for using Campus Route Finder!\n";
            break;
        }

        if (choice == 1) {
            showLocations();
            continue;
        }

        if (choice < 2 || choice > 4) {
            cout << "Invalid choice. Select an option from 1 to 5.\n";
            continue;
        }

        showLocations();

        int start, destination;

        cout << "\nEnter starting location number: ";
        if (!(cin >> start)) {
            cout << "Invalid input. Please enter a number.\n";
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            continue;
        }

        cout << "Enter destination location number: ";
        if (!(cin >> destination)) {
            cout << "Invalid input. Please enter a number.\n";
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            continue;
        }

        if (!validLocation(start) || !validLocation(destination)) {
            cout << "Invalid location number. Choose between 1 and "
                 << N << ".\n";
            continue;
        }

        if (start == destination) {
            cout << "\nYou are already at "
                 << places[start - 1] << ".\n";
            cout << "Total distance: 0 metres.\n";
            continue;
        }

        start--;
        destination--;

        if (choice == 2) {
            runBFS(start, destination);
        } else if (choice == 3) {
            runDFS(start, destination);
        } else {
            runDijkstra(start, destination);
        }
    }

    return 0;
}