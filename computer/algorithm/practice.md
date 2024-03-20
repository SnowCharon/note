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




# 小米
## 反转链表
给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表
![](assets/Pasted%20image%2020240316210741.png)
**输入：head = [1,2,3,4,5]
输出：**[5,4,3,2,1]
**示例 2：**

![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

**输入：head = [1,2]
输出：**[2,1]

**示例 3：**

**输入：head = []
输出：**[]
```java
package xiaomi;  
public class A {  
    public static void main(String[] args) {  
        ListNode A = new ListNode(1);  
        ListNode B = new ListNode(2);  
        ListNode C = new ListNode(3);  
        ListNode D = new ListNode(4);  
        ListNode E = new ListNode(5);  
        A.next = B;  
        B.next = C;  
        C.next = D;  
        D.next = E;  
  
        Solution solution = new Solution();  
        ListNode listNode = solution.reverseList(A);  
        System.out.println(listNode);  
  
    }  
}  
  
// Definition for singly-linked list.  
class ListNode {  
    int val;  
    ListNode next;  
  
    ListNode() {  
    }  
    ListNode(int val) {  
        this.val = val;  
    }  
  
    ListNode(int val, ListNode next) {  
        this.val = val;  
        this.next = next;  
    }  
  
    @Override  
    public String toString() {  
        return "ListNode{" +  
                "val=" + val +  
                ", next=" + next +  
                '}';  
    }  
}  
  
class Solution {  
    public ListNode reverseList(ListNode head) {  
        if (head == null) {  
            return head;  
        }  
        ListNode L = new ListNode();  
        L.next = head;  
        ListNode p = L.next;  
        L.next = null;  
        while (p != null) {  
            ListNode q = p.next;  
            p.next = L.next;  
            L.next = p;  
            p = q;  
        }  
        return L.next;  
    }  
}
```

## 二叉树的层序遍历
给你二叉树的根节点 `root` ，返回其节点值的 **层序遍历** 。 （即逐层地，从左到右访问所有节点）。

**示例 1：**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```java
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[9,20],[15,7]]

示例 2：
输入：root = [1]
输出：[[1]]

示例 3
输入：root = []
输出：[]
```
```java
/**

 * Definition for a binary tree node.

 * public class TreeNode {

 * int val;

 * TreeNode left;

 * TreeNode right;

 * TreeNode() {}

 * TreeNode(int val) { this.val = val; }

 * TreeNode(int val, TreeNode left, TreeNode right) {

 * this.val = val;

 * this.left = left;

 * this.right = right;

 * }

 * }

 */

class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {

        List<List<Integer>> res = new ArrayList<>();
        fun(res, root, 0);
        return res;
    }
    public void fun(List<List<Integer>> res,TreeNode node,int level) {
        if(node==null){
            return;
        }
        if(res.size() <= level){
            List<Integer> arr = new ArrayList<>();
             res.add(arr);
        }
          res.get(level).add(node.val);
         fun(res,node.left,level+1);
         fun(res,node.right,level+1);
    }
}
```






## 回文数
```java
class Solution {
    public boolean isPalindrome(int x) {
        String s = String.valueOf(x);
        StringBuilder reverse = new StringBuilder(s).reverse();
        if (s.contentEquals(reverse)) {
            return true;
        } else {

            return false;
        }
    }
}
```

## 编辑距离
```java


```

## 2024年3月16日（12）

### 104.二叉树的最大深度

```java
class Solution {
    public int maxDepth(TreeNode root) {
        // 用DFS做，只需要判断层数即可，每一次都用max去取一个最大的，并且+1（当前层的深度）返回给上层
        if(root==null) return 0;
        return Math.max(maxDepth(root.left), maxDepth(root.right))+1;
    }
}
```

### 206.反转链表

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null,cur = head; // pre是前节点，cur是后节点
        while(cur != null){ // 对于最终的结果，pre是前结点，而cur则是到了原链表的终点null，所以条机是cur！=null
            ListNode tmp = cur.next; // 存一下cur的下一个节点
            cur.next = pre; // 存完把他next指向前节点，也就是pre
            pre = cur; // pre此时挪到已经反转的结点cur上
            cur = tmp; // cur指针移动到tmp，也就是下一个结点上
        }
        return pre; // pre在前面，所以返回的是pre
    }
}
```

### 102.二叉树层序遍历

```java
// BFS
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new ArrayDeque<>(); // 用来遍历的队列
        List<List<Integer>> list = new ArrayList<>(); // 用来返回的List
        if (root != null) {
            queue.add(root); // 第一步先把root放到queue中
        }
        while(!queue.isEmpty()) { // 这里while条件是队列取完了
            int n = queue.size(); // 这里记录一下当前层的size，因为下面会在遍历队列的时候添加值
            List<Integer> tmpList = new ArrayList<>();
            for(int i = 0;i<n;i++){ // 此次用for只是为了遍历固定的次数
                TreeNode node = queue.poll();
                tmpList.add(node.val);
                if(node.left !=null){
                    queue.add(node.left);
                }
                if(node.right !=null){
                    queue.add(node.right);
                }
            }
            list.add(tmpList);
        }
        return list;
    }
}
```

```java
// DFS
class Solution {
   	public List<List<Integer>> list = new ArrayList<>();
   	public List<List<Integer>> levelOrder(TreeNode root) {
 	       dfs(root,0);
 	       return list;
 	}

