---
cover: >-
  https://images.unsplash.com/photo-1550921464-9f7a27f99edc?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw4fHxzd2l0Y2h8ZW58MHx8fHwxNjQ4MTA3OTU1&ixlib=rb-1.2.1&q=85
coverY: 0
---

# 🛼 第一章 交换机 Switching Hub

## 明确问题

![img](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FNP7xx3RQhhjazkAY6UMO%2Fuploads%2F1cso67ZmTGtURwMsvgHw%2Fimage.png?alt=media\&token=05238729-2ca8-47e1-abb7-bdef47e75901)

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
* 對於接收到來自 Controller 的封包轉送到指定的連接埠（ Packet-Out ）。 首先，利用 Packet-In 的功能來達到 MAC 位址的學習。 Controller 使用 Packet-In 接收來自交換器的封包之後進行分析，得到連接埠相關資料以及所連接的 host 之 MAC 位址。 在學習之後，對所收到的封包進行轉送。將封包的目的位址，在已經學習的 host 資料中進行檢索，根據檢索的結果會進行下列處理。
* 如果是已經存在記錄中的 host：使用 Packet-Out 功能轉送至先前所對應的連接埠
* 如果是尚未存在記錄中的 host：使用 Packet-Out 功能來達到 Flooding

## 确定技术方案

使用ryu+mininet-wifi开发

## 实现控制器

### 交换机类的定义及初始化

```python
class SimpleSwitch13(app_manager.RyuApp):	# 繼承了 ryu.base.app_manager.RyuApp
# 类似handshake这类定義好讓 OpenFlow 交換器和 Controller 之間進行通訊時使用的函数RYU很全
    OFP_VERSIONS = [ofproto_v1_3.OFP_VERSION]	# 使用 OpenFlow 1.3

    def __init__(self, *args, **kwargs):
        super(SimpleSwitch13, self).__init__(*args, **kwargs)
        self.mac_to_port = {}	# 定义 MAC 位址表的 mac_to_port
```

### 事件管理 Event handler

```python
"""
       接受到任何一個 OpenFlow 訊息即會產生一個相對應的事件
       事件管理（ Event Handler ）使用修飾器（ Decorator ）标记
       set_ev_cls 則指定事件類別得以接受訊息和交換器狀態作為參數。
       名稱的規則為 ryu.controller.ofp_event.EventOFP + <OpenFlow訊息名稱>

       =========================================== ===============================
       Negotiation phase                           Description
       =========================================== ===============================
       ryu.controller.handler.HANDSHAKE_DISPATCHER Sending and waiting for hello
                                                   message
       ryu.controller.handler.CONFIG_DISPATCHER    Version negotiated and sent
                                                   features-request message
       ryu.controller.handler.MAIN_DISPATCHER      Switch-features message
                                                   received and sent set-config
                                                   message
       ryu.controller.handler.DEAD_DISPATCHER      Disconnect from the peer.  Or
                                                   disconnecting due to some
                                                   unrecoverable errors.
       =========================================== ===============================
       """
    @set_ev_cls(ofp_event.EventOFPSwitchFeatures, CONFIG_DISPATCHER)
```

### 新增 Table-miss Flow Entry

```python
@set_ev_cls(ofp_event.EventOFPSwitchFeatures, CONFIG_DISPATCHER)
    def switch_features_handler(self, ev):
        """
         OpenFlow 交換器的握手協議完成之後，新增 Table-miss Flow Entry
         到 Flow table 中為接收 Packet-In 訊息做準備。
        接收到 Switch features（ Features reply ）訊息後就會新增 Table-miss Flow Entry。
        """
        # ev.msg 是用來儲存對應事件的 OpenFlow 訊息類別實體。
        # 这里是ryu.ofproto.ofproto_v1_3_parser.OFPSwitchFeatures
        # msg.datapath 這個訊息是用來儲存 OpenFlow 交換器的 ryu.controller.controller.Datapath 類別所對應的實體。
        datapath = ev.msg.datapath
        ofproto = datapath.ofproto
        parser = datapath.ofproto_parser
# install table-miss flow entry
        #
        # We specify NO BUFFER to max_len of the output action due to
        # OVS bug. At this moment, if we specify a lesser number, e.g.,
        # 128, OVS will send Packet-In with invalid buffer_id and
        # truncated packet data. In that case, we cannot output packets
        # correctly.  The bug has been fixed in OVS v2.1.0.
        match = parser.OFPMatch()  # 空的 match 將被產生為了 match 所有的封包。match 表示於 OFPMatch 類別中。
        """
        為了轉送到 Controller 連接埠， OUTPUT action 類別（ OFPActionOutput ）的
        實例將會被產生。Controller 會被指定為封包的目的地，OFPCML_NO_BUFFER 會被設定為 max_len 以便接下來的封包傳送。
        """
        actions = [parser.OFPActionOutput(ofproto.OFPP_CONTROLLER,
                                          ofproto.OFPCML_NO_BUFFER)]
        """
        Table-miss Flow Entry 的優先權為 0 即最低的優先權，而且此 Entry 可以 match 所有的封包。
        這個 Entry 的 Instruction 通常指定為 output action ，並且輸出的連接埠將指向 Controller。 因此當封包沒有 match 任何一個普通 Flow Entry 時，則觸發 Packet-In。
        將優先權設定為 0（ 最低優先權 ），然後執行 add_flow() 方法以發送 Flow Mod 訊息
        """
        self.add_flow(datapath, 0, match, actions)
```

