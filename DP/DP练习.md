[TOC]



### 1.HUD - 2084 	（数塔）

在讲述DP算法的时候，一个经典的例子就是数塔问题，它是这样描述的： 
有如下所示的数塔，要求从顶层走到底层，若每一步只能走到相邻的结点，则经过的结点的数字之和最大是多少？ 
已经告诉你了，这是个DP的题目，你能AC吗?

Input

>输入数据首先包括一个整数C,表示测试实例的个数，每个测试实例的第一行是一个整数N(1 <= N <= 100)，表示数塔的高度，接下来用N行数字表示数塔，其中第i行有个i个整数，且所有的整数均在区间[0,99]内。 

Output

>对于每个测试实例，输出可能得到的最大和，每个实例的输出占一行。 

Sample Input

```
1
5
7
3 8
8 1 0 
2 7 4 4
4 5 2 6 5
```

Sample Output

```
30
```

边界，边界，边界...  

​	从倒数第二层开始到第一层：N - 2 -> 0 //每行：for (j = 0; j <= i + 1; j++) // i 为上一行的行数。又因为是从 0开始，所以为     `<=`  

状态转移方程：`an[i][j] += max (an[i + 1][j], an[i + 1][j + 1])  `

代码：

```c++
#include <iostream>
#include <algorithm>
using namespace std;
int number[101][101];

int main() {
	int a;
	cin >> a;
	while (a--) {
		int N;
		cin >> N;
		for (int i = 0; i < N; i++) 
			for (int j = 0; j <= i; j++) 
				cin >> number[i][j];

		for (int i = N - 2; i >= 0; i--) 
			for (int j = 0; j <= i + 1; j++) 
				number[i][j] += max(number[i + 1][j], number[i + 1][j + 1]);
		cout << number[0][0] << endl; //最后一次，即第一行只剩一个元素了，他加上下面的最大的，他当然就是最终结果了
	}
	return 0;
}
```



### 2.HDU - 4540	（类似数塔）

威威猫最近不务正业，每天沉迷于游戏“打地鼠”。 
每当朋友们劝他别太着迷游戏，应该好好工作的时候，他总是说，我是威威猫，猫打老鼠就是我的工作！ 
无话可说... 
我们知道，打地鼠是一款经典小游戏，规则很简单：每隔一个时间段就会从地下冒出一只或多只地鼠，玩游戏的人要做的就是打地鼠。 
假设： 
1、每一个时刻我们只能打一只地鼠，并且打完以后该时刻出现的所有地鼠都会立刻消失；
2、老鼠出现的位置在一条直线上，如果上一个时刻我们在x1位置打地鼠，下一个时刻我们在x2位置打地鼠，那么，此时我们消耗的能量为abs( x1 - x2 )； 
3、打第一只地鼠无能量消耗。 
现在，我们知道每个时刻所有冒出地面的地鼠位置，若在每个时刻都要打到一只地鼠，请计算最小需要消耗多少能量。 

Input

>输入数据包含多组测试用例； 
>每组数据的第一行是2个正整数N和K（1 <= N <= 20, 1 <= K <= 10 )，表示有N个时刻，每个时刻有K只地鼠冒出地面； 
>接下来的N行，每行表示一个时刻K只地鼠出现的坐标（坐标均为正整数，且<=500）。 

Output

>请计算并输出最小需要消耗的能量，每组数据输出一行。 

Sample Input

```
2 2
1 10
4 9
3 5
1 2 3 4 5
2 4 6 8 10
3 6 9 12 15
```

Sample Output

```
1
1
```
嗯...DP 很蒙。

DP 会把所有结果都试一遍，本题第一个例子：3 -> 4 	3 -> 9	5 -> 4	5 -> 9...

还会把大问题化为小的子问题嘛，所以，先找出前两个时刻的所有可能的最小 （样例2：即在 1 时的最小（1 -> 2, 1 -> 4... 都试一遍找出最小），然后在 2 时的最小（同在 1 时），在 3 时的最小...）

暴力....

代码：

