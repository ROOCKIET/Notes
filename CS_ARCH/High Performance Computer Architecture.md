z佐治亚理工学院 cs6290 计算机体系结构

## part1

### L1 Introduction

什么是体系结构？

计算机的设计方法-》对应于他的应用

为什么需要？

提高性能、改善能力、

需要制造工艺+电路设计的进步

build未来的计算机

摩尔定律：18-24个月晶体管翻倍

所以：速度翻倍、能耗减半、内存翻倍

内存墙：memory wall、

mem latency 进展缓慢

cpu 与mem 的速度产生gap -》cache



cpu考虑：速度、成本、功耗

能耗：动态/静态

动态：p=0.5xc（电容正比于芯片面积）xv^2(电压的平方)xf（pinlv）x a（激活比例） -》降低电压

静态：减小电压，漏电流增大

L19

制造成本：芯片面积越大，晶圆的良率（晶圆上可用芯片比例）越小

摩尔定律让成本降低（面积减小

### L2 Measurement Evaluation 



performance：latency throughput 吞吐量

流水线时吞吐量加大

speedup：加速比

benchmark：程序和输入数据 用来测量性能

benchmark 种类：

真实应用、应用内核-应用里时间密集的部分、合成的benchmark（行为与应用内核类似，但是方便编译）、peak performance

benchmark标准：

TPC/EEMBC（面向嵌入式）/

SPEC(面向处理器)

算术/几何平均执行时间

性能铁律：cpu time=指令数（算法与编译器与指令集）*cpi*（指令集、处理器设计）x时钟周期（处理器设计、电路设计、晶体管速度）

cpi是变化的

阿姆达尔定律：、amdahl

对于整个程序，只speedup了部分程序

![image-20210421132936009](notepic/image-20210421132936009.png)

暗示：对时间密集的程序做加速

maike common case fast

lhadma定律（阿姆达尔反过来：不要忽略uncommon case

Diminishing Returns：收益递减（边际效应

balance

### L3 Pipelining

流水线 lantency不变 throghput翻n级（n级流水线）

 取址-解码-执行-mem（访存）-写回（到reg）

cpi>1

stall 流水线阻塞 -->bubble

flushes 分支跳转引起

control dependence 控制依赖-执行与否取决于分支 -》分支指令预测

流水线越深 flush的惩罚越大

data dependence  

