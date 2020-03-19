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

### Client-Server Architecture (客户端-服务器架构)

   - 在CS（Client-Server）架构中架构中有一个“处理程序模式”的熟悉示例。“server”是硬件或者软件的管线，它给“客户端”提供了服务。服务端的工作是等待来自客户端的“服务请求”，根据提供的请求服务反馈“服务请求”，然后等待更多的请求。服务端的例子包括：打印服务，文件服务，窗口服务，数据库服务，应用服务和web服务。如果你经常在网上冲浪，你是在和服务器互动，每当你访问一个新的地址，你的web游览器发送一个请求到web服务器，你请求的web页会反馈你的请求。
   - Wesey Chun 在“Python 编程核心”的16章中提供了一个简短、清晰的解释来描述“客户端-服务器“架构的基础。（我想稍微的修改了一下文本来提高技术的连贯性）。想象一下，Chun 说：
      - 一个既不吃也不睡，也不休息的空闲柜员，在一个从来看不到尾的流水线中服务一个又一个顾客。流水线可能会很长或者有时会空闲，但在任何时候，顾客都有可能会出现。当然，这些柜员是多年前幻想出来的，但是现在的自动柜员机（ATMs）是很接近这个模型的。
      - 当然，这些柜员是在无限运行的服务器。每个客户端都要发送一个请求服务器的“服务请求”。客户端的请求到达服务器，并以先来先服务的方式处理请求。一次交互完成，客户就走了，服务端要么服务下一个客户，要么等待直到下一个随之而来的客户。
   - 这有一个案例，描述了“处理程序模式”这个术语。
   - 图【client-serer Handlers pattern】
   - 每个银行顾客代表了一个从客户端发送来的服务请求（事件，事务）。
   - 客户排队或者等候服务。服务端和不知疲劳的柜员很相似，因为它们都是事件处理程序的集合，都能处理不同的事件请求。并且银行柜员的“无限循环”也就是“分发模块”的事件循环。

### Messaging Systems (消息系统)

   - “消息系统”代表了“程序处理模式”的极端版本。“消息系统”的目的是，在发送者和接收者在不同的物理位置或者运行在不同平台上的情况下，从事件的生成者（发送者）获取事件（消息）并处理（接收者）消息。
   - 在“消息系统”中，消息通常发送给特定的接收者，所以分发函数（决定接收者应该接收什么消息）是很普通的函数。一个熟悉的“消息系统”的例子是邮局。一个发送者发送一个消息（信件或者包裹）到邮局（“消息系统”）。邮局在消息中读取收信者的地址，并且传送信息给收信者。
   - 图【Messaging System】
   - E-mail “消息系统”本质上和邮局的功能差不多。唯一的不同是E-mail的消息是电子编码而不是物理编码。
   - 可能大多数精密的消息系统是企业的消息系统，使用“面向消息的中间件”或者MOM。在MOM系统中，发送者和接收者是计算机应用，而不是人。MOM系统允许计算机应用在物理分离或者运行在不同的软硬件平台下互相通信。例如，一个大公司的办公区和服务器在地理位置上分散。MON软件允许在公司的LA处的管理入口系统以电子的形式发送给在Chicago服务器上的应用一个指令结束，也可以在纽约服务器上的程序管理报道，全都不需要人干预。
   - 再加上，对于这种点对点的通讯模式，MOM产品也支持发布/订阅模式。在发布/订阅模式中，接收者成为了通过主题订阅的订阅者，发送者要发送消息到主题中，而不是个人订阅者）。当主题收到消息，这个主题会将消息发送给所有订阅它的接收者。
   - 图【publish/subscribe model】
   - 在MOM系统中，电子通信问题（队列问题，发布/订阅模式的执行问题）使系统的处理程序方面变得简单。然而，这么做的目的使帮助理解MOM系统，作为一个极端简单且专用的处理程序模式的例子。

## Frameworks (框架)

