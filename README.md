# EventDrivenProgramming: Introduction, Tutorial, History

   - 《事件驱动编程:介绍，教程，历史》
   - 作者：Stephen Ferg
   - 翻译：[SmalBox](https://smalbox.top)

<hr/>

## In The Beginning - Transaction Analysis (开始-事务分析)

   - 我的故事开始于1970年后期。在那些日子里，经典的计算机系统是批处理系统。输入数据还是经典的在带子上的顺序文件。这些文件带子旋转，被其他程序处理,然后写入到到其他文件，文件又被其他程序处理，如此反复。计算机系统的标准模式是一个流水线。原始数据从一个门进入，然后他们被反复的处理，最后结果会从另一个门出来。
   - 这种思维模式奠定了1970年代“结构化”系统开发方式。结构化方法之父是 拉里·康斯坦丁（Larry L. Constantine）。他的父公司是IBM的系统研究所。最成功的结构化倡导者是 爱德华·尤登（Edward Yourdon） 。到这种程度以致 尤登（Yourdon）和结构化分析和设计方法 几乎成为了同义词。
   - 在1974年，起初结构化这个术语出现在IBM系统之旅的文章中,它们被G,W,L，叫做结构化设计。在1975年，Larry Constantine 的一名学生 G M，在IBM SRI 发表了一篇“通过复合设计获得可靠软件”的文章。然后在1977和1978年，几乎同时地，几个关于结构化方法的重要的书出现了。它们是“机构化设计”-Ed Yourdon，Larry Constantine，“结构化分析和系统详述”-Tom De Marco,“结构化系统分析”-Chris Gane, Trish Sarson,“结构化系统开发”-Ken Orr。也许最有影响力的是Tom De Marco 的 “结构化分析和系统详述” ，这本书在伦敦出版社发布。

### Dataflow Diagrams (数据流图)

   - 结构化分析使用“数据流图”（DFDs）去展示计算机系统的逻辑结构。在DFD中,顺序文件中的记录被概念化为，通过管道 或 沿着传送带移动的数据包，成为数据流。数据包在各个一系列工作区中传递被叫做“处理”，他们被筛选、使用、增强或者改变，最后通往下一个工作区。这有一个数据流图，来自 “数据分析和系统详述” 的316页。
   - 图【transform analysis】
   - 用这种方式描述的系统被叫做 “transform analysis”
   - D M又简短的介绍了第二种分析方式，叫做 “transaction analysis” 并且提供了这张图。
   - 图【transaction analysis】
   - 他阐述了“transform”和“transaction”分析的不同如下：
      - “transform analysis” 应用于那些能明确区分出 输入流、处理中心、输出流的应用。在数据流图术语中,“transform” 由线性网格表示。
      - "transaction analysis" 应用于有着突发并行数据流特性应用的“事务中心”。
   - D M 实际上只花了很少的时间来条论“事务分析”。但是在“结构化设计”这本书里的这个主题却受到了跟多的关注。在第11章，Y和C将第一次描述事务分析归功于P。Y和C描述事务分析是“一个更灵活，SAPTAD技术的更复杂更新”。
   - “事务分析”建议通过数据流图表示，类似于图11.1，一个“transform”分裂出一个输入流，和若干个输出子流。这就是一个事件驱动的原型图。
   - 图【11.1】
   - 他说：一个“事务”是从一些“元素的数据、控制、信号、事件、或者状态的改变”开始，然后发送到事务中心做处理。
   - 一个“事务中心”必须具备如下条件：
      - 以原始的形式（获取和相应）获得事务
      - 分析每一个事务，确定他的类型
      - 根据事务类型分发
      - 完成每一个事务的处理

### Structure Charts (结构图)

   - 一个数据流图展示了一个系统必须执行的逻辑功能，但没有说明执行这些功能的程序设计。在结构化分析和设计中，一个不同的图叫做“结构图”，它用来展示程序设计。在“结构图”中，矩形代表模块（函数或子程序）。矩形按等级排列，呼叫模块在上方，被呼叫模块在下方。
   - 从事务处理数据流图 转换到的结构图如下：
   - 图【11.2】
   - 在这个图中，虚线箭头代表控制流从顶部流入事务中心。事务被“GETTRAN”函数获得。一旦事务被捕获，将被分析判断他的类型（它的事务代码）并向上传递给“事务中心”。之后，将发送给“分发”模块，它将根据事务类型分发给不同的事务处理模块。

## The Handlers Design Pattern (处理程序设计模式)

   - 如果Y和C的文章写在今天，他们很有可能将他们的“事务分析”的概念叫做设计模式。我将叫它们“处理程序模式”。
   - 这有一个“处理程序模式”的图.这个图是接着图11.1的事务分析原始数据流图。
   - 图【Handlers pattern】
   - 在图中可以看到：
      - 有一个数据流叫做“事件”（Y和C叫它“事务”）
      - 有一个“分发模块”（Y和C叫它“事务中心”）
      - 还以一系列的处理程序
   - “分发模块”的工作是获取每个进来的事件，分析事件决定事件的类型，然后发送事件到可以处理对应类型事件的处理程序中。
   - “分发模块”必须处理一连串的输入事件流，所以它的逻辑必须包括一个“事件循环”，所以它可以获取事件、分发它，然后循环回来在输入流中处理下一个事件。
   - 一些应用（例如，控制硬件的应用）可能把事件流看作是无线有效的。但是对于大多数事件处理应用，事件流是有限的，通过在流的最后用一些特殊的事件(一个文件结尾的标志，或者按ESCAPE键，或在GUI上左键点击关闭按钮)标识。在这些应用中，“分发模块”逻辑必须包含“退出”能力来在事件流末尾被发现去结束事件循环。
   - 在一些情况下，“分发模块”做出的决定可能不能恰当的处理这个事件，它们会丢弃这个事件或者引发（抛出）一个异常。GUI应用一般对类似鼠标按钮点击之类的事件感兴趣，但是对鼠标运动事件不感兴趣。所以在GUI应用中，事件没有处理程序时，通常会被抛弃。对于大多数其他类型的应用，一个不能被识别的事件在输入流中组成一个错误，一个适当的操作是是引出异常。
   - 这是经典的分发模块的伪代码，来展示一下所有特性：
      - 事件循环
      - “退出”操作
      - 判断事件类型，在类型的基础上选择一个恰当的处理程序。
      - 对没有相应处理程序的处置。
      - 图【分发模块 伪代码】

### The Headless Handlers Pattern (无头处理程序模式)

   - 这里有几个处理程序模式的变形体。其中一个是“无头处理程序”模式。在这个模式中，“分发模块”要么确实，要么不是随时可见。去掉“分发模块”，剩下的全部都是事件处理程序的集合。
   - 图【Headless Handlers Pattern】

### The Extended Handlers Pattern (扩展处理程序模式)

   - 另一种变体是“扩展处理程序”模式。在这种变体中，模式包括一个“事件生成器”组件，这个生成器可以生成“分发程序”可以处理的事件流。
   - 图【Extended Handlers Pattern】

### The Event Queue (事件队列)

   - 在一些案例中，“分发模块”和处理程序可能不能够在事件来的时候尽快的处理他们。在这些案例中，解决方案是将事件输入流存入缓冲区，在事件生成和事件分发当中将“事件队列”引入事件流。事件可以快速的添加到队列的末尾，“分发模块”也可以以最快的速度从队列的前端取出它们。
   - GUI应用通常都有事件队列。重要的事件例如鼠标点击可能需要一些事件来处理。在处理当中，其他的事件如鼠标移动事件就要累加在缓冲区中。当缓冲区再次空闲时，它就可以立即抛弃可忽略的事件-鼠标移动事件，然后快速清空序列。

### Some Examples of the Handlers Pattern (处理程序-举例)

   - 现在我们已经介绍了处理程序模式，我应该给你看一些关于这个模式的例子。这些方法和技术可能很相似，但是可能你从来没有在“事件驱动编程”的角度去思考它们。

### Objects (对象)

   - 在90年代，面向对象的技术方法逐渐的使七八十年代的结构化的方法失去了光芒。软件方法论者开始用新的图表标记法做实验，去阐述面向对象的概念。与此同时，“对象图”是这些流行的图之一（由GradyBooch发明）。这有一个对象图表的例子。
   - 图【object diagram of a STACK object】
   - 在这个“对象图”中，“Stack”是一个对象类型（或者叫做“类”）。“Push”、“pop”、“peek”是他的方法。想要使用“Stack”类，你需要创建stack对象，然后使用对象的方法去做一些事情。
   - 图【create and usage stack object】
   - 我喜欢“对象图”，因为它们清楚的展示了一个对象在“无头程序处理模式”中的示例。一个“对象图”，同样的也是一个基本的“无头处理程序”，除了事件是从左面来的而不是从顶部来的。举个例子，Stack类是可以处理“push”、“push”、“pop”事件的一系列事件处理程序（面向对象中叫做“方法”）的集合
   - 写到这里，如果你是一个面向对象的程序员，你应该已经知道什么是事件驱动编程了。毫不夸张地说，当你在写对象的方法的时候，你就是在写事件的处理程序。

### System (系统)

   - 正如我们所看到的，在“结构化系统分析“中，计算机系统被概念化为工厂。原始的原料流入工厂，在传送机的带子（数据流）上前向流动，穿过工作站（处理）最终结束旅程的产品被从门推出。
   - 原始原料的提供者是“源（sources）”，最终产品的消费者是“盥洗盆（sinks）”。“sources”和“sinks”是数据流的“终端（terminator）”-（它们是数据流开始和结束的地方）。
   - “上下文关系图”用来展示系统在终端上下文中关系的状况。这有一个上下文关系图，来自De Marco 的“结构化分析和系统详述”中第59页。
   - 图【context diagram】
   - 在1984年，Stephe McMenamin 和 John Palmer 发表了 “基本系统分析（Essential Systems Analysis）”（ESA）。ESA 建立并扩展了结构分析的早期工作，但是它也介绍了计算机系统的概念模式中基本的变化。
   - ESA 认为计算机系统不是一个工厂，是一个激励/反馈的机器。激励是在外部世界中，通过终端发送到系统的事件。系统自己概念化为一系列事件处理程序（基本活动）。当一个事件到达，系统就活动了起来，基本活动开始处理事件，然后系统再次进入睡眠状态（静默），直到下一个事件到来。
   - 基本活动反馈事件是通过在系统核心的数据存储中读写，和产生输出数据流的方式。系统的数据存储构成了“基本内存（essential memory）”。
   - 这张图展示了计算机系统的基本部分，他们是概念化的ESA。
   - 图【Characteristic shape of an event-partitioned DFD】
   - 基本上，ESA展示了一个简单的计算机系统，是一个巨大的面型对象风格对象。系统的基本活动是对象的方法，基本内存是他的内部数据。这个对象是“无头处理程序模式”的例子，它的方法扮演了处理程序的角色，所以在这里完整的计算机系统是一个“无头处理程序模式”的例子，基本活动扮演了处理程序的角色。
   - “处理程序模式”中，最完善的系统概念化是JSD（Jackson System Development），都写在了 Michael Jackson 的“系统开发”（1983）这本书中。JSD 是好方法，以论证的方式第一次真的面向对象分析和设计方法。
   - JSD系统的设计用SID（system implementation diagram 系统执行图）展示。这是一个典型的SID，来自“系统开发”的293页。
   - 图【SID】
   - 在图的顶部，我们看到的“分发模块”在JSD中叫做“调度程序（scheduler）”。事件流从外部系统到达；它叫做SCIN（“调度输入程序 scheduler input”）。事件被发送到CUST-1或ENQ（事件处理程序）。系统内部数据存储到数据库表中，这个表叫做“状态向量（state vector）”文件（成为SVFILE）。EREPLIES是响应中生产出的答复流，去处理询问事件。
   - CUST-1 和 ENQ 不是函数；他们是完整对象。这意味着JSD在两个层面使用了“程序处理模式”。在上层，整个系统以完整的“处理程序模式”为出发点，调度程序作为分发模块，对象做事件处理者。第二层面向对象方式的对象，这些方法函数作为事件的处理程序，处理从调度者或者其他对象来的事件。
   - ESA 和 JSD 标志着我们的思想从早期的结构化方法中发生巨大的转变。发生巨大转变的原因是，快速发展的数据库技术的出现。在早期的结构化分析的日子里，“数据库管理系统（DBMSs）”基本上不存在。但是，到ESA和JSD出现的时候，计算机化数据处理快速发展，从批处理系统处理顺序文件到在线系统处理数据库。首先它是链接起来的表 DBMSs （IBM 的 IMS， Cullinane的 Cullinet，和 Cincom 的 Total）。紧接着是反向列表DBMSs（adabas，模式204），接下来是关系数据库（DB2，Ingres，Oracle）。DBMS技术的进步便随着数据库设计方法学的开发。
   - 随着数据库技术的进步和其大规模的使用，大量的开发者都来使用数据库（不是访问他的软件）最为计算机系统的核心。因此，计算机系统的新模式 -作为一系列事件处理程序周边并提供接口给作为系统核心的数据库- 在那时候是非常棒的产品。