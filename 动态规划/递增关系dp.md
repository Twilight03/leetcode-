## 300 最长递增子序列 673 最长递增子序列长度

这种题目主要强调要寻找一个递增的东西，dp数组一般是设为以第i个为递增结尾的最长序列，

因为只有这样设计，才能找到递推关系。

而一般来讲未优化的需要n平方复杂度，就是j从0到i搜索看看能把不能找到构造最长子序列



## 646 最长数对链 354俄罗斯套娃问题

这两道题和子序列就不太一样，因为子序列，只允许在原有序列的顺序的基础上，寻找最大的递增序列

但是这两道题目允许将数组中的东西随意组合寻找最大递增序列，这种题目给的数组一般也是二维的，递增关系也复杂一些。

这种题目重点是要先定制一个排序，排好序保证构造递增序列时每个元素作为结尾只会和前面的构成序列，也就是广义的从小到大排序，这样问题就转化成了前一类问题。


## 二分查找，优化dp
几乎之前的所有题目，都可以用这个方法优化
dp数组改成一个 list来做，
for(int i=0;i<n;i++)
{
list对每一i的含义是
list.get(j)是前i个序列中，长度为j+1的子序列最后一个元素的最小可能值

具体做法
if(nums[i]>list.get(list.size()-1))
{
list.add(nums[i]);
}else
{
 int index=binarysearch(list,nums[i]);
 list.set(index,nums[i]);
}
}
index是找到list中list(k)<nums[i]<list[k+1]，这样k+1长的序列最小的结尾可以更新为nums[i]
binarysearch(list,target)
{
int low=0;int high=list.size()-1;
while(low<high)
{
int mid=(low+high)/2;
if(list.get(mid)<target)
{
low=mid+1
}else
{
high=mid
}
}
return low;
}

## 等差序列寻找
这种题首先注意必须要定长度才能做
不定长度要穷举让他定长度,也就是等差的差要是确定的
这种题由有一个不错的优化
建一个hashmap
for(int v:nums)
{
	map.put(v,map.getOrDefault(v-difference,0)+1);
    maxlen=Math.max(maxlen,map.get(v));
}
map还是以i为结尾的最长等差数列的长度
