# 2.2.3 使用 BBR 提升互联网数据传输效率

随着全球化互联网迅速普及，网卡由最初的 Mbps 到 Gbps，路由网关的内存也从 KB 到 GB，跨洋长链路的出现，4G/5G/WiFi无线网络的应用等等因素，造成的影响就是：带宽时延积越来越大，链路层和物理层误码导致的随机丢包会时常出现，这就导致丢包和拥塞之间的关系也变得愈发微弱，

而常规的拥塞控制算法从 TCP-Reno 到 Linux 默认的 Cubic 算法，主要的思想是基于丢包的拥塞控制策略，依靠丢失数据包的迹象作为减缓发送速率的信号。在高 BDP 环境中基于丢包来判断拥塞这种由事件驱动调整拥塞窗户非常被动，难在现代网络环境中发挥作用，无法充分利用带宽。


TCP 的 BBR（Bottleneck Bandwidth and Round-trip propagation time，BBR）是谷歌在2016年开发的一种新型的 TCP 拥塞控制算法。BBR 尝试通过使用全新的拥塞控制来解决这个问题，它使用基于延迟而不是丢包作为决定发送速率的主要因素。


## BBR的改善

使用 BBR，可以获得显著的网络吞吐量的提升和延迟的降低。

吞吐量的改善在远距离路径上尤为明显，比如跨太平洋的文件或者大数据的传输，尤其是在有轻微丢包的网络条件下。

延迟改善主要体现在最后一公里的路径上，而这一路径经常受到缓冲膨胀（BtlBufSize）的影响。当网络链路拥塞时，就会发生缓冲膨胀，从而导致数据包在这些超大缓冲区中长时间排队。在先进先出队列系统中，过大的缓冲区会导致更长的队列和更高的延迟，并且不会提高网络吞吐量。由于 BBR 并不会试图填满缓冲区，所以在避免缓冲区膨胀方面往往会有更好的表现。

