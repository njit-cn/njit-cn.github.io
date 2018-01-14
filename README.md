# 计算机组成复习

## 计算机系统概述

1. CPI
    - CPI, Cycles Per Instruction, 单指令周期
    - 运行时间 = 指令条数 * CPI

1. XX位机, XX 是指什么位数
    - 数据通路宽度 (数据总线一次能并行传送的位数)

1. MAR, MDR, PC 的作用和位数
    - `MAR`, `M`emory `A`ddress `R`egister, 内存地址寄存器 - 存储地址
    - `MDR`, `M`emory `D`ata `R`egister, 内存数据寄存器 - 存储数据
    - `PC`, `P`rogram `C`ounter, 程序计数器

1. 冯诺依曼机的基本工作方式
    - 总特征: 按地址访问并顺序执行指令(串行顺序处理)
    - 运行顺序: 编制程序 -> 将程序(包含指令和数据)存入主存 -> 按指令序列读取并执行程序

1. 设计高性能计算机的主要途径
    - 并行处理 (采用非冯诺依曼结构)

1. 五大部件
    - 运算器
        - 算数逻辑运算单元
        - 寄存器
    - 控制器, Control Unit - 指令译码, 产生一系列控制信号, 指挥协调各部分工作
        - 程序计数器, PC, Program Counter, 提供下一条要执行的指令的地址 (一般为自增操作)
        - 指令寄存器, IR, Instruction Register, 存放当前从主存中取出的指令 (用于分析操作性质及操作数所在的地址)
        - 指令译码器, ID, Instruction , , 对 IR 中的操作码进行译码
        - 地址形成部件, 根据指令的寻址方式, 形成有效地址
        - 时序部件, 产生机器中各种时序信号, 对各操作实施时间上的控制
            - 时钟脉冲发生器, 机器的主频信号, 为机器提供时间基准 (是一个外接高进度晶振)
            - 起停线路, 用于开放和封锁脉冲, 控制时序信号的发生和停止
            - 节拍信号发生器, 分为节拍电位和节拍脉冲, 多用于控制数据通路中代码的传送或数据的运算
        - 中断控制逻辑, 控制中断处理的硬件逻辑
    - 存储器
        - 主存储器(内存), Memory
        - 辅助存储器(外存), Storage
    - 输入设备
    - 输出设备

## 数据的表示和运算

1. ALU(算数逻辑运算单元)的作用
    - 按照算数规则进行运算 (`+`, `-`, `*`, `/`, `|x|`, `-x`)
    - 实现比较, 移位, 与, 或, 非, 异或等运算

1. Booth 算法 (补码一位乘)
    - 核心思想
        - 不区分两乘数的正负
        - 使用补码表示两个乘数
        - 运算中符号位与数值为一同进行乘法运算
        - 得出补码表示的结果 -> 得出结果的原码
    - 计算步骤 如 X*Y, 其中 Y 有 N 位, P 为部分积, 即乘积高位
        - 计算出 [X]补, [Y]补, [-X]补
        - [Y]补 末尾添 0
        - 循环计算
            - Y(末位值 - 前一位值)
                - = 0 => P + 0
                - = 1 => P + [X]补
                - = -1 => P + [-X]补
            - Y, P 右移一位 (第 N 次时, 计算完成不移动)

1. IEEE754 单精度浮点表示范围
    - 结构
        - 符号位 S, 1 bit
        - 指数位 Exp, 8 bits
        - 尾数域 Fraction, 23 bits

1. 原码加减交替法 (除法)
    - 计算步骤 如 X/Y, 其中 X 有 N 位, 其中 R 为余数, Q 为商, 有 N+1 位
        - R = X, Q = 0
        - R = R - Y
        - 循环计算
            - R > 0 => Q 末尾 +1, R = R - Y
            - R < 0 => Q 末尾 +0, R = R + Y
            - 左移一位 (第 N+1 次计算完成)

1. 小端, 大端模式 (如 uint16 a = 129 [0x8001]十六进制)
    - 小端 -> 低位字节排放在内存的低地址端 a == 0x80 0x01
    - 大端 -> 高位字节排放在内存的低地址端 a == 0x01 0x80

1. 计算机中原码加减法运算的实现
1. 数值数据的表示
1. 已知一个数的补码, 求其相反数的补码

## 存储系统

1. Cache 的命中率

1. Cache 的替换策略
    - FIFO 先进先出
    - LRU 最近最少使用

1. Cache 的映射
    - 直接映像 `j = i mod 2^c` (其中 j 为 Cache 块号, i 为 内存块号, c 为 Cache 块数)
        - 规定主存中的某一块只能映射到 Cache 中的一个特定块中
    - 全相联映像
        - 规定内存的每一块可映像到 Cache 中的任一块
        - 特点
            - Cache 标记与映射的主存块号对应
    - 组相联映像 `j = (i mod 2^c) * 2^r + k` (其中 j 为 Cache 块号, i 为 内存块号, c 为 Cache 块数, r 为)
        - 将主存和 Cache 均分组, 且主存中的一个组内的块数和 Cache 中的分组数相同

