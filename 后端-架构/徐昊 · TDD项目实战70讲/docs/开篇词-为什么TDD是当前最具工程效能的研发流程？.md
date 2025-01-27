你好，我是徐昊，欢迎和我一起学习测试驱动开发（Test-Driven Development，TDD）。

对于测试驱动开发，稍有了解而全无实践的人，会认为是天方夜谭，甚至无法想象为什么需要这样的方式来开发：

1. 为什么要开发人员来写测试？这难道不是测试人员的工作吗？难道开发写了测试，测试人员就不用再测了嘛？
2. 又要写测试，又要写生产代码，效率是不是太低了？只写生产代码效率应该更高吧？
3. 不写测试我也能写出可以工作的软件，那么写测试能给我带来什么额外的好处呢？

的确，从直觉上来看，测试驱动开发相当令人困惑：它将我们通常认为的辅助性工作——测试，作为程序员编码的主要驱动力；它主张通过构造一系列自动化测试（由程序员编写），为编写生产代码（Production Code）做指引；它甚至建议，如果不存在失败的测试，就不要编写生产代码。

看起来，似乎测试驱动开发有些 **过分强调测试** 对于程序员的重要性了。

那么我们就需要仔细思考，“测试”在所谓的“正常软件开发模式”中，到底发挥着怎样的作用。当明晰了测试驱动开发的这个核心逻辑之后，我们才能讨论是不是过分强调了。

## 隐式程序员测试（Implicit Developer Testing）

直觉和经验告诉我们，在所谓的“正常软件开发模式”中，貌似测试只是最后的验收步骤，程序员很少直接参与。但事实却不是这样，就算是所谓的“正常软件开发模式”，也蕴含着非常多“程序员测试”的步骤。只不过这些“程序员测试”并不表现为自动化测试，而是由“测试应用”、“跑一下”和“调试”等隐含手段体现的。

### “测试应用”（Testing Application）

所谓“测试应用”并不是某个技巧正式的名字，但是所有人都熟知这一技巧：

- 构造一个简单的控制台应用（Console Application），并提供main入口函数（Entry point）；
- 在main函数中，调用所编写的代码，并通过与控制台的交互（各种println、writeline之类的函数），将结果输出在控制台上；
- 再通过观察控制台的输出，判断结果正确与否。

让我们看一个具体的例子。假设我们需要将某个对象存储到数据库中，以Java中的JPA（Jakarta Persistence API）为例，那么我们大概可以构造出这样的“测试应用”：

如视频中展示的，这个测试应用符合我们对于验证测试的一切认知：有需要被测试的行为，有明确的执行结果，以及针对结果的验证。那么我们实际上可以很容易地将它改写为自动化测试：

对比这两种做法，从意图上，我们可以粗略地认为，它们是对于同一种意图的两种不同实现： **无计划的手动验证** 与 **有计划的自动化验证**。所以如果你曾经使用过“测试应用”，那么你就曾经在项目中做过“程序员测试”。

### “跑一下”（Run it in a local testing environment）

同样，“跑一下”也不是某个技巧的正式名字。从严谨的角度出发，“跑一下”甚至不能算是它真正的名字。它真正的名字应该叫“在我本地的测试环境中跑一下”。同样，所有人也都熟知这一技巧，就真的是“在我本地的测试环境中跑一下”。因为当代应用通常都在受控环境中运行（Managed Environment），所以当验证某个功能时，需要连通其所在的受控环境一起执行。

让我们再看一个具体的例子。假设我们需要实现REST API，以Java中的JAX-RS（Jakarta Restful WebService）为例，那么我们大概会这样来跑一下：

如视频中展示的，无论是通过浏览器直接观测结果，还是通过Postman验证，都符合我们对于验证测试的一切认知：有需要被测试的行为，有明确的执行结果，以及针对结果的验证。那么我们实际上可以很容易地将它改写为自动化测试：

