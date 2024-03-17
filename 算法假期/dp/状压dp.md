## 状压dp
带路径的递归，如果路径信息很复杂是无法改成动态规划的，但是有一类dp路径信息可以用位信息表示，主要就是所选取的元素 选过 或 没有选过 分别对应 这个位上的0 或 1
一般能用状压dp解决的问题数据量都不大，小于20

### 例题1
// 我能赢吗
// 给定两个整数n和m
// 两个玩家可以轮流从公共整数池中抽取从1到n的整数（不放回）
// 抽取的整数会累加起来（两个玩家都算）
// 谁在自己的回合让累加和 >= m，谁获胜
// 若先出手的玩家能稳赢则返回true，否则返回false
// 假设两位玩家游戏时都绝顶聪明，可以全盘为自己打算
// 测试链接 : https://leetcode.cn/problems/can-i-win/

这道题位信息 status 某位置为1，表示可以选择   某位置为0，表示不可以选择
// f的含义 :
// 数字范围1~n，当前的先手，面对status给定的数字状态
// 在累加和还剩rest的情况下
// 返回当前的先手能不能赢，能赢返回true，不能赢返回false
public static boolean f(int n, int status, int rest, int[] dp) {
    if (rest <= 0) {
        return false;     //basecase
    }
    if (dp[status] != 0) {
        return dp[status] == 1;  //记忆化搜索
    }
    // rest > 0
    boolean ans = false;
    for (int i = 1; i <= n; i++) {   
        //尝试的这个函数就是考察所有的物品，首先判断一下这个数可以选才能接着走递归分支，走递归的分支就ez了，在对方拿到新status时失败，自己就会成功
        if ((status & (1 << i)) != 0 && !f(n, (status ^ (1 << i)), rest - i, dp)) {
            ans = true;
            break;  //有一个分支跑通了就行
        }
    }
    dp[status] = ans ? 1 : -1;
    return ans;
}
dp数组是只有状态维度这一个可变参数，因为状态可以决定rest这些可变参数，但是在递归函数的定义上还是加上这些信息比较简单。

### 例题二
// 火柴拼正方形
// 你将得到一个整数数组 matchsticks
// 其中 matchsticks[i] 是第 i 个火柴棒的长度
// 你要用 所有的火柴棍 拼成一个正方形
// 你 不能折断 任何一根火柴棒，但你可以把它们连在一起，而且每根火柴棒必须 使用一次
// 如果你能拼出正方形，则返回 true ，否则返回 false
// 测试链接 : https://leetcode.cn/problems/matchsticks-to-square/

首先分析在主函数做一些特判，比如所有数的和不是4的倍数就不行，然后调用递归函数
我们尝试的策略时永远朝一个方向搞定第1，2，3，4条边，这样尝试更清晰

我们的真正可变参数依然是status，但是 还剩几条边没有搞定，以及· 正在搞定的边搞定了多少长度都是有用的信息，递归函数还是要带上这些信息。
// limit : 每条边规定的长度
// status : 表示哪些数字还可以选
// cur : 当前要解决的这条边已经形成的长度
// rest : 一共还有几条边没有解决
// 返回 : 能否用光所有火柴去解决剩下的所有边
public static boolean f(int[] nums, int limit, int status, int cur, int rest, int[] dp) {
		if (rest == 0) {  //basecase 边全搞定 并且不剩下火柴 就true
			return status == 0;
		}
		if (dp[status] != 0) {
			return dp[status] == 1;  //记忆化搜索
		}
		boolean ans = false;
		for (int i = 0; i < nums.length; i++) {
			// 考察每一根火柴，先根据位信息确定这个火柴没用过
			//没用过就可以尝试走分支，还有个限制：不能使得长度超过限制
			if ((status & (1 << i)) != 0 && cur + nums[i] <= limit) {
				if (cur + nums[i] == limit) {
					ans = f(nums, limit, status ^ (1 << i), 0, rest - 1, dp);       //两种分支展开方式
				} else {
					ans = f(nums, limit, status ^ (1 << i), cur + nums[i], rest, dp);
				}
				if (ans) {
					break;    //有一条分支走通就跳出了
				}
			}
		}
		dp[status] = ans ? 1 : -1;
		return ans;
	}
	
	
	拼正方形这道题，本质上是能否把一个集合上的数字，分成和相等的k个部分这样的问题
	这种题都一样，无非就是sum/k，rest初始为k
	
### 例题三
// 售货员的难题 - TSP问题
// 某乡有n个村庄(1<=n<=20)，有一个售货员，他要到各个村庄去售货
// 各村庄之间的路程s(1<=s<=1000)是已知的
// 且A村到B村的路程，与B到A的路大多不同(有向带权图)
// 为了提高效率，他从商店出发到每个村庄一次，然后返回商店所在的村，
// 假设商店所在的村庄为1
// 他不知道选择什么样的路线才能使所走的路程最短
// 请你帮他选择一条最短的路
// 测试链接 : https://www.luogu.com.cn/problem/P1171

村庄数量不多，可以选择用邻接矩阵建图更简单

位信息依旧是熟悉的每个点去没去过组成的位信息status，同时目前来到哪个村也是递归中的重要参数，共同组成两个可变参数
// s : 村里走没走过的状态，1走过了不要再走了，0可以走
// i : 目前在哪个村
public static int f(int s, int i) {
    if (s == (1 << n) - 1) {
        // n : 000011111
        return graph[i][0];   //basecase就是所有村都走过了，最后加上会起始村庄的路程
    }
    if (dp[s][i] != -1) {
        return dp[s][i];    //记忆化搜索
    }
    int ans = Integer.MAX_VALUE;
    for (int j = 0; j < n; j++) {
        // 穷举所有村庄，前提是这个村庄没去过
        //没去过就可以去，你并且展开分支，找出最短的ans返回并且放入缓存表
        if ((s & (1 << j)) == 0) {
            ans = Math.min(ans, graph[i][j] + f(s | (1 << j), j));
        }
    }
    dp[s][i] = ans;
    return ans;
}
