---
cover: >-
  https://images.unsplash.com/photo-1644851722825-448a109e259f?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE2NDY4ODY3NjI&ixlib=rb-1.2.1&q=85
coverY: 0
---

# 🎙 mininet-wifi examples简述

## 4address

IEEE 802.11 (WLAN)帧的报头有四个地址字段。为了在无线分配系统(WDS)链路上透明地传输以太网数据包，IEEE 802.3(以太网)帧被封装在IEEE 802.11 (WLAN)帧中。在这种状况下，使用全部四个地址字段 ,包括：

•以太网帧的发送方

•以太网帧的接收器&#x20;

•WLAN帧的发送器

&#x20;•WLAN帧的接收器&#x20;

以太网帧的发送方和接收方仅从传输的以太网帧复制。其他的字段容许接收方识别帧，并容许它向(WLAN)发送方确认帧的接收。然而，一般只须要这四个字段中的三个，所以大多数驱动程序不知道如何处理使用全部四个地址字段的帧。

换句话说:WDS最重要的组成部分是支持4个地址头。

```python
#!/usr/bin/python

"""This example shows how to enable 4-address
Warning: It works only when network manager is stopped"""

import sys

from mininet.node import Controller
from mininet.log import setLogLevel, info
from mn_wifi.link import wmediumd, _4address
from mn_wifi.cli import CLI
from mn_wifi.net import Mininet_wifi
from mn_wifi.wmediumdConnector import interference


def topology(args):
    "Create a network."
    net = Mininet_wifi(controller=Controller, link=wmediumd,
                       wmediumd_mode=interference, config4addr=True)

    info("*** Creating nodes\n")
    ap1 = net.addAccessPoint('ap1', ssid="ap1-ssid", mode="g",
                             channel="1", position='30,30,0')
    ap2 = net.addAccessPoint('ap2', ssid="ap2-ssid", mode="g",
                             channel="1", position='40,60,0')
    ap3 = net.addAccessPoint('ap3', ssid="ap3-ssid", mode="g",
                             channel="1", position='50,30,0')
    sta1 = net.addStation('sta1', ip="192.168.0.1/24", position='31,32,0')
    sta2 = net.addStation('sta2', ip="192.168.0.2/24", position='32,34,0')
    sta3 = net.addStation('sta3', ip="192.168.0.3/24", position='41,62,0')
    sta4 = net.addStation('sta4', ip="192.168.0.4/24", position='42,64,0')
    sta5 = net.addStation('sta5', ip="192.168.0.5/24", position='51,32,0')
    sta6 = net.addStation('sta6', ip="192.168.0.6/24", position='52,34,0')
    c0 = net.addController('c0')

    info("*** Configuring Propagation Model\n")
    net.setPropagationModel(model="logDistance", exp=4.5)

    info("*** Configuring wifi nodes\n")
    net.configureWifiNodes()

    info("*** Adding Links\n")
    net.addLink(ap1, ap2, cls=_4address)  # ap1=ap, ap2=client 
    net.addLink(ap1, ap3, cls=_4address)  # ap1=ap, ap3=client
    net.addLink(sta1, ap1)
    net.addLink(sta2, ap1)
    net.addLink(sta3, ap2)
    net.addLink(sta4, ap2)
    net.addLink(sta5, ap3)
    net.addLink(sta6, ap3)

    if '-p' not in args:
        net.plotGraph(max_x=100, max_y=100)

    info("*** Starting network\n")
    net.build()
    c0.start()
    ap1.start([c0])
    ap2.start([c0])
    ap3.start([c0])

    info("*** Running CLI\n")
    CLI(net)

    info("*** Stopping network\n")
    net.stop()


if __name__ == '__main__':
    setLogLevel('info')
    topology(sys.argv)

```

{% hint style="info" %}
![](<../../.gitbook/assets/image (8) (1).png>)

开启服务：net = Mininet\_wifi(controller=Controller, link=wmediumd, wmediumd\_mode=interference, **config4addr=True**)

从属关系，ap2是ap1的从机：net.addLink(ap1, ap2, cls=\_4address) # ap1=ap, ap2=client

ap见用以太网，sta与ap使用wlan连接

![](<../../.gitbook/assets/image (5) (1) (1) (1) (1).png>)
{% endhint %}

