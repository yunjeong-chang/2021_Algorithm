```c
# include <stdio.h>
# include <stdlib.h>
# pragma warning(disable:4996)
# define swap(x, y) do {int tmp = x; x = y; y = tmp;}while(0)
# define MIN(a, b) (((a) < (b)) ? (a) : (b))
# define MAX 9999

int** graph;
int* visited;
int* queue;
int rear, front;

void createGraph(int n) {
	rear = 0, front = 0;
	graph = (int**)malloc(sizeof(int*) * (n + 1));
	visited = (int*)malloc(sizeof(int) * (n + 1));
	queue = (int*)malloc(sizeof(int) * (n + 1));

	for (int i = 1; i <= n; i++) {
		graph[i] = (int*)malloc(sizeof(int) * (n + 1));
		visited[i] = 0;
		for (int j = 1; j <= n; j++) {
			graph[i][j] = 0;
		}
	}
}

void insertEdge(int a, int b) {
	graph[a][b] = 1;
	graph[b][a] = 1;
}

void BFS(int start, int n) {
	queue[++rear] = start;
	visited[start] = 1;
	while (front != rear) {
		int nodeStart = queue[++front];
		printf("%d\n", nodeStart);
		for (int i = 1; i <= n; i++) {
			if (visited[i] == 0 && graph[nodeStart][i] == 1) {
				visited[i] = 1;
				queue[++rear] = i;
			}
		}
	}
}

void freeGraph(int n) {
	for (int i = 1; i <= n; ++i)
		free(graph[i]);
	free(graph);
	free(visited);
}

int main() {
	int n, m, s;
	scanf("%d %d %d", &n, &m, &s); //n개의 정점, m개의 간선, 순회 시작 정점 번호 s
	createGraph(n);

	int edge1, edge2;
	for (int i = 0; i < m; i++) {
		scanf("%d %d", &edge1, &edge2); //무방향 간선 (edge1, edge2) 입력
		insertEdge(edge1, edge2);
	}
	BFS(s, n);
	freeGraph(n);
}
```
