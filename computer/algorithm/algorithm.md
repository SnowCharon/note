# 算法基础
## 概念
数据结构：是一种存储和组织数据的方式，旨在便于访问和修改。
算法：任何良定义的计算过程，该过程取某个值或值的集合作为输入并产生某个值或值得集合作为输出。
**衡量一个算法优劣的标准是看它所消耗的时间和空间**

算法的复杂性是指算法执行所需的计算资源和时间的量度。通常用时间复杂度和空间复杂度来衡量算法的复杂性。
时间复杂度是指算法执行所需的时间，通常用大0符号表示，例如，0(1)表示常数时间复杂度，0(n)表示线性时间复杂度，0(n^2)表示平方时间复杂度等等。时间复杂度越低，算法执行所需的时间越短，算法效率越高。
空间复杂度是指算法执行所需的内存空间，也通常用大0符号表示。例如，0(1)表示常数空间复杂度，0(n)表示线性空间复杂度，0(n^2)表示平方空间复杂度等等。空间复杂度越低，算法执行所需的内存空间越小，算法效率越高。
衡量算法的复杂性可以帮助我们选择最优的算法来解决问题，提高程序的效率和性能
## 分治思想
分治模式在每层递归时都有三个步骤：**分解、解决和合并**
分解：原问题为若干子问题，这些子问题是原问题的规模较小的实例
解决：递归求解子问题
合并：子问题的解成原问题的解
![](assets/026497674537261a6f3b885db0466ed.jpg)
 分治策略常会用到递归式，而求解递归式的三种方法：
 1. 代入法
 2. 递归树法
 3. 主方法
### 最大子数组问题
最大子数组使用分支策略，将数组分为两半，即A\[low……mid]和A\[mid……high]
则最大子数组有三种情况：
- 仅在A\[low……mid]区域
- 仅在A\[mid……high]区域
- 横跨过mid点，即在A\[mid……high]也在A\[low……mid]区域
第一种情况和第二种情况还是再求一个数组的最大子数组，不过是规模变小了，因此要求的应该是第三种，横跨两边的情况
伪代码
```java
FIND-MAX-CROSSING-SUBARRAY(A,low,mid,high)
	left-sum = -∞
	sum = 0
	for i = mid downto low
		sum = sum + A[i]
		if sum > left-sum
			left-sum = sum
			max-left = i
	//1-7求出左半部分A[low……mid]最大子数组
	right-sum = -∞
	sum = 0
	for j = mid + 1 to high
		sum = sum + A[j]
		if sum > right-sum
			right-sum = sum
			max-right = j
	//9-15求出右半部分A[mid……reight]最大子数组
	return (max-left,max-right,left-sum+right-sum)
```
示例代码
```java
package study.january.nineteen;  
  
import java.util.Arrays;  
  
public class MaxArray {  
    public static void main(String[] args) {  
        int[] arr = new int[]{1, 2, -4, 9, 7, 6, 2, -5};  
        System.out.println(Arrays.toString(arr));  
  
        Result result = maxCrossArray(arr, 0, arr.length / 2, arr.length - 1);  
        System.out.println(result);  
    }  
  
    private static Result maxCrossArray(int[] arr, int low, int mid, int high) {  
        int sum = 0;  
        int max_left = 0;  
        int left_sum = -Integer.MAX_VALUE;  
        for (int i = mid; i >= low; i--) {  
            sum += arr[i];  
            if (sum > left_sum) {  
                left_sum = sum;  
                max_left = i;  
            }  
        }  
        sum = 0;  
        int max_right = 0;  
        int right_sum = -Integer.MAX_VALUE;  
        for (int i = mid + 1; i <= high; i++) {  
            sum += arr[i];  
            if (sum > right_sum) {  
                right_sum = sum;  
                max_right = i;  
            }  
        }  
        return new Result(max_left, max_right, left_sum + right_sum);  
    }  
}  
  
class Result {  
    private Integer begin;  
    private Integer end;  
    private Integer sum;  
  
    public Result(Integer begin, Integer end, Integer sum) {  
        this.begin = begin;  
        this.end = end;  
        this.sum = sum;  
    }  
  
    @Override  
    public String toString() {  
        return "Result{" +  
                "begin=" + begin +  
                ", end=" + end +  
                ", sum=" + sum +  
                '}';  
    }  
  
    public Integer getBegin() {  
        return begin;  
    }  
  
    public void setBegin(Integer begin) {  
        this.begin = begin;  
    }  
  
    public Integer getEnd() {  
        return end;  
    }  
  
    public void setEnd(Integer end) {  
        this.end = end;  
    }  
  
    public Integer getSum() {  
        return sum;  
    }  
  
    public void setSum(Integer sum) {  
        this.sum = sum;  
    }  
}
```
![](assets/Pasted%20image%2020240119213330.png)
在这个测试用例中，最大子数组就横跨左右
一个数组包含n个元素（即n = high - low + 1），由于两个for循环每次迭代话费O(1)时间，因此仅需要统计一共进行了多少次迭代：
（mid - low + 1) + (high - mid) = high - low + 1 = n