  	public void dfs(TreeNode root, int num){
        if (root == null){
        	return;
    	}
        if (list.size() > num){
            list.get(num).add(root.val);
        }else {
            List<Integer> arrayList = new ArrayList<>();
            arrayList.add(root.val);
            list.add(arrayList);
        }
        dfs(root.left,num+1);
        dfs(root.right,num+1);
    }
}
```

### 3.无重复字符的最长子串

```java
// abba
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s.length()==0) return 0;
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        int max = 0;
        int left = 0;
        for(int i = 0; i < s.length(); i ++){
            if(map.containsKey(s.charAt(i))){
                left = Math.max(left,map.get(s.charAt(i)) + 1); // 防止map中的旧值影响，所以要取max
            }
            map.put(s.charAt(i),i);
            max = Math.max(max,i-left+1); // 每一次窗口滑动，都计算一次窗口大小
        }
        return max;
    }
}
```

### 15.三数之和

```java
class Solution {
    public static List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> ans = new ArrayList();
        int len = nums.length;
        if(nums == null || len < 3) return ans;
        Arrays.sort(nums); // 排序
        for (int i = 0; i < len ; i++) {
            if(nums[i] > 0) break; // 如果当前数字大于0，则三数之和一定大于0，所以结束循环
            if(i > 0 && nums[i] == nums[i-1]) continue; // 去重
            int L = i+1;
            int R = len-1;
            while(L < R){
                int sum = nums[i] + nums[L] + nums[R];
                if(sum == 0){
                    ans.add(Arrays.asList(nums[i],nums[L],nums[R]));
                    while (L<R && nums[L] == nums[L+1]) L++; // 去重
                    while (L<R && nums[R] == nums[R-1]) R--; // 去重
                    L++;
                    R--;
                }
                else if (sum < 0) L++;
                else if (sum > 0) R--;
            }
        }        
        return ans;
    }
}
```

### 215.数组中第K大的元素

```java
// 通过小顶堆找到第k大的元素
class Solution {
    public int findKthLargest(int[] nums, int k) {
        Queue<Integer> pq = new PriorityQueue<>();   // 将数组加入小顶堆，堆中维护当前值最大的k个数
        for(int num: nums){
            pq.offer(num);
            if(pq.size() > k){
                pq.poll();   // 堆中元素超过k个，弹出最小的那个
            }
        }
        return pq.peek();    // 最后堆顶的即为第k大的数
    }
}
```

```java
// 快速排序
class Solution {
    public int findKthLargest(int[] nums, int k) {
        // 分治策略算法(典型用例就是快速排序)
        int n = nums.length;
        return partition(nums, 0, n - 1, n - k);
    }

