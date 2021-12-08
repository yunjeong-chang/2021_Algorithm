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

int find(int i) {
	while (parent[i] != i)
		i = parent[i];
	return i;
}

void union_bag(int i, int j) {
	int a = find(i);
	int b = find(j);
	parent[a] = b;
}

void kruscalMST() {
	int total_cost = 0;
	int edgeidx = 0;
	for (int i = 0; i < n; i++)
		parent[i] = i;

	while (edgeidx < n - 1) {
		int min = 9999, a = -1, b = -1;
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				if (find(i) != find(j) && G[i][j] < min) {
					min = G[i][j];
					a = i;
					b = j;
				}
			}
		}
		union_bag(a, b);
		edgeidx++;
		total_cost += min;
		printf(" %d", min); //MST 간선 무게
	}
	printf("\n");
	printf("%d", total_cost); //MST 총 무게
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
			G[i][j] = 9999;

	for (int i = 0; i < m; i++) {
		scanf("%d %d %d", &v1, &v2, &w); //정점, 정점, 간선 무게
		G[v1 - 1][v2 - 1] = w;
		G[v2 - 1][v1 - 1] = w;
	}

	kruscalMST();
	return 0;
}
```
