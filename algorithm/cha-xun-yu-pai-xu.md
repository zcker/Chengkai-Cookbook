---
cover: >-
  https://images.unsplash.com/photo-1647343469788-090e87c71dae?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE2NDc5MDk0Mzk&ixlib=rb-1.2.1&q=85
coverY: 0
---

# ⛺ 查询与排序

## 1.求阶乘,打印字符

```java
package 查找与排序;

/**
 * @author zcker
 * @version 1.0.0
 * @ClassName Example1.java
 * @Description 求阶乘,打印字符
 * @createTime 2020年09月13日 09:23:00
 */

public class Example1 {
    public static void main(String[] args) {
        System.out.println(factorial(3));
        System.out.println("------------------------");
        printNum(3,6);
    }

    /**
     * factorial(int num):求n的阶乘-->f(n-1)求n-1的阶乘
     * 找重复: n*(n-1)的阶乘,求n-1的阶乘是原问题的重复（规模更小)—子问题
     * 找变化∶变化的量应该作为参数
     * 找边界:出口
     * @param num
     */
    static int factorial(int num){
        if (num==1) {
            return 1;
        }
        return num*factorial(num-1);
    }

    /**
     * 找重复:打印字符
     * 找变化:变化的量应该作为参数
     * @param i
     * @param j
     */
    static void printNum(int i,int j){
        if (i>j) {
            return;
        }
        System.out.println(i);
        printNum(i+1,j);
    }
}
```

## 2.数组求和与翻转字符串

```java
package 查找与排序;

/**
 * @author zcker
 * @version 1.0.0
 * @ClassName Example2.java
 * @Description 数组求和，翻转字符串
 * @createTime 2020年09月13日 16:01:00
 */

public class Example2 {
    public static void main(String[] args) {
        int[] arr={1,2,3,4,5};
        System.out.println(arrSum(arr,0));
        System.out.println(reserveStr("abcd",3));
    }

    /**
     * 数组求和
     * 找重复：从第一个开始，每次增加一个，将一个值加上去
     * 找变换：index都在向后推
     * 找边界:加到第一个
     *
     * @param arr
     * @param index
     * @return
     */
    public static int arrSum(int[] arr, int index) {
        if (index == arr.length - 1) {
            return arr[index];
        }
        return arr[index] + arrSum(arr, index + 1);
    }

    /**
     * 翻转字符串
     * 找重复：从最后一个开始翻，翻到第一个
     * 找变换：end在向前推进
     * 找边界:end=0
     * @param str
     * @param end
     * @return
     */
    public static String reserveStr(String str,int end){
        if (end==0){
            //只返回str.charAt(0)的话是char类型，需要加个“”转换下类型
            return ""+str.charAt(0);
        }
        return str.charAt(end)+reserveStr(str,end-1);
    }
}
```

## 3.斐波那契数列

```java
package 查找与排序;

/**
 * @author zcker
 * @version 1.0.0
 * @ClassName Example3.java
 * @Description 斐波那契数列
 * @createTime 2020年09月13日 16:30:00
 */

/*
    Question:
    大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0，第1项是1）
 */

public class Example3 {
    public static void main(String[] args) {
        System.out.println(fibonacci(5));
    }

    /**
     * 斐波那契数列
     * 形式为：0、1、1、2、3、5、8、13、21、34
     * 找重复：f(n)+f(n+1)=f(n+2)
     * 找变换：n变化
     * 找边界:到头
     * @param n
     * @return fibonacci
     */
    public static int fibonacci(int n) {
        if (n == 0) {
            return 0;
        } else if (n == 1) {
            return 1;
        }
        return fibonacci(n - 1) + fibonacci(n - 2);
    }
}
```

注意，斐波那契数列的形式可以抽象为二叉树，且为深度优先遍历。

## 4.辗转相除求最大公因数

```java
package 查找与排序;

/**
 * @author zcker
 * @version 1.0.0
 * @ClassName Example4.java
 * @Description 辗转相除求最大公因数
 * @createTime 2020年09月16日 19:17:00
 */

/**
 * 算法原理：若a除以b的余数为r , 则有 (a , b) = ( b ,r )  （(a,b)表示a和b的最大公约数）
 *
 * 假如需要求 1997 和 615 两个正整数的最大公约数,用欧几里德算法，是这样进行的：
 * 1997 / 615 = 3 (余 152)
 * 615 / 152 = 4(余7)
 * 152 / 7 = 21(余5)
 * 7 / 5 = 1 (余2)
 * 5 / 2 = 2 (余1)
 * 2 / 1 = 2 (余0)
 * 至此，最大公约数为1
 *
 */

public class Example4 {
    public static void main(String[] args) {
        System.out.println(commonDivisor(1997,615));
    }

    public static int commonDivisor(int x,int y){
        if (x%y==0){
            return y;
        }
        return commonDivisor(y,x%y);
    }
}
```

这里强烈安利一个帖子，讲的很好：`https://zhuanlan.zhihu.com/p/31824895`

## 5.递归形式插入排序



![img](<../.gitbook/assets/0 (4)>)