### Packet-in处理

```python
"""
    class OFPPacketIn(MsgBase):
    ============= =========================================================
    Attribute     Description
    ============= =========================================================
    buffer_id     ID assigned by datapath   接收封包的內容若是存在 OpenFlow 交換器上時所指定的ID 如果在沒有 buffer 的狀況下，則設定 ryu.ofproto.ofproto_v1_3.OFP_NO_BUFFER
    total_len     Full length of frame  接收封包的資料長度
    reason        Reason packet is being sent.

                  | OFPR_TABLE_MISS
                  | OFPR_APPLY_ACTION
                  | OFPR_INVALID_TTL
                  | OFPR_ACTION_SET
                  | OFPR_GROUP
                  | OFPR_PACKET_OUT
    table_id      ID of the table that was looked up
    cookie        Cookie of the flow entry that was looked up
    match         Instance of ``OFPMatch``  ryu.ofproto.ofproto_v1_3_parser.OFPMatch 類別的實體，用來儲存接收封包的 Meta 資訊。
    data          Ethernet frame    接收封包本身的 binary 資料
    ============= =========================================================
    """

    @set_ev_cls(ofp_event.EventOFPPacketIn, MAIN_DISPATCHER)
    def _packet_in_handler(self, ev):
        # If you hit this you might want to increase
        # the "miss_send_length" of your switch
        if ev.msg.msg_len < ev.msg.total_len:
            self.logger.debug("packet truncated: only %s of %s bytes",
                              ev.msg.msg_len, ev.msg.total_len)
        msg = ev.msg
        datapath = msg.datapath
        ofproto = datapath.ofproto
        parser = datapath.ofproto_parser
```

### 更新 MAC 位址表

```python
# 從 OFPPacketIn 類別的 match 得到接收埠（ in_port ）
        in_port = msg.match['in_port']

        pkt = packet.Packet(msg.data)
        eth = pkt.get_protocols(ethernet.ethernet)[0]

        if eth.ethertype == ether_types.ETH_TYPE_LLDP:
            # ignore lldp packet
            return
        #  目的 MAC 位址和來源 MAC 位址使用 Ryu 的封包函式庫，從接收到封包的 Ethernet header 取得。
        dst = eth.dst
        src = eth.src

        dpid = format(datapath.id, "d").zfill(16)
        self.mac_to_port.setdefault(dpid, {})

        self.logger.info("packet in %s %s %s %s", dpid, src, dst, in_port)

        # learn a mac address to avoid FLOOD next time.
        self.mac_to_port[dpid][src] = in_port
```

### 判断封包的连接地址

```python
"""
        目的 MAC 位址若存在于 MAC 位址表，則判斷該連接埠的號碼為輸出。
        反之若不存在于 MAC 位址表則 OUTPUT action 類別的實體並生成 flooding（ OFPP_FLOOD ）給目的連接埠使用。
        """
        """
        在 OpenFlow 中，有個邏輯連接埠叫做 NORMAL 並在規範中被列為選項
        （ 也就是說可以不進行實作 ）。當被指定的連接埠為 NORMAL 時，
        傳統 L2/L3 的功能將會被啟用來處理封包。 意思是當把所有輸出的埠均設定為 NORMAL 
        時，交換器將會視作一個普通的交換器而存在。跟一般交換器的差別在於我們是使用 
        OpenFlow 來達到這樣的功能。
        """
        if dst in self.mac_to_port[dpid]:
            # 若是找到了目的 MAC 位址，則在交換器的 Flow table 中新增
            out_port = self.mac_to_port[dpid][dst]
        else:
            out_port = ofproto.OFPP_FLOOD
        # Table-miss Flow Entry 包含 match 和 action，並透過 add_flow() 來新增
        actions = [parser.OFPActionOutput(out_port)]
```