完整伪代码
```java
FIND-MAXIMUM-SUBARRAY(A,low,high)
	if high == low
		return (low,high,A[low])
	else mid = ⌊(low + high) / 2⌋
		(left-low,left-high,left-sum) = FIND-MAXIMUM-SUBARRAY(A,low,mid)
		(right-low,right-high,right-sum) = FIND-MAXIMUM-SUBARRAY(A,mid+1,high)
		(cross-low,cross-high,cross-sum) = FIND-MAX-CROSSING-SUBARRAY(A,low,mid,high)
		if left-sum >= right-sum and left-sum >= cross-sum
			return (left-low,left-right,left-sum)
			
		else if right-sum >= left-sum and right-sum >= cross-sum
			return (right-low,right-right,right-sum)
		else return (cross-low,cross-right,cross-sum)
```
完整代码
```java
package study.january.nineteen;  
  
import java.util.Arrays;  
  
public class MaxArray {  
    public static void main(String[] args) {  
        int[] arr = new int[]{13,-3,-25,29,-3,-16,-23,18,20,-7,12,-5,-22,15,-4,7};  
        System.out.println(Arrays.toString(arr));  
  
        Result result = maxArray(arr, 0, arr.length - 1);  
        System.out.println(result);  
    }  
  
    private static Result maxArray(int[] arr, int low, int high) {  
        if (high == low) {  
            return new Result(low, high, arr[low]);  
        } else {  
            int mid = (low + high) / 2;  
            Result left = maxArray(arr, low, mid);  
            Result right = maxArray(arr, mid + 1, high);  
            Result cross = maxCrossArray(arr, low, mid, high);  
            if (left.getSum() >= right.getSum() && left.getSum() >= cross.getSum()) {  
                return left;  
            } else if (right.getSum() >= left.getSum() && right.getSum() >= cross.getSum()) {  
                return right;  
            } else {  
                return cross;  
            }  
        }  
  
    }  
  
    private static Result maxCrossArray(int[] arr, int low, int mid, int high) {  
        int sum = 0;  
        int max_left = 0;  
        int left_sum = -Integer.MAX_VALUE;  
        for (int i = mid; i >= low; i--) {  
            sum += arr[i];  
            if (sum > left_sum) {  
                left_sum = sum;  
                max_left = i;  
            }  
        }  
        sum = 0;  
        int max_right = 0;  
        int right_sum = -Integer.MAX_VALUE;  
        for (int i = mid + 1; i <= high; i++) {  
            sum += arr[i];  
            if (sum > right_sum) {  
                right_sum = sum;  
                max_right = i;  
            }  
        }  
        return new Result(max_left, max_right, left_sum + right_sum);  
    }  
}  
  
class Result {  
    private Integer begin;  
    private Integer end;  
    private Integer sum;  
  
    public Result(Integer begin, Integer end, Integer sum) {  
        this.begin = begin;  
        this.end = end;  
        this.sum = sum;  
    }  
  
    @Override  
    public String toString() {  
        return "Result{" +  
                "begin=" + begin +  
                ", end=" + end +  
                ", sum=" + sum +  
                '}';  
    }  
  
    public Integer getBegin() {  
        return begin;  
    }  
  
    public void setBegin(Integer begin) {  
        this.begin = begin;  
    }  
  
    public Integer getEnd() {  
        return end;  
    }  
  
    public void setEnd(Integer end) {  
        this.end = end;  
    }  
  
    public Integer getSum() {  
        return sum;  
    }  
  
    public void setSum(Integer sum) {  
        this.sum = sum;  
    }  
}
```
测试数据：
```
13,-3,-25,29,-3,-16,-23,18,20,-7,12,-5,-22,15,-4,7
```
测试结果：
![](assets/Pasted%20image%2020240119230038.png)
### 矩阵乘法
给定两个矩阵A=$（a_{ij})$,B=$(b_{ij})$,二者皆为 n \* n方阵，定义C=A \* B,则$$c_{ij}=\sum_{k=1}^na_{ij} * b_{ij}$$
暴力破解的话肯定是需要O($n^3$)时间
通过Strassen可以让n * n矩阵乘法只花费O($n^{lg_7}$)——即O($n^{2.81}$)
![](assets/317e53188f69fa8498f219f1125167c.jpg)
同样是将原来的矩阵分解，在普通的分治算法中是求8个矩阵乘法，而Strassen只求7次，实现了O($n^{lg_7}$)时间消耗
1. 将A、B和C分解为 $n/2 * n/2$的矩阵，采用下标计算方法
2. 创建10个$n/2 * n/2$矩阵，递归计算$S_1$……$S_7$
3. 递归计算$P_1$……$P_7$, 每个也都是$n/2 * n/2$的矩阵
4. 通过$P_i$矩阵的不同组合进行加减计算，极端出结果矩阵C的子矩阵