### Object-Oriented Event-Driven Programming (面向对象事件驱动编程)

   - 现在让我们来看一个全景 - “程序处理”模式在现代计算机的不同方面是怎么体现的。然我们在代码层面看“程序处理”模式是怎样工作的。
   - 考虑到与客户打交道的业务。业主自然想有一个信息系统来存储、恢复、更新他顾客信息的账户。他想要的系统可以处理各种事件：需要添加一个新的客户账户，可以修改账户名、关闭账户，诸如此类。所以系统必须有处理各种类型事件的事件处理程序。
   - 图【business information system】
   - 在面向对象编程出现之前，这些事件处理程序作为子程序来执行。这些代码在分发模块的事件循环中，像下面这样：
   - 图【dispatcher pseudo-code】
   - 子程序的类似这样：
   - 图【subroutines pseudo-code】
   - 现在，使用面向对象技术，事件处理程序作为对象的方法执行。这些代码在分发模块的事件循环中，像下面这样：
   - 图【dispatcher OO pseudo-code】
   - “账户”类和他的方法（事件处理函数），像下面这样：
   - 图【account class methods】
   - 使用面向对象技术这种方式没有很激动。基本上来说，我们只是用对象取代了数据库记录；换句话说，数据处理的过程没什么变化。
   - 但是它变得更有趣……

### Frameworks (框架)

   - 使用面向对象技术可以相对容易的开发普遍的、可复用的类。这是面向对象技术的一个优势。
   - 举个例子，假设有一个商业的，多用途的业务类产品-“通用业务”。通用业务是一个软件框架，可以直到怎样展示多样的一般化商业功能（打开顾客账号，关闭顾客账号，诸如此类）。显然，因为所有的业务不同，通用业务允许可以根据业务特定的需求定制框架。
   - 假设接下来，Bob是一个小的业务员，他买了一个通用业务软件。在他使用软件之前，Bob需要根据他的需求定制软件。我们能想到Bob有很多定制化的事情要做：他的名字、他卖的东西的名字、它允许使用哪种信用卡，诸如此类。但是经过讨论，让我们看看Bob最急迫的需求，他想定制通用业务来使用MySQL存储账号信息。
   - 通用业务是已经写好的。他不能预测到企业将使用哪个DBMS。这意味着通用业务（就算知道怎样打开和关闭用户账号）不知道怎样持久化数据库的账户数据。当然，他也不可能知道企业用什么DBMS（Oracle， Sybase， DB2， MySQL，Postgres），通用业务不知道在账户类的“persist()”方法中该写什么代码。
   - 这意味着Bob必须自己在他的“persist()”方法中写代码。
   - 也就是说，通用业务有个问题，它怎么确保像Bob这样的用户会给“persist()”方法写代码呢？
   - 通用业务给出的解决方法不是提供一个全功能的账户类，而是提供半成品类（仅实现全功能账户类的某些方法）。实现的这部分，我们叫它通用账号类。通用账号类提供一些方法的完整实现，并且给像Bob这样的业务员预留了“插入点”，Bob必须添加他的业务代码。
   - “插入点”是代码中的位置，在软件框架中期望事件处理程序插入的地方。事件处理程序本身叫做“插件（plug-ins）”
   - 包含“插入点”的半成品类的技术学名叫做“抽象类”。不同语言提供不同的方式定义插入点。例如，java提供了关键字“abstract”和叫做“abstract methods”的插入点。
   - 抽象方法不是真的方法。而是方法的占位符；一个可以插入具体方法的地方。包含抽象方法的Java类叫做抽象类。抽象类不能实例化。使用抽象类的唯一方法是创建一个子类开扩展它，然后在子类中定义具体方法（实现抽象类中每一个抽象方法）。Java强制执行此要求。Java不能编译通过企图实例化抽象类的程序。
   - 这意味着，Bob使用通用业务的通用账号类的方法是他自己创建一个具体的类来扩展它，并且实现抽象方法。如果通用账号类如下：
   - 图【an abstract lass GenericAccount】
   - Bob的账号类如下：
   - 图【extends GenericAccount】
   - Python,一个动态语言，以不同的方式支持“插入点”和抽象类。在Python中，实现一个“插入点”最简单（其他实现抽象方法的方式在附录A中）的方法是定义一个什么都不做的方法，但是提出异常（raise exception）。如果方法没有完全实现，并且有程序调用它，那么就会触发运行时异常。这有一个python写的抽象方法的例子：
   - 图【python abstract method】
   - 一条软件的通用术语是这样的工作方式（定义“插入点”，然后需要插件补充）是“框架”。如果你Google搜索术语“框架”以你会获得这样的一些定义。每个定义都包含框架的一部分。一个框架是：
      - 用于支撑或者封闭其他的一些东西的骨架结构。
      - 一个广泛的概述、大纲或者框架，可想向其中添加细节。
      - 一个可扩展的软件环境，可以根据特定需求定制。
      - 一系列类，为一些应用的问题提供通用的解决方案。一个框架通常精炼以通过专业化或其他类或类型解决特定问题。
      - 一个组件，它允许通过写插件模块（框架扩展）的方式扩展其功能性。扩展的开发者通过从框架中定义的类接口写自己的类。
      - 软件主体设计成高复用模式，加上特定功能的插件以适应特定的系统的功能性需求。当安装了插件，系统将围绕插件表现出相应的行为。
   - 这个框架模式的基本概念是“处理程序模式”。“框架扩展”或者“插件”是“事件处理”模块。
   - 图【framework and entensions(handlers)】