对比这两种做法，从意图上，我们可以粗略地认为，它们是对于同一种意图的两种不同实现： **无计划的手动验证** 与 **有计划的自动化验证**。所以如果你曾经也类似这样“跑过一下”你的应用，那么你就曾经在项目中做过“程序员测试”。

### “调试”（Debug）

我想你已经发现了模式，你肯定要猜测“调试”也是一种验证测试，但并不是这样！“测试应用”和“跑一下”这两种技巧更多地关注在发现问题上，可以看作是“ **验证测试**”。而“调试”通常发生在已经明显知道有错误的代码中，是一个定位错误的过程。让我们来看个例子：

如视频中展示的，“调试”是一种启发式过程（Heuristic Procedure），更像是探索测试（Exploratory Testing），根据出现的错误寻找可能出现错误的位置，然后设置断点，判断该断点处的状态是否正确。

除了调试之外，我们还可以将代码划分成更小的单元，逐一排查以定位错误。那么我们就可以将对于某段代码的调试过程，转化成对于一组更小粒度单元的验证测试：

在软件开发中，一直都存在 **验证性测试** 和 **定位性测试** 两种测试。这也很好理解，我们既要知道代码有没有错误，还要知道当错误发生时，错误发生在哪里。

从定位性测试的角度出发，对比这两种做法，从意图上，我们可以粗略地认为，它们是对于同一种意图的两种不同实现： **手动的启发式定位** 与 **有计划的逐模块自动化排查**。所以如果你曾经也类似这样“调试”过你的应用，那么你就曾经在项目中做过“程序员定位测试”。

## 测试驱动开发的核心逻辑

除去我们讨论的三种，在所谓的“正常软件开发模式”中，还存在很多其他常用的手段，也都可以看作是自发性的“程序员测试”。任何有过严肃编程经验的从业者，都能根据自己过往的经历，回想起这些年所做过的“程序员测试”。

我们构造软件的过程，就是通过一系列验证测试（测试应用、跑一下等），证明我们在朝着正确的方向前进；如果验证的过程中发现出了错误，那么再通过一系列定位测试（调试等），找到问题的根源，加以改进。

如此往复，直到完成全部功能（如下图所示）。

