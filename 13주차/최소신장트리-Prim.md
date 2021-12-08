```c
# include <stdio.h>
# include <stdlib.h>
# pragma warning(disable:4996)
# define swap(x, y) do {int tmp = x; x = y; y = tmp;}while(0)
# define MIN(a, b) (((a) < (b)) ? (a) : (b))
# define MAX 9999

int n, m;
int** G;
int* parent;
int* weight;
int* locator;

int find_min_weight() {
	int min = 9999;
	int minidx = 0;

	for (int i = 0; i < n; i++) {
		if (locator[i] == 0 && weight[i] < min) {
			min = weight[i];
			minidx = i;
		}
	}
	return minidx;
}

void primMST() {
	for (int i = 0; i < n; i++) {
		locator[i] = 0;
		weight[i] = 9999;
	}

	weight[0] = 0;
	parent[0] = -1;

	for (int i = 0; i < n; i++) {
		int u = find_min_weight();
		locator[u] = 1;
		printf(" %d", u + 1); //MST 생성시 추가되는 정점
		for (int j = 0; j < n; j++) {
			if (G[u][j] != 0 && locator[j] == 0 && G[u][j] < weight[j]) {
				parent[j] = u;
				weight[j] = G[u][j];
			}
		}
	}
	int total_weight = 0;
	for (int i = 0; i < n; i++) {
		total_weight += weight[i];
	}
	printf("\n");
	printf("%d", total_weight); //MST 총 무게
}

int main() {
	int v1, v2, w;
	scanf("%d %d", &n, &m); //정점의 개수 n, 간선의 개수 m
	G = (int**)malloc(sizeof(int*) * n);
	weight = (int*)malloc(sizeof(int) * n);
	parent = (int*)malloc(sizeof(int) * n);
	locator = (int*)malloc(sizeof(int) * n);

	for (int i = 0; i < n; i++)
		G[i] = (int*)malloc(sizeof(int) * n);

	for (int i = 0; i < n; i++)
		for (int j = 0; j < n; j++)
			G[i][j] = 0;

	for (int i = 0; i < m; i++) {
		scanf("%d %d %d", &v1, &v2, &w); //정점, 정점, 간선 무게
		G[v1 - 1][v2 - 1] = w;
		G[v2 - 1][v1 - 1] = w;
	}

	primMST();
	return 0;
}
```