### 新增 Flow Entry 的处理

```python
"""
    對於 Flow Entry 來說，設定 match 條件以分辨目標封包、設定 instruction 
    以處理封包以及 Entry 的優先權和有效時間。
    """
    def add_flow(self, datapath, priority, match, actions, buffer_id=None):
        ofproto = datapath.ofproto
        parser = datapath.ofproto_parser
        """
        對於交換器的的實作，Apply Actions 是用來設定那些必須立即執行的 action 所使用。
        最後透過 Flow Mod 訊息將 Flow Entry 新增到 Flow table 中。
        """
        inst = [parser.OFPInstructionActions(ofproto.OFPIT_APPLY_ACTIONS,
                                             actions)]
        if buffer_id:
            """
            Flow Mod 訊息的類別為 OFPFlowMod 。使用 OFPFlowMod 所產生的實體透過 Datapath.send_msg() 
            方法來發送訊息給 OpenFlow 交換器。
            OFPFlowMod 類別的建構子參數相當的多，但是在大多數的情況下都有其預設值，原始碼中括號內的部分即是預設值。
            """
            """
            class OFPFlowMod(MsgBase):
            Modify Flow entry message
            The controller sends this message to modify the flow table.
            ================ ======================================================
            Attribute        Description
            ================ ======================================================
            cookie           Opaque controller-issued identifier    Controller 所設定儲存的資料，在 Entry 的更新或者刪除的時所需要使用的資料都會放在這邊，當做過濾器使用，而且不可以作為封包處理的參數。
            cookie_mask      Mask used to restrict the cookie bits that must match
                             when the command is ``OPFFC_MODIFY*`` or
                             ``OFPFC_DELETE*``
                             Entry 的更新或刪除時，若是該值為非零，則做為指定 Entry 的 cookie 使用。
            table_id         ID of the table to put the flow in 指定 Flow Entry 的 Table ID 
            command          One of the following values.
        
                             | OFPFC_ADD    	Flow Entry 新增
                             | OFPFC_MODIFY     Flow Entry 更新
                             | OFPFC_MODIFY_STRICT      嚴格的 Flow Entry 更新
                             | OFPFC_DELETE     Flow Entry 刪除
                             | OFPFC_DELETE_STRICT      嚴格的 Flow Entry 刪除
            idle_timeout     Idle time before discarding (seconds)  Flow Entry 的有效期限，以秒為單位。Flow Entry 如果未被參照而且超過了指定的時間之後， 該 Flow Entry 將會被刪除。如果 Flow Entry 有被參照，則超過時間之後會重新歸零計算。在 Flow Entry 被刪除之後就會發出 Flow Removed 訊息通知 Controller 
            hard_timeout     Max time before discarding (seconds)   Flow Entry 的有效期限，以秒為單位。跟 idle_timeout 不同的地方是， hard_timeout 在超過時限後並不會重新歸零計算。 也就是說跟 Flow Entry 與有沒有被參照無關，只要超過指定的時間就會被刪除。跟 idle_timeout 一樣，當 Flow Entry 被刪除時，Flow Removed 訊息將會被發送來通知 Controller。
            priority         Priority level of flow entry   Flow Entry 的優先權。數值越大表示權限越高。
            buffer_id        Buffered packet to apply to (or OFP_NO_BUFFER)     指定 OpenFlow 交換器上用來儲存封包的緩衝區 ID。 緩衝區 ID 會放在通知 Controller 的 Packet-In 訊息中，並且和接下來的 OFPP_TABLE 所指定的輸出埠和 Flow Mod 訊息處理時可以被參照。 當發送的命令訊息為 OFPFC_DELETE 或 OFPFC_DELETE_STRICT 時，會忽略本數值。如果不指定緩衝區 ID 的時候，必須使用 OFP_NO_BUFFER 作為其設定值。
            out_port         For ``OFPFC_DELETE*`` commands, require matching
                             entries to include this as an output port
                             OFPFC_DELETE 和 OFPFC_DELETE_STRICT 命令用來指定輸出埠的參數。 命令為 OFPFC_ADD、OFPFC_MODIFY、OFPFC_MODIFY_STRICT 時則可以忽略。若要讓本參數無效則指定輸出埠為 OFPP_ANY 。
            out_group        For ``OFPFC_DELETE*`` commands, require matching
                             entries to include this as an output group
                             跟 out_port 一樣，作為一個輸出埠，但是轉到特定的 group。若要使其無效，則指定為 OFPG_ANY 。
            flags            Bitmap of the following flags.
        
                             | OFPFF_SEND_FLOW_REM  Flow Entry 被移除的時候，對 Controller 發送 Removed 訊息。
                             | OFPFF_CHECK_OVERLAP  使用 OFPFC_ADD 時，檢查是否有重複的 Flow Entry 存在。 若是有則觸發 Flow Mod 失敗，並返回錯誤訊息。
                             | OFPFF_RESET_COUNTS   重設該 Flow Entry 的 packet counter 和 byte counter。
                             | OFPFF_NO_PKT_COUNTS  關閉該 Flow Entry 的 packet counter 功能。
                             | OFPFF_NO_BYT_COUNTS  關閉該 Flow Entry 的 byte counter 功能。
            importance       Eviction precedence
            match            Instance of ``OFPMatch``   設定 match
            instructions     list of ``OFPInstruction*`` instance
            datapath         OpenFlow 交換器以及 Flow table 的操作都是透過 Datapath 類別的實體來進行。 在一般的情況下，會由事件傳遞給事件管理的訊息中取得，例如：Packet-In 訊息。
            ================ ======================================================
            """
            mod = parser.OFPFlowMod(datapath=datapath, buffer_id=buffer_id,
                                    priority=priority, match=match,
                                    instructions=inst)
        else:
            mod = parser.OFPFlowMod(datapath=datapath, priority=priority,
                                    match=match, instructions=inst)
        datapath.send_msg(mod)
```

