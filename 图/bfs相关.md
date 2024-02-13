[1162. 地图分析](https://leetcode.cn/problems/as-far-from-land-as-possible/)

这里好多题都是在二维数组中搜索，在二维数组中搜索的方法就是

// 0:上，1:右，2:下，3:左
	public static int[] move = new int[] { -1, 0, 1, 0, -1 };
	//                                      0  1  2  3   4
	//                                               i
	// (x,y)  i来到0位置 : x + move[i], y + move[i+1] -> x - 1, y
	// (x,y)  i来到1位置 : x + move[i], y + move[i+1] -> x, y + 1
	// (x,y)  i来到2位置 : x + move[i], y + move[i+1] -> x + 1, y
	// (x,y)  i来到3位置 : x + move[i], y + move[i+1] -> x, y - 1
	每次来到一个新位置要判断新生成的xx=x + move[i]  yy=y + move[i+1]是否合法，并且要判断是否重复搜索到这个点，常见就是用visited数组标记一下，要不就是用一些set结构
	if(xx >= 0 && xx < n && yy >= 0 && yy < m && !visited[xx][yy])
	
有时我们会更加关注bfs的层数，这个时候我们最好在队列中一层一层处理元素
代码：
int level=1;
while(!queue.isEmpty())
{
	int size=queue,size();
	while(size--!=0)
	{
		queue.pollFirst();
		处理
	}
	level++;
}

691. 贴纸拼词
https://leetcode.cn/problems/stickers-to-spell-word/
这道题首先得发现只跟各个字母的个数相关，这个时候排序就很重要，先排好序。
然后这道题bfs的重要剪枝就是只搜索当前剩下单词的第一个字母存在的贴纸，也就是优先想办法把把剩下单词的第一个字母全部消掉。当然也要安排一个set防止重复

对字符串排序，是。toCharArray先转成字符数组，对字符数组排序，然后用s=String.vauleOf(charArray)转成字符串，好像toString不行

StringBuilder builder = new StringBuilder();
可以append
builder.toString()就可以转成字符串
由于 StringBuilder 相较于 StringBuffer 有速度优势，所以多数情况下建议使用 StringBuilder 类
append(String s)  reverse()  delete(int start, int end)  	
insert(int offset, String str)将 str 参数的字符串插入此序列中。
replace(int start, int end, String str)
String substring(int start, int end)返回一个新的 String，它包含此序列当前所包含的字符子序列。

01BFS

![image-20231029093257609](C:/Users/DELL/AppData/Roaming/Typora/typora-user-images/image-20231029093257609.png)

补充：每次从双端队列弹出的点都代表着他的distance在弹出时已经最少了

2290. 到达角落需要移除障碍物的最小数目
https://leetcode.cn/problems/minimum-obstacle-removal-to-reach-corner/
1368. 使网格图至少有一条有效路径的最小代价
https://leetcode.cn/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/
这两道题其实并不明显时01图的题干，但是都在bfs搜索的时候，总是往一些方向上走代价是0，往另一些方向走代价是1.代价这个词很重要



二维接雨水https://leetcode.cn/problems/trapping-rain-water-ii/
这道题虽然是bfs但是不用队列用小根堆，这是因为因为总需要找到外围最容易流出的点，维护一个小根堆PriorityQueue<int[]>按照水线高度排序，搜索的时候将搜索到的新点放入小根堆，水线是触发这个点的水线和这个点的高度的最大值。每次弹出点可以更新储水量

126. 单词接龙 II
https://leetcode.cn/problems/word-ladder-ii/
这道题分析一下就是从beginword搜索他能变到哪个单词，然后进行bfs，做好去重的工作。bfs的同时创建反图。构造返图为了之后的到全部路径做准备。这里观察数据特点，由于单词很短，所以搜索能变到哪个单词用把单个单词的字母用26个英文字母替代，再搜索字典有没有。而不是直接查找字典中谁和他可以变换。
构造反图后从endword到beginword进行dfs搜搜索

记住一般来说无向图上bfs比dfs快很多，但是搜索全全部路径只能dfs来做
此题就是bfs建图搜索，dfs得到路径