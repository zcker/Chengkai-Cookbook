---
cover: >-
  https://images.unsplash.com/photo-1597484661643-2f5fef640dd1?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw3fHx0b29sfGVufDB8fHx8MTY0NjAwOTM4NQ&ixlib=rb-1.2.1&q=85
coverY: 0
---

# 📈 typora画图

## 流程图

![](<../.gitbook/assets/image (9) (1) (1) (1) (1) (1).png>)

```mermaid
graph TD;
start(A)-->nextB{{b}};
nextB{{b}}---nextC>C];
nextC>C]---|C->D|nextD{D};
nextD{D}-- D->E ---nextE((E));
nextB{{b}}-->|起飞|F;
F-.->nextG(G);
nextG-.肉蛋冲击.->next{H};
nextB{{b}} ==> B["特殊字符:#9829;"];
B["特殊字符:#9829;"]-.->支持fontawesome(fa:fa-camera-retro);
```

* TB - 从上到下
* BT - 从下到上
* RL - 从右到左
* LR - 从左到右
* TD - 与TB相同

## 子图

![](<../.gitbook/assets/image (10) (1) (1) (1) (1) (1).png>)

```mermaid
graph TB
    c1-->a2
    subgraph one
    a1-->a2
    end
    subgraph two
    b1-->b2
    end
    subgraph three
    c1-->c2
    end
```

## 时序图

![](<../.gitbook/assets/image (8) (2) (1).png>)

```mermaid
sequenceDiagram 
Alice->>John:Hello John
John->>Alice:Hello
```

## 状态图

![](<../.gitbook/assets/image (6) (1) (1) (1) (1).png>)

```mermaid
stateDiagram
	[*]-->s1
	s1-->[*]
```
