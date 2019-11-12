# Computer Networking A TOP-DOWN APPROACH

## Chapter 1 计算机网络和因特网

### 1.1 什么是因特网
1. 从因特网软硬件构成描述
- 互联了全世界计算设备的网络. 这些设备称为**主机** (host) 或**端系统** (end system). 
- 端系统通过**通信链路** (communication link) 和**分组交换机** (packet switch) 连接到一起.
- 通信链路
  - 组成: 不同物理媒体. 同轴电缆, 铜线, 光纤, 无线电频谱等
  - 传输速率: 比特/秒 (bit/s, bps)
  - 分组: 发送端系统将数据分段, 并给每段加上首部字节.
- 分组交换机
  - 路由器
    - 通常用于网络核心中
  - 链路层交换机
    - 通常用于接入网中
  - 路径: 从发送端到接收端, 一个分组所经历的一系列通信链路和分组交换机
- ISP
  - 端系统通过因特网服务提供商 (Internet Service Provider, ISP) 接入互联网. 
  - 每个ISP自身就是一个由多台分组交换机和多段通信链路组成的网络. 
  - ISP提供不同类型的网络接入, 如线缆调制解调器, DSL, 高速局域网接入, 移动无线接入等. 
  - ISP也提供因特网服务, 低层级的ISP通过高层级ISP互联, 高层级ISP由通过高速光纤链路互联的高速路由器组成. ISP独立管理, 运行IP协议, 遵从一定命名和地址规则.
- 协议
  - 控制信息的接受发送
  - TCP/IP (Transmission Control Protocol, Internet Protocol)
- 因特网标准由因特网工程任务组 (Internet Engineering Task Force, IETF) 研发.

2. 从为分布式应用提供服务的联网基础设施描述
