```c
# include <stdio.h>
# include <stdlib.h>
# pragma warning(disable:4996)
# define swap(x, y) do {int tmp = x; x = y; y = tmp;}while(0)
# define MIN(a, b) (((a) < (b)) ? (a) : (b))
# define MAX 9999

typedef struct Edge {
	int edge_number;
	struct Edge* next;
}Edge;

typedef struct Node {
	int node_number;
	int visited;
	Edge* list;
}Node;

Node** graph;

Edge* createEdge(int edge_number) {
	Edge* newEdge = (Edge*)malloc(sizeof(Edge));
	newEdge->next = NULL;
	newEdge->edge_number = edge_number;
}

void createGraph(int n) { //정점의 개수 n
	graph = (Node**)malloc(sizeof(Node*) * (n + 1));
	for (int i = 1; i <= n; i++) {
		graph[i] = (Node*)malloc(sizeof(Node));
		graph[i]->node_number = i;
		graph[i]->list = createEdge(0);
		graph[i]->visited = 0;
	}
}

void insertEdge(int a, int b) {
	Edge* horse = graph[a]->list;
	Edge* tmp = horse;
	Edge* newEdge = createEdge(b);
	while (horse != NULL) {
		if (horse->edge_number > b)
			break;
		tmp = horse;
		horse = horse->next;
	}
	tmp->next = newEdge;
	newEdge->next = horse;

}

void DFS(int start) { //순회 시작 정점 번호 start
	graph[start]->visited = 1;
	printf("%d\n", graph[start]->node_number); //방문 순서대로 정점 번호 출력

	Edge* horse = graph[start]->list->next;
	while (horse != NULL) {
		int visit = horse->edge_number;
		if (graph[visit]->visited == 0)
			DFS(visit);
		horse = horse->next;
	}
}

void freeGraph(int n) {
	for (int i = 1; i <= n; ++i) {
		Edge* horse = graph[i]->list;
		Edge* next = horse->next;
		while (next != NULL) {
			free(horse);
			horse = next;
			next = next->next;
		}
		free(graph[i]);
	}
	free(graph);
}

int main() {
	int n, m, s;
	scanf("%d%d%d", &n, &m, &s); //n개의 정점, m개의 간선, 순회 시작 정점 번호 s
	createGraph(n);

	int edge1, edge2;
	for (int i = 0; i < m; ++i) {
		scanf("%d%d", &edge1, &edge2); //무방향 간선 (edge1, edge2) 입력
		insertEdge(edge1, edge2);
		insertEdge(edge2, edge1);
	}

	DFS(s); //순회 시작 정점 번호 s
	freeGraph(n);
}
```
