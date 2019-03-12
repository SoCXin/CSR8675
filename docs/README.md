### 蓝牙物理链路

蓝牙底层核心协议：
- BaseBand
- 基于BaseBand的LMP、L2CAP
- 基于LMP、L2CAP 的 SDP

蓝牙规范还定义了主机控制器接口（HCI），它为基带控制器、连接管理器、硬件状态和控制寄存器提供命令接口。HCI可以位于L2CAP的下层，也可以可位于L2CAP的上层。

在底层核心协议之上，蓝牙协议定义了多种面向应用的协议，包括在设备之间发送和接收文件电缆替代协议RFCOMM 和Object Exchange（OBEX），RFCOMM和服务发现协议SDP处于同一层级。
如果想发送和接收流数据RFCOMM更好，反之如果想发送对象数据以及关于负载的上下文和元数据，则OBEX最好。
RFCOMM是通过不同的频道（channel）来提供不同的Profile的，RFCOMM可以通过SDP找到要用的服务在设备上的哪个频道上。


蓝牙的Profile定义了设备如何实现一种连接或者应用，可以理解为连接层或者应用层协议，常用的profile包括：

通用访问配置文件(Generic Access Profile, GAP)
服务发现应用配置文件(Service Discovery Application Profile, SDAP)
串行端口配置文件(Serial Port Profile, SPP)
通用对象交换配置文件(Generic Object Exchange Profile, GOEP)

SPP：蓝牙串行端口基于SPP协议（Serial Port Profile），能在蓝牙设备之间创建串口进行数据传输，俗称蓝牙串口。

RFCOMM：一个简单传输协议，其目的为了解决如何在两个不同设备上的应用程序之间保证一条完整的通信路径，并在它们之间保持一通信段的问题。

其中的SPP是基于RFCOMM的，spp协议处于rfcomm的上层，spp的应用需走rfcomm层。如果你使用RFCOMM能够实现，那么也就不需要使用SPP，而却速度还会比PP来做快，因为省略了采用profile的一些数据包头等。不过，还是推荐采SPP来做，兼容性有保证，这也是为什么蓝牙本质上数据和语音的传送却出现HFP，HSP，SPP，OPP等诸多具体应用profile的原因。

逻辑传输层(位于物理链的上一层)

SCO：面向连接的同步逻辑传输(Synchronous Connection Oriented),面向同步连接，HFP协议语音走的SCO。

ACL：面向连接的异步逻辑传输(Asynchronous Connection-Oriented)，在主从设备之间以分组交换方式传输数据，可以支持异步应用也可以支持同步应用。


### 蓝牙音频编码

SBC：子带编码（sub band code）是一种以中等比特率传递高质量音频数据的低计算复杂度的音频编码算法，蓝牙传输在不支持AAC/aptx的时候都用SBC传输，音质一般，大多数都是这种格式。A2DP协议中强制支持SBC。
有损音频格式，蓝牙通信120ms-200ms左右延时。

AAC：当蓝牙支持AAC格式的文件，手机也支持AAC传输时，音质比SBC好很多。无损音频格式，蓝牙通信有120mS左右延时。

APTX：是蓝牙传输的一种无损格式，由csr（高通）推广，要支持APTX就必须要购买这个软件费用，APTX并不是大多数手机都支持，虽然效果好，但真正支持的设备端不多。
APT-X-LL：无损音频格式，32mS超低延时；APT-X-L：无损音频格式，60mS低延时。


SPP：蓝牙串行端口基于SPP协议（Serial Port Profile），能在蓝牙设备之间创建串口进行数据传输，俗称蓝牙串口。

RFCOMM：一个简单传输协议，其目的为了解决如何在两个不同设备上的应用程序之间保证一条完整的通信路径，并在它们之间保持一通信段的问题。

SDP：服务搜索协议(SDP)提供了应用发现可用服务以及确定可用服务特点的方法

L2CAP协议：逻辑链路控制和适配协议（Logical Link Control and Adaptation Protocol），是蓝牙系统中的核心协议，负责适配基带中的上层协议。它同链路管理器并行工作，向上层协议提供定向连接的和无连接的数据业务。这个上层具有L2CAP的分割和重组功能，使更高层次的协议和应用能够以64KB的长度发送和接收数据包。它还能够处理协议的多路复用，以提供多种连接和多个连接类型（通过一个空中接口），同时提供服务质量支持和成组通讯。逻辑链路控制和适配协议（L2CAP）是基带的上层协议，可以认为它与LMP并行工作，它们的区别在于当业务数据不经过LMP时，L2CAP为上层提供服务。L2CAP向上层提供面向连接的和无连接的数据服务，它采用了多路技术、分割和重组技术、群提取技术。L2CAP允许高层协议以64K字节收发数据分组。虽然基带协议提供了SCO和ACL两种连接类型，但L2CAP只支持ACL。

HCI：主机控制器接口

### 格式协议

- APT-X-LL：无损音频格式，32mS超低延时；
- APT-X-L：无损音频格式，60mS低延时；支持设备：三星、HTC 等手机
- AAC：无损音频格式，120mS延时；支持设备：iphone 手机和 IPAD 
- SBC：有损音频格式，延时120mS-200mS；支持设备：一般蓝牙设备的基本音频格式。 

蓝牙源的系统音频延迟接近700ms，AUX源的系统音频延迟接近400ms，TWS是一个低延迟的蓝牙协议，音频延迟不到200ms，TWS不支持SPDIF输入源。

A2DP强制支持SBC编码，也支持非A2DP编码格式。

TWS模式下，master和slave之间的音频数据走的是A2DP协议，控制命令走的是AVRCP协议