### 传输封包

```python
 data = None
        if msg.buffer_id == ofproto.OFP_NO_BUFFER:
            data = msg.data
        """
        Packet-Out message

        The controller uses this message to send a packet out throught the
        switch.
        
         ================ ======================================================
        Attribute        Description
        ================ ======================================================
        buffer_id        ID assigned by datapath (OFP_NO_BUFFER if none)    指定 OpenFlow 交換器上封包對應的緩衝區。 如果不想使用緩衝區，則指定為 OFP_NO_BUFFER 。
        match            Instance of ``OFPMatch``
                         (``in_port`` is mandatory in the match field)
        actions          list of OpenFlow action class  指定 actions list。
        data             Packet data of a binary type value or
                         an instances of packet.Packet.
                         設定封包的 binary data 。主要用在 buffer_id 為 OFP_NO_BUFFER 的情況。 如果使用了 OpenFlow 交換器的緩衝區則可以省略。
        datapath         指定 OpenFlow 交換器對應的 Datapath 類別實體
        in_port          指定接收封包的連接埠號。 如果不想使用的話就指定為 OFPP_CONTROLLER 。
        ================ ======================================================
        交換器的實作時，在 Packet-In 訊息中指定 buffer_id。若是 Packet-In 訊息中 buffer_id 被設定為無效時。Packet-In 的封包必須指定 data 以便傳送。
        """
        out = parser.OFPPacketOut(datapath=datapath, buffer_id=msg.buffer_id,
                                  in_port=in_port, actions=actions, data=data)
        datapath.send_msg(out)
```

### 全文件

