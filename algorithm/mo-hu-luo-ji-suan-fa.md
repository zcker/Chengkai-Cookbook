---
cover: ../.gitbook/assets/image (12).png
coverY: 0
---

# 🚜 模糊逻辑算法

## 基本概念

模糊逻辑系统就是模糊的逻辑系统。模糊是相较于传统的逻辑系统而言的。在传统逻辑中，非黑即白，就是0或者1，不是0，就是1，不存在其他的情况。而模糊逻辑，可以存在0到1之间的其他情况即灰色地带。

需要指出的是，模糊逻辑系统有两项基本原则。一是模糊逻辑的含义是让计算机以一种接近人类行为的方式去解决问题。二是模糊逻辑的本质是一切都和程度有关。

### **经典的布尔逻辑**

布尔数值就是0和1，是和非，也是计算机逻辑的基础，其基本运算就是**"与、或、非"。**

### **隶属度(Degree of Membership)**

0到1之间数值的大小，代表该事件属于0或者1的程度的大小即隶属度。我们可以编写隶属函数来判断隶属度，或者说概率来表示这个状态。隶属度表示程度，它的值越大，表明这个状态的概率越高，反之则表明这个状态的概率越低 。

比如对于10mm降雨，隶属于小雨的隶属度为0.5, 中雨的隶属度为0.4，大雨的隶属度为0.1；对于100mm降雨，小雨的隶属度为0, 中雨的隶属度为0.3，大雨的隶属度为0.7。

将逻辑的输入数值(降雨量)转化成各个集合(小雨，中雨，大雨)的隶属度的过程就叫做Fuzzification。 也是模糊逻辑的第一步。

## 基本流程

![](<../.gitbook/assets/image (10).png>)

{% hint style="info" %}
* 模糊化：根据隶属度函数从具体的输入得到对模糊集隶属度的过程；
* 推理方法：从模糊规则和输入对相关模糊集的隶属度得到模糊结论的方法；
* 去模糊化：将模糊结论转化为具体的、精确的输出的过程。

**输入（采集明确数据）→ 模糊化（根据隶属度函数，如分段函数、分布函数，再从具体的输入，得到 隶属度模糊集合（特征数据）） → 模糊规则库 + 推理方法 → 模糊结论 → 去模糊化**
{% endhint %}

### **模糊化（Fuzzyfication）**

模糊系统的输入是明确的数字。在模糊化的过程中，我们要将这些明确的值，根据隶属函数，对应到模糊集中的隶属度。

传统逻辑例如布尔逻辑的隶属函数如下图所示：

![](<../.gitbook/assets/image (3).png>)

当输入数据小于 x0 时，则结果取false，而当输入数据大于 x0 时，结果取true，false和true之间没有中间值，即传统逻辑中的非黑即白，非假即真。

这在现实生活中做判断的时候就很不合理了，举个例子，假如 x0=170 斤，则任何人超过170斤就是超重，低于170斤的就是不超重，即使某人169斤，仍然被视为不超重，这样对于日常生活中的判断过于极端，并不实用。但是如果模糊隶属函数，则可以让我们实现从 false 到 true 或者说从不超重到超重之间的逐渐转移。

对于上述传统布尔逻辑归属函数，我们可以做一个简单拓展将其转换为模糊逻辑的隶属函数

![](<../.gitbook/assets/image (5).png>)

0到1之间的逐渐变化。小于x0时，则结果取false即隶属度为0，而大于 x1 的值，结果取true即隶属度为1。介于 x0 到 x1 之间的值，隶属度则呈线性变化。我们可以很轻松的给出这个隶属度的方程式：

![](<../.gitbook/assets/image (7).png>)

回到体重例子。让该函数代表超重隶属。令x0 等175，而x1等于195。如果他重185斤，则根据计算，他的隶属度为0.5，也就是有点超重。现在就有了模糊的概念。