### SAX - an example of a framework (框架举例-SAX)

   - 框架有各种形状尺寸，从大到小都有。开看一下真实的框架是怎么使用的，然我们来看一个小框架：SAX（实际上，SAX不是一个框架。他可以在框架中实现的API。但是为了保持事情简单，我们就当它是个框架）。
   - XML越来越流行。只有一个结果，就是很多开发者第一时间在SAX（一个简单的XML API）框架中遇到了事件驱动编程。SAX是事件驱动的XML语法分析器。它的工作是打开（解析）XML成可理解的片段。例如，SAX分析器可以分析以下字符串：
   - 图【XML string】
   - 解析成如下三个片段：
   - 图【three pieces】
   - 使用SAX解析器，你给他一大块XML字符串。它解析XML文本成不同的片段，然后调用适当的预先确定的插件（事件处理程序）去处理片段。
   - SAX为解析各种XML的特征，诸如打开和关闭标签（startElement，endElement），标签中间的文本，注释，处理指令等等，制定了预定义的插入点。
   - SAX框架提供了“解析器（Parser）”类和一个抽象类“内容处理器（ContentHandler）”。使用它，首先要创建ContentHandler的子类，并且写一个具体的方法（事件处理模块）去覆写抽象方法。这有个用python写的简单例子。它在控制台打印XML的标签名和标签中的数据。（完整的python SAX例子可以在附件B）。
   - 图【over-ride abstract methods】
   - 你扩展“内容处理程序”类和指定事件处理程序：
      - 使用SAX的 make_parser 工厂函数解析器对象。
      - 实例化“CustomHandler”类创建一个“myContentHandler”对象
      - 告诉解析器对象使用“myContentHandler”对象处理XML内容
      - 将XML文本传给解析器，让事件处理模块去工作
   - 这有用Pyhton怎样完成的过程：
   - 图【python parse XML in file】

### Why programming with a framwork is hard (为什么用框架编程很难)

   - 到处理只由输入XML文本给解析器（没有更多了）构成的程序的最后一步了。对于一个面向过程背景的程序员，对事件驱动编程会很困惑。在面向过程编程中，控制的主要流程在主程序之内。附属程序或者模块仅仅是公用程序或者帮助调用展示低等级的任务。主程序的控制流通常很长并且复杂，它的复杂让应用有特定的逻辑结构。程序具有形状，并程序员可以看到该形状。
   - 但是当面向过程的程序员开始使用框架编程，他失去了所有的控制权。没有了清晰的控制流（主程序除了开始了框架的事件循环什么也没做）。并且一旦事件循环开始了，隐藏在框架中的代码就驱动开始动作。该程序剩余的部分仅仅是帮助程序模块（事件处理程序）的集合。总之，程序结构看起来被彻底搞砸了。（框架和公共库是不同的。当使用公共库时，程序员可以完全控制程序流程，当他们需要公共库时调用它们，主动权在程序员。但是在框架中，是在框架需要时，框架负责调用程序员写的事件处理程序模块。主动权在框架。这是谁负责的问题。）
   - 所以面向过程的程序员经常发现，在它们第一次遇到事件驱动和框架驱动编程时觉得完全不能理解！经验和熟悉会逐步减少这种感觉，这是毫无疑问的（从面向过程编程转移到事件驱动编程时一个很大的心里范式的转变）。这就是Robin Dunn 和 Dafydd Rees 在文章开始时描述的范式转变。