![](https://static001.geekbang.org/resource/image/a7/3d/a70a4edfd0bbd5554175yy832a75d63d.jpg?wh=2284x912)

现在让我们回到最初的问题：测试驱动开发过分强调测试对于程序员的重要性了吗？答案是：并没有！

验证测试与定位测试，本身就贯穿了整个软件构造的过程。 **测试构成了整个开发流程的骨架，功能开发可以看作填充在测试与测试之间的血肉**。这就是测试驱动开发的核心逻辑：以测试作为切入点，可以提纲挈领地帮助我们把握整个研发的过程。

一个个测试就像一个个里程碑点（Milestone），规划着研发活动的进度。围绕这些里程碑点，我们就可以持续对成本和效率进行管理和改进。 **也就是说，测试驱动开发将个体的开发活动，变成了工程化的研发过程。** 这也是为什么，三十年以来，测试驱动开发在敏捷方法族中，都扮演着工程实践基石的角色。

因为测试是如此重要，我们需要非常高效地实现它们。那么“ **无计划的手动验证**”与“ **手动的启发式定位**”都是无法容忍的低效手段，必须将它们替换为“ **有计划的自动化验证测试**”和“ **有计划的逐模块自动化排查**”。从而才有了我们熟知的测试驱动开发（红/绿/重构循环），以及令没有做过测试驱动开发的人费解的对于自动化的偏执。

到这里，我想你应该就会明白了，测试驱动开发并不是关于“怎么写测试”、“怎么消除测试人员”、“怎么让开发人员多干一份活”的编码技巧。它是以测试为里程碑点的工程化研发过程；同时，测试驱动开发将软件流程中无时无处不在的低效测试手段，以可重复的、高效的自动化测试代替，以获得更高的工程效能。

这就是隐藏在测试驱动开发反直觉的工程实践背后的核心逻辑。

## 为什么要学习测试驱动开发？

测试驱动开发最直接的收益，就是可以提高开发者的 **工程效能**。

工程效能不仅仅是开发功能的效能，还包含发现问题、定位问题以及修复问题的效能。从理论上来说，后面三个并不是根本的复杂性问题，但在实际中却大量存在，甚至占据一半以上的有效工作时间。因而高效地完成这些非根本性问题，就可以显著地提高效率。

其中发现错误，并准确定位错误，通过发现问题的测试和定位问题的测试可以高效实现。而如果说发现问题的测试，还有后置或外包于他人的可能，那么定位问题的测试，无论如何都没有办法了。所以实际上 **高效能的研发过程**， **至少** 需要我们提供 **可工作的代码**，以及一组可用于 **定位问题的测试**。

从这个角度出发，那么测试驱动开发仍是时至今日最具有工程效能的研发流程，没有之一。

## 学习测试驱动开发的难点在哪里？

学习测试驱动开发是困难的，很多信服于测试驱动开发理念而自发实践的人也会被各种问题困扰：

1. 测试从哪里来？为什么我写了很多测试，功能却没有进展？
2. 写什么样的测试既能驱动功能进展，又不会在重构中被破坏？
3. 社区里很多人都非常推崇单元测试，但我就是要测一段SQL，单元测试怎么测？

测试驱动开发从来都不是一种即插即用的技能，它是一种工作习惯和思维方式，背后还对深层的胜任力（Competency）——分析性思考有极高的要求。某种程度上讲，测试驱动开发有点像物理，定理写出来很简单，但需要我们 **在不同的场景下练习**，才能应用得得心应手。

正是因为这样的特点，我们的课程以视频 **展示为主**，文字讲解为辅。希望你能在具体的场景下，体会使用TDD和平时开发的差异。具体而言，我们的课程是这样设计的。

首先我们将从一个编码练习（Code Kata）级别的小例子入手， **展示** 使用TDD开发的全过程。因为大多数人，对于TDD没有一个感性、直观的认识。因而在任何讲解之前，我需要让你亲眼看一看，如何通过TDD的方法实现一个非常简单的功能。

然后我们会围绕这个例子，详细讲解TDD的核心理念与方法。我们将深入讨论TDD中的测试到底是什么样的测试，TDD是如何驱动我们的开发。我们会介绍TDD的经典学派（芝加哥学派）与伦敦学派不同的切入点。

在这部分的最后，我将会总结TDD作为工程方法的核心优势在什么地方。如果你需要说服周围的同事、朋友、领导开始采用TDD方法，这将给你提供足够的弹药。

然后我们将进入实战项目环节。我将以几个技术框架为例（IoC容器、RESTful框架、DB Mapper框架等），展示如何使用TDD的方式从头来实现它们，TDD实战的细节将一览无遗。

![](https://static001.geekbang.org/resource/image/03/48/03d9471cfe3c52c13b678771b1eab348.jpeg?wh=2062x1370)

## 开篇寄语

作为中国最早一批测试驱动开发的实践者，从2003年开始，我就将测试驱动开发作为主要工作方式了。

在加入Thoughtworks之后，对内对外我讲述了大量测试驱动开发的课程。曾经有一段时间，每一位新入职的Thoughtworker我都会通过6周的时间，教会他们进行测试驱动开发。

当我主持Thoughtworks委培生计划——小巨人项目（Small Giant Program）时。测试驱动开发与学习管理，是最早也是最重要的工作习惯。近年我研发的高效能工程方法SEELE（Scalable Engineering Experience for Large Enterprise）也是将测试驱动开发作为核心流程，从而简化知识传递成本并提高杠杆率。

测试驱动开发伴随了我职业生涯的每一个阶段。我相信，我掌握了测试驱动开发那天，我才成为了 **可靠、高效的职业程序员**。如果你对程序员这个职业抱有严肃的态度，那么测试驱动开发是必须要掌握的。

最后，我也很想听听你对TDD的看法，以及实践TDD过程中的体会。欢迎你分享在留言区，我们下节课再见！