    public int partition(int[] nums, int left, int right, int key) {
        // 当子数组长度小于1时终止递归
        if (left >= right)
            return nums[key];
        // 根据哨兵划分
        int i = left, j = right;
        while (i < j) {
            while (i < j && nums[j] >= nums[left]) {
                j--; // 从右向左找首个小于基准数的元素
            }
            while (i < j && nums[i] <= nums[left]) {
                i++; // 从左向右找首个大于基准数的元素
            }
            // swap
            swap(nums, i, j);
        }
        swap(nums, i, left);// 将基准数至于两子数组之间
        // 递归子数组（这里与quick_sort算法不同）

        if (key <= j) {
            while (j > 0 && nums[j - 1] == nums[i])//跳过与哨兵相同的下标
                j--;
            return partition(nums, left, j - 1, key);
        } else {
            while (j < key && nums[j + 1] == nums[i])//跳过与哨兵相同的下标
                j++;
            return partition(nums, j + 1, right, key);
        }
    }

    public void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

### 21.合并两个有序链表

```java
// 递归
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        }
        else if (l2 == null) {
            return l1;
        }
        else if (l1.val < l2.val) {q
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        }
        else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
}
```

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode prehead = new ListNode(-1);

        ListNode prev = prehead;
        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                prev.next = l1;
                l1 = l1.next;
            } else {
                prev.next = l2;
                l2 = l2.next;
            }
            prev = prev.next;
        }

        // 合并后 l1 和 l2 最多只有一个还未被合并完，我们直接将链表末尾指向未合并完的链表即可
        prev.next = l1 == null ? l2 : l1;

        return prehead.next;
    }
}
```

### 88.合并两个有序数组

```java
// 暴力
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        for (int i = 0; i != n; ++i) {
            nums1[m + i] = nums2[i];
        }
        Arrays.sort(nums1);
    }
}
```

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int p1 = 0, p2 = 0;
        int[] sorted = new int[m + n];
        int cur;
        while (p1 < m || p2 < n) {
            if (p1 == m) {
                cur = nums2[p2++];
            } else if (p2 == n) {
                cur = nums1[p1++];
            } else if (nums1[p1] < nums2[p2]) {
                cur = nums1[p1++];
            } else {
                cur = nums2[p2++];
            }
            sorted[p1 + p2 - 1] = cur;
        }
        for (int i = 0; i != m + n; ++i) {
            nums1[i] = sorted[i];
        }
    }
}
```

### 876.链表的中间结点

```java
 public ListNode middleNode(ListNode head) {
        if (head == null) {
            return null;
        }
        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
 }
```