## GUI programming (GUI编程)

### Why GUI programming is hard (为什么GUI编程很难)

   - 现在我们看到了框架时如何工作的，来让我们看一下最常用的框架和事件驱动编程：GUIs（图形用户界面 graphical user interfaces）
   - GUI编程时困难的，们个人都这么认为。
   - 首先，只需的指定GUI的外观就需要大量工作。每个窗口小部件（每个按钮、标签、菜单、输入框、列表框等等）都必须告诉它应该长什么样（形状、大小、前景色、背景色、边框样式、字体等等），他应该能确定他自己的位置，比如怎样在GUI大小改变时做出适配，怎样适应整个GUI的层次结构。仅仅是指出GUI怎么展现就有大量的工作。（这就是为什么框架中会有IDEs和屏幕画家。它们的工作就是减轻GUI编程的负担）
   - 第二（与本文主题最相关），GUI编程很难是因为在GUI中有各种事件要处理。几乎每个GUI中的窗口小部件（每个按钮、多选框、单选框、数据输入区域、列表框（包括列表中每个项目）、文本框（包括水平和垂直滑块）、菜单栏、菜单栏图表、下拉菜单、下拉菜单中的每一项 等等很多）。几乎每个都是事件生成器，都能生成各种类型的事件。这还没完，硬件输入设备也是事件生成器。鼠标可以生成左键点击、右键点击、左键双击、右键双击、按钮按下事件（为了初始化拖拽操作）、鼠标移动事件、按钮抬起事件（为了结束拖拽操作）等一些其他事件。键盘上每个字母、数字、可敲击的按键和功能按键（包括单独和与SHIFT、ALT、CONTROL组合的按键）都可以生成事件。有大量的事件被送入“分发”模块的事件循环，GUI程序员必须为每个GUI用户可能生成的事件写好事件处理程序。
   - 第三，事实上每个GUI工具包都以框架的形式提供给程序员。GUI框架（像SAX框架）的目的是减轻GUI程序员的负担。举个例子，GUI框架提供事件循环和事件队列来缓解程序员的工作。但是，正如我们看到的SAX，使用框架就意味着大块的程序控制流被隐藏在框架的封装机器当中，并且对与GUI程序员不可见。这意味着GUI程序员必须掌握范式的转变，转移到事件驱动编程。
   - 这有很多事要处理。这还不是全部，这又“观察者”模式需要掌握……

