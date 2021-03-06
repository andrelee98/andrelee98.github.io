---
title: Papers
category: IOT Firmware
order: 1
---
- [Identifying Privilege Separation Vulnerabilities in IoT Firmware with Symbolic Execution](#identifying-privilege-separation-vulnerabilities-in-iot-firmware-with-symbolic-execution)
  * [主要工作](#----)
  * [流程](#--)
- [USENIX19： Discovering and Understanding the Security Hazards in the Interactions between IoT Devices, Mobile Apps, and Clouds on Smart Home Platforms](#usenix19--discovering-and-understanding-the-security-hazards-in-the-interactions-between-iot-devices--mobile-apps--and-clouds-on-smart-home-platforms)
  * [新家用物联网设备Lifecycle](#--------lifecycle)
- [Sharing More and Checking Less: Leveraging Common Input Keywords to Detect Bugs in Embedded Systems](#sharing-more-and-checking-less--leveraging-common-input-keywords-to-detect-bugs-in-embedded-systems)
  * [流程](#---1)
  * [实现](#--)
  * [问题](#--)
- [TensorFuzz: Debugging Neural Networks with Coverage-Guided Fuzzing](#tensorfuzz--debugging-neural-networks-with-coverage-guided-fuzzing)
- [2019 S&P:NEUZZ: Efficient Fuzzing with Neural Program Smoothing](#2019-s-p-neuzz--efficient-fuzzing-with-neural-program-smoothing)
- [2020 S&P:Neutaint: Efficient Dynamic Taint Analysis with Neural Networks](#2020-s-p-neutaint--efficient-dynamic-taint-analysis-with-neural-networks)
  * [效果](#--)
  * [DTA问题](#dta--)
  * [流程](#---2)
- [Usenix20 GreyOne: Data Flow Sensitive Fuzzing](#usenix20-greyone--data-flow-sensitive-fuzzing)
- [Usenix20 FuzzGuard: Filtering out Unreachable Inputs in Directed Grey-box Fuzzing through Deep Learning](#usenix20-fuzzguard--filtering-out-unreachable-inputs-in-directed-grey-box-fuzzing-through-deep-learning)
- [NDSS19 Neuro-Symbolic Execution: Augmenting Symbolic Execution with Neural Constraints](#ndss19-neuro-symbolic-execution--augmenting-symbolic-execution-with-neural-constraints)
  * [流程](#---3)
- [CCS19 Poster: Fuzzing IoT Firmware via Multi-stage Message Generation](#ccs19-poster--fuzzing-iot-firmware-via-multi-stage-message-generation)
- [S&P20: KARONTE: Detecting Insecure Multi-binary Interactions in Embedded Firmware](#s-p20--karonte--detecting-insecure-multi-binary-interactions-in-embedded-firmware)
- [Detecting Authentication-Bypass Flaws in a Large Scale of IoT Embedded Web Servers](#detecting-authentication-bypass-flaws-in-a-large-scale-of-iot-embedded-web-servers)
- [ACCESS 19: Staged Method of Code Similarity Analysis for Firmware Vulnerability Detection](#access-19--staged-method-of-code-similarity-analysis-for-firmware-vulnerability-detection)
- [ACCESS 20: FIRMCORN: Vulnerability-Oriented Fuzzing of IoT Firmware via Optimized Virtual Execution](#access-20--firmcorn--vulnerability-oriented-fuzzing-of-iot-firmware-via-optimized-virtual-execution)
- [SAC 20: OVER: overhauling vulnerability detection for iot through an adaptable and automated static analysis framework](#sac-20--over--overhauling-vulnerability-detection-for-iot-through-an-adaptable-and-automated-static-analysis-framework)
- [ACCESS 20: Large-Scale IoT Devices Firmware Identification Based on Weak Password](#access-20--large-scale-iot-devices-firmware-identification-based-on-weak-password)
- [NDSS 15: Firmalice - Automatic Detection of Authentication Bypass Vulnerabilities in Binary Firmware](#ndss-15--firmalice---automatic-detection-of-authentication-bypass-vulnerabilities-in-binary-firmware)
- [NDSS 18:Staged method of code similarity analysis for firmware vulnerability detection](#ndss-18-staged-method-of-code-similarity-analysis-for-firmware-vulnerability-detection)
- [TOOLS:NDSS16: Towards Automated Dynamic Analysis for Linux-based Embedded Firmware](#tools-ndss16--towards-automated-dynamic-analysis-for-linux-based-embedded-firmware)
- [TOOLS: S&P iot19: FirmFuzz: Automated IoT Firmware Introspection and Analysis](#tools--s-p-iot19--firmfuzz--automated-iot-firmware-introspection-and-analysis)
- [TOOLS: Usenix19：Firm-AFL: High-Throughput Greybox Fuzzing of IoT Firmware via Augmented Process Emulation](#tools--usenix19-firm-afl--high-throughput-greybox-fuzzing-of-iot-firmware-via-augmented-process-emulation)
- [SURVEY 21.3: Automatic Vulnerability Detection in Embedded Devices and Firmware: Survey and Layered Taxonomies](#survey-213--automatic-vulnerability-detection-in-embedded-devices-and-firmware--survey-and-layered-taxonomies)
- [算法入门简介](#------)
  * [污点分析](#----)




[固件下载](https://www.cnblogs.com/from-zero/p/12355492.html)

[固件逆向](http://blog.binpang.me/2018/07/10/firmware-reverse/)

[固件逆向](https://www.jianshu.com/p/66dcf89beb7f)

[会议截稿时间](https://zhuanlan.zhihu.com/p/27853093)

[这儿有些动态污点传播的可用sink](https://www.hindawi.com/journals/scn/2018/7693861/)

现在NLP发展到什么程度了?对于特定漏洞，如除零错误或buffer overflow，感觉可以吧?

binary similarity, 用图卷积觉得可以啊?

## Identifying Privilege Separation Vulnerabilities in IoT Firmware with Symbolic Execution

[github](https://github.com/daumbrella/Gerbil)

### 主要工作

0)提出权限分离漏洞: 物联网设备的一些操作仅在与云交互时执行，另一些仅在用户物理按压按钮时执行。如果两组操作对应的指令交集不为空，则存在漏洞。

1)自动提取物联网固件的加载信息的工具。

主要是找硬编码

2)辅助工具来分割物联网固件中最有可能存在权限分离漏洞的部分，并分割这部分代码用于符号执行。

收集流行单片机制造商和物联网平台提供商的官方GitHub转帖中广泛使用的物联网库。编译比较。如果找到一个匹配，我们直接得到它的语义，并能够在静态和动态分析中恢复它。

3）基于符号执行的路径探索方案。它可以通过库函数识别跳过复杂的库函数，以减轻路径爆炸，还能够恢复间接调用，以探索更深的路径。

如果要跳转到库函数，则库函数名和寄存器值作为约束。有些库函数直接跳过，赋返回值。

另外，对于非直接调用（如回调、消息队列），在符号化执行开始时记录调用者的addr，如果在执行时匹配，则跳到非直接调用的地址。

还搞了个用户自定义分片规范的辅助工具(没啥东西)。

The collection of functions in the execution path from receiving a message to executing a specific command can be considered as an individual **call sub-graph** of the whole call graph in IoT firmware. 

The **caller function** represents the highest node (i.e, start point) of a call sub-graph.

We refer to a function shared by two or more call sub-graphs as **shared function**.

The **command function** represents the nearest node to the last shared functions in a call sub-graph, which is always the first function used to perform a specific command (e.g., cmd reboot and the cmd unbind in sub-graph U and R in Fig.1). Commnad function调用共用的函数是可以的。

### 流程

1)找command function: 用户提供俩，搜字符串，看对应的command function最近的公共父节点，它可能调用其他的command function。

2)找caller function: 先找接收网络输入的函数(用户指定库函数)，然后找包含command function和接收输入的函数之间的所有路径，找最高点。

3)结果生成：首先将caller function作为起点集合，command function作为终点集合输入到符号执行引擎。然后，符号执行引擎根据路径探索方案探索所有可能的路径，并输出从调用者函数到命令函数的可能路径和相应的路径约束。将可以从一个以上caller function访问的所有函数(command function除外)标记为超特权共享函数。

## USENIX19： Discovering and Understanding the Security Hazards in the Interactions between IoT Devices, Mobile Apps, and Clouds on Smart Home Platforms

**Device ID**：设备唯一表示，用于认证。

**Identity Information**：用于生成Device ID的信息。

**Legitimacy Information**：不用来生成Device ID， 但用于合法性检查。

**Phantom Devices**： 写的python，模拟真实设备。

主要有三种：物联网设备固件 手机APP IOT云三者交互

app发请求，IOT云控制，固件响应

### 新家用物联网设备Lifecycle

关键点在于伪造一个设备，然后偷认证所需的信息。

[ali github](https://github.com/alibaba/AliOS-Things)

[合作制造商 github](https://github.com/espressif/esp8266-alink-v1.0)

[youtube poc视频](https://www.youtube.com/watch?v=MayExk_PKhs&feature=youtu.be)

[youtube poc视频2](https://www.youtube.com/watch?v=fufEAtQq2_g&feature=youtu.be)

1）设备发现

2）接入无线网

3）设备注册 两类云平台：第一类：注册时，设备将其Device ID发送到其所属的物联网云。然后，云生成一个设备标识，并将其返回给设备；第二类：生产设备时预生成，硬编码在设备里，跳过注册。

4）设备绑定

第一类：绑定请求由移动app直接发送到物联网云，物联网云负责维护绑定信息并执行权限检查。第二类：移动应用首先将账户信息发送到设备。具有设备标识和用户账户的设备向物联网云发出绑定请求。值得注意的是，这里云无条件接受来自设备的绑定请求。这种设计基于一种自然的假设，即实际拥有设备的客户应该对其拥有完全的控制权。

5）设备登录

设备使用其设备ID登录物联网云。然后，它与云建立连接，以保持状态更新，并准备好执行远程命令。当设备长时间失去与云的连接时，它会尝试自动重新登录。

6）执行命令

7）解绑/重置

对于第一类平台，云会直接删除绑定信息。然而，对于第二类平台，由于绑定信息也存储在本地设备上，所以从云向设备发送一个附加命令来擦除绑定信息。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210302143624802.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE3MzgyNTc=,size_16,color_FFFFFF,t_70)

如果攻击者与受害设备在同一个wifi-arrange里，通过嗅探无线探测请求，可以轻易拿到MAC地址。wifi的基本缺陷。

讲了个提取硬编码然后转手设备的故事，智能家居旅馆房间之类的。

## Sharing More and Checking Less: Leveraging Common Input Keywords to Detect Bugs in Embedded Systems

Fuzz的问题：只能覆盖一小部分执行流。

静态：少前后端交互。

本文主要目的是找到前端输入的变量在后端的对应变量，进而污点传播找到前端用户输入可能到达的敏感函数。作者认为，前端变量和后端变量所使用的字符串多为相同或相似，可以依次找到对应关系。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210316101234431.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE3MzgyNTc=,size_16,color_FFFFFF,t_70)

### 流程

1)**提取关键字**： xml, http 通过正则, js通过抽象语法树(AST)。"action="这种关键字被称为action keywords，在作为污点传播source的时候有优先级。作为参数传播的keywords称为parameter keywords，如usrname, id等。

2)**过滤关键字**：①过滤带有特殊符号的；②过滤短于阈值的；③如果形如a=b,则保留左边，去掉右边；④若js被多个html引用，则可能为共享库，不考虑；⑤一个关键字被多个前端文件引用，则可能是常用字符串，不考虑。

3)**识别边界binary**: 边界binary(border binary)是负责前后端交互的binary，找匹配字符串最多的binary，可能为边界binary。可以作为污点传播的源。

3)**识别入口点**: 在边界binary中找包含关键字的字符串。优先搜索包含action keywords和函数指针的function call，函数指针被认为是action handler。

对于后端有富余的(后端关键字没有对应前端)，称为implicit关键字。只要有一个入口点L，L周围有相似的代码模式的函数调用也被认为是入口点。(逻辑：相似功能的代码可能被排列的比较近)。如下图中，前端没有action,但因passwd那行(行5)是入口点，行5和行4代码模式相似，所以也把行4认为是入口点。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210316185706743.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE3MzgyNTc=,size_16,color_FFFFFF,t_70)

4)**跨进程入口点**：数据设置位置(第一次赋值)和数据使用位置往往使用同一个关键字。主要支持两种跨进程通讯：NVRAM和环境变量。

5)**污点传播**：有三个优化

①粗粒度：三个原则：1.source与输入相关 2.平衡accuracy和efficiency（这个感觉能针对自己的数据集调优）3.只跟踪从source到潜在sink。

②污点规范： summarizable function：库函数； general function: 不调别的，或者只调库函数；剩下的是 nested function。传播规则大体是下面的算法：

对于summerizable,当做指令，基于语义传播；对于general，跟到函数内，对于nested，如果有返回值，把所有参数的污点标记给返回值，否则把所有指针参数标记污点(感觉应该是action的情况)。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210316202001974.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE3MzgyNTc=,size_16,color_FFFFFF,t_70)

6)**路径探索**: guide function:调sink function的函数。在探索路径前，对于每个source，看看控制流图是否有到sink的路径。还有合并不同路径，即通过keyword和入口点聚类，在进入guide function之后，细粒度分析，否则做之前的粗粒度分析。

7)**路径优先度策略**：

优先选短的。也解决两个问题，第一是parser函数中隐含路径，如第五行case，解决方案和karonte一致。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210317101817453.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE3MzgyNTc=,size_16,color_FFFFFF,t_70)

第二是加密导致的路径爆炸。通过以下算法，只对最长路径感兴趣。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210317102340504.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE3MzgyNTc=,size_16,color_FFFFFF,t_70)

### 实现

我们用大约2500行Python代码和7300行C代码实现了原型系统。输入关键词提取模块基于标准的XML处理库和JavaScript解析库Js2Py [12]实现。输入条目识别模块是基于Ghidra库和扩展的KARONTE的CPF实现的，该CPF包含与NVRAM共享的污染变量[6]。输入敏感分析的污点引擎建立在多架构二进制分析框架angr [30]之上。路径选择部分是基于Ghidra库实现的[23]。为了使SaTC更适用于MIPS架构，我们修复了angr的二进制加载器和KARONTE的寄存器误用问题。现在原型系统支持多种架构，包括x86、ARM、MIPS。

### 问题

主要有三个问题：

1.隐式数据依赖，没关键字（这感觉没办法）

2.完整和效率取舍 (水话)

3.加密和混淆

还有个问题：因为source是border binary，实际上忽略了前端对于输入的处理，可能导致过污染。



## TensorFuzz: Debugging Neural Networks with Coverage-Guided Fuzzing

fuzz已训练好的模型哪儿薄弱，感觉和对抗有点联系。

[解读](https://blog.csdn.net/qq_33935895/article/details/105789728)

包括：Input Chooser(最近的输入选取几率大)，mutator(要么规定定方差白噪声，要么规定L-无限大模)，Objective Function(在这里定什么是不符合要求的)，Coverage Analyzer(检查覆盖率)。

检查是否新覆盖：最好检测激活矢量(全文没解释，觉得应该是每个激活函数的输入或输出)是否接近先前观察到的矢量。实现这一点的一种方法是使用近似最近邻算法。当我们得到一个新的激活向量时，我们可以查找它的最近邻居，然后检查最近邻居在欧几里德距离中有多远，如果距离大于某个量L，则将输入添加到语料库中。

在计算距离时，把激活投影到最重要的方向上，只考虑这些方向。

## 2019 S&P:NEUZZ: Efficient Fuzzing with Neural Program Smoothing
[github](http://github.com/dongdongshe/neuzz)
作者认为，漏洞挖掘本身是个优化问题，其目标是找到在给定的测试时间内最大化漏洞数量的程序输入。但因为安全漏洞往往稀疏且不规则地分布在程序中，目标函数非凸，有很多突变处，因此没法用梯度引导的优化。

神经网络本身是个连续且可导的线性函数(of class C1)，这篇文章关键点在于用神经网络拟合非class C1函数，并在神经网络上应用梯度搜索。样本是程序输入，标签是ground truth, 应该是从内存里掏出来的运行时关键值。这篇文章专注于将分支行为作为ground truth，因此是bool的。定义损失函数如下，含义是预测值和ground truth的差距。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210327173929507.png)

缓解多重共线性(multicollinearity)：各标签可能存在强相关性。由训练数据执行的边缘覆盖通常是有偏差的，因为它只包含程序中所有边缘的一小部分的标签。为了避免这种情况，作者遵循降维的常见机器学习实践，将训练数据中总是一起出现的边合并成一条边。此外，作者只考虑在训练数据中至少被激活一次的边缘。这些步骤将标签数量从平均约65536个显著减少到约4000个。

梯度搜索：在作者的设置中，基于梯度的引导的目标是找到将对应于不同边缘的最终层神经元的输出从0改变为1的输入。

增量学习: 在有新数据时，将新数据中可以获得新边缘覆盖的数据加入到旧训练数据中再训练。

仨全连接，层之间relu，输出层sigmoid，50 epoch，95%准确率。

### 实验
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210327231159842.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE3MzgyNTc=,size_16,color_FFFFFF,t_70)

算法1显示了我们的梯度引导输入生成过程的轮廓。关键思想是识别具有最高梯度值的输入字节，并对它们进行变异，因为它们对神经网络指示更高的重要性，从而更有可能导致程序行为的重大变化(例如，翻转分支)。从一个种子开始，我们迭代地生成新的测试输入。如算法1所示，在每次迭代中，我们首先利用梯度的绝对值来识别输入字节，该输入字节将导致输出神经元中对应于未捕获边缘的最大变化。接下来，我们检查每个字节的梯度符号，以决定突变的方向(例如，增加或减少它们的值)，从而最大化/最小化目标函数。从概念上讲，我们对梯度符号的使用类似于对抗性输入生成方法。我们还将每个字节的突变绑定在其合法范围内(0-255)。第6行和第10行表示使用裁剪函数来实现这种边界。我们从一个小的变异目标(算法1中的k)开始输入生成过程，并指数增长要变异的目标字节数，以有效覆盖大的输入空间。
应该是每个变异只改一个字节?这个字节随梯度步进1到255个值。
还讨论了一下单隐藏层和多隐藏层的模型表现，水话而且只比较了三隐层和单隐层。

选了10个程序，每个在AFL上跑1小时，平均2000个输入，5:1分训练集测试集。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210328000706111.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE3MzgyNTc=,size_16,color_FFFFFF,t_70)

如图2所示，NEUZZ迭代运行以生成1M突变，并逐步重新训练神经网络模型。我们首先使用算法1中描述的突变算法来生成1M突变。我们将参数i设置为10，这为种子输入生成了5120个突变输入。接下来，我们随机选择100个输出神经元，代表目标程序中100个未探索的边，并从两个种子中生成10240个突变输入。最后，我们使用AFL的分叉服务器技术[54]用1M突变输入执行目标程序，并使用覆盖新边的任何输入进行增量再训练。


为了检测不一定会导致崩溃的内存错误，我们用[AddressSanitizer](https://github.com/google/sanitizers)编译程序二进制文件。我们通过比较AddressSanitizer报告的堆栈跟踪来测量发现的独特内存错误。对于不会导致AddressSanitizer生成错误报告的崩溃，我们检查执行跟踪。整数溢出错误是通过手动分析触发无限循环的输入发现的。我们使用[UndefinedBehaviorSanitizer](https://clang.llvm.org/docs/UndefinedBehaviorSanitizer.html)进一步验证整数溢出错误。



其中用了[LAVA-M数据集](https://sites.google.com/site/steelix2017/home/lava)，作者写LAVA-M触发漏洞需要一个4byte的magic number，讲先跑AFL一小时生成训练集，然后找梯度最大byte，之后变异它和它周围三字节。1）那么怎么算梯度？2）变异单字节可以触发漏洞？否则还是256的4次方。

Fuzz对magic bytes只能试啊，能不能和symbolic execution结合？因为真实世界程序对magic bytes的检查往往是1）哈希/加解密校验（这个如果能判断可以直接放弃）2）分支条件（这个往往是未经复杂处理的用户输入，比如只经过parse），其实可以解对应输入满足的要求。



**这有个RNN引导的fuzz:**

Rajpal, M., Blum, W., & Singh, R. (2017). Not all bytes are equal: Neural byte sieve for fuzzing. arXiv preprint arXiv:1711.04596.

## NDSS20 DEEPBINDIFF: Learning Program-Wide Code Representations for Binary Diffing

[github](https://github.com/deepbindiff/DeepBinDiff)

这个是做二进制差分。旨在定量测量两个给定二进制码之间的相似性，并产生细粒度的基本块级匹配。它不仅给出了关于整个二进制规模的差异的精确、细粒度和定量的结果，而且明确揭示了代码如何在不同版本或优化级别之间演变。

突破在于①之前没有机器学习的算法可以在基本块级别的细粒度上执行有效的程序范围的二进制区分。②之前没有一种基于学习的技术在分析过程中同时考虑程序范围的依赖信息和基本块语义信息。③无监督学习。

参考文献不少，可以看看。

当前工作主要有：

传统方法，包括：

1）静态分析：控制流图、数据流图匹配。缺点是只考虑指令的语法，而不考虑语义，而且一些图匹配算法消耗大。

2）动态分析：二进制文件切片或污染。缺点是扩展性较差，代码覆盖率较低。

机器学习方法：

图表示学习技术、代码信息嵌入(embedding，即高维数值向量)、NLP。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210331092216999.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE3MzgyNTc=,size_16,color_FFFFFF,t_70)

### 流程


**预处理**

1)CFG生成: IDA

2)特征向量生成：魔改word2vec,造了个token embedding模型，包括：

随机漫步：依靠控制流图，五个基本块。这个相当于word2vec里的句子。

标准化：用特定字符串替换变量名、指针、buffer。

用Bag-of-Words(CBOW)方法训练出个token embedding模型。

每个指令包含一个opcode embedding和多个operand embedding，作者用operand embedding平均值和opcode embedding连接，生成指令embedding，然后加在一起生成block feature embedding。

用了个TF-IDF模型，来区分重要的操作和不重要的。

3）Text-associated DeepWalk训练模型：

合并两个图，将外部库调用和系统调用表示为虚节点来联系两个图。



## 2020 S&P:Neutaint: Efficient Dynamic Taint Analysis with Neural Networks

可以用于Taint-guided fuzz: 动态污点传播相当于给fuzz做个初始化。

比较的工具：

[libdft](http://www.cs.columbia.edu/~vpk/research/libdft/)

[triton](https://triton.quarkslab.com/documentation/)

[DFSan](https://clang.llvm.org/docs/DataFlowSanitizer.html)

### 效果

评估结果表明，在6个真实世界程序上，NEUTAINT平均可以达到68%的准确率，比第二好的污点工具Libdft提高了10%，同时减少了40倍的运行时开销。当用于污点引导的fuzz时，NEUTAINT还实现了61%以上的边缘覆盖，这表明了所识别的影响字节的有效性。我们还评估了NEUTAINT检测真实世界软件攻击的能力。结果表明，NEUTAINT能够成功检测不同类型的漏洞，包括缓冲区/堆/整数溢出、被零除等。最后，NEUTAINT可以检测98.7%的总流量，是所有污点分析工具中最高的。

### DTA问题

主要提出了三个问题：

1）传播规则准确性: 如s = x*c，若c一直为零，s不受x影响

2）累计错误，如c=a+b;d=c-b; d实际与b无关。

3）运行时开销，因为每个指令都要检查，以决定使用什么规则。

### 流程

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210325112952218.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE3MzgyNTc=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210325113008714.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE3MzgyNTc=,size_16,color_FFFFFF,t_70)



![在这里插入图片描述](https://img-blog.csdnimg.cn/20210325113023385.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE3MzgyNTc=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210325113034391.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE3MzgyNTc=,size_16,color_FFFFFF,t_70)






## Usenix20 GreyOne: Data Flow Sensitive Fuzzing

Fuzzing-driven Taint Inference：一次变异输入的一个字节，看看目标变量有没有变化。

Taint-Guided Mutation：选择影响更多未触碰分支的字节做变异。

Conformance-Guided Evolution：往fuzz种子队列里加与新分支需要的值差距最小的，思想有点类似梯度下降。

## Usenix20 FuzzGuard: Filtering out Unreachable Inputs in Directed Grey-box Fuzzing through Deep Learning

使用的定向灰盒fuzz器是AFLGO。

这篇文章2.2部分有deep learning指导fuzz的一些相关文章。

三层CNN。

定向fuzz指希望对程序中某一段进行fuzz。对于由多个条件约束(可能上千个)决定的分支，大量的输入到达不了分支。这篇文章主要讲怎么生成可以到指定分支的输入样本。

主要有三个问题：

1）样本不平衡，可达的太少，文中例子是第2260万个才到。且不能数据增强。

2）不同路径可能到达相同代码段，使得样本间可能大不相同。

3）dl预测成本要小于fuzz试错成本。

对于问题一，选择能够到达指定代码段之前的分支的样本作为训练集。对于问题二，从随机变异中按某种方法有效抽取数据运行，减少数据总量。对于问题三，从运行结果中更新模型，需仔细选择模型更新时间。只过滤不可达输入，而不干涉随机变异。



## NDSS19 Neuro-Symbolic Execution: Augmenting Symbolic Execution with Neural Constraints

### 流程

先静态分析找到可能有除零或buffer溢出的地方，然后解使得相应分母或buffer index触发漏洞需要满足什么条件。

1）预处理：静态分析，生成调用图；找candidate vulnerability points (CVP),在本文中，指①未检查除零错误的除法处；②未检查buffer越界的buffer使用。记录分母和buffer越界关键索引(例如对于buffer[i]，记录i)

2）传统CSE模式：通常情况下，使用klee的标准DSE模式

3）Neural模式：在四种情况下使用neural模式：①路径探索展开卡住②路径爆炸导致内存不足③SMT solver解不开SAT/UNSAT问题④DSE遇到未知调用/外部调用。转到Neural模式时，复制所有约束和具体值。

4）生成神经约束：当神经模式被触发时，NEUEX查询调用图，以识别从最新符号状态静态可达的所有CVP。它为每种类型的bug选择最接近的k个CVP，其中k是默认设置为150的可配置参数。为了训练神经网络，NEUEX将符号分支和每个CVP位置分别视为起点和终点。NEUEX将起点和终点之间的代码片段视为神经网络近似的黑盒。在卡住的点产生随机突变，执行后在终点收集样本。训练好的神经网络作为神经约束。通过生成20000个样本执行来看看是否可达；一直到能收集10000个可达样本为止。

5）约束求解。SMT求解：Z3；Neural求解：基于梯度搜索，找loss最少的值。多随机初始化求解，避免陷入局部最优。为了同时满足SMT和Neural，提出以下算法：①在约束和变量之间建立有向无环图，如下图。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210325093740279.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE3MzgyNTc=,size_16,color_FFFFFF,t_70)

②区分纯约束和混合约束，区分纯神经约束和纯符号约束，优先求解纯约束。纯约束求解顺序不影响结果，直接用上面的方法求。求完纯约束求混合约束。

③混合约束求解方法a)：先求混合约束中的符号约束，如G3中，先求V5,V6,V8,V9,给V8,V9赋值后，根据N2求V7。在N2求解后，如果没有课满足的V7，则在符号约束中加入反例，例如![在这里插入图片描述](https://img-blog.csdnimg.cn/20210325104227648.png)

最多迭代Max2次。

混合约束求解方法b)：a)实际是将神经限制转换为符号限制，因为引入太多符号限制，提出可以把符号限制转变为神经限制。(靠loss函数)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210325105955783.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE3MzgyNTc=,size_16,color_FFFFFF,t_70)

6）检验?: 每次枚举后，我们用求解器生成的具体输出来执行程序，以检查它们是否确实满足神经符号约束。





## CCS19 Poster: Fuzzing IoT Firmware via Multi-stage Message Generation



## S&P20: KARONTE: Detecting Insecure Multi-binary Interactions in Embedded Firmware

## Detecting Authentication-Bypass Flaws in a Large Scale of IoT Embedded Web Servers

## ACCESS 19: Staged Method of Code Similarity Analysis for Firmware Vulnerability Detection

## ACCESS 20: FIRMCORN: Vulnerability-Oriented Fuzzing of IoT Firmware via Optimized Virtual Execution

## SAC 20: OVER: overhauling vulnerability detection for iot through an adaptable and automated static analysis framework

## ACCESS 20: Large-Scale IoT Devices Firmware Identification Based on Weak Password

## NDSS 15: Firmalice - Automatic Detection of Authentication Bypass Vulnerabilities in Binary Firmware

基础符号化执行，见第一次组会ppt。

## NDSS 18:Staged method of code similarity analysis for firmware vulnerability detection

无binary fuzz，先对app污点传播找到会发送网络信息的函数的入口函数，再执行变异，见第一次组会ppt。

## TOOLS:NDSS16: Towards Automated Dynamic Analysis for Linux-based Embedded Firmware

## TOOLS: S&P iot19: FirmFuzz: Automated IoT Firmware Introspection and Analysis

## TOOLS: Usenix19：Firm-AFL: High-Throughput Greybox Fuzzing of IoT Firmware via Augmented Process Emulation

[github](https://github.com/zyw-200/FirmAFL)

黑盒：无任何反馈。

白盒：动态污点分析、符号化执行。

灰盒：轻量级监控，根据反馈指导生成输入。tiant-guided，coverage-guided

这篇文章的关键在于能模拟用户态用户态，syscall的时候才模拟kernel态，所以提高了throughput。

## SURVEY 21.3: Automatic Vulnerability Detection in Embedded Devices and Firmware: Survey and Layered Taxonomies

## 算法入门简介

[符号化执行简介](https://zhuanlan.zhihu.com/p/26927127)

[简单理解污点传播](https://www.k0rz3n.com/2019/03/01/%E7%AE%80%E5%8D%95%E7%90%86%E8%A7%A3%E6%B1%A1%E7%82%B9%E5%88%86%E6%9E%90%E6%8A%80%E6%9C%AF/)

[污点传播_ctf_all_in_one,这个比较清楚](https://github.com/firmianay/CTF-All-In-One/blob/master/doc/5.5_taint_analysis.md)

### 污点分析

基于数据流的污点分析：在不考虑隐式信息流的情况下，可以将污点分析看做针对污点数据的数据流分析。根据污点传播规则跟踪污点信息或者标记路径上的变量污染情况，进而检查污点信息是否影响敏感操作。

基于依赖关系的污点分析：考虑隐式信息流，在分析过程中，根据程序中的语句或者指令之间的依赖关系，检查 Sink 点处敏感操作是否依赖于 Source 点处接收污点信息的操作。

**应该是一个正一个反。**

不同标签的污染，判断每个分支由哪些变量影响，

再求偏导？

能静动态结合吗？

**gan能用来生成fuzz数据吗？** 

fuzz的时候学习潜在的约束关系，生成样本？

问题是往往约束条件苛刻。

~~**mutation用数据增强取代？**~~ 数据增强目的是保持原有标签，mutation的目的是探索新路径(生成新标签数据)