### 143.重排链表（重点，复习到了876、206、21）

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public void reorderList(ListNode head) {

        if(head == null) return;

        // 这部分是求链表中点

        ListNode fast = head;
        ListNode low = head;

        while(fast != null && fast.next != null){
            low = low.next;
            fast = fast.next.next;
        }

        ListNode temp = low;
        low = low.next;
        temp.next = null;

        // 这部分是链表逆序
        ListNode pre = null;
        ListNode cur = low;

        while(cur != null){
            ListNode tmp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = tmp;
        }

        // 这部分是链表合并

        ListNode tmpHead = new ListNode(-1);
        while(head != low && pre != null){
            tmpHead.next = head;
            head = head.next;
            tmpHead = tmpHead.next;
            tmpHead.next = pre;
            pre = pre.next;
            tmpHead = tmpHead.next;
        }
        
        tmpHead.next = pre==null?head:pre;

        head = tmpHead.next;
    }
}
```

### 美团笔试1-格式检查

```java
// Abc 或 ABC 或 abc 均正确
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.next();

        int lowerNum = Meituan21.getLowerNumber(str.substring(1));
        int upperNum = str.length() - lowerNum - 1;
        int upperRet = Integer.MAX_VALUE;
        int lowerRet = Integer.MAX_VALUE;
        if (Character.isUpperCase(str.charAt(0))) { // 如果首字母为大写，则需要将后面的全部小写或者大写
            upperRet = Math.min(upperNum, lowerNum);
        } else { // 如果首字母为小写 aAAAAb
            lowerRet = Math.min(upperNum, lowerNum + 1);
        }
        System.out.println(Math.min(upperRet, lowerRet));
    }

    public static int getLowerNumber(String str) {
        int num = 0;
        for (char c : str.toCharArray()) {
            if (Character.isLowerCase(c)) num++;
        }
        return num;
    }
}
```

### 美团笔试2-员工号验证

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        for (int i = 0; i < n; i++) {
            check(sc.next());
        }
    }

    public static void check(String str) {
        if (str.length() != 10) {
            System.out.println("No");
            return;
        }

        int yy = Integer.parseInt(str.substring(0, 2));

        if (yy >= 70 && yy <= 99) yy += 1900;
        else yy += 2000;
        int mm = Integer.parseInt(str.substring(2, 4));
        int aa = Integer.parseInt(str.substring(4, 6));

        long sum = 0;
        for (char c : str.toCharArray()) {
            sum += Long.parseLong(String.valueOf(c));
        }

        if (yy < 1970 || yy > 2023) {
            System.out.println("No");
            return;
        }

        if (mm < 1 || mm > 12) {
            System.out.println("No");
            return;
        }

        if (aa < 1 || aa > getDayInMonth(yy, mm)) {
            System.out.println("No");
            return;
        }

        if (Long.parseLong(str) % 13L != 0) {
            System.out.println("No");
            return;
        }

        System.out.println("Yes");
    }

    public static int getDayInMonth(int year, int month) {
        if (month == 2) {
            if (isLeapYear(year)) {
                return 29;
            } else {
                return 28;
            }
        } else if (month == 4 || month == 6 || month == 9 || month == 11) {
            return 30;
        } else {
            return 31;
        }
    }

    public static boolean isLeapYear(int year) {
        return (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
    }
}

```

## 2024年3月17日（11）

### 22.括号生成

```java
// DFS + 剪纸算法 生成括号，通过隐形树结构
class Solution {
    List<String> list = new ArrayList<>();

    public List<String> generateParenthesis(int n) {
        fun(n,n,"");
        return list;
    }

    public void fun (int left,int right,String str){
        if(left>right){
            return;
        }

        if(left==0 && right==0){
            list.add(str);
            return;
        }

        if(left != 0) {
            fun(left-1,right,str+"(");
        }

        if(right != 0){
            fun(left,right-1,str+")");
        }
    }
}
```

### 977.有序数组的平方

```java
// 直接平方然后排序
class Solution {
    public int[] sortedSquares(int[] nums) {
        int[] ans = new int[nums.length];
        for (int i = 0; i < nums.length; ++i) {
            ans[i] = nums[i] * nums[i];
        }
        Arrays.sort(ans);
        return ans;
    }
}
```

```java
// 前后指针，还有一个pos定位，哪个大就放到pos的位置，然后pos后移
class Solution {
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        for (int i = 0, j = n - 1, pos = n - 1; i <= j;) {
            if (nums[i] * nums[i] > nums[j] * nums[j]) {
                ans[pos] = nums[i] * nums[i];
                ++i;
            } else {
                ans[pos] = nums[j] * nums[j];
                --j;
            }
            --pos;
        }
        return ans;
    }
}
```

### 1143.最长公共子序列

```java
// dp 定义 f[i][j]表示字符串text1的[1,i]区间和字符串text2的[1,j]区间的最长公共子序列长度（下标从1开始）。
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int n = text1.length(), m =  text2.length();
        int[][] f = new int[n + 1][m + 1];
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= m; ++j) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    f[i][j] = f[i - 1][j - 1] + 1;
                } else {
                    f[i][j] = Math.max(f[i - 1][j], f[i][j - 1]);
                }
            }
        }
        return f[n][m];
    }
}
```

### 141.环形链表

