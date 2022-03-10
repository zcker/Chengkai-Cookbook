---
cover: >-
  https://images.unsplash.com/photo-1644256193432-2c16dc37bc9a?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE2NDYwNTIyMzY&ixlib=rb-1.2.1&q=85
coverY: 0
---

# 😁 Mininet入门与实战

### Mininet入门与实战

### 一、Mininet简介

![](../../.gitbook/assets/0)

![](<../../.gitbook/assets/1 (4)>)

![](<../../.gitbook/assets/2 (3) (1)>)

### 二、Mininet安装部署

![](<../../.gitbook/assets/3 (3) (1)>)

![](<../../.gitbook/assets/4 (2) (1)>)

![](<../../.gitbook/assets/5 (2)>)

![](../../.gitbook/assets/6)

![](<../../.gitbook/assets/7 (1)>)

### 三、Mininet命令详解

### 网络构建启动参数

![](<../../.gitbook/assets/8 (2)>)

![](<../../.gitbook/assets/9 (2)>)

![](../../.gitbook/assets/10)

![](<../../.gitbook/assets/11 (3)>)

![](<../../.gitbook/assets/12 (2)>)

![](<../../.gitbook/assets/13 (1)>)

### 内部交互命令

![](<../../.gitbook/assets/14 (3)>)

![](../../.gitbook/assets/15)

使用py时添加节点后记得在交换机上添加对应的端口

switch.attach()

还有将主机设置IP

![](../../.gitbook/assets/16)

帮助文档

py dir('设备名') # 命令概览\
py help('设备名') # 详细命令

### 四、Mininet可视化

![](<../../.gitbook/assets/17 (3)>)

![](../../.gitbook/assets/18)

`controller设置port和remote controller`

`switch的DPIP为16位,switch设置ovsk,NetFlow和sFlow设置开启查看信息`

`全体配置设置start CLI=True 和ovsk模式`

`保存后使用py 文件名的方式运行`

### 五、玩转流表

### 案例背景

![](<../../.gitbook/assets/19 (2)>)

![](../../.gitbook/assets/20)

### 执行操作

![](<../../.gitbook/assets/image (3) (1).png>)

![](<../../.gitbook/assets/image (13).png>)

![](<../../.gitbook/assets/image (12).png>)

![](<../../.gitbook/assets/image (9) (1).png>)

![](<../../.gitbook/assets/image (4).png>)

![](../../.gitbook/assets/image.png)

![](<../../.gitbook/assets/image (1).png>)

![](<../../.gitbook/assets/image (14).png>)

### 注意事项

如果手动添加流表，需要注意添加优先级，否则是默认在交换机中的先生效。

### 六、模拟多数据中心流量带宽 <a href="#header-n117" id="header-n117"></a>

### 案例背景 <a href="#header-n118" id="header-n118"></a>

![](<../../.gitbook/assets/0 (3)>)

![](<../../.gitbook/assets/1 (3)>)

![](<../../.gitbook/assets/2 (1)>)

### 案例要求 <a href="#header-n122" id="header-n122"></a>

![](<../../.gitbook/assets/3 (2)>)

![](<../../.gitbook/assets/4 (2)>)

![](<../../.gitbook/assets/5 (1)>)

![](<../../.gitbook/assets/6 (2)>)

![](../../.gitbook/assets/7)

![](<../../.gitbook/assets/8 (3)>)

![](../../.gitbook/assets/9)

![](<../../.gitbook/assets/10 (1)>)

### 添加拓扑 <a href="#header-n138" id="header-n138"></a>

![](../../.gitbook/assets/11)

![](../../.gitbook/assets/12)

![](../../.gitbook/assets/13)

![](../../.gitbook/assets/14)

![](<../../.gitbook/assets/15 (2)>)

![](<../../.gitbook/assets/16 (3)>)

### 案例执行 <a href="#header-n141" id="header-n141"></a>

![](<../../.gitbook/assets/17 (2)>)

一秒记录一次数据

## 参考资料

* 未来网络学院  [https://www.bilibili.com/video/BV1ft4y1a7ip](https://www.bilibili.com/video/BV1ft4y1a7ip)