### The Observer Pattern (观察者模式)

   - 观察者模式在GUI框架的事件驱动编程中使用的很广泛。所以我们用迂回的方式来解释观察者模式。等到这个话题结束，我们将返回到GUI编程上面来，并给出观察者模式是怎样在GUI编程中使用的。
   - 第一次命名和描述观察者模式是在有名的“Gang of Four”的书“设计模式”中（出自 Gamma，Helm， Johonson， Vlissides （Addison-Wesley， 1995））。 观察者模式的基本想法Dafydd Rees引用了导言中的原理。这个原理是“好莱坞原理”：“不要来找我，我会叫你”。当然这个名字来自于演戏或者电影的演员试镜中。导演（电影中掉选角色的人），不想受到想演他的戏还没得到角色的演员找他的困扰，所以他告诉大家，“不要来找我们，我会回来找你”。
   - 好莱坞原理更长的版本是：
      - “不要来找我们，给我们你的电话，当我们想给你一份工作的时候我们会找你”。
   - 这就是观察者模式的本质。
   - 在观察者模式中，有一个主体和多个观察者实体。在试演场景中，导言就是主体，演员就是观察者。当事件发生（导演对他们感兴趣）的时候观察者想要主体通知它们，所以它们在主体那里注册（留下能找到他们的方式说明）。当感兴趣的事情（比如选好了演员）发生时，主体就会通知它们。为了完成这项工作，主体要保存一个列表，列表中记录了所有注册观察者的名字和地址。当有趣的事情发生时，他会通过他的观察者列表去通知在这类事件中注册此类事件的哪个观察者。
   - 观察者模式也叫做“发布/订阅”模式。回想一下订阅报纸的例子。在“发布/订阅”模式中，主体就是信息或出版物的发布者。观察者就是出版物的订阅者。注册的过程叫做订阅，通知的过程叫做发刊。“发布/订阅”是个合适的名字，适用于长时间内重复发生通知/发布的情况，相同的通知发送给多个订阅者，并且订阅者可以取消订阅。
   - 观察者模式是事件处理模式中的特殊例子，原因是我觉得更应该叫做订阅程序处理模式。
   - 下一页使用python实现的观察者模式。在这个例子中，还继续使用好莱坞的主题，观察者是演员。主体是一个天才代理人叫做HotShots。演员在天才代理人那里注册，当有一个演员角色的试镜，代理人会通知演员。自从代理人不断地给演员发通知，这个例子就有了发布/订阅模式的味道。
   - （如果你是Java程序员，请不要担心，事实上我们是想减少定义CastingCall和Observer类。这在像python这种动态语言中是可能的，现在这里是为了保持代码简短。在Java中，你需要声明类和它们的实例变量的细节）。
   - 图【Python showing Observer pattern】
   - 正如你在代码中看到的那样，天才代理人（主体）是它自己的事件处理程序。具体的说，他的“notify”方法是“CastingCall”事件的事件处理程序。
   - 天才代理人通过发送“CastingCall”到它自己的“notifyActors”方法来处理“CastingCall”。“notifyActors”方法通过通过代理人的订阅人/观察者/演员列表找到相应人选并且发送通知给合适的订阅者。
   - 在真实的应用中，通知的过程将包括与每个演员（事件处理程序）联系，演员会通过选角的方式来回应。然而，在这个简单的例子中，通知的过程只包含了打印一个信息来说明这个演员被通知了。
   - 如果你运行这个程序，会得到如下输出：
   - 图【Python showing Observer pattern Output】

### Event Objects (事件对象)

   - 这个程序有个功能，你应该特别注意。它使用了CastingCall对象的时候带着参数（实例化变量）“source”和“role”。在这里，CastingCall对象是事件对象（控制事件的对象）。
   - 事件对象是事件驱动编程中非常好的工具。在面向对象的编程语言中，事件或者事务都是极端受限的。在一些案例中，如果你想发送一个事件到以事件处理器中，你发送的所有都是一个包含事务代码的字符串。但是面向对象技术通过允许我们创建和忽略事件对象完全的改变了它。事件对象的本质是将我们需要的时间信息打包起来。
   - 一个事件对象当然也可以携带触发它事件种类的名字。但是，依赖与应用，它可以携带更多信息。在这个HotShots天才代理的案例中，事件对象包含选角色的“source”和role信息。
   - 在其他应用中，我们可能想放入大量完整的信息到我们的事件对象中。考虑一下著名的Model-View-Controller（MVC）模式。观察者模式就是MVC的核心。在MVC中，Model是一个管理数据和一些应用域的对象（它是观察者模式中的主体）。Views在Model中作为观察者注册。当Controller改变了Model，Model就会通知订阅它的观察者（Views）这个（Model）改变了。
   - 最简单的MVC版本被叫做“pull”（拉取）版本。在这个版本中，事件对象（当Model变化时，Model向Views发送的通知）几乎不包含什么信息。Model的描述没有改变，他只是通知Views发生了某种变化。当Views收到了这样的通知，它们必须从Model拉取信息。这样，它们必须询问Model当前状态的信息，并且从这些信息中刷新它们自己的状态。
   - 大多数复杂的MVC版本叫做“push”（推送）版本。在这个版本中，Model 推送变化信息给Views。事件对象发送给Views，对象包含了大量复杂的信息（Model产生的一个完整详尽的变化描述）。当Views收到了这些信息，它就拥有了所有信息，他会根据这些信息更改他自己。
   - MVC的“push”和“pull”版本的根本区别简单来说就是放入事件对象包中的信息总数不同。