```python
# Copyright (C) 2011 Nippon Telegraph and Telephone Corporation.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
# implied.
# See the License for the specific language governing permissions and
# limitations under the License.

from ryu.base import app_manager
from ryu.controller import ofp_event
from ryu.controller.handler import CONFIG_DISPATCHER, MAIN_DISPATCHER
from ryu.controller.handler import set_ev_cls
from ryu.ofproto import ofproto_v1_3
from ryu.lib.packet import packet
from ryu.lib.packet import ethernet
from ryu.lib.packet import ether_types


class SimpleSwitch13(app_manager.RyuApp):
    OFP_VERSIONS = [ofproto_v1_3.OFP_VERSION]

    def __init__(self, *args, **kwargs):
        super(SimpleSwitch13, self).__init__(*args, **kwargs)
        self.mac_to_port = {}

    """
       接受到任何一個 OpenFlow 訊息即會產生一個相對應的事件
       事件管理（ Event Handler ）使用修飾器（ Decorator ）标记
       set_ev_cls 則指定事件類別得以接受訊息和交換器狀態作為參數。
       名稱的規則為 ryu.controller.ofp_event.EventOFP + <OpenFlow訊息名稱>

       =========================================== ===============================
       Negotiation phase                           Description
       =========================================== ===============================
       ryu.controller.handler.HANDSHAKE_DISPATCHER Sending and waiting for hello
                                                   message
       ryu.controller.handler.CONFIG_DISPATCHER    Version negotiated and sent
                                                   features-request message
       ryu.controller.handler.MAIN_DISPATCHER      Switch-features message
                                                   received and sent set-config
                                                   message
       ryu.controller.handler.DEAD_DISPATCHER      Disconnect from the peer.  Or
                                                   disconnecting due to some
                                                   unrecoverable errors.
       =========================================== ===============================
       """
    @set_ev_cls(ofp_event.EventOFPSwitchFeatures, CONFIG_DISPATCHER)
    def switch_features_handler(self, ev):
        """
         OpenFlow 交換器的握手協議完成之後，新增 Table-miss Flow Entry
         到 Flow table 中為接收 Packet-In 訊息做準備。
        接收到 Switch features（ Features reply ）訊息後就會新增 Table-miss Flow Entry。
        """
        # ev.msg 是用來儲存對應事件的 OpenFlow 訊息類別實體。
        # 这里是ryu.ofproto.ofproto_v1_3_parser.OFPSwitchFeatures
        # msg.datapath 這個訊息是用來儲存 OpenFlow 交換器的 ryu.controller.controller.Datapath 類別所對應的實體。
        datapath = ev.msg.datapath
        ofproto = datapath.ofproto
        parser = datapath.ofproto_parser

        # install table-miss flow entry
        #
        # We specify NO BUFFER to max_len of the output action due to
        # OVS bug. At this moment, if we specify a lesser number, e.g.,
        # 128, OVS will send Packet-In with invalid buffer_id and
        # truncated packet data. In that case, we cannot output packets
        # correctly.  The bug has been fixed in OVS v2.1.0.
        match = parser.OFPMatch()  # 空的 match 將被產生為了 match 所有的封包。match 表示於 OFPMatch 類別中。
        """
        為了轉送到 Controller 連接埠， OUTPUT action 類別（ OFPActionOutput ）的
        實例將會被產生。Controller 會被指定為封包的目的地，OFPCML_NO_BUFFER 會被設定為 max_len 以便接下來的封包傳送。
        """
        actions = [parser.OFPActionOutput(ofproto.OFPP_CONTROLLER,
                                          ofproto.OFPCML_NO_BUFFER)]
        """
        Table-miss Flow Entry 的優先權為 0 即最低的優先權，而且此 Entry 可以 match 所有的封包。
        這個 Entry 的 Instruction 通常指定為 output action ，並且輸出的連接埠將指向 Controller。 因此當封包沒有 match 任何一個普通 Flow Entry 時，則觸發 Packet-In。
        將優先權設定為 0（ 最低優先權 ），然後執行 add_flow() 方法以發送 Flow Mod 訊息
        """
        self.add_flow(datapath, 0, match, actions)

    """
    對於 Flow Entry 來說，設定 match 條件以分辨目標封包、設定 instruction 
    以處理封包以及 Entry 的優先權和有效時間。
    """
    def add_flow(self, datapath, priority, match, actions, buffer_id=None):
        ofproto = datapath.ofproto
        parser = datapath.ofproto_parser
        """
        對於交換器的的實作，Apply Actions 是用來設定那些必須立即執行的 action 所使用。
        最後透過 Flow Mod 訊息將 Flow Entry 新增到 Flow table 中。
        """
        inst = [parser.OFPInstructionActions(ofproto.OFPIT_APPLY_ACTIONS,
                                             actions)]
        if buffer_id:
            """
            Flow Mod 訊息的類別為 OFPFlowMod 。使用 OFPFlowMod 所產生的實體透過 Datapath.send_msg() 
            方法來發送訊息給 OpenFlow 交換器。
            OFPFlowMod 類別的建構子參數相當的多，但是在大多數的情況下都有其預設值，原始碼中括號內的部分即是預設值。
            """
            """
            class OFPFlowMod(MsgBase):
            Modify Flow entry message
            The controller sends this message to modify the flow table.
            ================ ======================================================
            Attribute        Description
            ================ ======================================================
            cookie           Opaque controller-issued identifier    Controller 所設定儲存的資料，在 Entry 的更新或者刪除的時所需要使用的資料都會放在這邊，當做過濾器使用，而且不可以作為封包處理的參數。
            cookie_mask      Mask used to restrict the cookie bits that must match
                             when the command is ``OPFFC_MODIFY*`` or
                             ``OFPFC_DELETE*``
                             Entry 的更新或刪除時，若是該值為非零，則做為指定 Entry 的 cookie 使用。
            table_id         ID of the table to put the flow in 指定 Flow Entry 的 Table ID 
            command          One of the following values.
        
                             | OFPFC_ADD    	Flow Entry 新增
                             | OFPFC_MODIFY     Flow Entry 更新
                             | OFPFC_MODIFY_STRICT      嚴格的 Flow Entry 更新
                             | OFPFC_DELETE     Flow Entry 刪除
                             | OFPFC_DELETE_STRICT      嚴格的 Flow Entry 刪除
            idle_timeout     Idle time before discarding (seconds)  Flow Entry 的有效期限，以秒為單位。Flow Entry 如果未被參照而且超過了指定的時間之後， 該 Flow Entry 將會被刪除。如果 Flow Entry 有被參照，則超過時間之後會重新歸零計算。在 Flow Entry 被刪除之後就會發出 Flow Removed 訊息通知 Controller 
            hard_timeout     Max time before discarding (seconds)   Flow Entry 的有效期限，以秒為單位。跟 idle_timeout 不同的地方是， hard_timeout 在超過時限後並不會重新歸零計算。 也就是說跟 Flow Entry 與有沒有被參照無關，只要超過指定的時間就會被刪除。跟 idle_timeout 一樣，當 Flow Entry 被刪除時，Flow Removed 訊息將會被發送來通知 Controller。
            priority         Priority level of flow entry   Flow Entry 的優先權。數值越大表示權限越高。
            buffer_id        Buffered packet to apply to (or OFP_NO_BUFFER)     指定 OpenFlow 交換器上用來儲存封包的緩衝區 ID。 緩衝區 ID 會放在通知 Controller 的 Packet-In 訊息中，並且和接下來的 OFPP_TABLE 所指定的輸出埠和 Flow Mod 訊息處理時可以被參照。 當發送的命令訊息為 OFPFC_DELETE 或 OFPFC_DELETE_STRICT 時，會忽略本數值。如果不指定緩衝區 ID 的時候，必須使用 OFP_NO_BUFFER 作為其設定值。
            out_port         For ``OFPFC_DELETE*`` commands, require matching
                             entries to include this as an output port
                             OFPFC_DELETE 和 OFPFC_DELETE_STRICT 命令用來指定輸出埠的參數。 命令為 OFPFC_ADD、OFPFC_MODIFY、OFPFC_MODIFY_STRICT 時則可以忽略。若要讓本參數無效則指定輸出埠為 OFPP_ANY 。
            out_group        For ``OFPFC_DELETE*`` commands, require matching
                             entries to include this as an output group
                             跟 out_port 一樣，作為一個輸出埠，但是轉到特定的 group。若要使其無效，則指定為 OFPG_ANY 。
            flags            Bitmap of the following flags.
        
                             | OFPFF_SEND_FLOW_REM  Flow Entry 被移除的時候，對 Controller 發送 Removed 訊息。
                             | OFPFF_CHECK_OVERLAP  使用 OFPFC_ADD 時，檢查是否有重複的 Flow Entry 存在。 若是有則觸發 Flow Mod 失敗，並返回錯誤訊息。
                             | OFPFF_RESET_COUNTS   重設該 Flow Entry 的 packet counter 和 byte counter。
                             | OFPFF_NO_PKT_COUNTS  關閉該 Flow Entry 的 packet counter 功能。
                             | OFPFF_NO_BYT_COUNTS  關閉該 Flow Entry 的 byte counter 功能。
            importance       Eviction precedence
            match            Instance of ``OFPMatch``   設定 match
            instructions     list of ``OFPInstruction*`` instance
            datapath         OpenFlow 交換器以及 Flow table 的操作都是透過 Datapath 類別的實體來進行。 在一般的情況下，會由事件傳遞給事件管理的訊息中取得，例如：Packet-In 訊息。
            ================ ======================================================
            """
            mod = parser.OFPFlowMod(datapath=datapath, buffer_id=buffer_id,
                                    priority=priority, match=match,
                                    instructions=inst)
        else:
            mod = parser.OFPFlowMod(datapath=datapath, priority=priority,
                                    match=match, instructions=inst)
        datapath.send_msg(mod)

    """
    class OFPPacketIn(MsgBase):
    ============= =========================================================
    Attribute     Description
    ============= =========================================================
    buffer_id     ID assigned by datapath   接收封包的內容若是存在 OpenFlow 交換器上時所指定的ID 如果在沒有 buffer 的狀況下，則設定 ryu.ofproto.ofproto_v1_3.OFP_NO_BUFFER
    total_len     Full length of frame  接收封包的資料長度
    reason        Reason packet is being sent.

                  | OFPR_TABLE_MISS
                  | OFPR_APPLY_ACTION
                  | OFPR_INVALID_TTL
                  | OFPR_ACTION_SET
                  | OFPR_GROUP
                  | OFPR_PACKET_OUT
    table_id      ID of the table that was looked up
    cookie        Cookie of the flow entry that was looked up
    match         Instance of ``OFPMatch``  ryu.ofproto.ofproto_v1_3_parser.OFPMatch 類別的實體，用來儲存接收封包的 Meta 資訊。
    data          Ethernet frame    接收封包本身的 binary 資料
    ============= =========================================================
    """

    @set_ev_cls(ofp_event.EventOFPPacketIn, MAIN_DISPATCHER)
    def _packet_in_handler(self, ev):
        # If you hit this you might want to increase
        # the "miss_send_length" of your switch
        if ev.msg.msg_len < ev.msg.total_len:
            self.logger.debug("packet truncated: only %s of %s bytes",
                              ev.msg.msg_len, ev.msg.total_len)
        msg = ev.msg
        datapath = msg.datapath
        ofproto = datapath.ofproto
        parser = datapath.ofproto_parser
        # 從 OFPPacketIn 類別的 match 得到接收埠（ in_port ）
        in_port = msg.match['in_port']

        pkt = packet.Packet(msg.data)
        eth = pkt.get_protocols(ethernet.ethernet)[0]

        if eth.ethertype == ether_types.ETH_TYPE_LLDP:
            # ignore lldp packet
            return
        #  目的 MAC 位址和來源 MAC 位址使用 Ryu 的封包函式庫，從接收到封包的 Ethernet header 取得。
        dst = eth.dst
        src = eth.src

        dpid = format(datapath.id, "d").zfill(16)
        self.mac_to_port.setdefault(dpid, {})

        self.logger.info("packet in %s %s %s %s", dpid, src, dst, in_port)

        # learn a mac address to avoid FLOOD next time.
        self.mac_to_port[dpid][src] = in_port
        """
        目的 MAC 位址若存在于 MAC 位址表，則判斷該連接埠的號碼為輸出。
        反之若不存在于 MAC 位址表則 OUTPUT action 類別的實體並生成 flooding（ OFPP_FLOOD ）給目的連接埠使用。
        """
        """
        在 OpenFlow 中，有個邏輯連接埠叫做 NORMAL 並在規範中被列為選項
        （ 也就是說可以不進行實作 ）。當被指定的連接埠為 NORMAL 時，
        傳統 L2/L3 的功能將會被啟用來處理封包。 意思是當把所有輸出的埠均設定為 NORMAL 
        時，交換器將會視作一個普通的交換器而存在。跟一般交換器的差別在於我們是使用 
        OpenFlow 來達到這樣的功能。
        """
        if dst in self.mac_to_port[dpid]:
            # 若是找到了目的 MAC 位址，則在交換器的 Flow table 中新增
            out_port = self.mac_to_port[dpid][dst]
        else:
            out_port = ofproto.OFPP_FLOOD
        # Table-miss Flow Entry 包含 match 和 action，並透過 add_flow() 來新增
        actions = [parser.OFPActionOutput(out_port)]

        # install a flow to avoid packet_in next time
        if out_port != ofproto.OFPP_FLOOD:
            # 不同於平常的 Table-miss Flow Entry ，這次將加上設定 match 條件。
            # 本次交換器實作中，接收埠（ in_port ）和目的 MAC 位址（ eth_dst ）已指定。例如，接收到來自連接埠 1 的封包就傳送到 host B。
            match = parser.OFPMatch(in_port=in_port, eth_dst=dst, eth_src=src)
            # verify if we have a valid buffer_id, if yes avoid to send both
            # flow_mod & packet_out
            if msg.buffer_id != ofproto.OFP_NO_BUFFER:
                # 在這邊指定 Flow Entry 優先權為 1，而優先權的值越大，表示有更高的優先權。
                # 因此，這邊新增的 Flow Entry 將會先於 Table-miss Flow Entry 而被執行。
                self.add_flow(datapath, 1, match, actions, msg.buffer_id)
                return
            else:
                self.add_flow(datapath, 1, match, actions)
            # 综上，連接埠 1 接收到的封包，若是要轉送至 host B（ 目的 MAC 位址 B) 的封包則轉送至連接埠 4。

        data = None
        if msg.buffer_id == ofproto.OFP_NO_BUFFER:
            data = msg.data
        """
        Packet-Out message

        The controller uses this message to send a packet out throught the
        switch.
        
         ================ ======================================================
        Attribute        Description
        ================ ======================================================
        buffer_id        ID assigned by datapath (OFP_NO_BUFFER if none)    指定 OpenFlow 交換器上封包對應的緩衝區。 如果不想使用緩衝區，則指定為 OFP_NO_BUFFER 。
        match            Instance of ``OFPMatch``
                         (``in_port`` is mandatory in the match field)
        actions          list of OpenFlow action class  指定 actions list。
        data             Packet data of a binary type value or
                         an instances of packet.Packet.
                         設定封包的 binary data 。主要用在 buffer_id 為 OFP_NO_BUFFER 的情況。 如果使用了 OpenFlow 交換器的緩衝區則可以省略。
        datapath         指定 OpenFlow 交換器對應的 Datapath 類別實體
        in_port          指定接收封包的連接埠號。 如果不想使用的話就指定為 OFPP_CONTROLLER 。
        ================ ======================================================
        交換器的實作時，在 Packet-In 訊息中指定 buffer_id。若是 Packet-In 訊息中 buffer_id 被設定為無效時。Packet-In 的封包必須指定 data 以便傳送。
        """
        out = parser.OFPPacketOut(datapath=datapath, buffer_id=msg.buffer_id,
                                  in_port=in_port, actions=actions, data=data)
        datapath.send_msg(out)
```