```
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;

const int maxn = 20 + 5;
int num[maxn][maxn], dp[maxn][maxn];

int main(){
	int n, k;
	while(cin >> n >> k){
		memset (num, 0, sizeof(num));
		memset (dp, 0, sizeof(dp));

		for (int i = 0; i < n; i++)
			for (int j = 0; j < k; j++)
				cin >> num[i][j];

		for (int i = 1; i < n; i++)
			for (int j = 0; j < k; j++)
				for (int l = 0; l < k; l++){
					if (l == 0) 		//即：第一种情况，没法比较大小，直接赋进去
						dp[i][j] = dp[i - 1][l] + abs(num[i - 1][l] - num[i][j]);
					else 				//和之前的情况进行比较
						dp[i][j] = min(dp[i - 1][l] + abs(num[i - 1][l] - num[i][j]), dp[i][j]);
				}
		//也可以给 dp[][] 赋一个很大的初值，
		//然后直接：dp[i][j] = min(dp[i - 1][l] + abs(num[i - 1][l] - num[i][j]), dp[i][j]); 就够了，就可以省去了
		// if else 。
		int minn=0x3f3f3f3f;
         for(int i=0;i<k;i++)
             if(dp[n-1][i]<minn)
                 minn=dp[n-1][i];
        cout << minn << endl;
	}
	return 0;
}
```



### 3.HDU - 1087	（最大子序列）

如今，一种名为“超级跳跃！跳！跳！“在HDU中非常流行。也许你是个好孩子，对这个游戏知之甚少，所以我现在就把它介绍给你。
该游戏可以由两个或两个以上的玩家玩。它由一个棋盘（棋盘）和一些棋子（棋子）组成，所有棋子都用正整数或“开始”或“结束”标记。玩家从起点开始，最后跳入终点。在跳跃过程中，玩家将访问路径中的棋子，但每个人都必须从一个棋子跳到另一个更大的棋子（可以假设起点是最小值，终点是最大值）。所有玩家都不能倒退。一个跳跃可以从一个棋子走到另一个，也可以穿过许多棋子，甚至你可以直接从起点到达终点。当然你在这种情况下得到零点。根据他的跳跃解决方案，当且仅当他能获得更大分数时，球员才是赢家。请注意，你的分数来自你跳跃路线上棋子价值的总和。
你的任务是根据给定的棋子列表输出最大值。

Input

>输入包含多个测试用例。每个测试用例的描述如下：
>N value_1 value_2 ... value_N
>保证N不超过1000，并且所有value_i都在32-int的范围内。
>以0开始的测试用例会终止输入，并且不会处理该测试用例。

Output

>对于每种情况，根据规则打印最大值，并且打印一行一个案例。

示例输入
```
3 1 3 2
4 1 2 3 4
4 3 3 2 1
0
```
示例输出
```
4
10
3
```
找最大子序列。

注意 `bn[0]` 要给一个初始值。

`bn[i]` 为从 0开始到 i 说能得到的最大的子列值。

看到 DP 先想办法给拆出来子问题啊，本题的子问题就是 0 - i 所能取得的最大值。

状态转移方程：如找 bn[i]  即：max (bn[0 - > i - 1]) && an[j] < an[i]

代码：

```
#include <iostream>
#include <algorithm>
using namespace std;

const int maxn = 1003;
int an[maxn];
int bn[maxn];

int main()
{
  int n;
  while (cin >> n && n != 0) {
      for (int i = 0; i < n; i++)
          cin >> an[i];
      memset(bn, 0, sizeof(bn));
      bn[0] = an[0];
      int maxn = -1000;
      for (int i = 1; i < n; i++) {
          int max1 = 0;
          for (int j = 0; j < i; j++) {
              if(an[i] > an[j] && bn[j] >= max1)
                  max1 = bn[j];
          }
          bn[i] = an[i] + max1;
          maxn = max(maxn, bn[i]);
      }
      cout << maxn << endl;
  }
return 0;
}
```



### 4.HDU - 1176	（数塔扩展）