一般而言，我们关注的是输入变量根据隶属函数，对应到模糊集合中的程度。例如，我们想知道某人的体重是超重、太瘦、或者理想。就此而言，我们要设立模糊集合，这样我们才能根据隶属函数，观察它的隶属度是落在哪个区间的，即代表的是什么程度。我们假设有太瘦、理想和超重三个体重的模糊集合。

![](<../.gitbook/assets/image (6) (1).png>)

有了这样的集合，我们就能在这三个集合里，计算每个输入值的隶属度。如果一个人太瘦的隶属度为0，理想的隶属度为0.75，超重的隶属度为0.15。那我们可以比较这来自三个模糊集合的隶属度，从而进行推论，此人的体重是理想的，而且具有75%（0.75）的隶属度。

![](<../.gitbook/assets/image (11).png>)

上图是考试分数和学生成绩的隶属度关系。 比如考0分，Poor的隶属度为1，Avg. 和Good为0。 考试为32.5分(红黑交叉点)，Poor的隶属度为0.5，Avg.为0.5， Good为0。 在任意一点都能找到其所对应的集合的隶属度。

常见的隶属度函数包括：高斯隶属函数、广义钟型隶属函数、三角形隶属函数、梯形隶属函数、Z型隶属函数等。

![](<../.gitbook/assets/image (23) (1).png>)

### 模糊规则

将明确的输入都模糊化后，接着要做的是构建一组规则，以某种逻辑方式结合输入数据，生成某些输出结果。和传统逻辑运算一样，我们引入模糊逻辑运算符<mark style="color:orange;">交集（AND）、联集（OR）以及补集（NOT）</mark>。对于模糊变量来说，OR逻辑运算符被定义为操作数中的最大值，AND逻辑运算符被定义成操作数中的最小值，NOT运算符则是1减去操作数的隶属度。在传统逻辑的布尔系统中，每条规则会逐一运算，直到有条规则为真为止，然后就开始运行此结论。在模糊逻辑的系统中，所有的规则都会同时进行运算，每条规则都会运行（因为每条规则都是部分真），然而，运行的强度或程度则各不相同。每条规则的前提的逻辑运算结果，会产生该规则结论的强度。换句话说，每条规则的强度代表的是输出的模糊集合中的隶属程度。

经过模糊逻辑运算后，我们可以得到<mark style="color:purple;">多维真值表</mark>，根据每个维度的模糊结果来选择行动或者打分。我们以一个简单例子为例，是关于空战的例子，根据我机指向对手角度的好坏、距离对手相对距离大小，给出“方位/距离态势”评分，得分越高态势越有利

![](<../.gitbook/assets/image (21) (1) (1).png>)

左边是两个模糊集合，分别是距离和指向角度，右边是模糊规则，表中的数字表示方位/距离态势评分，得分越高态势越有利。比如，如果经过输入数据模糊化，得到相对距离很近以及指向角度很正的模糊结果，我们就会查表得3分，即认为态势非常有利。

#### 与或非例

举个例子，A = Poor: 0.5（Poor的隶属为0.5 ）B = Good：0.2， 那么C= A与B是多少呢？

其实有好多计算C的方法，这里介绍一个最简单的“最小隶属法(MIN implication)”，于是 C=A与B中最小那个（0.2）。于是C= Good: 0.2.

那D = A或B怎么计算呢？还是介绍一个最简单的“最大隶属法(MAX implication)”，即 C=A或B中最大的那个(0.5)。 于是D= Poor: 0.5

"非"的运算就更简单了，直接如下图所示取相反的折线就完了。

![](<../.gitbook/assets/image (25) (1).png>)

#### 决策规则例

你要编写一个程序来给自己分配食物，食物的多少由你饥饿的程度（Hunger）和食物的味道(Taste)共同决定的，两者分别有少、中、多(S, M, L)三个集合