### The Registered Handlers pattern in GUI applications (GUI应用程序中的“注册处理程序”模式)

   - 现在你已经熟悉了基本的 观察者/注册 处理程序模式，来让我们看看这个模式在GUI编程中是如何使用的。
   - 这有另一个程序的代码。在结构上，这段代码和Hotshots天才代理的代码很像，但是名字用了处理程序模式的专用术语，事件是GUI事件。
   - 在这个例子中，“主体”是分发模块（工作在GUI中的事件循环）。
   - 为了保持代码简短，程序只包含观察者（“demoHandler”函数是双击鼠标左键的事件处理函数）。
   - 展示这个处理程序的动作，程序生成一个模仿鼠标左键双击的事件（LeftMouseDoubleClick 事件）。
   - 图【Python showing Registered Handlers pattern】
   - 如果你运行这个代码，会得到如下输出：
      - 图【Handling LeftMouseDoubleClick from mouse】
   - 在如下语句中：
      - 图【demoDispatcher.registerObserver( demoHandler, MOUSE_LEFT_DOUBLE)】
   - 和这句：
      - 图【observer.eventHandler = argEventHandler】
   - 程序在床底“demoHandler”函数对象的引用。它这是将函数作为“全功能对象”（支持所有操作，基本的包括：作为参数传递、从函数返回、赋值给其他变量）。这不是所有的编程语言都支持的，也就是意味着，在不同的编程语言中，实现观察者/注册处理程序模式的方式会很不一样。

### Registering Event-Handlers in Python - "Binding" (Pyhton中的注册事件处理程序-“绑定”)

   - 这些是“注册处理程序”模式后的基本思想。现在让我们看一下在Python和Java的GUI应用中“注册处理程序”模式是什么样的。
   - 开源动态语言如：Python、Perl 和 Ryby 通常提供多样的开源GUI框架接口开支持GUI编程。在Python中，也提供了一些这样的接口。最流行的是tkinter（向Tcl/Tk提供的接口）和wxPython（向wxWidgets提供的接口）。
   - 使用这些接口的基本概念是非常相似的。在接下来的讨论中，我会用Python和tkinter的例子，来阐述在GUI编程中“注册处理程序”模式的样子。
   - 在Python和tkinter中，所有的GUI事件都术语一个单独的类：“event”。事件处理程序带着GUI窗口小部件（按钮之类的）注册到程序中，为了处理特定类型的事件，比如鼠标点击，按键按下。在PYthon中，注册一个事件处理程序的过程叫做“binding”（）绑定。
   - 这有个简单的例子。假设我们的程序已经有了一个事件处理程序（一个函数或者方法）叫做“OKButtonEventHandler”。它的工作是处理发生在GUI中的“OK”按钮上的事件。
   - 注意，在Python中没有特别或者神奇的事情发生，Python中没有叫做“OkButtonEventHandler”的程序（只要我们想，我们可以将这个程序的名字叫做“Floyd”或者“Foobar”都可以）。我们将要看到是Python和Java中唯一不同的地方。
   - 接下来的代码片段创建了在观察者模式中的“subject 主体”窗口小部件。这个主体是一个GUI窗口小部件（一个按钮），它可以展示文本“OK”。这个OK按钮对象是一个Tkiner.Button类的实例对象。
   - 图【OkButton = Tkinter.Button(parent, text="Ok")】
   - 在调用Button类的构造函数中，“parent”参数链接了按钮对象到其所有者的GUI对象上（可能是框架或者窗口）。
   - Tkinter小部件提供了一个叫做“bind 绑定”的方法来将事件绑定到窗口小部件上。更精确的说，“bind”方法提供了一种绑定或者说结合三个不同的事情：
      - 一个事件类型（例如，鼠标左键点击，或者在键盘按下回车键）。
      - 一个窗口小部件（例如，在GUI上一个特定的按钮窗口小部件）。
   - 例如，我们想在窗口的“关闭”按钮上绑定一个鼠标左键单击的函数或者方法：“closeProgram”。想要的效果是，当用户鼠标点击“关闭”按钮时，调用“closeProgram”关闭窗口。
   - 这有一个代码片段，绑定了一个OKButtonEventHandler程序和键盘事件（“\<Return>”）的组合到OKButton窗口小部件：
   - 图【OkButton.bind("\<Return>", OkButtonEventHandler)】
   - 该语句将作为观察者的OkButton窗口小部件和OkButtonEventHander函数注册到键盘事件“\<Return>”中（当Ok按钮对象获得键盘焦点时发生的事件）。
   - 这有另一段代码片段（可能就在同一个程序中发生，就在上一个代码之后）。它绑定了OkButtonEventHandler程序和鼠标左键点击事件到Ok按钮窗口小部件：
   - 图【OkButton.bind("\<Button-1>", OkButtonEventHandler)】
   - 当这两个事件中的任何一个发生了，事件将事件对象作为参数发送给OkButtonEventHandler函数。OkButtonEventHandler函数可以（如果他想）询问事件对象，并且决定是否触发一个按键按下或者鼠标点击事件。

