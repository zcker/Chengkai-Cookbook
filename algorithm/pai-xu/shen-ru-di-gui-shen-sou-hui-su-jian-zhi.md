# 深入递归,深搜,回溯,剪枝

## 深入递归,深搜,回溯,剪枝

### 1. 数值型"逐步生成结果”解法

自下而上的递归(递推,数学归纳,动态规划)

* 解决简单情况下的问题
* 推广到稍复杂情况下的问题.
* `如果递推次数很明确,用迭代`
* 如果有封闭形式,可以直接求解

### 2. 三步问题

解本题，首先找规律：

![](<../../.gitbook/assets/0 (2)>)

```java
package 深入递归;

/**
 * @author zcker
 * @version 1.0.0
 * @ClassName Example1.java
 * @Description 三步问题
 * @createTime 2020年09月15日 18:22:00
 */

/**
 * 三步问题。有个小孩正在上楼梯，楼梯有n阶台阶，小孩一次可以上1阶、2阶或3阶。
 * 实现一种方法，计算小孩有多少种上楼梯的方式。结果可能很大，你需要对结果模1000000007。
 * <p>
 * 示例1:
 * <p>
 * 输入：n = 3
 * 输出：4
 * 说明: 有四种走法
 * 示例2:
 * <p>
 * 输入：n = 5
 * 输出：13
 * 提示:
 * <p>
 * n范围在[1, 1000000]之间
 * <p>
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/three-steps-problem-lcci
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 */

public class Example1 {
    static final int mod = 1000000007;

    public static void main(String[] args) {
        System.out.println(waysToStep1(5));
    }

    /**
     * 递归方式计算，很方便就可以列出式子，但是时间复杂度为O(3的N次方)
     * 可以想象开辟的占空间与时间复杂度很长
     *
     * @param n
     * @return num
     */
    public static int waysToStep1(int n) {
        if (n == 1) {
            return 1;
        } else if (n == 2) {
            return 2;
        } else if (n == 3) {
            return 4;
        } else {
            return waysToStep1(n - 1) % mod + waysToStep1(n - 2) % mod + waysToStep1(n - 3) % mod;
        }
    }

    /**
     * 相比于递归占用大量空间，那就自下而上思考，替代递归
     * 迭代：不断用变量的旧值递推新值的过程
     * 这里设置3个变量，不断用新的值去迭代，这样的话，时间复杂度仅为O(n)
     * @param n
     * @return
     */
    public static int waysToStep2(int n) {
        //前面一样
        if (n == 1) {
            return 1;
        } else if (n == 2) {
            return 2;
        } else if (n == 3) {
            return 4;
        } else {
            //从第一级台阶开始迭代
            int step1 = 1, step2 = 2, step3 = 4;
            //原理同上
            for (int i = 0; i < n - 3; i++) {
                int temp = step1;
                //想象走上了一级台阶，那么从最后一级开始,每个台阶自然+1
                step1 = step2;
                step2 = step3;
                //新的step3=旧的step1+step2+step3
                step3 = ((step1 + step2) % mod + temp) % mod;
            }
            return step3;
        }
    }
}
```

### 3. 机器人走方格

#### 3.1递归写法

![](<../../.gitbook/assets/1 (4)>)

```java
package 深入递归;

/**
 * @author zcker
 * @version 1.0.0
 * @ClassName Example2.java
 * @Description 机器人走方格
 * @createTime 2020年09月16日 10:43:00
 */

/**
 * 链接：https://www.nowcoder.com/questionTerminal/e8bb8e68434e42acbcdff0341f2a32c5
 * 来源：牛客网
 *
 * 有一个mxn的网格，一个机器人只能走格点且只能向右或向下走，要从左上角走到右下角。请设计一个算法，计算机器人有多少种走法。
 *
 * 给定两个正整数int m,int n，请返回机器人的走法数目。保证m＋n小于等于12。
 *
 * 测试样例：
 * 2,2
 * 返回：2
 */

public class Example2 {
    public static void main(String[] args) {
        System.out.println(countWays1(4,4));
    }

    /**
     * 递归解法，思想全在图上面，不多写了，倦了
     * @param m
     * @param n
     * @return
     */
    public static int countWays1(int m, int n) {
        // write code here
        if(m==1||n==1){
            return 1;
        }
        return countWays1(m-1,n)+countWays1(m,n-1);
    }
}
```

#### 3.2 迭代写法

![](<../../.gitbook/assets/2 (3)>)

```java
package 深入递归;

/**
 * @author zcker
 * @version 1.0.0
 * @ClassName Example2.java
 * @Description 机器人走方格
 * @createTime 2020年09月16日 10:43:00
 */

/**
 * 链接：https://www.nowcoder.com/questionTerminal/e8bb8e68434e42acbcdff0341f2a32c5
 * 来源：牛客网
 *
 * 有一个mxn的网格，一个机器人只能走格点且只能向右或向下走，要从左上角走到右下角。请设计一个算法，计算机器人有多少种走法。
 *
 * 给定两个正整数int m,int n，请返回机器人的走法数目。保证m＋n小于等于12。
 *
 * 测试样例：
 * 2,2
 * 返回：2
 */

public class Example2 {
    public static void main(String[] args) {
        System.out.println(countWays2(4,4));
    }

    /**
     * 这里使用迭代时，就将路径的变化看为上图所画的表，像斐波那契数列一样的
     * @param m
     * @param n
     * @return
     */
    public static int countWays2(int m, int n) {
        // write code here
        int[][] matrix=new int[m+1][n+1];
        //将第一行的值赋为1
        for (int i = 1; i <= m; i++) {
            matrix[i][1]=1;
        }
        //将第一列的值赋为1
        for (int i = 1; i <=n; i++) {
            matrix[1][i]=1;
        }
        for (int i = 2; i <=m; i++) {
            for (int j = 2; j <=n; j++) {
                matrix[i][j]=matrix[i-1][j]+matrix[i][j-1];
            }
        }
        return matrix[m][n];
    }
}
```