都说天上不会掉馅饼，但有一天gameboy正走在回家的小径上，忽然天上掉下大把大把的馅饼。说来gameboy的人品实在是太好了，这馅饼别处都不掉，就掉落在他身旁的10米范围内。馅饼如果掉在了地上当然就不能吃了，所以gameboy马上卸下身上的背包去接。但由于小径两侧都不能站人，所以他只能在小径上接。由于gameboy平时老呆在房间里玩游戏，虽然在游戏中是个身手敏捷的高手，但在现实中运动神经特别迟钝，每秒种只有在移动不超过一米的范围内接住坠落的馅饼。现在给这条小径如图标上坐标： 
![img](https://odzkskevi.qnssl.com/3d6e7172eea0b64638702b3468ac5ad2?v=1520297755)
为了使问题简化，假设在接下来的一段时间里，馅饼都掉落在0-10这11个位置。开始时gameboy站在5这个位置，因此在第一秒，他只能接到4,5,6这三个位置中其中一个位置上的馅饼。问gameboy最多可能接到多少个馅饼？（假设他的背包可以容纳无穷多个馅饼） 

Input

>输入数据有多组。每组数据的第一行为以正整数n(0<n<100000)，表示有n个馅饼掉在这条小径上。在结下来的n行中，每行有两个整数x,T(0<T<100000),表示在第T秒有一个馅饼掉在x点上。同一秒钟在同一点上可能掉下多个馅饼。n=0时输入结束。 

Output

>每一组输入数据对应一行输出。输出一个整数m，表示gameboy最多可能接到m个馅饼。 
>提示：本题的输入数据量比较大，建议用scanf读入，用cin可能会超时。 

Sample Input

```
6
5 1
4 1
6 1
7 2
7 2
8 3
0
```

Sample Output

```
4
```

和数塔很像很像很像，把每时刻当一层，每个位置可以取它的正下（即原位不换位置），左下（向左），右下（向右）中的一个，取最大那个（因为同一点，同一时间可能掉落多个饼）。

有时候会想，这一时刻在这点，然而，下一时刻的点离这点很远，然后怎么选，这个是正着想，但是，可以倒着想啊，从最后一层开始，倒数第二层每个点都取最大，然后，倒数第三自然依然是最大，一直往上推，最后得出总的最大。DP 的子问题嘛。

状态转移方程：`an[i][j] = max(an[i + 1][j], max(an[i + 1][j - 1], an[i+ 1][j + 1]))`

注意边界问题，如 `an[i][0], an[i][10]`  一个不能向左，一个不能向右

代码：

```
#include <iostream>
#include <algorithm>
#include <cstdio>

using namespace std;
int an[100004][13];

int main()
{
	int n;
	while (cin >> n && n) {
		int a, b;
		memset(an, 0, sizeof(an));
		int t = 0;
		for (int i = 0; i < n; i++) {
			scanf("%d%d", &a, &b);
			an[b][a]++;
			t = max(t, b);
		}
		for (int i = t - 1; i >= 0 ; i--) {
			an[i][0] += max(an[i + 1][0], an[i + 1][1]);
			an[i][10] += max(an[i + 1][10], an[i + 1][9]);
			for (int j = 1; j < 10; j++) {
				an[i][j] += max(an[i + 1][j - 1], max(an[i + 1][j], an[i + 1][j + 1]));
			}
		}
		cout << an[0][5] << endl; //时间为 1 -> t an[0][5]为最初的位置，在 t == 0时，只有 5这一个位置。
	}
	return 0;
}
```



### 5.HDU - 1159	（最长公共子序列（不连续））

最长公共子序列（LCS）

A subsequence of a given sequence is the given sequence with some elements (possible none) left out. Given a sequence X = <x1, x2, ..., xm> another sequence Z = <z1, z2, ..., zk> is a subsequence of X if there exists a strictly increasing sequence <i1, i2, ..., ik> of indices of X such that for all j = 1,2,...,k, xij = zj. For example, Z = <a, b, f, c> is a subsequence of X = <a, b, c, f, b, c> with index sequence <1, 2, 4, 6>. Given two sequences X and Y the problem is to find the length of the maximum-length common subsequence of X and Y. 
The program input is from a text file. Each data set in the file contains two strings representing the given sequences. The sequences are separated by any number of white spaces. The input data are correct. For each set of data the program prints on the standard output the length of the maximum-length common subsequence from the beginning of a separate line. 

Input

```
abcfbc abfcab
programming contest 
abcd mnp
```

Output

```
4
2
0
```

**最优子结构性质：**
设序列 `X=<x1, x2, …, xm>` 和 `Y=<y1, y2, …, yn>` 的一个最长公共子序列 `Z=<z1, z2, …, zk>`，则：

1. 若 `xm = yn`，则 `zk = xm = yn` 则 `Zk-1` 是 `Xm-1` 和 `Yn-1` 的最长公共子序列；
   ![img](https://segmentfault.com/img/bVlvzm)
2. 若 `xm ≠ yn`， 要么`Z`是 `Xm-1` 和 `Y` 的最长公共子序列，要么 `Z` 是`X`和 `Yn-1` 的最长公共子序列。
   2.1 若 `xm ≠ yn` 且 `zk≠xm` ，则 `Z`是 `Xm-1` 和 `Y` 的最长公共子序列；
   2.2 若 `xm ≠ yn 且 zk ≠yn` ，则 `Z` 是`X`和 `Yn-1` 的最长公共子序列。
   综合一下2 就是求二者的大者
3. `dp[i][j]` X 串的前 i 个字符和 Y 串的前 j 个字符中最大的公共子序列长度。

代码：

```
#include <iostream>
#include <algorithm>
#include <cstdio>

using namespace std;
char an[1000],bn[1000];
int dp[1000][1000];

int main()
{
	while (~scanf("%s%s", an + 1, bn + 1)) {
		int an_len = strlen(an + 1);
		int bn_len = strlen(bn + 1);
		for (int i = 1; i <= an_len; i++)
			for (int j = 1; j <= bn_len; j++) {
				if (an[i] == bn[j])
					dp[i][j] = dp[i - 1][j - 1] + 1;
				else
					dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
			}
		cout << dp[an_len][bn_len] << endl;
	}
	return 0;
}
```



### 6.HUD - 1231	（最大连续子序列和（字串）（连续））

给定K个整数的序列{ N1, N2, ..., NK }，其任意连续子序列可表示为{ Ni, Ni+1, ..., 
Nj }，其中 1 <= i <= j <= K。最大连续子序列是所有连续子序列中元素和最大的一个，
例如给定序列{ -2, 11, -4, 13, -5, -2 }，其最大连续子序列为{ 11, -4, 13 }，最大和 
为20。 
在今年的数据结构考卷中，要求编写程序得到最大和，现在增加一个要求，即还需要输出该 
子序列的第一个和最后一个元素。 

Input

> 测试输入包含若干测试用例，每个测试用例占2行，第1行给出正整数K( < 10000 )，第2行给出K个整数，中间用空格分隔。当K为0时，输入结束，该用例不被处理。 

Output

> 对每个测试用例，在1行里输出最大和、最大连续子序列的第一个和最后一个元 素，中间用空格分隔。如果最大连续子序列不唯一，则输出序号i和j最小的那个（如输入样例的第2、3组）。若所有K个元素都是负数，则定义其最大和为0，输出整个序列的首尾元素。 

Sample Input

```
6
-2 11 -4 13 -5 -2
10
-10 1 2 3 4 -5 -23 3 7 -21
6
5 -8 3 2 5 0
1
10
3
-1 -5 -2
3
-1 0 -2
0
```

Sample Output

```
20 11 13
10 1 4
10 3 5
10 10 10
0 -1 -2
0 0 0
```

最大连续子序列和只可能是以位置`0～n-1`中某个位置结尾。当遍历到第i个元素时，判断在它前面的连续子序列和是否大于0，如果大于0，则以位置i结尾的最大连续子序列和为元素i和前门的连续子序列和相加；否则，则以位置i结尾的最大连续子序列和为元素i。 

状态转移方程： `sum[i]=max(sum[i-1]+a[i],a[i])`

用 `start[i]` 记录到 i 的起始元素值，如果 `sum[i - 1] + a[i] >  a[i]` 那么，`start[i] = start[i - 1]` 否则 `start[i] = an[i]`

用 `ans` 记录最大连续字串和，最后把`dp[]` 从头到尾遍历，第一个等于`ans` 即为结果，同时符合，有多个相同最大值时，输出序号最小的那个。

代码：

```
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;
const int maxn = 10000 + 3;
int an[maxn], dp[maxn], start[maxn];
int main() {
	int k;
	while (cin >> k && k) {
		memset(an, 0, maxn);
		memset(dp, 0, maxn);
		memset(start, 0, maxn);
		for (int i = 0; i < k; i++) {
			cin >> an[i];
		}
		int ans = -100;
		start[0] = an[0];
		for (int i = 0; i < k; i++) {
			if (dp[i - 1] + an[i] > an[i]) {
				start[i] = start[i - 1];
			}
			else
				start[i] = an[i];
			dp[i] = max(dp[i - 1] + an[i], an[i]);
			ans = max(ans, dp[i]);
		}
		if (ans >= 0) {
			for (int i = 0; i < k; i++) {
				if (dp[i] == ans) {
					cout << ans << " " << start[i] << " " << an[i] << endl;
					break;
				}
			}
		}
		else {
			cout << "0 " << an[0] << " " << an[k - 1] << endl;
		}
	}
	return 0;
}
```