### Registering Event-Handlers in Java - "listeners" (Java中的注册事件处理程序-“监听”)

   - Java也支持GUI事件处理程序注册技术，但是它的方式和Python的有些不一样。
   - **Pass**

### Callback programming (回调编程)

   - 当你读GUI框架的文档的时候，你会注意到观察者/事件处理程序叫做“callback”，因为主体窗口小部件会“回调”它们去处理事件。所以你经常看到这种编程类型叫做“回调编程”。

### GUI programming - summary (GUI编程-概要)

   - 这里我们全身心的投入到GUI应用中事件驱动编程的论题中。或者应该说，我们讨论的是GUI编程的上下文环境中的处理程序模式。
   - 从根本上来说，GUI编程和其他那些我们见过的处理程序模式的例子没有太多不同。GUI编程和其他处理程序模式的形式不同在于，它大多总是牵扯到“观察者模式”。这就是为什么：GUI城边大多总是包含注册或绑定的过程（事件处理程序（观察者）捆绑到（注册到）事件生成器（主体））。
   - 注册的过程开起来就是基础的事件处模式上的一个小变化（这只是一个结合事件处理程序和事件生成者的特殊方式）。

## Maintaining State (维持状态)

   - 许多事件驱动应用程序时无状态的。者意味着当应用程序处理完成了一个事件，应用程序没有被事件改变。
   - 与无状态应用对立的时有状态应用。有状态的应用程序可以通过事件来改变应用的进程。具体来说，有状态程序可以记住或保持事件之间的信息，它们所记住的信息（它们的状态信息）可以通过事件来改变。
   - 锤子是个无状态的工具，如果你要钉钉子，锤子就和你钉钉子之前没区别，锤子还是之前那样。订书机时有状态的工具，如果你用它钉一些纸，钉的这个动作就会改变它的状态（在钉这个动作之后，订书机就会比之前少一个订书钉）。
   - 在大多数基本Web游览的类型中，一个Web服务器收到一个展示的特定页的请求，返回请求页，在结束之后什么都没记住。这就是无状态的应z。大多数有经验的网页会展示访问次数（“自从2000年1月1日，这个页面已经被访问了876532次”）。要做到这个，这个应用的背后网页必须记住被访问的次数，并且在每次网页被访问时增长次数。这个应用就是有状态的。

