---
cover: >-
  https://images.unsplash.com/photo-1645653025752-d6604441e9de?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE2NDcxMzQ2MDQ&ixlib=rb-1.2.1&q=85
coverY: 0
---

# 📀 多属性决策模型

## 应用范围

理论和方法在工程设计、经济、管理和军事等诸多领域中有着广泛的应用。比如：投资决策、项目评估、产业部门发展排序和经济效益综合评价等等。

## 多属性决策内容

**原理**：利用已有的决策信息通过移动的方式对一组（有限个）备选方案进行排序或者择优。

**组成内容**：

* 获取决策信息：属性权重和属性值（实数、区间数和语言），先收集数据
* 通过一定的方式对决策信息进行集结并对方案进行排序和择优，再分析

**信息集结**的方法：

* 加权算术平均算子（WAA）
* 加权几何平均算子（WGA）
* 有序加权平均算子（OWA）

### 加权算术平均算子（WAA）

**定义：**

![](<../.gitbook/assets/image (10) (1) (1).png>)

**例子（GPA）：**

![](<../.gitbook/assets/image (15) (1) (1) (1) (1) (1).png>)

得分当然是越高越好，这样的属性值类型也称为**效益型**；但也有些其他的属性值可能是数值越低越好，这类属性类型称为**成本型**，比如某公司的某件产品的生产价格；还有一些其他的，都在下面列出：

### 属性值归一化处理

![](<../.gitbook/assets/image (12) (1) (1).png>)

![](<../.gitbook/assets/image (13) (1) (1) (1) (1) (1).png>)

通过将不同属性类型的属性值经过上述公式规范化为统一量纲的数值就，就可以使用我们前面说的加权算术平均算子了。

### 算法总结

1. 建模构建决策矩阵
2. 属性值归一化
3. 对各个属性构造成对比较矩阵计算属性权重
4. 计算每个公司的WAA

## 建模举例

### 例1

**问题：**某投资银行拟对某市4家企业进行投资，抽取下列5项指标进行评估：产值（万元）、投资成本（万元）、销售额（万元）、国家收益币种、环境污染程度。投资银行考查了上年度4家企业的上述指标情况（其中污染程度由环保部门及时检测并量化），所得评估结果如下表所示。在各项指标中，投资成本、环境污染程度为成本型，其他为效益型。属性权重信息完全未知，试确定最佳投资方案。

**解法：**

　　**1.**先写出归一化处理前的决策矩阵（题目已给出的那个表，博主懒得画两遍就在这里显示吧hh），其中**x**表示企业**，u1**到**u5**表示产值（万元）、投资成本（万元）、销售额（万元）、国家收益币种、环境污染程度这5项指标，则

|    | u1 产值（万元） | u2 投资成本（万元） | u3 销售额（万元） | u4 国家收益币种 | u5 环境污染程度 |
| -- | --------- | ----------- | ---------- | --------- | --------- |
| x1 | 8350      | 5300        | 6135       | 0.82      | 0.17      |
| x2 | 7455      | 4952        | 6527       | 0.65      | 0.13      |
| x3 | 11000     | 8001        | 9008       | 0.59      | 0.15      |
| x4 | 9624      | 5000        | 8892       | 0.74      | 0.28      |

　　**2.**根据我们前面起到的归一化公式，将上述指标值代入相应的公式，就能得到归一化处理后的决策矩阵，如下：

|     |  u1     | u2      | u3      | u4      | u5      |
| --- | ------- | ------- | ------- | ------- | ------- |
| x1  |  0.7455 | 0.9343  | 0.6811  | 1.0000  | 0.7647  |
| x2  |  0.6777 | 1.0000  | 0.7246  | 0.7926  | 1.0000  |
| x3  |  1.0000 |  0.6189 | 1.0000  | 0.7195  | 0.8667  |
| x4  |  0.8749 | 0.9904  | 0.9871  |  0.9024 | 0.4643  |

