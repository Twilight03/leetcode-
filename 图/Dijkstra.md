## Dijkstra单源图最短路算法（有权图）

主要用到数据结构是  一个distance数组初始全是正无穷大，一个visited数组记录这个元素是否从小根堆中被弹出过，还有就是一个小根堆

初始化distance全是无穷大，visited全是false
先把起点加入小根堆，distance起点是0
之后做这样的循环操作：
从小根堆取出一个元素，从visited数组中获取到这个元素是否之前弹出过，如果弹出过直接continue，没弹出过继续操作
记住弹出这次元素时代表这个元素的distance已经最小确定了，要把visited这个元素改成true。
之后考察这个点所关联的所有边，这个边的to点之前visited过了就continue。这条边不能使得to点的distance更新就continue。这两个判断都过了就可以把to点的distance更新，然后把（to，disatnce【to】）这个数组放入小根堆。

大致代码：
	int[] distance = new int[n + 1];
		Arrays.fill(distance, Integer.MAX_VALUE);
		distance[s] = 0;
		boolean[] visited = new boolean[n + 1];
		// 0 : 当前节点
		// 1 : 源点到当前点距离
		PriorityQueue<int[]> heap = new PriorityQueue<>((a, b) -> a[1] - b[1]);
		heap.add(new int[] { s, 0 });
		while (!heap.isEmpty()) {
			int u = heap.poll()[0];
			if (visited[u]) {
				continue;
			}
			visited[u] = true;
			for (int[] edge : graph.get(u)) {
				int v = edge[0];
				int w = edge[1];
				if (!visited[v] && distance[u] + w < distance[v]) {
					distance[v] = distance[u] + w;
					heap.add(new int[] { v, distance[u] + w });
				}
			}
		}