```java
// 快慢指针
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode low = head;
        ListNode fast = head;
        while(fast != null && fast.next != null){
            low = low.next;
            fast = fast.next.next;
            if(fast == low){
                return true;
            }
        }
        return false;
    }
}
```

### 142.环形链表Ⅱ

```java
// 在找到环的情况下，找到环入口
public class Solution {
    public ListNode detectCycle(ListNode head) {
        // 这里是判断有没有环
        ListNode low = head;
        ListNode fast = head;
        boolean cycle = false;
        while(fast != null && fast.next != null){
            low = low.next;
            fast = fast.next.next;
            if(fast == low){
                cycle = true;
                break;
            }
        }

        if(!cycle) return null;

        // 这里是一个指针指向头，一个指针指向会合点
        ListNode idx1 = head;
        ListNode idx2 = low;

        while(idx1 != idx2){
            idx1 = idx1.next;
            idx2 = idx2.next;
        }

        return idx1;
    }
}
```

### （没太理解）93.复原IP地址

```java
// 暴力
class Solution {
    public List<String> restoreIpAddresses(String s) {
        // IP地址结果集
        List<String> res = new ArrayList<String>();
        int len = s.length();
        // 单个IP地址
        StringBuffer ip = new StringBuffer();
        // 我用了四重循环，你骂我吧
         for (int a=1; a<=3; a++){
             for (int b=1; b<=3; b++){
                 for (int c=1; c<=3; c++){
                     for (int d=1; d<=3; d++) {
                        // A、B、C、D 四段所占字符总数 一定是需要等于len(s)的
                        if (a+b+c+d == s.length()) {
                            int A = Integer.parseInt(s.substring(0, a));
                            int B = Integer.parseInt(s.substring(a, a+b));
                            int C = Integer.parseInt(s.substring(a+b, a+b+c));
                            int D = Integer.parseInt(s.substring(a+b+c));

                            // 都 <= 255就可以了
                            if (A<=255 && B<=255 && C<=255 && D<=255){
                                ip.append(A).append(".").append(B).append(".").append(C).append(".").append(D);
                                // 这里应该知道吧，一个IP地址长度 = 原s长度 + 3(3个点字符)
                                if( ip.length() == len + 3){
                                    // 这里为什么要判断呢：
                                    // 比如s=101023, 拆分成1，0，1，023
                                    // 023会被我们 字符串 转数字抹去0，已经缺少 s中的字符了，所以我们这里需要额外判断
                                    res.add(ip.toString());
                                }
                                ip = new StringBuffer();
                            }
                        }   
                     }
                 }
             }
         }
        return res;
    }
}
```

### 75.颜色分类

```java
// 把0和1按顺序排序即可
class Solution {
    public void sortColors(int[] nums) {
        int n = nums.length;
        int ptr = 0;
        for (int i = 0; i < n; ++i) {
            if (nums[i] == 0) {
                int temp = nums[i];
                nums[i] = nums[ptr];
                nums[ptr] = temp;
                ++ptr;
            }
        }
        for (int i = ptr; i < n; ++i) {
            if (nums[i] == 1) {
                int temp = nums[i];
                nums[i] = nums[ptr];
                nums[ptr] = temp;
                ++ptr;
            }
        }
    }
}
```

### 快速排序

```java
import java.util.Arrays;

public class Main{
    public static void main(String[] args) {
        int[] arr ={4,7,6,5,3,2,8,1};
        quickSort(arr,0,arr.length-1);
        System.out.println(Arrays.toString(arr));
    }

    public static void quickSort(int[] arr,int startIndex,int endIndex) {
        if (startIndex >= endIndex) {
            return;
        }

        int pIndex=partition(arr,startIndex,endIndex);

        quickSort(arr,startIndex,pIndex-1);
        quickSort(arr,pIndex+1,endIndex);
    }

    public static int partition(int[] arr,int startIndex,int endIndex){
        int p = arr[startIndex]; // 基准值
        int l = startIndex; // 左指针
        int r = endIndex; // 右指针

        while(l != r){
            while (r>l && arr[r] > p) r--;
            while (r>l && arr[l] <= p) l++;
            if(r>l) {
                int temp = arr[l];
                arr[l] = arr[r];
                arr[r] = temp;
            }
        }

        arr[startIndex] = arr[l];
        arr[l] = p;
        return l;
    }
}
```