如下图，表格为规则，隶属度函数在表格下面，Taste 和 Hunger 的取值为了方便起见，分别设为5.625和5。因此模糊化之后 Taste(S)=0：Taste(M)=0.75，Taste(L)=0.25, Hunger(S)=0, Hunger(M)=5, Hunger(L)=0。规则就是比如：Taste(M)与Hunger(M)输出M，Hunger(M)与Taste(L)输出L。

![](<../.gitbook/assets/image (24) (1).png>)

可见除了规则表中标记成红色和绿色的部分，其他的输出都为0。

表中：

* 红色M是Taste(M)与Hunger(M)的输出结果可以用最小法则Min(1, 0.75)=0.75
* 绿色L是Taste(L)与Hunger(M)的输出结果可以用最小法则Min(1, 0)=0。

同样的如果Taste和Hunger分别取5.625和4.375，如图会有下面结果

![](<../.gitbook/assets/image (14) (1).png>)

表中：

* 红色S是Taste(M)与Hunger(S)的输出结果可以用最小法则Min(0.75, 0.25)=0.25
* 绿色M是Taste(M)与Hunger(M)的输出结果可以用最小法则Min(0.75, 0.75)=0.75
* 黄色M是Taste(L)与Hunger(S)的输出结果可以用最小法则Min(0.25, 0.25)=0.25
* 紫色L是Taste(L)与Hunger(M)的输出结果可以用最小法则Min(0.25, 0.75)=0.25
* 其他输出为0。

### 解模糊

在前面我们说过，每条规则都会得到某个输出模糊集合中的隶属程度。在推理得到的模糊集合中取一个最能代表这个模糊集合的单值的过程称作解模糊。常用的方法有**最大隶属度法、最大平均法、面积均分法和重心法**。

#### **加权平均判决法**

![](<../.gitbook/assets/image (20) (1).png>)\
其中FS就是上一步输出的Fire Strength, OW (Output Wight)是权重。权重通常是取每个集合的中间值，分配食物的例子OW(S)=2.5, OW(M)=5, OW(L)=7.5。

如果Taste = 5.625, Hunger = 4.375\
FS1(S)=0.25, FS2(M)=0.75, FS3(M)=0.25, FS4(L)=0.25.\
用上面的公式可以求得\
Output = (0.25_2.5+0.75_5+0.25_5+0.25_7.5)/(0.25+0.75+0.25+0.25)=5

这个输出有什么用呢？你可以通过这个输出决定是否分配食物，设定一个阈值，比如为4，如果小于等于这个阈值就不分配食物，大于这个阈值就分配食物。这里输出是5，那么就可以给自己分配食物了。

## 实例应用

实际应用中模糊逻辑系统就涉及到模糊控制器的设计，包括<mark style="color:red;">模糊化过程（模糊化方法、模糊集划分、模糊函数优化）、知识库（控制规则类型、一致性、完备性、交互作用性）、推理决策（模糊推理与解模糊方法）和优化计算（调整与优化模糊控制器）</mark>这五个主要内容。

我们以一个实例来说明怎样设计一个模糊控制器，实例是倒立摆控制系统。

![](<../.gitbook/assets/image (22) (1) (1).png>)

被控制的对象包含倒置单摆、小车、导轨和电机，控制场景：长度为2L，质量为m的单摆用铰链固定在质量为M的小车上，通过铰链摆可以在一个平面上自由摆动，小车受电机操控，对电机通过电流的方式来在水平上产生控制力，使小车在有限长的导轨上运动，保持倒立摆不倒

首先明确输入变量：角度(-90º\~90º)，角速度（实数域）；输出变量：推力（实数域）。然后我们设定隶属度函数，将输入输出模糊化

![](<../.gitbook/assets/image (2).png>)

建立模糊规则：

![](<../.gitbook/assets/image (8).png>)