后面的代入法、递归树和主方法解递归树的方法太抽象了😅😅😅，全是公式，看不懂写起来也费劲就不写了😅

## 概率分析和随机算法
输入的不确定性会导致输出的不确定，并且结果可能是天差地别的
### 生日悖论
即求在一个房间最少多少人就会有两个人生日在同一天，约定不考虑闰年，且每个人生日独立
经过推导求出：**$$e^{-k(k-1)/2n}\le \cfrac12$$**
当${-k(k-1)/2n}\le \cfrac12$成立，n默认为365，则必有$$k\ge23$$
### 礼券收集问题
每个随机变量$n_i$服从几何分布，成功的概率是** $\frac{b-i+1}b$ **
假设需要收集k种礼券，则大约需要收集 $blnb$张随机礼券才能成功
## 算法思维收集
### 余数问题
问：今天是星期日，100天后是星期几？
100%7=2——所以是星期二
$10^8$天呢？
100000000%7=2——还是星期二
$10^100$天呢？
如果直接计算，数值太大根本计算不了，需要找到规律：
#### 思路
![](assets/Pasted%20image%2020240121233201.png)
规律是显而易见的：每增加6个0星期数就相同，因此将0的个数处于6即可
$100\%6=4$
因此答案是星期四——是和上方的规律对应起来得出的星期四，即四个零所以是星期四
### 乘方问题
$1234567^{987654321}$的个位数是什么呢?
#### 思路
我们无需对整个数字乘方，那样就算只是3次方也很大，我们只需要关注最后一位——7的阶乘就可以
| 指数 | 个位数字 |
| ---- |:--------:|
| 0    |    1     |
| 1    |    7     |
| 2    |    9     |
| 3    |    3     |
| 4    |    1     |
| 5    |    7     |
| 6    |    9     |
| 7    |    3     |
可以看出周期为4，则只需要用987654321%4=1，则答案为7

