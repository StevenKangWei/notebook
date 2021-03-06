# 04 - cache 存储器

计算机的存储器被组织成层次结构

- 处理器内的寄存器
- 一级或多级告诉缓存
- 主存：通常由动态随机存取存储器(DRAM) 构成

以上都被认为是系统内部的存储器。

存储层次继续往外划分外部存储器。通常是固定硬盘、可装卸存储设备（如光盘、磁带机、U盘）

沿着存储器层次结构自顶向下，存储器成本逐层下降，容量增大，存取事件变长。

通常处理器将要访问的主存位置极有可能是刚被访问过的或其临近位置，这就是程序访问的**局部性原理**。所以cache会自动保存一些来自近期被使用过的DRAM字的副本。

## 计算机存储系统概述

- 存储位置：是指存储器处于计算机的内部或外部。内部存储器通常指主存。
- 存储容量：
- 传送单元：
    - 字：存储器组织的“自然”单元。字长通常与一个整数的数据位数和指令长度相等，但也有很多例外。
    - 可寻址单元：在某些系统中，可寻址的单元是字，但是许多系统允许在字节上寻址。
    - 传输单元：对于主存储器，这是指每次读出或写入的存储器的位数。
- 存取方法：
    - 顺序存取：存储器组织成许多称为记录的数据单元，它们以特定的线性顺序方式存取。
    - 直接存取：
    - 随机存取：存储器每一个可寻址的存储位置有唯一的物理编排的寻址机制。存取给定存储位置的时间是固定的，不依赖于前面存取的序列。
    - 关联存取：
    - 存取时间：
    - 存储周期时间：
    - 传输率：
- 性能
- 物理类型
- 物理特性
- 组织

## cache 存储器原理

cache存储器的目的是使存储器的速度逼近可用的最快的存储器的速度，同事以较便宜的半导体存储器的价格提供一个大的存储器容量。

## cache 的设计要素

### cache 地址

### cache 容量

我们希望 cache 的容量足够小，以至于整个存储系统的平均价格接近于单个主存储器的价格，同事我们也希望 cache 足够大，从而使得整个存储系统的平均存取时间接近于单个cache的存取时间。还有几个减小cache容量的动机。cache越大，寻址所需要电路门就会越多，结果是大的cache比小的稍慢，即使是采用相同的集成电路技术制造并放在芯片的电路板的同一位置。cache 容量也受芯片和电路板面积的限制，因为cache 的性能对工作负载的性能十分敏感，所以不可能有“最右”的cache容量。

## 映射功能

由于cache的行比主存储器的块要少，因此需要一种算法来实现主存块到cache行的映射。还需要一种方法来确定当前那一块占用了 cache 行，映射方法的选择决定了 cache 的组织结构，通常采用三种方法：

**直接映射**

直接映射是最简单的映射技术，将主存中的每个快映射到一个固定可用的cache 行中，直接映射可表示为：

    i = j mod m

其中i = cache 行号， j = 主存储器的块号，m = cache 的行数。

直接映射技术简单，实现花费少。主要缺点是：对于任意给定的块，它所对应的 cache 位置时固定的。因此，如果一个程序恰巧重复访问两个需要映射到同一行中且来自不同块的字，则这两个块将不断地被交换到cache中。cache的命中率将会降低（一种所谓的抖动现象）

**全相联映射**

全相连映射克服了直接映射的缺点，它允许每一个主存块装入cache中的任意行。这种情况下，cache 控制逻辑将存储地址简单地表示为一个标记域加一个字域。标记域用来唯一标识一个主存块。为了确定某块是否在 cache 中，cache 控制逻辑必须同时对每一行中的标记进行检查，看其是否匹配。地址中无对应行号的字段，所以cache中的行号不由地址格式决定。

对于全相联映射，当新的块读入 cache 中时，替换旧的一块很灵活。权相联映射的主要缺点时需要复杂的电路来并行检查所有的cache行标记。

主存中的一个地址可被映射进任意cache line，问题是：当寻找一个地址是否已经被cache时，需要遍历每一个cache line来寻找，这个代价很高。就像停车位可以大家随便停一样，停的时候简单，找车的时候需要一个一个停车位的找了。

全相联映射方式比较灵活，主存的各块可以映射到Cache的任一块中，Cache的利用率高，块冲突概率低，只要淘汰Cache中的某一块，即可调入主存的任一块。但是，由于Cache比较电路的设计和实现比较困难，这种方式只适合于小容量Cache采用。

**组相联映射**

组相连映射是一种折中的方法，它既体现了直接映射和全相联映射的有点，又避免了两者的缺点。

主存和Cache都分组，主存中一个组内的块数与Cache中的分组数相同，组间采用直接映射，组内采用全相联映射。主存块存放到哪个组是固定的，至于存到该组哪一块则是灵活的。即主存的某块只能映射到cache的特定组中的任意一块。

在组相联映射中，cache 分为 v 个组，每组包含k个行，他们的关系为：

m = v * k
i = j mod v

其中: i = cache 行号
j = 主存储器的块号
m = cache 的行数
v = 组数
k = 每组中的行数

这被称为 k 路组相联映射。

常采用的组相联结构Cache，每组内有2、4、8、16块，称为2路、4路、8路、16路组相联Cache。以上为2路组相联Cache。组相联结构Cache是前两种方法的折中方案，适度兼顾二者的优点，尽量避免二者的缺点，因而得到普遍采用。

### 替换算法

一旦 cache 行被占用，当新的数据块装入 cache 中时，原存在的块必须被替换掉。对于直接映射，任意特殊块都只有唯一的一行可以使用，没有选择的可能。对于全相联和组相联映射技术，则需要一种替换算法。

- 最近最少使用算法(LRU)：

- 先进先出算法(FIFO)：

- 最不经常使用算法(LFU)：

- 随机化算法：

## 参考资料

- [1] 计算机组成与体系结构 [美] William Stallings 著
- [2] [主存到Cache直接映射、全相联映射和组相联映射](https://blog.csdn.net/dongyanxia1000/article/details/53392315)