写后读 read after writte -flow/**true** dependence -在流水线里stall

写后写 								output/false

读后写 									anti/false dependence

依赖和冒险 dependence and hazards 《-真依赖（距离三条指令（对于五级流水））

处理冒险：flush/stall/forward（利用alu反馈）

perf最好：30-40级流水

考虑power：10-15级流水

### L4 Branches

分支：条件跳转

预测错误需要flush 流水线

在取值的时候，因为没有解码，我们不知道分支指令是分支指令。只能根据指令地址去预测

需要解决：

他是分支吗？ 分支历史表

他被taken吗？ 分支历史表 里的计数器

目标地址？ 分支目标地址缓冲器

现在处理器流水线比较深+超标量，继续研究指令分支预测器还是有收益的

1.not taken 不做任何预测 直接取下一条指令

更好的预测：利用分支历史

btb branch target buffer 存储 pcnow-pctarget对

btb size-低的latency 与 准确率之间的平衡

bht branch history table ：history+counter

每次取址都要访问bht

1bit计数器适用于总是taken或者总是不taken 一次异常-》两次错误预判

2bit预测器（2bp，2bc

00 strong not taken

01 weak nt

10 weak taken

11 strong t

3bit预测器用来解决 tttt ntnt ttt

history based predictors：可以根据跳转的模式数选 历史记录bit避免浪费（每个历史模式都要有自己的计数器）

为每个history parrtern维护一个计数器？-？》history with shared counters

多个历史映射到一个计数器：通常用pc与历史xor异或（可以有很长的history bit pattren）

pshare预测器 p-每个分支都有自己的历史 share-共享计数器

gshare预测器 global 所有分支共用一个历史entry gshare可以做数据相关的推测



pshare+gshare+meta（2bit count） tournament 预测器

Hierarchical Predictors：good +ok

ok就足够使用时 就不更新good预测器 减小cost

ras return address stack 要小 ：替换策略删除老的 ：保证细粒度的小函数正确预测 

怎么在fetch阶段确定是return指令?

1.简单的预测器

pre-decoding：cache取cacheline时增加bit位

### L5 Predication

编译器对分支的优化

Predication ：同时fetch分支两侧

应用于小的if-else



if的转换（编译器）

if-else两个分支的结果都计算出来

用mov指令代替分支指令

movz/movn（某个flag为0/不为0 赋值）

可以解决很难预测的分支，不过需要更多寄存器，执行更多指令

需要条件mov指令

full predication：每条指令都是条件执行

## part2

### L6 ILP

指令分支预测器和if转换 用来消除控制冒险

如何解决数据依赖性？

forwarding技术：在exe阶段，alu执行后就将结果传给之后的指令

raw依赖，写后读

waw依赖 保证最后写入的数据正确

移除假依赖：war/waw（依赖只是应为同一个寄存器）name dependencies 太复杂

解决方法：duplicating reg alue，将写到同一个寄存器的值都记下来

另一个方案：reg 重命名 （编译器来做）

RAT register allocation table，在rat里维护最新的寄存器地址

理想处理器只受 真数据依赖 的影响

ilp指令并行性是程序的属性，而不是处理器

structural/资源冲突在ilp计算时不存在，因为是理想处理器，所有指令一个cycle完成

control dependence 假设所有预测都正确

ilp是理想处理器上 只考虑真数据依赖的 ipc

宽发射（同时执行多条）、消除假依赖、乱序（利用宽发射）

### L7 Tomasulo

托马苏洛算法（英語：Tomasulo algorithm）是IBM罗伯特·托马苏洛1967年所研发用来改善处理器乱序执行指令级并行性的硬件算法。

改善ipc

**消除控制依赖**

**真依赖raw 用乱序解决**

![image-20210525102341569](hpca_.assets/image-20210525102341569.png)

dispatch

执行单元播报结果，使rs队列中某些指令得到全部操作数，从而执行

rs中多个指令ready，选哪个执行：一般选用oldest，也可以用其他指令最依赖的（实现复杂），或者random

write sesult/broadcast：多单元个想播报，给执行慢的单元更高优先级

在tomasulo算法中，load/store与计算指令不同，in-order（现代处理器 l/s也可以乱序）

issue-dispatch-broadcast

## part3

### L8 ROB

异常的时候如何处理reordering mess

即从异常返回时，引起异常的指令后的n条指令已经被执行了，寄存器值被后边的指令改写

所以乱序执行需要：

顺序发射、乱序执行、乱序播报、顺序写回寄存器

需要一个reorder buffer：用来维持程序写寄存器的顺序，和暂时存储指令的结果

预测错误的恢复：清空rob+将rat指向regs+清空rs与alu

异常的恢复：将异常视为一种值

每次commit 更新reg的值，即使不是最新执行的值

统一reservation stations：增加利用率

超标量处理器：

fetch/decode/issue/dispatch/broadcast/commit>1，环节要平衡

out of order:只有执行和广播是乱序的

### L9 Memory Ordering

P56

之前讲的都是register dependency

如何解决memory dependency

在commit时写内存

load-store queue LSQ

load在lsq中查找到需要的值就不必在去memory里

go anyway option 性能最好 load不等之前的store完毕就 查找lsq/mem 错了就recover

in order /out of ouder memory exe

66

load-store-queue 可以减少load的次数（从未commit的store中寻找） 

### L10 Compiler ILP

如何通过编译器增加ilp？

**limited window现象**：依赖链太长，硬件看不到后边的独立指令，但是编译器可以看到。

tree height reduction：尽量做独立的运算

比如 r8=r2+r3+（r4+r5）； 操作符有结合律才可以用

如何让后方独立的指令被处理器看到？**指令调度/循环展开/trace调度**

指令调度：将指令提前，并将受影响的指令修正（包括rename）

**if-conversion**：分支转换成顺序，就可以指令调度了

loop不适合if-conversion

loop 展开：将loop里的程序块调大，以便进行指令调度（空间换时间）

指令数变少，ilp增加

**循环展开缺点**：代码膨胀，循环次数未知，循环次数除不尽

函数inlining：减少调用函数的开销，方便指令调度（同样，空间换时间）

其他提高ipc的编译技术：软件流水线，trace调度



### L11 VLIW

very long instruction word

ipc= =1,执行一条指令，做很多工作

不用寻找后边的独立指令

硬件开销比超标量小（不用reorder等等）

非常依赖于编译器

指令独立性不好，代码的size比普通的大。（空操作填补long instr）

优点：编译器做复杂工作，硬件简单，可以高能效，在循环和常规代码（数组，矩阵操作）中表现很好

缺点：latency不相同，应用的非常规代码很多（指针操作），代码膨胀

VLIW指令特点：

有普通指令的操作码，支持预测（因为编译器调度过指令），寄存器很多（指令调度需要的），分支暗示（branch hints）,指令压缩DSP

DSP处理器常用VLIW：浮点运算，循环

VLIW很受cachemiss影响（等待不同操作数）



### L12 Cache Review

局部性：刚刚访问了x地址，接下来很有可能再次访问x地址，或者x附近的地址。

cache：内存的小且快的备份

平均内存访问时间：=命中延时（小且快）+（大或聪明）缺失率*缺失惩罚

现代处理器里，L1cache大概16-64kb

小的cachehit time 1-3cycle

cache line/block size：cache 操作的最小单位，每个cache tag对应的数据有多少byte，一般32-128byte（根据程序的空间局部性）

cacheline size：太小不能充分利用空间局部性，太大缺失惩罚高且浪费cache空间。

cacheline开始地址对齐，所以每个数据副本只会在一个cacheline里出现。

134

全相联/直接映射/N路组关联

写策略：

写分配，写不分配

写穿，写回

替换策略：随机，fifo，lru

lru实现：可替换的block（n）个lru计数器，每次L/W,刷新lru计数器们

cache结构：有效位+脏位+tag+data



## part4

### L13 Virtual Memory

为什么要有vm？：方便程序员视角，让每个程序以为自己占有所有内存，方便多程序共同运行。

将vm映射到物理内存pm：页式管理，一个页大小一般为4k，每个进程维护一个页表，存储页帧（vm视角）到页框（pm视角）的映射。

没有被映射到内存的程序页帧在硬盘disk（缺页中断加载进内存）

一级页表：4g/4k *4B=4Mb，每个进程维护4m的页表？太大，多级页表

一条vm地址：outer pagetable num|inner pagetable num| page offset，理论是页表由连续的地址作为索引，所以一级页表系统所有entry都要加载到内存。多级页表，只有最顶层的页表需要完全加载进内存，其他按需加载进内存即可，大大节省了进程的页表在内存中占用的空间。时间换空间。

page size的确定：kb-mb，x86是4kb，随着物理内存越来越大，pagesize也有增大趋势，因为不太在乎内存碎片的浪费了。

page size大的话，页表的size就小了，而且缺页中断频率也降低。但是，内存操作粒度变大，内部碎片增多。

页表放在内存中，地址虚实转换需要访存？--》tlb解决，translation look aside buffer

TLB：页表的cache，

（为什么不直接用cache，

因为tlb所需的size小，单独设计，索引更快；

cache会记录下一层页表(多级页表结构)的地址，tlb直接记录最终物理地址

）

当tlb miss了，需要执行虚实转换并更新tlb，谁来做？

1.硬件，mmu来做，对软件屏蔽细节，更快，硬件成本更高，高性能使用

2.软件，os来做，硬件只提供tlb速度慢但是硬件成本低，常被嵌入式core采用（嵌入式场景代码不多变，tlbmiss也会较低）

tlb：常用全相联，因为小

size？一般64-512entry

想要更高命中率？多层tlb-几千entry

tlb每次miss装载？



### L14 Advanced Caches

通过（平均内存访问时间）AMAT优化：AMAT=命中延迟 + 缺失率 x 缺失惩罚



#### 1.减少hit time

：cache size减小（影响缺失率），减少组关联（影响缺失率）

其他方法：

1.**overlap cachehit** 与cache hit

overlap cachehit 与tlb hit

优化lookup 对于常规情况

替换状态的刷新更快



1.overlap cachehit 与cache hit

**piplined cache**，

41

如果cache 访问大于一个cycle，将访问分成不同的stage进行流水线处理。比如寻组和tag比较作为stage1，数据读出作为stage2，这样可以减少cache读取指令的等待时间，减少hit time，l1cache常为1-3cycle，可以流水线化。

cache的hit 的latency还要包括 tlb取到物理地址的时间。

纯虚地址的cache:

也要查tlb获得是否可以读写的权限信息,进程切换需要flush所有的cache

VIPT,virtual index physically tag:

如果cache的index bit侵占了页表号，就会有别名（多副本且不同步）的问题可能出现。cache比较小（index+offset位数小）就可以避免别名

发生了别名处理方法：每次检查是否有其他别名存在，刷新/invaild 之

cache size <= 关联度 *page size 现实处理器基本都取等号



关联度高：冲突减少，缺失率下降，vipt的cachesize可以做大，但是hit time上升。

弥补手法：

**way prediction**，路预测，预测手法，mru，高位某些位亦或做映射，或者最简单的，预设优先级

way prediction 可以减少hit time更可以减少功耗（不用每次都预取所有tag做比较）



**替换策略对hit time**也有影响，random无影响，lru需要检查，刷新，lru计数器，hit time增大，且功耗增大。

解决方法：

lru：每个cacheline 都有log2n位的lru计数器

NMRU:not most resently used,只记录最近使用的block，每个set用log2N位的指针指向，其他的其他的NMRU 随机选择一个替换

PLRU:pseudo LRU,伪LRU。每个cacheline 有1bit的flag位，初始化为0，访问过后将0置为1，表示最近使用过。替换时在flag位为0的line中随机选择。当set中只剩1个block的flag为0时，等效于LRU，此时要替换的话，将其置为1，并将其他block的flag置为0，退化为NMRU。

所以PLRU处于LRU和NMRU之间。



#### 2.减少miss rate

为什么会miss？

三种缺失：强制缺失（冷缺失），容量缺失，冲突缺失



几种不影响hit time的方法：

**1.增加block size**：在大的cache中，适当增加blocksize可以减小缺失率，但是在空间局部性不好的程序中，增大blocksize会取很多junk数据进cache，反而missrate增加。

在空间局部性好的程序里，增大block size可以减少三种缺失。

**2.预取** prefetching

预测哪个cache block会被访问，在访问之前将其从内存取到cache

错误的预测带来cache污染

预取方法：在程序中加入预取指令，预取的时机不能太早也不能太晚，而且与硬件强相关(处理器/内存速度)

gcc有 build-in-prefetch函数

prefetch ，miss penalty/ hit latency 个元素。

硬件预取：68

stream buffer prefetcher:顺序的，取相邻的block

stride prefetcher，跨步，根据访存的固定步长来预测预取

correlating prefetcher：一张table记录访存次序的关系，a-》b，下次访问a就会预取b

**3.loop interchange** 循环交换

编译器的工作，比如，两重循环初始化矩阵，按列初始化。局部性很差。编译器可探测到这种情况，将两重循环交换，达到按行初始化的效果。

**4.改善冲突失效** VictimCache

cache和它从下一级存储器调用数据的通路之间设置一个全相联的小Cache，用于存在被替换出去的块（Victim），以备重用





#### 3.减少miss penalty



**1.overlap misses**

阻塞性cache：一次cache miss 将数据取回前，其他的cache miss会被block（从内存取数据，只能串行的访问memory。

**非阻塞型cache**：一次cache miss正在处理，其他的cache miss 也会被并行处理。可以并行的访问memory。

（hit under miss /miss under miss）

方法：mshr，miss status holding register

mshr记录访存指令的pc。

cache miss后检查mshr有无匹配，无匹配则认为是新的block miss，分配一个 reg。匹配则合并处理，将pc记录与之前的mshr上。

mshr多大比较好？看硬件支持度，越大越好，可以并行处理的访存。



mpki指标， miss per kilo instruction



cache hier 的inclusion/exclusion策略：在l1cache的必 在/不在 l2cache中。其他hier同理。

 不特意施加inclusion的话，l1的block可能在 /可能不在l2中：因为l1的hit不会刷新l2的lru计数器，最终导致在l1中的块被替换出去。

施加inclusion：l2中增加inclusion bit，标记l1中是否有此block，为1的话不替换他。



### L15 Memory 

sram和dram：

ram：随机访问存储器，可以O1复杂度访问

S和D：s，开启电源则一直保存，D，开启电源且一直refresh才可保持数据。

sram结构，6t，bl，blb，两个bitline

dram，1t1c，靠电容放电检测状态（打开wl，放电，sence，recharge）。1t1c? no,trench cell

dram 结构：row decoder，bitline vcc/2，敏感放大器，row buffer，column decoder，recharge

刷新：刷新周期内每一行都被读一遍

刷新计数器，refresh counter

写过程:写到row buffer 再recharge

fast page mode：读的时候先查 row buffer 是不是和上一次是同一行，（每一行有几千bits，是的话直接读写rowbuffer。更换row之前写回就可以。（空间局部性的重要性）

cpu-memory controller-dram



### L16 storage

支持虚拟内存

指标，吞吐量和延迟（延迟巨大）

可靠性

磁盘，硬盘:柱面，扇区，磁道，磁头

访问时间=寻道时间（最重要）+延迟时间（旋转到指定扇区）+传输时间

光盘与硬盘区别：不用磁头，用激光，不怕灰尘

磁带：大容量，顺序读取，作为backup

ssd--》flash（比dram慢）

可以似乎用flash 作为 disk的cache

连接和io：sata（磁盘），pcie（显卡），usb（外设）



## part5

### L17  Fault TOL

可靠性，可用性，错误容忍

fault：潜在的错误（某些情况下出错）

error：函数出错

failure：导致系统出错

可靠性的量化：服务提供，服务中断

mttf： mean time to failure

mttr：  ~ repair

可用性：可用的时间占总时间的比例

fault 种类：hw/design/operation/environment/

falut种类:永久/断断续续的/短暂的

增加可靠性：减少发生/增加容忍度eg：ecc/加速修复



**缺陷容忍技术**：

checkpointing：暂停，保存状态，检查，恢复状态

2路冗余：两个模块做相同的工作，比较结果，不同则回滚

3路冗余可以检错和恢复

N路冗余：2/3/5



内存检错:奇偶校验/ECC（一位纠正，两位检错）

RAID:冗余 阵列 独立 硬盘

raid用来解决更大粒度的错误，硬盘fail，扇区错误之类，每个硬盘有纠错码解决bit的错误。

raid0，无冗余，增加性能：增加吞吐量，不同扇区并行访问。将同一个文件分散到不同硬盘中。

raid1：两份copy，增加吞吐量，可以纠错检错1个disk

raid4：三块disk使用raid0的用法，另外一块存其他三块的奇偶校验位。读性能好，写性能差（更新校验位）

dram奇偶校验位的blk与数据blk分开放：为了通用性，和保护更大粒度。

保护位离得越远，就可以避免越大的错误。

raid5：奇偶校验位循环分布在4个disk中，数据也是循环分布在4个diak中。

raid6：两个check blk，可以检查两个disk的错误.RAID6同RAID5最大的区别就是在RAID5的基础上除了具有P校验位以外,还加入了第2个校验位Q位。P,亦或，Q，Galois Field变换。

当两块磁盘上的数据出现错误或者丢失的时候,恢复方法为:利用上边给出的P,Q的生成公式,联立方程组,无论受损的数据是否包括P或者Q,总是能够解出损失的两位的数据。





### L18 Multi Processing

弗林分类法：

sisd，simd（向量处理器），misd（少见），mimd（多核）

为什么不单核：1.单核超标量带来的收益边际递减到4宽度左右，2.为提升频率，增大电压，功耗上升

多线程编程和debug更难！

perf增长随core线性增长很难



集中共享内存的问题:mem要大，所以mem较慢；miss变多(核多了)，带宽要大（带宽很影响多核性能）。所以只适用于少核，2，4，8，16

uma框架：均匀内存访问时间

smp：对称多核处理器

uma/smp/mp一个概念



分布内存系统，每个core只能访问自己的mem，需要其他core的数据用network 通信 --》cluster（强迫程序员考虑多核通信）

numa（分布式）内存分配：os考虑数据的分配，每个core对不同数据的使用率，，



smp系统:线程同步，加锁解锁，内存barrier，



uma/numa两者的不同:通信（hw/sw），数据分布（手动/自动），hw支持（复杂/简单），编程难易，性能好坏（）；

时间分享单核的多线程

smt：同时多线程（单核）

smt需要的硬件:多个pc，rat，reg

多线程的cache：歧义，重命名

tlb总是需要thread-aware：每个entry加bit记录thread

smt与cache关系：有好有坏，数据依赖比较多，cachehit节省时间，数据相关少，对于每个线程cache容量小了很多，miss增加。



### L19 Coherence

一致性定义：

1.读入一个地址，值应该是最近被写入此地址（所有核）的值

2.多核向同一地址写需要序列化：所有核看到的都是同一写入顺序

获得一致性：

no chche，shared l1cache

better:写更新/写失效 + snoppying/directory

1.写更新+snoppying：写，广播，更新

优化1：dirty bit，write back，减少mem读写，但是也要监听读信号（cache里是最新副本

优化2：增加shared bit，记录有没有其他core有备份，如果没有则write不用在总线上广播。

2.写失效+snoppying：写，广播，失效+nonshare

write back 需要配合读监听

（写更新的优势场景，一个core一直wr，一个core一直读 相同地址）

写更新写失效各有优劣，现代处理器都用写失效：因为进程切换到另外的core上时，我们不想维护其他core上cache的老数据。

基于监听的经典**mesi协议：**

local read -->E/S,

local write --》M

remote read --》S

remote write --》I



- M：Modified，该Cacheline有效，数据被修改了，和内存中的数据不一致，数据只存在于本Cache中；

  local read / local write --》M

  remote read --》S

  remote write --》I

- E：Exclusive，该Cacheline有效，数据和内存中一致，数据只存在于本Cache中；

  local read -->E,local write-->M

  remote read --》S

  remote write --》I

- S：Shared，该Cacheline有效，数据和内存中一致，数据被多个Cache共享；

  local read -->S, local write-->M

  remote read --》S

  remote write --》I

- I：Invalid，该Cacheline无效；

  local read -->E/S, 

  local write-->M

  remote read --》S

  remote write --》I

- o：owned，结合s和m，O(Owned)是MESI中S和M的一个合体，表示本Cache line被修改，和内存中的数据不一致，不过其它的核可以有这份数据的拷贝，状态为S。

![moesi](C:/Users/shengkui/OneDrive - Synopsys, Inc/Pictures/moesi.PNG)



directory based:

多核8-16以上，总线既负责传输数据又要维护一致性，总线拥挤，成为瓶颈。

基于目录的一致性协议适合numa系统。

中央控制器有一个目录，每个block分配一个entry，记录其uncached，shared，exclusive

entry记录了这个block被哪几个core拥有，消息只在这几个core中间流动，而不是像广播一样通知所有core。

对不同blk的访问可以并行处理



一致性缺失：真共享/假共享



### L20 Synchronization

critical session

同步：lock/mutex

原子读写指令--》硬件支持

原子指令：原子交换（能效低，浪费带宽），test and write（影响流水线，需要把mem阶段调长）

load linked /store conditional：ll/sc

load linked：ld --》link reg

store condition： 检查 link reg（标志位记录是否被写过），然后写入/fail，返回0/1

barrier sync：global wait，等待所有线程执行到那里

实现：计数器（计算多少县城到达）和flag（线程==n） （计数器检查放在critical session）



lamport面包店算法:

- 面包店一次只能接待一位顾客的采购。
- 已知有n位顾客要进入面包店采购，按照次序安排他们在前台登记一个签到号码。该签到号码逐次增加1。
- 顾客根据签到号码的由小到大的顺序依次入店购货。
- 完成购买的顾客在前台把其签到号码归0。如果完成购买的顾客要再次进店购买，就必须重新排队

算法规定如果两个线程的排队签到号码相等，则线程id号较小的具有优先权。



### L21 Consistency

**Coherence模型**主要考虑对于同一内存位置的写操作对于所有的处理器的可见性。

*存储的**Consistency模型**的范围则广得多。和**Coherence**相比，其主要差别有：**1.Consistency**不仅针对同一内存区域的访问；**2.Consistency**中的内存访问包括读和写两种。*

最理想的情况就是所有指令的执行顺序和程序里写的一模一样，这样当然不会违反内存的Consistency

目前，为了最大程度地优化程序，一些平台放弃了所有的内存**Consistency**约束，而将保证多线程程序正确性的工作转移给程序员。

所以我们才会在这么多项目（如**linux kernel**）中看到这么多**memory barrier**语句。



Consistency：定义对不同位置的访问顺序

乱序+多线程影响consistency

Sequential consistency：l/s inorder，之前所有accsess 完成后再进行access

改进：单核内可以乱序，根据rob的顺序，如果逆序且有其他core使用此数据，则replay该load



relaxed consistency：

四种访问顺序：

write a --》 write b

w a --》  read b

r a --> r b

r a --> w b

Sequential需要保持四种都遵守程序顺序



relaxed：可以不遵守四种的某种

eg程序员不要期待读的顺序是和程序写的一致

x86： msync指令（不会被reorder）

查询 - msync -释放



data race free program



同步，使乱序程序执行结果像顺序结果一样。



### L22 Many Cores

core 增加 -》**conhenrency traffic 增加** --》 bus 瓶颈 --》基于目录的

net：

mesh：格点，每个节点和上下左右相连。core增加，link增加，吞吐量增加

torus mesh：带环的mesh



core 增加 -》off chip traffic 增加/引脚数增加不快 --》瓶颈 --》降低内存访问，LLC --》分布式LLC



llc逻辑上是一个cache，物理上分成不同的slice分布在各个core旁边

怎么找llc的slice？1.根据core id直接index 2.根据页号分配，空间局部性更高



core增加:**一致性目录太大**:

目录也是分布式的和slice llc一样

目录不用记录所有blk，只记录在私有cache中出现的blk

如果目录满了就替换/lru

替换之前需要将被替换的entry有此blk的cache设置为无效--》造成一种cache缺失



core增加：**功耗电源限制**，每个core的电压降低，单核性能下降

多核时正常功率，单核时turbo，增加频率。但是有功耗墙，芯片不能过热。



每个core可以有smt技术，同时多进程

**os**需要意识到不同的并行性，比如 chip级别，core级别，smt级别，充分利用cache资源