　　**3.**接下来我们需要应用到上一节用到的知识了，就是构建成对比较矩阵，为什么呢。大家还记得2.1讲的东西吗，其中第一点的决策信息就包括两部分——属性权重和属性值，现在我们已经求出归一化后的属性值了，我们还不知道属性的权重是多少，因此，我们需要通过构建成对比较矩阵，再将成对矩阵输入到我们的MATLAB程序里面，就能求出属性的权重了。为此，我们先写出成对比较矩阵，如下：

![](<../.gitbook/assets/image (9) (1) (1).png>)

将上述矩阵代入层次分析法就能得到u1到u5的权重分别为：\[0.4286, 0.1429, 0.1429, 0.1429, 0.1429].

**4.** 现在我们有了属性权重和归一化的属性值了，就可以利用加权算术平均算子了，分别能算出4个企业的加权算术平均值了，如下：

![](<../.gitbook/assets/image (3) (1).png>)

**企业1-4的得分分别为：0.80 0.79 0.89 0.85**

### **例2**

![](<../.gitbook/assets/image (15) (1) (1) (1) (1).png>)

![](<../.gitbook/assets/image (14) (1) (1) (1) (1).png>)

```python
#!/usr/bin/env python
# -*- coding:utf-8 -*-
# @Time    : 2022-3-13 上午 10:04
# Author:ZhangChengkai
# @File    : 模糊多属性决策.py
from math import sqrt

import numpy as np


def main():
    array = [
        [290, 'A', 'A', 'B', 'B'],
        [288, 'A', 'B', 'A', 'C'],
        [288, 'B', 'A', 'D', 'C'],
        [285, 'A', 'B', 'B', 'B'],
        [283, 'B', 'A', 'B', 'C'],
        [283, 'B', 'D', 'A', 'B'],
        [280, 'A', 'B', 'C', 'B'],
        [280, 'B', 'A', 'A', 'C'],
        [280, 'B', 'B', 'A', 'B'],
        [280, 'D', 'B', 'A', 'C'],
        [278, 'D', 'C', 'B', 'A'],
        [277, 'A', 'B', 'C', 'A'],
        [275, 'B', 'C', 'D', 'A'],
        [275, 'D', 'B', 'A', 'B'],
        [274, 'A', 'B', 'C', 'B'],
        [273, 'B', 'A', 'B', 'C']
    ]
    targetMap = {
        'A': (85, 90, 100), 'B': (75, 80, 85), 'C': (60, 70, 75), 'D': (50, 55, 60)
    }
    F = toTriangle(array, targetMap)
    print("F:", F)
    R = normalization(F, 1)
    print("R:", R)
    weightArray = [
        [0.5, 0.125, 0.125, 0.125, 0.125]
    ]
    W = toTriangle(weightArray, {})
    D = construction(R, W)
    Wp, Wl = ideal(D)
    dp, dl = distance(D, Wp, Wl)
    miu = choose(dp, dl)
    print("miu", miu)


def findMin(F):
    '''
    :param F:
    :return:  返回矩阵每列的最小值
    '''
    columnMin = []
    for column in range(0, F.shape[1]):
        amin = F[0, column][0]
        bmin = F[0, column][1]
        cmin = F[0, column][2]
        for row in range(0, F.shape[0]):
            target = F[row, column]
            if amin > target[0]:
                amin = target[0]
            if bmin > target[1]:
                bmin = target[1]
            if cmin > target[2]:
                cmin = target[2]
        columnMin.append([amin, bmin, cmin])
    return columnMin


def findMax(F):
    '''
    :param F:
    :return:  返回矩阵每列的最大值
    '''
    columnMax = []
    for column in range(0, F.shape[1]):
        amax = F[0, column][0]
        bmax = F[0, column][1]
        cmax = F[0, column][2]
        for row in range(0, F.shape[0]):
            target = F[row, column]
            if amax < target[0]:
                amax = target[0]
            if bmax < target[1]:
                bmax = target[1]
            if cmax < target[2]:
                cmax = target[2]
        columnMax.append([amax, bmax, cmax])
    return columnMax


def toTriangle(array, targetMap):
    '''
    Step1:指标数据的三角形模糊数表达
    :param array:           存储原始数据的二维数组，行为每个比较对象，列为不同指标
    :param targetMap:      一个定性指标到三角模糊数的映射关系
    :return:                返回未归一化的模糊指标矩阵
    '''
    F = []
    for targetList in array:
        row = []
        for target in targetList:
            ## 对于小数，需要截取
            if str(target).split('.')[0].isdigit():
                item = (target, target, target)
            else:
                item = targetMap[target]
            row.append(item)
        F.append(row)
    return F


def normalization(F, type):
    '''
    Step2: 模糊指标矩阵 F 归一化处理
    :param F:    原模糊指标矩阵
    :param type:  0表示成本型，1表示收益型
    :return:      归一化后的模糊指标矩阵,二维列表套元组形式
    '''
    R = []
    for i in range(0, len(F)):
        row = []
        ## 找最小值
        if type == 0:
            columnM = findMin(np.array(F))
        ## 找最大值
        elif type == 1:
            columnM = findMax(np.array(F))
        for j in range(0, len(F[i])):
            x = F[i][j]
            ## 无论最大最小均取出相应位置的最值
            xM = columnM[j]
            aM = xM[0]
            bM = xM[1]
            cM = xM[2]
            if type == 0:
                y = (aM / x[2], bM / x[1], min(cM / x[0], 1.0))
            elif type == 1:
                y = (x[0] / cM, x[1] / bM, min(x[2] / aM, 1.0))
            row.append(y)
        R.append(row)
    return R


def construction(R, W):
    '''
    Step3: 构造模糊决策矩阵
    :param R:  归一化的模糊指标矩阵
    :param W:   三角模糊数的权重向量
    :return:    模糊决策矩阵
    '''
    D = []
    for i in range(0, len(R)):
        for j in range(0, len(R[i])):
            R[i][j] = (R[i][j][0] * W[0][j][0], R[i][j][1] * W[0][j][1], R[i][j][2] * W[0][j][2])
    D = R
    print("D:", D)
    return D


def ideal(D):
    '''
    Step4: 确定模糊正理想 M + 与模糊负理想 M −
    :param D:  模糊决策矩阵
    :return:    模糊理想
    '''
    columnMin = findMin(np.array(D))
    columnMax = findMax(np.array(D))
    Mp = []
    Ml = []
    for i in range(0, len(columnMin)):
        for j in range(0, len(columnMin[i])):
            Mp.append(columnMax[i][j])
            Ml.append(columnMin[i][j])
    print("Mp", Mp)
    print("Ml", Ml)
    return Mp, Ml


def distance(D, Mp, Ml):
    # Step5:确定评价对象i 与 M +、M- 之间的距离 di+、di-
    dp = []
    dl = []
    for i in range(0, len(D)):
        sump = 0
        suml = 0
        for j in range(0, len(D[i])):
            for k in range(0, 3):
                residualp = D[i][j][k] - Mp[j * 3 + k]
                residuall = D[i][j][k] - Ml[j * 3 + k]
                sump = sump + residualp * residualp
                suml = suml + residuall * residuall
        dp.append(sqrt(sump))
        dl.append(sqrt(suml))
    print("dp:", dp)
    print("dl", dl)
    return dp, dl


def choose(dp, dl):
    # Step6:模糊优选决策
    miu = {}
    for i in range(0, len(dp)):
        miu[str(i + 1) + "号对象"] = dl[i] / (dp[i] + dl[i])
    return sorted(miu.items(), key=lambda term: term[1], reverse=True)


if __name__ == '__main__':
    main()

```

## 参考文献

* [https://www.cnblogs.com/Qling/p/9295414.html](https://www.cnblogs.com/Qling/p/9295414.html)
* [https://www.its404.com/article/qq\_25601345/107733598](https://www.its404.com/article/qq\_25601345/107733598)
* ****[https://blog.csdn.net/weixin\_44112790/article/details/88580969](https://blog.csdn.net/weixin\_44112790/article/details/88580969)