```java
package 查找与排序;

/**
 * @author zcker
 * @version 1.0.0
 * @ClassName Example5.java
 * @Description 递归形式插入排序
 * @createTime 2020年09月16日 21:47:00
 */

import java.util.Arrays;

/**
 * 将插入排序转化为递归形式去书写
 * 什么是插入排序，看动图就OK了
 */

public class Example5 {
    public static void main(String[] args) {
        int[] arr = {4, 2, 1, 3};
        insertSorting(arr, 3);
        System.out.println(Arrays.toString(arr));
    }

    /**
     * 分割：将0~index-1先排序，然后递归，将最后一个值在新开的区排序
     * 注意，函数的执行顺序是反复循环，循环到了index=0时，退出开始运行，去循环
     * 并不是从后往前运行的
     *
     * @param arr
     * @param index
     */
    public static void insertSorting(int arr[], int index) {
        //如果序号到0了，就递归到底了
        if (index == 0) {
            return;
        }
        //对前面index-1排序
        insertSorting(arr, index - 1);
        //将index插入前面并排序
        //将当前排序的值暂存
        int temp = arr[index];
        //将下标暂存，注意，因为数组从0开始数，所以是index-1
        int subscript = index - 1;
        //当前拿到的值小于暂存的比对的值，自然交换顺序
        while (subscript > -1 && temp < arr[subscript]) {
            arr[subscript + 1] = arr[subscript];
            subscript--;
        }
        arr[subscript + 1] = temp;
    }
}
```

## 6.小结

`找重复:`

1、找到—种划分方法

2、找到递推公式或者**等价转换**

都是父问题转化为求解子问题

`找变化的量:`

变化的量通常要作为参数

`找到出口:`

根据参数变化的趋势，对边界进行控制，适时终止递归

## 7.汉诺塔



![](<../.gitbook/assets/1 (4)>)

```java
package 查找与排序;

/**
 * @author zcker
 * @version 1.0.0
 * @ClassName Example6.java
 * @Description 汉诺塔
 * @createTime 2020年09月17日 12:37:00
 */

/**
 * 汉诺塔（港台：河内塔）是根据一个传说形成的数学问题：
 * <p>
 * 有三根杆子A，B，C。A杆上有 N 个 (N>1) 穿孔圆盘，盘的尺寸由下到上依次变小。要求按下列规则将所有圆盘移至 C 杆：
 * <p>
 * 每次只能移动一个圆盘；
 * 大盘不能叠在小盘上面。
 *
 * 解法的基本思想是递归。假设有 A、B、C 三个塔，A 塔有 {\displaystyle N}N 块盘，目标是把这些盘全部移到 C 塔。那么先把 A 塔顶部的 {\displaystyle N-1}{\displaystyle N-1} 块盘移动到 B 塔，再把 A 塔剩下的大盘移到 C，最后把 B 塔的 {\displaystyle N-1}{\displaystyle N-1} 块盘移到 C。
 * 如此递归地使用下去, 就可以求解。
 */

public class Example6 {
    public static void main(String[] args) {
        hanoiTower(3, "A", "B", "C");
    }

    /**
     * 1-N从A移动到B，C作为辅助
     * 等价于：
     * 1、1~N-1从A移动到C，B为辅助
     * 2、把N从A移动到B
     * 3、1-N-1从C移动到B，A为辅助
     *
     * @param num    叠的汉诺塔数
     * @param from   来自哪个地方
     * @param to     搬到哪去
     * @param helper 替手，暂存盘子的
     */
    public static void hanoiTower(int num, String from, String to, String helper) {
        if (num == 1) {
            System.out.println("把盘子" + num + "从" + from + "移到" + to);
            return;
        } else {
            //这里先将盘子移到辅助空间上
            hanoiTower(num - 1, from, helper, to);
            //顺利将盘子移开
            System.out.println("把盘子" + num + "从" + from + "移到" + to);
            //将盘子再从辅助空间搬回去
            hanoiTower(num - 1, helper, to, from);
        }
    }
}
 /**
     * 结果：
     * 把盘子1从A移到B
     * 把盘子2从A移到C
     * 把盘子1从B移到C
     * 把盘子3从A移到B
     * 把盘子1从C移到A
     * 把盘子2从C移到B
     * 把盘子1从A移到B
     */
```

## 8.二分搜索

```java
package code;

public class Q5 {
	public static void main(String[] args) {
		//二分搜索需要数组有序
		int[] arr = { 1, 3, 4, 6, 7, 9, 14, 18, 25, 68, 120, 126, 228 };
		System.out.println(binarySearch(arr, 0, 13, 68));
	}

	/**
	 * 二分排序，递归搜索数值
	 * @param arr
	 * @param begin
	 * @param end
	 * @param key
	 * @return 找到返回值，没有返回-1
	 */
	static int binarySearch(int arr[], int begin, int end, int key) {
		// 搜索到两个哨兵重合肯定结束了
		if (begin > end) {
			return -1;
		}
		// 中值下标为begin和end的中值
		int midSub = begin + ((end - begin) >> 1);
		int midValue = arr[midSub];
		// 中值小于搜索的key,在后面
		if (midValue < key) {
			return binarySearch(arr, midSub + 1, end, key);
		} else if (midValue > key) {
			return binarySearch(arr, begin, midSub - 1, key);
		} else {
			return midSub;
		}
	}
}
```