```shell
调试数据
mininet-wifi> sta1 iw dev sta1-wlan0 scan
BSS 02:00:00:00:06:00(on sta1-wlan0) -- associated
	last seen: 2835.511s [boottime]
	TSF: 1646875935515374 usec (19061d, 01:32:15)
	freq: 2412
	beacon interval: 100 TUs
	capability: ESS ShortSlotTime (0x0401)
	signal: -31.00 dBm
	last seen: 0 ms ago
	Information elements from Probe Response frame:
	SSID: ap1-ssid
	Supported rates: 1.0* 2.0* 5.5* 11.0* 6.0 9.0 12.0 18.0 
	DS Parameter set: channel 1
	ERP: Barker_Preamble_Mode
	Extended supported rates: 24.0 36.0 48.0 54.0 
	Supported operating classes:
		 * current operating class: 81
	Extended capabilities:
		 * Extended Channel Switching
		 * Multiple BSSID
		 * SSID List
		 * Operating Mode Notification
BSS 02:00:00:00:07:00(on sta1-wlan0)
	last seen: 2835.513s [boottime]
	TSF: 1646875935515442 usec (19061d, 01:32:15)
	freq: 2412
	beacon interval: 100 TUs
	capability: ESS ShortSlotTime (0x0401)
	signal: -82.00 dBm
	last seen: 0 ms ago
	Information elements from Probe Response frame:
	SSID: ap2-ssid
	Supported rates: 1.0* 2.0* 5.5* 11.0* 6.0 9.0 12.0 18.0 
	DS Parameter set: channel 1
	ERP: Barker_Preamble_Mode
	Extended supported rates: 24.0 36.0 48.0 54.0 
	Supported operating classes:
		 * current operating class: 81
	Extended capabilities:
		 * Extended Channel Switching
		 * Multiple BSSID
		 * SSID List
		 * Operating Mode Notification
BSS 02:00:00:00:08:00(on sta1-wlan0)
	last seen: 2835.513s [boottime]
	TSF: 1646875935515442 usec (19061d, 01:32:15)
	freq: 2412
	beacon interval: 100 TUs
	capability: ESS ShortSlotTime (0x0401)
	signal: -73.00 dBm
	last seen: 0 ms ago
	Information elements from Probe Response frame:
	SSID: ap3-ssid
	Supported rates: 1.0* 2.0* 5.5* 11.0* 6.0 9.0 12.0 18.0 
	DS Parameter set: channel 1
	ERP: Barker_Preamble_Mode
	Extended supported rates: 24.0 36.0 48.0 54.0 
	Supported operating classes:
		 * current operating class: 81
	Extended capabilities:
		 * Extended Channel Switching
		 * Multiple BSSID
		 * SSID List
		 * Operating Mode Notification
		
mininet-wifi> Connected to 02:00:00:00:06:00 (on sta1-wlan0)
	SSID: ap1-ssid
	freq: 2412
	RX: 696029 bytes (15759 packets)
	TX: 2278 bytes (93 packets)
	signal: -31 dBm
	tx bitrate: 1.0 MBit/s

	bss flags:	short-slot-time
	dtim period:	2
	beacon int:	100
```

## active\_scan

当sta须要链接特定频率（MHz），可使用active\_scan功能。

&#x20;如果网络使用的通道子集已知，则可使用此选项优化扫描，使其不发生在网络不使用的通道上。

示例:scan\_freq=“2412 2437 2462”， freq\_list=“2412 2437 2462”

```python
#!/usr/bin/python

'This example shows how to work with active scan'

from mininet.log import setLogLevel, info
from mn_wifi.node import UserAP
from mn_wifi.cli import CLI
from mn_wifi.net import Mininet_wifi


def topology():
    "Create a network."
    net = Mininet_wifi(accessPoint=UserAP)

    info("*** Creating nodes\n")
    net.addStation('sta1', passwd='123456789a,123456789a', encrypt='wpa2,wpa2',
                   wlans=2, active_scan=1, scan_freq='2412,2437',
                   freq_list='2412,2437', position='5,10,0')
    net.addStation('sta2', passwd='123456789a', encrypt='wpa2',
                   active_scan=1, scan_freq='2437', freq_list='2437',
                   position='45,10,0')
    ap1 = net.addAccessPoint('ap1', ssid="ssid-1", mode="g", channel="1",
                             passwd='123456789a', encrypt='wpa2',
                             position='10,10,0')
    ap2 = net.addAccessPoint('ap2', ssid="ssid-1", mode="g", channel="6",
                             passwd='123456789a', encrypt='wpa2',
                             position='40,10,0')
    c0 = net.addController('c0')

    info("*** Configuring wifi nodes\n")
    net.configureWifiNodes()

    net.plotGraph(max_x=120, max_y=120)

    info("*** Starting network\n")
    net.build()
    c0.start()
    ap1.start([c0])
    ap2.start([c0])

    info("*** Running CLI\n")
    CLI(net)

    info("*** Stopping network\n")
    net.stop()


if __name__ == '__main__':
    setLogLevel('info')
    topology()

```

{% hint style="info" %}
![](<../../.gitbook/assets/image (7) (1) (1) (1) (1).png>)

* 模拟添加了wap2加密的协议
* active\_scan标志位设置为1

![](<../../.gitbook/assets/image (2) (1) (1) (1).png>)
{% endhint %}

