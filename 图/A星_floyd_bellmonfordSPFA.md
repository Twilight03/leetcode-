A*
就是dijkstra在设计小根堆的排序方式时，判断条件不再是到原点的距离，而是     到原点的距离加上预估函数，预估函数的条件是必须小于这个点到终点的真实距离，而且越接近真实距离优化越大。其他的条件和dijkstra一样

在二维网格问题中，经常取曼哈顿距离（坐标横竖差绝对值相加），对角线距离（max(|x1-x2|,  |y1-y2|)),欧式距离（勾股定理）
在一般的图问题其实A*不好发挥因为很难找预估函数



	// A*算法
	// grid[i][j] == 0 代表障碍
	// grid[i][j] == 1 代表道路
	// 只能走上、下、左、右，不包括斜线方向
	// 返回从(startX, startY)到(targetX, targetY)的最短距离
	public static int minDistance2(int[][] grid, int startX, int startY, int targetX, int targetY) {
		if (grid[startX][startY] == 0 || grid[targetX][targetY] == 0) {
			return -1;
		}
		int n = grid.length;
		int m = grid[0].length;
		int[][] distance = new int[n][m];
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < m; j++) {
				distance[i][j] = Integer.MAX_VALUE;
			}
		}
		distance[startX][startY] = 1;
		boolean[][] visited = new boolean[n][m];
		// 0 : 行
		// 1 : 列
		// 2 : 从源点出发到达当前点的距离 + 当前点到终点的预估距离
		PriorityQueue<int[]> heap = new PriorityQueue<>((a, b) -> a[2] - b[2]);
		heap.add(new int[] { startX, startY, 1 + f1(startX, startY, targetX, targetY) });
		while (!heap.isEmpty()) {
			int[] cur = heap.poll();
			int x = cur[0];
			int y = cur[1];
			if (visited[x][y]) {
				continue;
			}
			visited[x][y] = true;
			if (x == targetX && y == targetY) {
				return distance[x][y];
			}
			for (int i = 0, nx, ny; i < 4; i++) {
				nx = x + move[i];
				ny = y + move[i + 1];
				if (nx >= 0 && nx < n && ny >= 0 && ny < m && grid[nx][ny] == 1 && !visited[nx][ny]
						&& distance[x][y] + 1 < distance[nx][ny]) {
					distance[nx][ny] = distance[x][y] + 1;
					heap.add(new int[] { nx, ny, distance[x][y] + 1 + f1(nx, ny, targetX, targetY) });
				}
			}
		}
		return -1;
	}
	
	// 曼哈顿距离
	public static int f1(int x, int y, int targetX, int targetY) {
		return (Math.abs(targetX - x) + Math.abs(targetY - y));
	}
	
	// 对角线距离
	public static int f2(int x, int y, int targetX, int targetY) {
		return Math.max(Math.abs(targetX - x), Math.abs(targetY - y));
	}
	
	// 欧式距离
	public static double f3(int x, int y, int targetX, int targetY) {
		return Math.sqrt(Math.pow(targetX - x, 2) + Math.pow(targetY - y, 2));
	}

弗洛伊德算法：
可以求有权图任意两点间距离的最小值，不过时间复杂度n的3次方很大，只适合很小的图
代码很好记，就是最外层循环是跳板点，然后里面两层for循环就是穷举所有点的组合，看看能不能用跳板更新最短距离

public static void floyd() {
		// O(N^3)的过程
		// 枚举每个跳板
		// 注意，跳板要最先枚举！跳板要最先枚举！跳板要最先枚举！
		for (int bridge = 0; bridge < n; bridge++) { // 跳板
			for (int i = 0; i < n; i++) {
				for (int j = 0; j < n; j++) {
					// i -> .....bridge .... -> j
					// distance[i][j]能不能缩短
					// distance[i][j] = min ( distance[i][j] , distance[i][bridge] + distance[bridge][j])
					if (distance[i][bridge] != Integer.MAX_VALUE 
							&& distance[bridge][j] != Integer.MAX_VALUE
							&& distance[i][j] > distance[i][bridge] + distance[bridge][j]) {
						distance[i][j] = distance[i][bridge] + distance[bridge][j];
					}
				}
			}
		}
		
Bellmon Ford
这个算法的特点在于可以处理边的权重是负数的情况，可以有负数边，但是不能有边权值和为负数的环，同时这还可以判断一个图中有没有负环

朴素：每一轮考察每条边，看看这个有向边能不能让to点的distance更新，能更新就更新。
一轮结束后如果上一轮有更新继续这个循环。直到又一轮一次更新都没有就成功了。

SPFA优化：

![image-20231103204443627](C:/Users/DELL/AppData/Roaming/Typora/typora-user-images/image-20231103204443627.png)

如果有n个点，n轮还没结束就代表有负环