1. DRAM 的刷新
    - 为什么要刷新
        - 为了保持存储信息的正确性, 必须反复地在放掉电荷之前再通以新的电流进行充电, 以恢复原来的电荷
    - 刷新方式
        - 集中刷新
            - 在运行的最大刷新间隔 (一般是 2ms) 内安排多个刷新周期, 刷新周期数等于组成存储器中容量最大的一个 DRAM 芯片的存储矩阵的行数
            - 优点
                - 系统的存取周期不受刷新工作的影响, 读写操作和刷新操作是分段进行的
                - 控制简单, 存取速度高
            - 缺点
                - 刷新期间不能读写
        - 分散刷新
            - 将每个存取周期分为 读写/保持 和 刷新 (刷新周期分散安排在每个读/写周期之后)
            - 优点
                - 控制简单
                - 主存没有长的死区
            - 缺点
                - 主存利用率低, 工作速度降一倍
        - 异步刷新
            - 按 DRAM 芯片行数决定刷新周期数, 并将各个刷新周期分散安排在 2ms 的最大刷新周期内
            - 优点 (结合 集中 和 分散)

1. DRAM 和 SRAM 的特点
    - DRAM
        - 断电丢失
        - 需要定时刷新
        - 破坏性读出
        - 集成度高
        - 功耗低, 成本低, 速度相对 SRAM 慢
    - SRAM
        - 断电丢失
        - 无需刷新
        - 非破坏性读出

1. 存储器芯片容量和引脚的关系

1. 三级存储体系
    - 高速缓冲存储器 (Cache)
    - 主存储器 (Memory)
    - 辅助存储器

1. 存取周期, 时钟周期, CPU周期, 指令周期
    - 存取周期

## 指令系统

1. RISC/CISC 的比较
    - RISC
        - 特点
            - 优先选用使用频率高以及有用但不复杂的指令, 避免复杂指令
            - 减少指令系统的寻址方式, 简化指令格式, 固定指令长度
            - 增加CPU中通用寄存器数目, 减少访存操作, 只有取数, 存数指令才能访问存储器
            - 大部分指令在一个或小于一个机器周期内完成
            - 硬布线为主, 少用或不用微程序控制
            - 通过精简指令以及优化设计编译程序, 以简单有效的方式支持高级语言的实现
        - 好处
            - 简化系统设计, 适合大规模集成电路实现
            - 提高机器的执行速度和运行效率
            - 降低成本, 提高系统可靠性
            - 可以提供直接支持高级语言的能力, 简化编译程序的设计
    - CISC
        - 特点
            - 指令系统多而复杂 => 可靠性降低, 成本高, 错误增多, 设计周期延长
            - 指令操作复杂, 执行速度降低
            - 高级语言的优化编译困难, 编译开销增加
            - 部分指令利用率很低

1. 扩展操作码技术

1. 寻址方式
    - 立即寻址
        - 操作数由指令的地址码部分直接给出
    - 直接寻址 `E = A`
        - 指令的地址码部分 直接给出操作数在存储器中的地址 `A`
    - 间接寻址 `E = (A)`
        - 指令的地址码部分 给出一个指示操作数有效地址的地址指示字 `A`, 通过该指示字找到操作数的有效地址 `(A)`, 再找到操作数 (可以扩大寻址范围, 以短地址码访问较大的空间)
    - 寄存器寻址 `E = R`
        - 指令的地址码部分 给出某一通用寄存器的地址 `R`
    - 寄存器间接寻址 `E = (R)`
        - 指令的地址码部分 指定的寄存器中的内容是操作数的有效地址
    - 相对寻址 `E = (PC) + DISP`
        - 将程序计数器 PC 当前的内容 `(PC)` 加上指令给出的形式地址 `DISP` (操作数地址与该指令地址的相对位置) 而形成操作数的有效地址
    - 变址寻址 `E = (Rx) + A`
        - 指令的地址码部分 给出的形式地址 `A` 与指令中指定的变址寄存器的内容 `(Rx)` 相加形成操作数的有效地址
    - 基址寻址 `E = (Rb) + A`
        - 指令的地址码部分 给出的形式地址 `A` 与基址寄存器中的内容 `(Rb)` 相加形成操作数的有效地址
        - 主要用于将用户编写程序时使用的地址转换为程序在运行时的实际地址
        - 适用场合
            - 解决程序在存储器中的定位问题
            - 扩大寻址空间
    - 复合寻址
        - 多种寻址方式结合

1. 指令, 指令系统, 为什么要引入指令系统
    - 指令
        - 是计算机执行某种操作的命令
        - 四要素
            - 操作码 - 反应操作的性质及功能
            - 操作数地址 - 指明操作数的来源
            - 操作结果的存储地址 - 反应操作结果的流向
            - 下一条指令的地址 - 反应指令的执行顺序
        - 格式 `| 操作码 OP | 地址码 |`
    - 指令系统
        - 是指一台计算机中所有的机器指令的集合
    - 为什么要引入指令系统
        - 为了适应硬件功能的发展, 指令的日益丰富
        - 缩小指令与高级编程语言之间的语意差异
        - 有利于操作系统的优化