Change-in-error指的是角速度偏差，error指的是角度偏差，表中的数值指的是推力u的模糊值。举例说明，如果我们得到change-in-error为-2，error为-2，查表可得u为2，这意味着当前反向偏差非常大，我们需要很大的正向推力来平衡倒立摆。然后我们利用一种解模糊方法来将输出模糊值转化为电机可以接受的信号，例中采用重心法解模糊。至此，整个模糊控制器设计完毕。

![](<../.gitbook/assets/image (4).png>)

![](<../.gitbook/assets/image (15) (1).png>)

## 实现方法

<details>

<summary>JavaScript</summary>

实现参考：[https://github.com/marcolanaro/JS-Fuzzy](https://github.com/marcolanaro/JS-Fuzzy)

### 使用方法

```javascript
# 实例化对象
var ai = new FuzzyLogic();
# 输出数据
ai.getResult(object);
```

### &#x20;配置说明

```javascript
# 数据配置
var obj = {
	crisp_input: [0.3, 0.8, 0.7],
	variables_input: [
	{
		name: "Meal time",
		setsName: ["Breakfast time", "Lunch time", "Dinner time"],
		sets: [
		# 矩形
		[0,0,0.2,0.3],
		[0.2,0.3,0.4,0.5],
		[0.4,0.5,0.6,0.7]
		]
	},
	{
		name: "Weather type",
		setsName: ["Freezing", "Cold", "Warm", "Hot", "Melting"],
		sets: [
		[0,0.1,0.2,0.4],
		[0.25,0.40,0.5,0.7],
		[0.55,0.6,0.65,0.75],
		[0.60,0.7,0.8,1],
		[0.7,0.8,0.9,1]
		]
	},
	{
		name: "Season",
		setsName: ["Autumn", "Winter", "Spring", "Summer"],
		sets: [
		[0,0,0.3,0.4],
		[0.2,0.3,0.4,0.5],
		[0.3,0.4,0.5,0.6],
		[0.5, 0.6, 0.7,1]
		]
	}
	],
	variable_output: {
		name: "Desirability",
		setsName: ["Undesirable", "Desirable", "Very Desirable"],
		sets: [
		[0,0,25,50],
		[25,50,50,75],
		[50,75,100,100]
		]
	},
	inferences: [
	[0,2,0],
	[0,1,2],
	[2,1,0]
	]
};dds
```

### 源码简析

```javascript
var FuzzyLogic = (function() {

    "use strict";

    var C = function() {};
    
    C.prototype = {
        //输入（采集明确数据）→ 模糊化（根据隶属度函数，如分段函数、分布函数，
        //再从具体的输入，得到 隶属度模糊集合（特征数据）） → 模糊规则库 + 推理方法 → 
        //模糊结论 → 去模糊化
        getResult: function(options) {
            var variables = this.construct(options.variables_input),
                fuzzy_input = this.fuzzification(options.crisp_input, variables),
                output_combination = this.output_combination(fuzzy_input, options.inferences, options.variable_output),
                fuzzy_output = this.takeMaxOfArraySet(output_combination),
                crisp_output = this.defuzzification(fuzzy_output, this.construct_variable(options.variable_output.sets));

            return crisp_output;
        },

        construct: function(variables) {
            var ob = [],
                i;

            for (i = variables.length - 1; i >= 0; i -= 1) {
                ob[i] = this.construct_variable(variables[i].sets);
            }

            return ob;
        },

        construct_variable: function(f) {
            var obv = [],
                i;

            for (i = f.length - 1; i >= 0; i -= 1) {
                obv[i] = {
                    a: f[i],
                    firstPoint: (f[i][0] === f[i][1]) ? 1 : 0,
                    lastPoint: (f[i][2] === f[i][3]) ? 1 : 0,
                    mUp: (1 / (f[i][1] - f[i][0])),
                    mDown: (1 / (f[i][3] - f[i][2]))
                };
            }
            return obv;
        },

        fuzzification: function(input, variables) {
            var value = [],
                i;

            for (i = variables.length - 1; i >= 0; i -= 1) {
                value[i] = this.fuzzification_variable(input[i], variables[i]);
            }

            return value;
        },

        fuzzification_variable: function(x, sets) {
            var valori = [],
                i;

            for (i = sets.length - 1; i >= 0; i -= 1) {
                valori[i] = this.fuzzification_function(x, sets[i]);
            }

            return valori;
        },

        fuzzification_function: function(x, set) {
            var f = 0;

            if (x <= set.a[0]) {
                f = set.firstPoint;
            } else {
                if (x < set.a[1]) {
                    f = set.mUp * (x - set.a[0]);
                } else {
                    if (x <= set.a[2]) {
                        f = 1;
                    } else {
                        if (x < set.a[3]) {
                            f = 1 - set.mDown * (x - set.a[2]);
                        } else {
                            if (x >= set.a[3]) {
                                f = set.lastPoint;
                            }
                        }
                    }
                }
            }

            return f;
        },

        output_combination: function(valori, inferences, variable_output) {
            var a = [],
                i,
                j;

            for (i = variable_output.sets.length - 1; i >= 0; i -= 1) {
                a[i] = [];
            }

            for (i = inferences.length - 1; i >= 0; i -= 1) {
                for (j = inferences[i].length - 1; j >= 0; j -= 1) {
                    if (inferences[i][j] >= 0) {
                        a[inferences[i][j]].push(valori[i][j]);
                    }
                }
            }

            return a;
        },

        defuzzification: function(outputSet, variable) {
            var num = 0,
                den = 0,
                i,
                v,
                point,
                h,
                b,
                a,
                a1,
                a2,
                area,
                y_baricentro,
                x_baricentro,
                amezzi,
                bmezzi,
                mmezzi;

            for (i = outputSet.length - 1; i >= 0; i -= 1) {
                v = variable[i];
                point = v.a;
                h = outputSet[i];
                b = point[3] - point[0];
                a1 = point[0];
                if (point[0] !== point[1]) {
                    a1 += h / v.mUp;
                }
                a2 = point[3];
                if (point[2] !== point[3]) {
                    a2 -= h / v.mDown;
                }
                area = 0;
                if (point[0] !== a1) {
                    area += (a1 - point[0]) * outputSet[i] / 2;
                }
                if (a1 !== a2) {
                    area += (a2 - a1) * outputSet[i];
                }
                if (a2 !== point[3]) {
                    area += (point[3] - a2) * outputSet[i] / 2;
                }
                a = a2 - a1;
                y_baricentro = (h / 3) * (b + 2 * a) / (a + b);
                amezzi = a1 + (a2 - a1) / 2;
                bmezzi = point[0] + (point[3] - point[0]) / 2;
                mmezzi = 0;
                if (amezzi - bmezzi !== 0) {
                    mmezzi = h / (amezzi - bmezzi);
                }
                x_baricentro = bmezzi;
                if (mmezzi !== 0) {
                    x_baricentro += y_baricentro / mmezzi;
                }
                num += area * x_baricentro;
                den += area;
            }

            return (den === 0 ? 0 : (num / den));
        },

        takeMaxOfArraySet: function(set) {
            var output = [],
                i;

            for (i = set.length - 1; i >= 0; i -= 1) {
                output[i] = this.takeMaxOfArray(set[i]);
            }

            return output;
        },

        takeMaxOfArray: function(arr) {
            var max = arr[0],
                j;

            for (j = 1; j < arr.length; j += 1) {
                max = arr[j] > max ? arr[j] : max;
            }

            return max;
        }

    };

    return C;

}());
```

</details>

## 参考文献

* [https://zhuanlan.zhihu.com/p/112891766](https://zhuanlan.zhihu.com/p/112891766)
* 作者：Hongtao洪滔\
  链接：https://www.jianshu.com/p/b316acff0f02\
  来源：简书\
  著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