### 5.最长回文子串

```java
// 中心扩散
class Solution {
    public String longestPalindrome1(String s) {
        if (s == null || s.length() == 0) {
            return "";
        }
        int strLen = s.length();
        int left = 0;
        int right = 0;
        int len = 1;
        int maxStart = 0;
        int maxLen = 0;

        for (int i = 0; i < strLen; i++) {
            left = i - 1;
            right = i + 1;
            while (left >= 0 && s.charAt(left) == s.charAt(i)) {
                len++;
                left--;
            }
            while (right < strLen && s.charAt(right) == s.charAt(i)) {
                len++;
                right++;
            }
            while (left >= 0 && right < strLen && s.charAt(right) == s.charAt(left)) {
                len = len + 2;
                left--;
                right++;
            }
            if (len > maxLen) {
                maxLen = len;
                maxStart = left;
            }
            len = 1;
        }
        return s.substring(maxStart + 1, maxStart + maxLen + 1);
    }
}
```

```java
// 最长公共字串 正序和倒序求一样的子串   ahch hcha
public String longestPalindrome(String s) {
    if (s.equals(""))
        return "";
    String origin = s;
    String reverse = new StringBuffer(s).reverse().toString(); //字符串倒置
    int length = s.length();
    int[][] arr = new int[length][length];
    int maxLen = 0;
    int maxEnd = 0;
    for (int i = 0; i < length; i++)
        for (int j = 0; j < length; j++) {
            if (origin.charAt(i) == reverse.charAt(j)) {
                if (i == 0 || j == 0) {
                    arr[i][j] = 1;
                } else {
                    arr[i][j] = arr[i - 1][j - 1] + 1;
                }
            }
            if (arr[i][j] > maxLen) { 
                maxLen = arr[i][j];
                maxEnd = i; //以 i 位置结尾的字符
            }

        }
	}
	return s.substring(maxEnd - maxLen + 1, maxEnd + 1);
}
```

### 14.最长公共前缀

```java
// 纵向查找
class Solution {
    public String longestCommonPrefix(String[] strs) {
        int length = strs[0].length();

        for(int i = 0; i<length; i++){
            char c = strs[0].charAt(i);
            for(int j = 0; j < strs.length ; j++){
                if(i >= strs[j].length() || c != strs[j].charAt(i)){
                    return strs[0].substring(0,i);
                }
            }
        }

        return strs[0];
    }
}
```

### 20.有效括号

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for(char c : s.toCharArray()){
            if(c == '('){
                stack.push(')');
            } else if (c == '{'){    
                stack.push('}');
            } else if(c == '['){
                stack.push(']');
            } else if(stack.isEmpty() || c != stack.pop()){
                return false;
            }
        }
        return stack.isEmpty();
    }
}
```

## 2024年3月18日

### 231.2的幂

```java
// 恒有 n & (n - 1) == 0
class Solution {
    public boolean isPowerOfTwo(int n) {
        return n > 0 && (n & (n - 1)) == 0;
    }
}
```

```java
// 递归计算
class Solution {
    public boolean isPowerOfTwo(int n) {
        if (n <= 0) {
            return false;
        }
        if (n == 1) {
            return true;
        }
        if (n % 2 != 0) {
            return false;
        }
        return isPowerOfTwo(n / 2);
    }
}
```

### 前中后序遍历

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        inorder(root, res);
        return res;
    }

    public void inorder(TreeNode root, List<Integer> res) {
        if (root == null) {
            return;
        }
        // 前序
        res.add(root.val);
        inorder(root.left, res);
        inorder(root.right, res);
        
        // 中序
        inorder(root.left, res);
		res.add(root.val);
        inorder(root.right, res);
        
        // 后续
        inorder(root.left, res);
        inorder(root.right, res);
		res.add(root.val);
    }
}
```

