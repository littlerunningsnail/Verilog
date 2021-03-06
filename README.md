# Verilog
绪论
1.	数字逻辑系统的基本单元是门、或门和非门，它们是由三极管、二极管和电阻等器件构成，并能执行相应的开关逻辑操作
2.	与门、或门和非门又可以构成各种触发器，实现状态记忆。
3.	为什么要设计专用的复杂数字系统？
1)	在实际应用中，有的数字信号处理对时间的要求非常苛刻，以至于用高速的通用微处理器芯片也无法在规定的时间内完成必要的运算。因此要设计一个专用的高速硬线逻辑电路，在高速FPGA器件上实现或制成高速专用集成电路。
2)	通用微处理器芯片是为一般目的而设计的，运算的步骤必须通过程序编译后生成的机器码指令加载到存储器，然后在微处理器芯片控制下，按时钟节拍，逐条取指令、分析指令和执行指令，直到结束。
3)	由于通用位处理器芯片中内部总线和运算部件为通用目的而设计，导致它不可能为某一个特殊算法设计一系列的专用运算电路，并且其内部总线宽度不能随意改变，只能通过修改程序，才能实现特殊算法，因此速度受限。
4.	数字信号处理系统DSP
1)	功能：实现复杂的数学运算和数据处理，并有实时响应的要求
2)	组成：由高速数据通道接口和高速算法电路
3)	设计流程：一个复杂的数字系统设计往往是一个从算法到硬线连接的门级逻辑结构，再映射到硅片的逐步实现过程
5.	设计复杂数字系统电路的四种方案
1)	以专用微处理机芯片为中心来完成算法所需的电路系统
2)	用高密度的FPGA(从几万门到几百万门)
3)	设计专用的大规模集成电路(ASIC)
4)	利用现成的微处理机的IP核并结合专门设计的高速ASIC运算电路
6.	专用硬线逻辑与微处理器比较
1)	以专用微处理器芯片为中心来完成算法所需的电路系统，利用现成的微处理开发系统，C语言验证算法的基础，设计周期短，可利用资源多；但是速度、能耗、体积等性能受微处理器芯片和外围电路限制
2)	高密度FPGA方案必须购置有关的FPGA开发环境、布局布线和编程工具，性能不如ASIC电路
3)	现有IP核搭载高速ASIC电路，综合了大规模集成电路和微处理的优点，但开发周期长，成本高，还需要对半导体厂家基本器件库和IP库的深入了解
7.	电路结构完全确定之前的多次仿真
1)	C语言的功能仿真
2)	C语言的并行结构仿真
3)	Verilog HDL的行为仿真
4)	Verilog HDL的RTL级仿真
5)	综合后门级结构仿真
6)	布局布线后仿真
7)	电路实现验证
8.	C语言与Verilog HDL配合使用的理由
1)	C语言灵活，查错能力强，可以通过PLI(编程语言接口)编写自己的系统任务，并直接与赢硬件仿真器结合使用
2)	C语言设计环境更为完整，编译环境可靠，语法完备，缺陷较少，可应用于多种领域，而Verilog的大部分软件都是商业软件，与C语言缺少使用，可靠性差
9.	C语言与Verilog HDL配合的困难
1)	C语法较多，如指针、不定次数的循环，而Verilog语法较少，相互转换有困难
2)	C程序是顺序执行，而Verilog是并行执行
3)	C语言输入输出函数多样，而Verilog较少
4)	C语言没有时间关系，转换后的Verilog程序必须做到没有任何外加的人工延时信号。
10.	课后问题
1)	什么是信号处理电路?它通常由哪两大部分组成？
信号处理电路：是进行一些复杂的数字运算和数据处理，并且又有实时响应要求的电路。它通常由高速数据通道接口和高速算法电路组成
2)	为什么要设计专用信号处理电路？  // 3
3)	为什么要用硬件描述语言来设计复杂的算法逻辑电路？
因为现代复杂数字逻辑系统的设计都是借助于EDA工具完成的，无论电路系统的仿真和综合都需要掌握硬件描述语言。