## 9. 希尔排序

![](<../.gitbook/assets/2 (4)>)

```java
package code;

import java.util.Arrays;

public class Q6 {
	public static void main(String[] args) {
		int[] arr = { 3, 5, 6, 7, 9, 1, 4, 14, 16, 16, 19, 24, 22, 35, 65 };
		shellSort(arr);
		System.out.println(Arrays.toString(arr));
	}

	/**
	 * 希尔排序
	 * @param arr
	 */
	private static void shellSort(int[] arr) {
		//确定gap，并逐渐缩小增量
		for (int interval = arr.length / 2; interval > 0; interval = interval / 2) {
			//从第gap个元素开始，直接插入排序，这里就是让分组有序的过程
			for (int i = interval; i < arr.length; i++) {
				int target = arr[i];
				int j = i - interval;
				// 小于对比
				while (j > -1 && target < arr[j]) {
					arr[j + interval] = arr[j];
					j -= interval;
				}
				arr[j + interval] = target;
			}
		}
	}
}
```

## 10.算法复杂度

![](<../.gitbook/assets/3 (4)>)

![](<../.gitbook/assets/4 (3)>)

![](<../.gitbook/assets/5 (3)>)

> * `n!的弱上界是n²`，因此增长速度非常快，这意味着单位时间内可求解的问题很小，换言之，超慢
> * 2^n这样的指数函数增长非常快，这种算法可以认为超慢
> * O(n²)和O(n³)增长很快，算法很慢，至少优化到nlgn，O(n2)的有冒泡排序，直接插入排序，选择排序
> * nlgn可以认为是及格的算法吧，一般分治法可以缩小层数为lgn，而每层的复杂度一般为O(n)，例如归并排序算法、快速排序算法
> * O (n)叫做线性算法，这种算法比较优秀，或者问题本身比较简单，比如求连续求和最大子数组的线性解
> * O(sqrt(n))当然比O(n)更快，不是没有，但这种很少
> * \*\*lgn就是很优秀的算法了，比如二分查找法，\*\*但是这种算法往往对输入数据的格式是有要求的，二分查找要求输入数据有序
> * 还有一种是常量，无论规模怎么扩大，都花固定时间，这是为数极少的效率最高的算法了，多数是数据很规则

## 11. 常见递归的复杂度

| 递归关系                    | 结果                | 举例             |
| ----------------------- | ----------------- | -------------- |
| T(n)=T(n/2)+O(1)        | T(n)=O(logn)      | 二分查找，辗转相除最大公因数 |
| T(n)=T(n-1)+O(1)        | T(n)=O(n)         | 线性查找           |
| T(n)=2T(n/2)+O(1)       | T(n)=O(n)         |                |
| T(n)=2T(n/2)+O(n)       | T(n)=O(nlogn)     | 归并、快排          |
| T(n)=2T(n/2)+O(nlogn)   | T(n)=O(n(logn)^2) |                |
| T(n)=T(n-1)+O(n)        | T(n)=O(n^2)       | 选择排序、插入排序      |
| T(n)=2T(n-1)+O(1)       | T(n)=O(2^n)       | 汉诺塔            |
| T(n)=T(n-1)+T(n-2)+O(1) | T(n)=O(2^n)       | 递归的斐波那契        |

## 12.排序算法的比较

| 排序方法 | 时间复杂度(平均) | 时间复杂度(最坏) | 时间复杂度(最好) | 空间复杂度     | 稳定性 |
| ---- | --------- | --------- | --------- | --------- | --- |
| 插入排序 | O(n^2)    | O(n^2)    | O(n)      | O(1)      | 稳定  |
| 希尔排序 | O(n^1.3)  | O(n^2)    | O(n)      | O(1)      | 不稳定 |
| 选择排序 | O(n^2)    | O(n^2)    | O(n^2)    | O(1)      | 不稳定 |
| 堆排序  | O(nlog2n) | O(nlog2n) | O(nlog2n) | O(1)      | 不稳定 |
| 冒泡排序 | O(n^2)    | O(n^2)    | O(n)      | O(1)      | 稳定  |
| 快速排序 | O(nlog2n) | O(n^2)    | O(nlog2n) | O(nlog2n) | 不稳定 |
| 归并排序 | O(nlog2n) | O(nlog2n) | O(nlog2n) | O(n)      | 稳定  |
| 计数排序 | O(n+k)    | O(n+k)    | O(n+k)    | O(n+k)    | 稳定  |
| 桶排序  | O(n+k)    | O(n^2)    | O(n)      | O(n+k)    | 稳定  |
| 基数排序 | O(n\*k)   | O(n\*k)   | O(n\*k)   | O(n+k)    | 稳定  |

## 13. 速算估计表

![](<../.gitbook/assets/6 (2)>)