### Rejection invalid transactions (拒绝无效事务)

   - 在之前的讨论中，我们注意到面向对象编程是一种事件驱动编程。在大多数案例中，面向对象编程包括的对象都是有状态的。比如一个“栈”在它的实例化变量中维持着它的状态，在下图中这个区域标记成“internal data 内部数据”。
   - 图【Stack object diagram】
   - 当我们实例化Stack类去创建stack对象时，stack开始为空。
   - 现在假设下列事件序列发生：
      - 一个“push(X)”事件到达。X是添加到栈中，改变栈的状态。
      - 一个“pop()”事件到达。X从栈中移除（再次改变栈状态）并且返回。栈现在空了。
      - 另一个“pop()”事件到达。
   - 现在我们遇到了一个问题。栈不能满足此需求。它不能移除并返回栈顶元素，因为栈顶没有元素了；栈是空的。
   - 这个例子阐述了重要的有状态应用的特性。一般来说，有状态应用定义了（至少隐式的）可接受“状态+事务”类型对的列表。在列表中的每一对，当应用处于指定状态时，应用可以处理指定类型的事务。但是如果到达的事务在当前的状态中不可接受，事务必须被拒绝。
   - 这有很多相似的这种例子：
      - 一般来说，一个成年人如果他想结婚，他就可以结婚，但是如果他已经结婚了，那么就不能再结婚了（X的配偶由文化决定）。
      - 一般来说，你可以从你的银行账户存取钱，但是你不能从你的账户中取出更多的钱。
      - 一般来说，你可以兑换你的飞行常客里程以免费飞行，但是航空公司不允许你再繁忙旅行的“断电时段”兑换，比如感恩节前一天。
      - 一般来说，数据库应用可以再数据中中更新一条记录，但是它不能更新被作者应用锁定的记录。
   - 应用程序由多种方法来相应无效的事务。当然最简单的就是忽略它。但是，一般来说,最好的相应方式是提出一个异常，并让被提交事务的模块处理异常。
   - 依赖于使用语言的能力，应用提出一个通用异常并伴随一条描述错误的信息，或者可以对指定的问题类型，定义指定的异常。一个栈的实例可以定义为：
      - “StackEmptyException” 栈空异常，当栈是空的并且受到“Pop()”请求时提出此异常。
      - “StackOverflowException” 栈溢出异常，当栈满了，并且收到了“Push()”请求时提出此异常。

### State Machines (状态机)

   - 在一些案例中，状态机是有效的思考具有生命周期对象（有状态的计算机应用）的方法。在它的生命周期中，有状态对象通过相应事务（事件）来在不同状态之间切换。
   - 对象的这种行为叫做“有限状态机 Finite State Machine”（FSM）或者“定向有限状态机 Directed Finite Automaton”（DFA）。描述有限状态机的典型方式是用“状态事务图 State Transition Diagram”（STD）。在STD中，圆圈代表状态，用事务走向的事件名标记的箭头代表了事务。
   - 这有个STD的例子，它代表了一个人的生活和婚姻历史（由文化规定，同一时间只能由一个配偶）。
   - 图【STD for life and marriage history of a person】
   - 在STD中，状态是：“SINGLE 单身”、“MARRIED 已婚”、“DEAD 死亡”。当一个人开始他的生活，它是单身的（开始的状态是SINGLE）。他可能还没有结婚就死亡了，或者他结婚了。他可能在结婚之后死亡，或者通过离婚或丧偶重新回到单身。DEAD是“终点状态”（没有事件事务从这里出去）。
   - 这个Person类的伪代码如下：
   - 图【Person class】
   - 在上述我们讨论的状态中，我们使用“state 状态”和“state information”状态信息指人的所有有状态信息，是对象实例化变量的完整集合。但是当讨论到STD和FSM时，我们在有限状态机中使用“state”这个词去指特定的没有关联的状态。
   - 遗憾的是，还没有标准术语来区分“state”这个单词的两种意思。Michael Jackson 使用术语“state vector 状态向量”来指第一种情况（“vector”变量列表包含一个对象的状态信息）。程序员经常使用“status”（如“status_flag”,“status_indicator”）来指第二种情况。
   - 在本文余下部分，一般上下文会指出我用的是哪种“state”。当有可能产生混淆时，我会使用“state vector”（或者“state_information”）和“status”去让事情保持清晰。