第一章	Verilog的基础知识
1.	系统设计工作分解
1)	逻辑设计(前端)
2)	电路实现(后端)
3)	验证
2.	硬件描述语言HDL
1)	HDL是一种用形式化方法来描述数字电路和系统的语言
2)	数字电路系统设计流程思路
①	用一系列分层次的模块来表示极其复杂的数字系统
②	利用EDA工具逐层进行仿真验证
③	把其中需要变为具体物理电路的模块组合经由自动综合工具转换到门级电路网表
④	再用ASIC或FPGA自动布局布线工具把网表转换为具体电路布线结构
⑤	用verilog的门级模型来替代具体基本原件
3)	HDL 用于数字电子系统设计，进行各种级别的逻辑设计，进行数字逻辑系统的仿真验证、时序分析、逻辑综合
4)	Verilog 适合系统级、算法级、寄存器传输级(RTL)、逻辑级、门级、电路开关级设计
5)	软核、固核和硬核的优缺点
①	在具体实现手段和工艺技术尚未确定的逻辑设计阶段，软核具有最大的灵活性，容易借助EDA综合工具与其他外部逻辑结合为一体
②	固核和硬核与其他外部逻辑结合为一体的灵活性差得多
6)	自顶向下的设计概念：从系统级开始，把系统划分为基本单元，然后再把每个基本单元划分为下一层次的基本单元，一直这样做下去，直到可以直接用EDA元件库中的基本元件来实现为止
7)	模块设计流程
①	设计开发  编写设计文档->综合到布局布线->电路生成这样一系列步骤
②	设计验证  各种仿真
3.	课后问题
1)	什么是硬件描述语言？它的主要作用是什么？
答：硬件描述语言是一种用形式化方式来描述数字电路和系统的语言。
作用：数字电路系统的设计者利用它可以从上层到下层逐步描述自己的设计思想，用一系列分层次的模块来表示极其复杂的数字系统
2)	目前世界上符合IEEE标准的硬件描述语言有哪两种？各有什么特点？
答：符合IEEE标准的有Verilog HDL 和 VHDL。
它们共同特点：①能够形式化地抽象表示电路的行为和结构；
②支持逻辑设计中层次与范围的描述；
③可借用高级语言的精巧结构来简化电路行为的描述；
④具有电路仿真与验证机制以保证设计的正确性
⑤ 支持电路描述由高层到底层的综合转换，硬件描述与实现工艺无关
		不同点：verilog HDL更容易学习掌握
3)	什么情况下需要采用硬件描述语言的设计方法？
答：在对逻辑电路及系统的设计的时间要求很短情况下
4)	硬件描述语言设计方法的优缺点？
答：
优点：与工艺无关，使得工程师在功能设计、逻辑验证阶段中不必过多考虑门级及工艺实现的具体细节，只需要利用系统设计时对芯片的要求，施加不同的约束条件，即可设计出实际电路。
缺点：需要相应的EDA工具，而EDA工具的稳定性有待提升。
5)	利用EDA工具结合硬件描述语言的设计方法和流程。
答：//2.6、2.7
6)	硬件描述语言可以用哪两种方式参与复杂数字电路的设计？
答：复杂电路设计和复杂数字电路仿真验证
7)	硬件描述语言设计的数字系统需要经过哪些步骤才能与具体的电路对应？
答：编写设计文件 -> 功能仿真 -> 优化 ->布局布线 ->布线后门级仿真
8)	为什么说用硬件描述语言设计的数字逻辑系统具有最大的灵活性并可以映射到任何工艺的电路上？
答：硬件描述语言的设计具有与工艺无关性。这使得工程师在功能设计、逻辑验证阶段可以不必过多考虑门级及工艺实现的具体细节，只需要利用系统设计时对芯片的要求，施加不同的约束条件，即可设计出实际电路
9)	Top_Down设计方法和硬件描述语言关系
答：Top_Down设计方法是首先从系统设计入手，从顶层进行功能划分和结构设计。系统的总仿真是顶层进行功能划分的总要环节，而该过程需要采用硬件描述语言的方法。
10)	System Verilog与Verilog有什么关系？适用于何种设计？
答：System Verilog是Verilog的扩展和延伸。Verilog适合系统级、算法级、寄存器级、逻辑级、门级和电路开关级设计，System Verilog更适合于可重用的可综合IP和可重用的验证，以及特大型基于IP的系统级设计和验证。