## 部署实施

| 名稱         | 數值       | 說明                        |
| ---------- | -------- | ------------------------- |
| topo       | single,3 | 交換器 1 台、host 3 台的拓璞       |
| mac        | 無        | 自動設定 host 的 MAC 位址        |
| switch     | ovsk     | 使用 Open vSwitch           |
| controller | remote   | 指定外部的 OpenFlow Controller |
| x          | 無        | 啟動 xterm                  |

命令：`$ sudo mn --topo single,3 --mac --switch ovsk --controller remote -x`

查看 Open vSwitch 的狀態：`root@ryu-vm:~# ovs-vsctl show`

設定 OpenFlow 的版本為 1.3：`root@ryu-vm:~# ovs-vsctl set Bridge s1 protocols=OpenFlow13`

檢查空白的 Flow table：`root@ryu-vm:~# ovs-ofctl -O OpenFlow13 dump-flows s1`

在controller c0中的xterm执行：`root@ryu-vm:~# ryu-manager --verbose ryu.app.simple_switch_13`

確認 Table-miss Flow Entry 已經被加入:`root@ryu-vm:~# ovs-ofctl -O openflow13 dump-flows s1`

優先權為 0，沒有 matcth，action 為 CONTROLLER，重送的資料大小為 65535 (0xffff = OFPCML\_NO\_BUFFER ）

## 验证方案

從 host 1 向 host 2 發送 ping。

1.  ARP request

    > 此時 host 1 並不知道 host 2 的 MAC 位址，原則上 ICMP echo request 之前的 ARP request 是用 廣播的方式發送。這樣的廣播方式會讓 host 2 和 host 3 都同樣接受到訊息。
2.  ARP reply

    > host 2 使用 ARP reply 回覆 host 1 要求。
3.  ICMP echo request

    > 現在 host 1 知道了 host 2 的 MAC 位址，因此發送 echo request 給 host 2。
4.  ICMP echo reply

    > host 2 此時也知道了 host 1 的 MAC 位址，因此發送 echo reply 給 host 1。

具体内容在[交換器（ Switching Hub ） — Ryubook 1.0 說明文件 (ryu-sdn.org)](https://book.ryu-sdn.org/zh\_tw/html/switching\_hub.html#id7)查看