## 奇偶问题
奇偶校验——计网中有
题目：
```txt
在一个小王国中，有8个村子(A~H)。如图所示，各个村之间有道路相连(黑
点表示村子，线表示道路)。而你要寻找流浪在这个王国的你唯一的恋人。
你的恋人住在这8个村子中的某一个里。她每过1个月便顺着道路去另一个村子，每个月都一定会换村子，然而选择哪个村子是随机的，预测不了。例如，如果恋人这个月住在G村，那么下个月就住在“C、F、H中的某个村子”。
目前你手头上掌握的确凿信息只有:1 年前(12个月前)，恋人住在 G村。请求出这个月恋人住在 A 村的概率。
```
![](assets/Pasted%20image%2020240121235847.png)
看似很难，其实也只是奇偶规律问题：
奇数次移动在A、C、F、H村之一
偶数次移动在B、D、E、G村之一
所以题目答案是0%
## 图论问题
![](assets/Pasted%20image%2020240122000627.png)
经典题目：一笔画走完七座桥
我们假设如此“完成了一笔画”，那么可能出现以下两种情况。
1. 起点和终点相同的情况
	一笔画成，也就意味着“边走边减”的结果是所有的顶点的度数变为(数。为什么呢?因为如果还存在度数不为0的顶点，那么也就存在没经过的边。
	经过“边走边减”之后，经过的顶点的性不变。由此我们可知度数变为0(数的经过点，在原图中本来就是偶点。此外，起点度数减1，终点度数也减 1，变为0。然而，起点和终点是相同的，因此相同顶点的度数减了 2，所以该顶点也变成了偶点。
	结论，在“起点和终点相同”的一笔画中，图中的顶点都是偶点
2. 起点和终点不同的情况
	和(1)相同的思路，经过的顶点全部是点。只有起点和终点是奇点。据此，在“起点和终点不同”的一笔画中，图中只有2个奇点。
至此，我们可知以下命题是成立的
**如果“可以一笔画成”=>“所有的顶点都是偶点，或者有2个奇点”**
图中四个顶点都是奇点，所以不能够一笔画






# 排序算法
![](assets/Pasted%20image%2020240121133230.png)
![](assets/Pasted%20image%2020240121133433.png)
## 冒泡排序
### 算法步骤
1. 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
2. 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数。
3. 针对所有的元素重复以上的步骤，除了最后一个。
4. 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。
### 动图演示
![](assets/bubbleSort.gif)
**当输入的数据已经是正序时最快，当输入的数据是反序时最慢**
### 代码实现
```java
package study.january.twentyone;  
  
/**  
 * 冒泡排序  
 * 普通冒泡：  
 *      数据量：80000  
 *      用时：8739  
 * 进阶冒泡：  
 *      数据量：80000  
 *      用时：8522  
 */  
public class BubbleSort {  
    public static void main(String[] args) {  
        int[] arr = new int[80000];  
        for (int i = 0; i < arr.length; i++) {  
            arr[i] = (int) (Math.random() * 80000);  
        }  
  
        long start = System.currentTimeMillis();  
        bubbleSortPlus(arr);  
  
        long end = System.currentTimeMillis();  
  
        System.out.println("最终结果：");  
        for (int item : arr) {  
            System.out.printf(item + " ");  
        }  
        System.out.println();  
        System.out.println("用时：" + (end - start));  
    }  
  
  
    private static void bubbleSort(int[] arr) {  
        for (int i = 0; i < arr.length - 1; i++) {  
            for (int j = 0; j < arr.length-1 - i; j++) {  
                if (arr[j] > arr[j + 1]) {  
                    int temp = arr[j];  
                    arr[j] = arr[j+1];  
                    arr[j+1] = temp;  
                }  
            }  
        }  
    }  
  
    private static void bubbleSortPlus(int[] arr) {  
        boolean flag = false;  
        for (int i = 0; i < arr.length - 1; i++) {  
            for (int j = 0; j < arr.length-1 - i; j++) {  
                if (arr[j] > arr[j + 1]) {  
                    flag = true;  
                    int temp = arr[j];  
                    arr[j] = arr[j+1];  
                    arr[j+1] = temp;  
                }  
            }  
            if (!flag){  
                break;  
            }else {  
                flag = false;  
            }  
        }  
    }  
}
```

## 选择排序
### 算法步骤
1. 首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置。
2. 再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。
3. 重复第二步，直到所有元素均排序完毕。
### 动图演示
![](assets/selectionSort.gif)
### 代码实现
```java
package study.january.twentyone;  
  
/**  
 * 选择排序  
 *      数据量：80000  
 *      用时：2458  
 */  
public class SelectSort {  
    public static void main(String[] args) {  
        int[] arr = new int[80000];  
        for (int i = 0; i < arr.length; i++) {  
            arr[i] = (int) (Math.random() * 80000);  
        }  
  
        long start = System.currentTimeMillis();  
        selectSort(arr);  
  
        long end = System.currentTimeMillis();  
  
        System.out.println("最终结果：");  
        for (int item : arr) {  
            System.out.printf(item + " ");  
        }  
        System.out.println();  
        System.out.println("用时：" + (end - start));  
    }  
  
    private static void selectSort(int[] arr) {  
        for (int i = 0; i < arr.length-1; i++) {  
            int min = i;  
            for (int j = i+1; j < arr.length; j++) {  
                if (arr[j]<arr[min]){  
                    min = j;  
                }  
            }  
            if (i!=min){  
                int temp = arr[i];  
                arr[i] = arr[min];  
                arr[min] = temp;  
            }  
        }  
    }  
}
```
## 插入排序
### 算法步骤
1. 将第一待排序序列第一个元素看做一个有序序列，把第二个元素到最后一个元素当成是未排序序列。
2. 从头到尾依次扫描未排序序列，将扫描到的每个元素插入有序序列的适当位置。（如果待插入的元素与有序序列中的某个元素相等，则将待插入元素插入到相等元素的后面。）

伪代码
```java
for i = 2 to A.length
	key = A[i]
	j = i - 1;
	while j > 0 and A[j] >key
		A[j+1] = A[j];
		j = j - 1;
	A[j+1] = key;
```

### 动图演示
![](assets/insertionSort.gif)
### 代码实现
```java
public class InsertSort {  
    public static void main(String[] args) {  
        int[] arr = new int[]{1, 2, 4, 6, 2, 8, 4, 9};  
        insertSort(arr);  
        for (int j : arr) {  
            System.out.print(j + " ");  
        }  
        System.out.println();  
    }  
  
    private static void insertSort(int[] arr) {  
        for (int i = 2; i < arr.length; i++) {  
            int j = i - 1;  
            int key = arr[i];  
            while (j > 0 && arr[j] > key) {  
                int temp = arr[j + 1];  
                arr[j + 1] = arr[j];  
                arr[j] = temp;  
                j--;  
            }  
            arr[j + 1] = key;  
        }  
    }  
}
```
## 堆排序