第二章	Verilog语法的基本概念
1.	Verilog HDL描述的电路设计就是该电路的Verilog HDL 模型，也成为模块。Verilog HDL既是一种行为描述的语言也是一种结构描述的语言。
2.	Verilog模型的不同级别的抽象
1)	行为级
①	 系统级：用语言提供的高级结构能够实现待设计模块的外部性能的模型
② 算法级：用语言提供的高级结构能够实现算法运行的模型
③RTL级：描述数据在寄存器之间的流动和处理、控制数据流动的模型，与逻辑电路有明确的对应关系
2)	门级：描述逻辑门以及逻辑门之间连接的模型
3)	开关级：描述器件中三极管和存储节点以及它们之间连接的模型
3.	Verilog模块的基本概念
1)	综合(synthesis)：通过计算机上的运行工具把一种Verilog HDL(a)模型通过某种中间形式自动转换为另外一种Verilog HDL(b)模型，通常b模型很容易与某种工艺的基本原件逐一对应起来。再通过布局布线工具自动转变为某种具体工艺的电路布线结构。
2)	实例化(实例引用)：引用现成元件或模块的方法
eg：
	moudule trist1(sout,sin,ena);
	output sout;
	input sin,ena;
		mytri tri_inst(.out(sout),.in(sin),.enable(ena));	//带”.”表示被引用模块的端口，名称必须与被引用模块mytri的端口定义一致，小括号中表示在本模块中与之连接的线路
	endmodule
3)	Verilog HDL程序是由模块构成的。每个模块的内容都是位于module和endmodule语句之间。
4.	verilog用于模块的测试
1)	verilog可用来描述变化的测试信号。
2)	描述测试信号的变化和测试过程的模块也叫做测试平台，它可以对上面介绍的电路模块进行动态的全面测试。通过观测被测试模块的输出信号是否符合要求，可以调试和验证逻辑系统的设计和结构正确与否。
5.	课后习题
1)	是否任意抽象的符合语法的verilog模块都可以通过综合工具转变为电路结构？
答：不能，要符合语法，还要符合一些基本规则的verilog模块才可以。
2)	什么叫综合？
答：通过综合工具把行为级描述的模块通过逻辑网表自动转换为门级形式的模块叫综合。综合使用EDA工具来完成的。
3)	通过综合产生的是什么？产生的结果又什么用处？
答：综合产生的是由与门、或门和非门组成的加法器、比较器等组合逻辑。产生的模块很容易与某种工艺的基本元件逐一对应起来，再通过布局布线工具自动的转变为某种工具工艺的电路布线结构。
4)	仿真是什么？为什么要进行仿真？
答：仿真时对电路模块进行动态的全面测试。通过观测被测试模块的输出信号是否符合要求可以调试和验证逻辑系统设计和结构准确与否，并发现问题及时修改。
5)	仿真可以在几个层面上进行？每个层面的仿真有什么意义？
答：仿真可以在前仿真、逻辑网表仿真、门级仿真和布线后仿真
前仿真、逻辑网表仿真和门级仿真可以调试和验证逻辑系统的设计和结构准确与否，并发现问题及时修改
布线后仿真：分析设计的电路模块运行是否正常。
6)	模块的端口如何描述？
答：用”.”表示被引用模块的端口
7)	在引用实例模块的时候，如何在主模块中连接信号线？
答：用小括号中来表示本模块中与之连接的模块
8)	如何产生连续的周期性测试时钟？
答：用always语句来产生连续的周期性测试模块
9)	如果不用initial块，能否产生测试时钟？
答：不能，没有initial块，就不知道时钟信号的初始值
