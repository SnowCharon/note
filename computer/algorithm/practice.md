## 动态规划
### 爬楼梯
#### topic
```
假设你正在爬楼梯。需要 阶你才能到达楼顶
每次你可以爬 1 或 2个台阶。你有多少种不同的方法可以爬到楼顶呢?
```

#### sample
示例 1:
```
输入: n = 2
输出: 2
解释:有两种方法可以爬到楼顶
1.1阶+1阶
2.2 阶
```
示例 2:
```
输入: n = 3
输出: 3
解释:有三种方法可以爬到楼顶
1.1阶+1阶+1阶
2.1阶+2 阶
3.2阶+1阶
```

> [!NOTE] tip
> 1 <= n <= 45

#### code
```java
class Solution {
	public int climbStairs(int n) {
		int[] arr = new int[46];
		arr[1] = 1;
		arr[2] = 2;

		for(int i = 3;i <= n;i++){
			arr[i] = arr[i-1] + arr[i-2];
		}
		
		return arr[n];
	}
}
```

### 杨辉三角
#### topic
给定一个非负整数 *numRows*，生成「杨辉三角」的前 *numRows* 行。
在「杨辉三角」中，每个数是它左上方和右上方的数的和。
![](assets/1626927345-DZmfxB-PascalTriangleAnimated2.gif)

#### sample
示例 1:

```
输入: numRows = 5
输出: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```

示例 2:
```
输入: numRows = 1
输出: [[1]]
```

> [!NOTE] tip
> 1 <= numRows <= 30

#### code
```java
class Solution {
	public List<List<Integer>> generate(int numRows) {
		Integer[][] dp = new Integer[numRows][];
		
		for(int i = 0;i < numRows;i++){
			dp[i] = new Integer[i+1];
			dp[i][0]=dp[i][i] =1;
			for(int j = 1;j < i;j++){
				dp[i][j] = dp[i-1][j-1] +dp[i-1][j];
			}
		}
		List<List<Integer>> result = new ArrayList<>();
		for(Integer[] row : dp){
			result.add(Arrays.asList(row));
		}
		return result;
	}
}
```

### 打家劫舍
#### topic
```
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。
```

#### sample
示例1：
```
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```
示例2：
```
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

> [!NOTE] tip
> 1 <= nums.length <= 100
> 0 <= nums[i] <= 400

#### code
```java
class Solution {
    public int rob(int[] nums) {
	    int max = 101;
		int[] dp = new int[max];
		dp[0] = nums[0];
		int length = nums.length;
		
		for(int i = 1;i < length;i++){
			if(i==1){
				dp[i] = Math.max(nums[0],nums[1]);
			}else{
				dp[i] = Math.max(dp[i-1],dp[i-2] + nums[i]);
			}
		}
		return dp[length-1];
    }
}
```




