```shell
mininet-wifi> sta1 iw dev sta1-wlan0 scan
BSS 02:00:00:00:04:00(on sta1-wlan0)
	last seen: 4265.703s [boottime]
	TSF: 1646877365709459 usec (19061d, 01:56:05)
	freq: 2437
	beacon interval: 100 TUs
	capability: ESS Privacy ShortSlotTime (0x0411)
	signal: -36.00 dBm
	last seen: 0 ms ago
	Information elements from Probe Response frame:
	SSID: ssid-1
	Supported rates: 1.0* 2.0* 5.5* 11.0* 6.0 9.0 12.0 18.0 
	DS Parameter set: channel 6
	ERP: Barker_Preamble_Mode
	Extended supported rates: 24.0 36.0 48.0 54.0 
	RSN:	 * Version: 1
		 * Group cipher: CCMP
		 * Pairwise ciphers: CCMP
		 * Authentication suites: PSK
		 * Capabilities: 1-PTKSA-RC 1-GTKSA-RC (0x0000)
	Supported operating classes:
		 * current operating class: 81
	Extended capabilities:
		 * Extended Channel Switching
		 * Multiple BSSID
		 * SSID List
		 * Operating Mode Notification
```

## adhoc

adhoc显示了如何使用adhoc模式设置实验，在这种模式下，工做站无需经过访问点而彼此链接。

```python
#!/usr/bin/python

"""
This example shows on how to enable the adhoc mode
Alternatively, you can use the manet routing protocol of your choice
"""

import sys

from mininet.log import setLogLevel, info
from mn_wifi.link import wmediumd, adhoc
from mn_wifi.cli import CLI
from mn_wifi.net import Mininet_wifi
from mn_wifi.wmediumdConnector import interference


def topology(args):
    "Create a network."
    net = Mininet_wifi(link=wmediumd, wmediumd_mode=interference)

    info("*** Creating nodes\n")
    kwargs = dict()
    if '-a' in args:
        kwargs['range'] = 100

    sta1 = net.addStation('sta1', ip6='fe80::1',
                          position='10,10,0', **kwargs)
    sta2 = net.addStation('sta2', ip6='fe80::2',
                          position='50,10,0', **kwargs)
    sta3 = net.addStation('sta3', ip6='fe80::3',
                          position='90,10,0', **kwargs)

    net.setPropagationModel(model="logDistance", exp=4)

    info("*** Configuring wifi nodes\n")
    net.configureWifiNodes()

    info("*** Creating links\n")
    # MANET routing protocols supported by proto:
    # babel, batman_adv, batmand and olsr
    # WARNING: we may need to stop Network Manager if you want
    # to work with babel
    protocols = ['babel', 'batman_adv', 'batmand', 'olsrd', 'olsrd2']
    kwargs = dict()
    for proto in args:
        if proto in protocols:
            kwargs['proto'] = proto

    net.addLink(sta1, cls=adhoc, intf='sta1-wlan0',
                ssid='adhocNet', mode='g', channel=5,
                ht_cap='HT40+', **kwargs)
    net.addLink(sta2, cls=adhoc, intf='sta2-wlan0',
                ssid='adhocNet', mode='g', channel=5,
                **kwargs)
    net.addLink(sta3, cls=adhoc, intf='sta3-wlan0',
                ssid='adhocNet', mode='g', channel=5,
                ht_cap='HT40+', **kwargs)

    info("*** Starting network\n")
    net.build()

    info("\n*** Addressing...\n")
    if 'proto' not in kwargs:
        sta1.setIP6('2001::1/64', intf="sta1-wlan0")
        sta2.setIP6('2001::2/64', intf="sta2-wlan0")
        sta3.setIP6('2001::3/64', intf="sta3-wlan0")

    info("*** Running CLI\n")
    CLI(net)

    info("*** Stopping network\n")
    net.stop()


if __name__ == '__main__':
    setLogLevel('info')
    topology(sys.argv)

```

{% hint style="info" %}
![](<../../.gitbook/assets/image (1) (1).png>)

如何传入参数：

![](<../../.gitbook/assets/image (14) (1) (1) (1) (1) (1).png>)
{% endhint %}

```shell
mininet-wifi> links
sta1-wlan0<->wifiAdhoc (OK wifiAdhoc) 
sta2-wlan0<->wifiAdhoc (OK wifiAdhoc) 
sta3-wlan0<->wifiAdhoc (OK wifiAdhoc) 
sta1-wlan0<->wifi (use iw/iwconfig to check connectivity) 
sta2-wlan0<->wifi (use iw/iwconfig to check connectivity) 
sta3-wlan0<->wifi (use iw/iwconfig to check connectivity)

mininet-wifi> sta1 iw dev sta1-wlan0 scan
 BSS 02:ca:ff:ee:ba:01(on sta1-wlan0) -- joined
	last seen: 8905.269s [boottime]
	TSF: 1646891440720863 usec (19061d, 05:50:40)
	freq: 2432
	beacon interval: 100 TUs
	capability: IBSS (0x0002)
	signal: -79.00 dBm
	last seen: 0 ms ago
	Information elements from Probe Response frame:
	SSID: adhocNet
	Supported rates: 1.0* 2.0* 5.5* 11.0* 6.0 9.0 12.0 18.0 
	DS Parameter set: channel 5
	IBSS ATIM window: 0 TUs
	Extended supported rates: 24.0 36.0 48.0 54.0 
	WMM: information: 01 00 
	
mininet-wifi> sta1 iw dev sta1-wlan0 link
Joined IBSS 02:ca:ff:ee:ba:01 (on sta1-wlan0)
	SSID: adhocNet
	freq: 2432
```
