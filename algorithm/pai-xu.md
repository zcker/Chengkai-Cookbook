---
cover: >-
  https://images.unsplash.com/photo-1616628188538-0bc97a2f741b?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwzfHxzb3J0fGVufDB8fHx8MTY0ODA0NjY4NA&ixlib=rb-1.2.1&q=85
coverY: 0
---

# 💵 排序

### 1. 快速排序

1. 分解:数组A\[p…r]被`划分为两个子数组`A\[p. .q-1]和A\[q+1,r],使得A\[q]为大小居中的数，左侧A\[p. .q-1]中的每个元素都小于等于它，而右侧A\[q+1,r]中的每个元素都大于等于它。其中计算下标q也是划分过程的一部分。
2. 解决:通过递归调用快速排序，对子数组A\[p. .q-1]和A\[q+1,r]进行排序
3.  合并:因为子数组都是原址排序的，所以不需要合并，数组A\[p. .r]已经有序

    那么,划分就是问题的关键

#### 1.1. 单向扫描法

![](<../.gitbook/assets/image (15) (1).png>)

![](<../.gitbook/assets/image (20).png>)

### 2. 树

![](<../.gitbook/assets/image (14).png>)

例子中是树的遍历：

![](<../.gitbook/assets/image (21) (1).png>)

```java
package 排序;

/**
 * @author zcker
 * @version 1.0.0
 * @ClassName Example2.java
 * @Description 树的前中后序遍历
 * @createTime 2020年09月22日 12:33:00
 */

/**
 * 树结构：常用数组形式表示
 * 树这种结构只是逻辑结构，不是真正的存储方式
 * 前序：根->左->右
 * 中序：左->根->右
 * 后序：左->右->根
 * 这个序主语是根，方便记忆
 */

public class Example2 {
    public static void main(String[] args) {
        int[] arr = {78, 56, 34, 43, 4, 1, 15, 2, 23};
        preorderTraversal(arr, 0);
        System.out.println();
        inOrderTraversal(arr, 0);
        System.out.println();
        postOrderTraversal(arr, 0);
    }

    /**
     * 先序遍历
     *
     * @param arr   数组
     * @param index 下标
     */
    public static void preorderTraversal(int[] arr, int index) {
        //序号超过数组长度
        if (index >= arr.length) {
            return;
        }
        System.out.print(arr[index] + "|");
        preorderTraversal(arr, 2 * index + 1);
        preorderTraversal(arr, 2 * index + 2);
    }

    /**
     * 中序遍历
     *
     * @param arr   数组
     * @param index 下标
     */
    public static void inOrderTraversal(int[] arr, int index) {
        //序号超过数组长度
        if (index >= arr.length) {
            return;
        }
        inOrderTraversal(arr, 2 * index + 1);
        System.out.print(arr[index] + "|");
        inOrderTraversal(arr, 2 * index + 2);
    }

    /**
     * 后序遍历
     *
     * @param arr   数组
     * @param index 下标
     */
    public static void postOrderTraversal(int[] arr, int index) {
        //序号超过数组长度
        if (index >= arr.length) {
            return;
        }
        postOrderTraversal(arr, 2 * index + 1);
        postOrderTraversal(arr, 2 * index + 2);
        System.out.print(arr[index] + "|");
    }
}
```

![](<../.gitbook/assets/image (24).png>)
