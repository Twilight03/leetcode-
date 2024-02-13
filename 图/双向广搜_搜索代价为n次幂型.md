主要例题如下
// 牛牛准备参加学校组织的春游, 出发前牛牛准备往背包里装入一些零食, 牛牛的背包容量为w。
// 牛牛家里一共有n袋零食, 第i袋零食体积为v[i]。
// 牛牛想知道在总体积不超过背包容量的情况下,他一共有多少种零食放法(总体积为0也算一种放法)。

// 给你一个整数数组 nums 和一个目标值 goal
// 你需要从 nums 中选出一个子序列，使子序列元素总和最接近 goal
// 也就是说，如果子序列元素和为 sum ，你需要 最小化绝对差 abs(sum - goal)
// 返回 abs(sum - goal) 可能的 最小值
// 注意，数组的子序列是通过移除原始数组中的某些元素（可能全部或无）而形成的数组。

简单来说如果暴力做的话，每个元素就要选择  要 与 不要 ，这样的话相当于时间复杂度是2的n次幂
而这道题用dp也需要及其大的数组也不行。

双向广搜的思路，把整个数组分成两部分，分别进行暴力展开，然后把尝试的结果merge，把2的n次幂，变成2的二分之n次幂

首先要学会如何穷举2的n次幂，利用如下的递归函数
public static int f( int idx ,int endIdx,int now,int lim,int[] ans,int fill)
{
	if(now>lim)
		return fill;
	if(idx==endIdx)
	{
		ans[idx++]=now;
	}else
	{
		fill=f(idx+1,endIdx,now,lim,ans,fill);  //不填第idx位置的数
		fill=f(idx+1,endIdx,now+nums[idx],lim,ans,fill);  //填第idx位置的数
	}
	return fill;
}
idx代表遍历到二叉树的哪一层，也就是对第idx个元素进行 选 与 不选 的分支
endIdx他设计的是idx能达到的最大值的下一位，也就是最深多少层，但最后加个1，方便
now是目前背包装了多少东西，lim是背包能够装的最大值
ans是遍历到了最后一层之后结算结果填到这个数组中。
j是当前ans数组填到的位置的下一位，也就是你往ans数组填数就填在j位置，j也是返回值
其实j做成全局变量更加方便。

接着就要把两边合并了

		Arrays.sort(lsum, 0, lsize);
		Arrays.sort(rsum, 0, rsize);
		int ans = Math.abs(goal);
		for (int i = 0, j = rsize - 1; i < lsize; i++) {
			while (j > 0 && Math.abs(goal - lsum[i] - rsum[j - 1]) <= Math.abs(goal - lsum[i] - rsum[j])) {
				j--;
			}
			ans = Math.min(ans, Math.abs(goal - lsum[i] - rsum[j]));
		}
		return ans;

我们一定要记住合并的时候，lsum和rsum先排好序，然后一个从前向后遍历另一个从后向前遍历，才好merge





从两个方向bfs

// 单词接龙
// 字典 wordList 中从单词 beginWord 和 endWord 的 转换序列
// 是一个按下述规格形成的序列 beginWord -> s1 -> s2 -> ... -> sk ：
// 每一对相邻的单词只差一个字母。
// 对于 1 <= i <= k 时，每个 si 都在 wordList 中
// 注意， beginWord 不需要在 wordList 中。sk == endWord
// 给你两个单词 beginWord 和 endWord 和一个字典 wordList
// 返回 从 beginWord 到 endWord 的 最短转换序列 中的 单词数目
// 如果不存在这样的转换序列，返回 0 。
// 测试链接 : https://leetcode.cn/problems/word-ladder/
public class Code01_WordLadder {

	public static int ladderLength(String begin, String end, List<String> wordList) {
		// 总词表
		HashSet<String> dict = new HashSet<>(wordList);
		if (!dict.contains(end)) {
			return 0;
		}
		// 数量小的一侧
		HashSet<String> smallLevel = new HashSet<>();
		// 数量大的一侧
		HashSet<String> bigLevel = new HashSet<>();
		// 由数量小的一侧，所扩展出的下一层列表
		HashSet<String> nextLevel = new HashSet<>();
		smallLevel.add(begin);
		bigLevel.add(end);
		for (int len = 2; !smallLevel.isEmpty(); len++) {
			for (String w : smallLevel) {
				// 从小侧扩展
				char[] word = w.toCharArray();
				for (int j = 0; j < word.length; j++) {
					// 每一位字符都试
					char old = word[j];
					for (char change = 'a'; change <= 'z'; change++) {
						// // 每一位字符都从a到z换一遍
						if (change != old) {
							word[j] = change;
							String next = String.valueOf(word);
							if (bigLevel.contains(next)) {
								return len;
							}
							if (dict.contains(next)) {
								dict.remove(next);
								nextLevel.add(next);
							}
						}
					}
					word[j] = old;
				}
			}
			if (nextLevel.size() <= bigLevel.size()) {
				HashSet<String> tmp = smallLevel;
				smallLevel = nextLevel;
				nextLevel = tmp;
			} else {
				HashSet<String> tmp = smallLevel;
				smallLevel = bigLevel;
				bigLevel = nextLevel;
				nextLevel = tmp;
			}
			nextLevel.clear();
		}
		return 0;
	}

}