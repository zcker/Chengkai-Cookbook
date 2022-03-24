---
cover: >-
  https://images.unsplash.com/photo-1550921464-9f7a27f99edc?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw4fHxzd2l0Y2h8ZW58MHx8fHwxNjQ4MTA3OTU1&ixlib=rb-1.2.1&q=85
coverY: 0
---

# 🛼 第一章 交换机 Switching Hub

## 明确问题

![](<../../../.gitbook/assets/image (22).png>)

可见，交换机的目的是交换报文，而工作的方式如下：

* 收到某网段（设为A）MAC地址为X的计算机发给MAC地址为Y的计算机的数据包。交换机从而记下了MAC地址X在网段A。这称为学习（learning）。
* 交换机还不知道MAC地址Y在哪个网段上，于是向除了A以外的所有网段转发该数据包。这称为泛洪（flooding）。
* MAC地址Y的计算机收到该数据包，向MAC地址X发出确认包。交换机收到该包后，从而记录下MAC地址Y所在的网段。
* 交换机向MAC地址X转发确认包。这称为转发（forwarding）。
* 交换机收到一个数据包，查表后发现该数据包的来源地址与目的地址属于同一网段。交换机将不处理该数据包。这称为过滤（filtering）。
* 交换机内部的MAC地址-网段查询表的每条记录采用时间戳记录最后一次访问的时间。早于某个阈值（用户可配置）的记录被清除。这称为老化（aging）。

## 设计解决方案

这里我们计划设计一个简单的交换机，支持以下功能：

* 對於接收到的封包進行修改或針對指定的連接埠進行轉送。
* 對於接收到的封包進行轉送到 Controller 的動作（ Packet-In ）。
* 對於接收到來自 Controller 的封包轉送到指定的連接埠（ Packet-Out ）。

首先，利用 Packet-In 的功能來達到 MAC 位址的學習。 Controller 使用 Packet-In 接收來自交換器的封包之後進行分析，得到連接埠相關資料以及所連接的 host 之 MAC 位址。

在學習之後，對所收到的封包進行轉送。將封包的目的位址，在已經學習的 host 資料中進行檢索，根據檢索的結果會進行下列處理。

* 如果是已經存在記錄中的 host：使用 Packet-Out 功能轉送至先前所對應的連接埠
* 如果是尚未存在記錄中的 host：使用 Packet-Out 功能來達到 Flooding

## 确定技术方案

使用ryu+mininet-wifi开发

## 实现控制器

### 交换机类的定义及初始化

