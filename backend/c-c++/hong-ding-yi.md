---
cover: >-
  https://images.unsplash.com/photo-1632538896458-f09b58492db7?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwxfHxjfGVufDB8fHx8MTY0ODIwODQ2Mw&ixlib=rb-1.2.1&q=85
coverY: 0
---

# 🪁 宏定义

## 定义类型

\#define只是简单的字符串替换，typedef则是为一个类型起新名字，因此typedef在有指针的场合要比#define要好

```
 typedef char *pStr1; ​ #define pStr2 char *; ​ pStr1 s1, s2; ​ pStr2 s3, s4; ​ 在上述的变量定义中，s1、s2、s3都被定义为char *，而s4则定义成了char，不是我们所预期的指针变量      而 char* pa, pb; // 这多数不符合我们的意图，它只声明了一个指向字符变量的指针，和一个字符变量； ​
```

## 结构体

用在旧的C代码中，帮助struct简化输入，声明struct新对象时，必须要带上struct，即形式为： struct 结构名对象名。