1. 指令的格式 (单地址, 单字长, 固定长度, 变长)
    - 单地址指令
        - 只有一个地址域
        - 固定使用某个寄存器存放第二操作数和操作结果
    - 单字长指令
        - 指令字长度等于机器字长度(同 xx位机中的 xx)的指令
    - 固定长度编码
        - 操作码长度固定且位置固定
    - 可变长度编码
        - 操作码长度不固定, 且分散在指令字的不同字段中

## 中央处理器

1. 存储器中的数据和指令如何区分
    - 按时间段区分: 在取指周期取出的即为指令, 在执行周期取出的即为数据
    - 按来源区分: 在PC指出的地址中取出的即为指令, 指令由操作码和地址码组成, 而指令的地址码所指即为数据

1. 控制器的组成
    - 程序计数器, PC, Program Counter, 提供下一条要执行的指令的地址 (一般为自增操作)
    - 指令寄存器, IR, Instruction Register, 存放当前从主存中取出的指令 (用于分析操作性质及操作数所在的地址)
    - 指令译码器, ID, Instruction , , 对 IR 中的操作码进行译码
    - 地址形成部件, 根据指令的寻址方式, 形成有效地址
    - 时序部件, 产生机器中各种时序信号, 对各操作实施时间上的控制
            - 时钟脉冲发生器, 机器的主频信号, 为机器提供时间基准 (是一个外接高进度晶振)
            - 起停线路, 用于开放和封锁脉冲, 控制时序信号的发生和停止
            - 节拍信号发生器, 分为节拍电位和节拍脉冲, 多用于控制数据通路中代码的传送或数据的运算
    - 中断控制逻辑, 控制中断处理的硬件逻辑

1. 设计数据通路

1. 微操作命令和节拍安排

1. 微程序控制器(微程序, 微指令, 微操作, 微命令)
    - 微命令
        - 控制部件通过控制线向执行部件发出的各种控制命令 (他们构成控制序列的最小单位)
    - 微操作
        - 执行部件接受微命令之后进行的操作称为微操作
    - 微指令
        - 在一个微周期中, 一组实现一定操作功能的微命令的组合
    - 微程序
        - 许多微指令组成的序列

1. 微控制存储器
    - 是微程序控制器的核心, 用于存放微程序
    - 容量和微指令的条数有关

1. 微指令, 和指令的关系
    - 一条机器指令的功能是由许多微指令组成的序列实现的

1. 微指令的格式(水平, 垂直)
    - 水平微指令格式
        - 一次能定义并执行多个并行微操作的微指令
        - 特点
            - 微指令字较长
            - 微指令中的微操作有高度的并行性 (一个微周期中, 一次能并行执行多个微指令)
            - 微指令译码简单, 一般采用直接控制和字段直接编码方式
        - 优点
            - 并行操作能力强, 效率高
            - 微程序较短, 执行速度快
            - 控制存储器的纵向容量小, 灵活性强
        - 缺点
            - 指令字比较长, 增加了控制存储器的横向容量
            - 与机器指令差别较大
            - 不易实现设计的自动化
    - 垂直微指令格式
        - 微指令中设置有微操作码的字段, 由微操作码指定微指令的功能
        - 特点
            - 微指令字短 (10 - 20 bit)
            - 并行微操作能力有限, 一个微指令一般只包含一个微操作
            - 微指令译码比较复杂 (通过对微操作码译码产生微命令)

1. 硬布线控制器的设计步骤
    - 设计时序系统 - 根据各类指令的需要, 设置机器周期, 节拍(时钟周期) 和 工作脉冲
    - 拟定指令流程 - 确定指令执行的基本步骤
        - 以指令为线索 - 按指令类型分类, 将每条指令归纳为若干微操作, 然后按操作的先后顺序画出指令流程图
        - 以机器周期为线索 - 按机器周期拟定各条指令在本周期内的操作流程
    - 列出微操作时间表 - 将各个微操作合理安排到各个机器周期的相应节拍和脉冲中去
    - 进行微操作信号的综合 - 对微操作进行综合分析
    - 实现电路
        - 用逻辑门电路组合实现
        - 用 PLA, PAL, GAL 实现
        - 用专门芯片连接实现 (半定制)
        - 即成为 CPU (全定制)

1. 指令流水线
    - 将每条指令分解为多个子过程, 并让各子过程操作重叠, 从而实现多条指令并行处理的技术
    - 程序中的指令任然是顺序执行, 但是可以预取若干条指令, 并在当前指令尚未执行完成, 提前启动后续指令的另一些操作子过程

1. 指令流水线的优势, 加速比
    - 优势
    - 加速比 (其中 S 表示加速比, m 表示子过程个数, n 表示完成任务个数)
        - S = m / [1 + (m - 1)/n]
        - 当 n 很大时 S = m => 理想情况下流水线处理机的加速性比等于流水线包含的子过程数

1. 指令译码的对象
    - 将存放在 IR (指令寄存器) 中的操作码翻译成一系列控制电位