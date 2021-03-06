---
title: 深度学习框架比较：Deeplearning4j、Torch、Theano、Caffe和TensorFlow
layout: cn-default
redirect_from: /zh-compare-dl4j-torch7-pylearn
---

# DL4J与Torch、Theano、Caffe、TensorFlow的比较

Deeplearning4j不是第一个开源的深度学习项目，但与此前的其他项目相比，DL4J在编程语言和宗旨两方面都独具特色。DL4J是基于JVM、聚焦行业应用且提供商业支持的**分布式深度学习框架**，其宗旨是在合理的时间内解决各类涉及大量数据的问题。它与Hadoop和Spark集成，可使用任意数量的GPU或CPU运行，而且发生任何问题都可以[联系服务热线](http://www.skymind.io/contact)。

<br />

<p align="left">
<a href="quickstart" type="button" class="btn btn-lg btn-success" onClick="ga('send', 'event', ‘quickstart', 'click');">Deeplearning4j入门教程</a>
</p>

<br />

### 目录

* <a href="#tensorflow">TensorFlow</a>
* <a href="#theano">Theano、Pylearn2及其生态系统</a>
* <a href="#torch">Torch</a>
* <a href="#caffe">Caffe</a>
* <a href="#cntk">CNTK</a>
* <a href="#dsstne">DSSTNE</a>
* <a href="#licensing">许可</a>
* <a href="#speed">速度</a>
* <a href="#java">DL4J：为什么用Java？</a>
* <a href="#ecosystem">DL4J：生态系统</a>
* <a href="#scala">DL4S：基于Scala语言的深度学习</a>
* <a href="#ml">机器学习框架</a>
* <a href="#tutorial">扩展阅读</a>

### <a name="tensorflow">TensorFlow</a>

* 目前**TensorFlow**还不支持所谓的“内联（inline）”矩阵运算，必须要复制矩阵才能对其进行运算。复制非常大的矩阵会导致成本全面偏高。TF运行所需的时间是最新深度学习工具的四倍。谷歌表示正在解决这一问题。
* 和大多数深度学习框架一样，TensorFlow是用一个Python API编写的，通过C/C++引擎加速。这种解决方案并不适合Java和Scala用户群。
* TensorFlow的用途不止于深度学习。TensorFlow其实还有支持强化学习和其他算法的工具。
* 谷歌似乎也已承认TF的目标是招募人才。众所周知，他们最近公布了为期一年的谷歌大脑（Google Brain）人才培训项目。真是明智的举措。
* TensorFlow不提供商业支持，而谷歌也不太可能会从事支持开源企业软件的业务。谷歌的角色是为研究者提供一种新工具。
* 和Theano一样，TensforFlow会生成计算图（如一系列矩阵运算，例如z = simoid(x)，其中x和z均为矩阵），自动求导。自动求导很重要，否则每尝试一种新的神经网络设计就要手动编写新的反向传播算法，没人愿意这样做。在谷歌的生态系统中，这些计算图会被谷歌大脑用于高强度计算，但谷歌还没有开放相关工具的源代码。TensorFlow可以算是谷歌内部深度学习解决方案的一半。
* 从企业的角度看，许多公司需要思考的问题在于是否要依靠谷歌来提供这些工具。
<br />

#### 利与弊:

* (+) Python + NumPy
* (+) 与Theano类似的计算图抽象化
* (+) 编译时间比Theano快很多
* (+) 用TensorBoard进行可视化
* (+) 同时支持数据并行和模型并行
* (-) 速度比其他框架慢
* (-) 比Torch笨重许多；更难理解
* (-) 已预定型的模型不多
* (-) 计算图纯粹基于Python，所以速度较慢

### <a name="theano">Theano及其生态系统</a>

深度学习领域的学术研究者大多依赖[**Theano**](http://deeplearning.net/software/theano/)，Theano是深度学习框架中的元老，用Python编写。Theano和NumPy一样，是处理多维数组的学习库。Theano可与其他学习库配合使用，非常适合数据探索和研究活动。

现在已有大量基于Theano的开源深度学习库，包括[Keras](https://github.com/fchollet/keras)、 [Lasagne](https://lasagne.readthedocs.org/en/latest/)和[Blocks](https://github.com/mila-udem/blocks)。这些学习库试着在Theano有时不够直观的界面之上添加一层便于使用的API。（截至2016年3月，另一个与Theano相关的学习库[Pylearn2似乎已经停止开发](https://github.com/lisa-lab/pylearn2)。）

相比之下，Deeplearning4j的目标是成为深度学习领域的Scikit-learn，力求以可扩展、多个GPU或CPU并行的方式让尽可能多的控制点实现自动化，在需要时与Hadoop和[Spark](../spark)集成。
<br />

#### 利与弊:

* (+) Python + NumPy
* (+) 计算图是良好的抽象化方式
* (+) RNN与计算图匹配良好
* (-) 原始的Theano级别偏低
* (+) 高级的包装界面（Keras、Lasagne）减少了使用时的麻烦
* (-) 错误信息可能没有帮助
* (-) 大型模型的编译时间可能较长
* (-) 比Torch笨重许多；更难理解
* (-) 对已预定型模型的支持不够完善

### <a name="torch">Torch</a>

[**Torch**](http://torch.ch/)是用Lua编写的计算框架，支持机器学习算法。谷歌DeepMind、Facebook等大型科技公司使用Torch的某些版本，由内部团队专门负责定制自己的深度学习平台。Lua是上世纪九十年代早期在巴西开发的多范例脚本语言。

Torch7虽然功能强大，[但其设计并不适合在两个群体中大范围普及](https://news.ycombinator.com/item?id=7929216)，即主要依赖Python的学术界，以及普遍使用Java的企业软件工程师。Deeplearning4j用Java编写，反映了我们对行业应用和使用便利的重视。我们认为可用性是阻碍深度学习实施工具广泛普及的限制因素。我们认为可扩展性应当通过Hadoop和Spark这样的开源分布式运行时系统来实现自动化。我们还认为，从确保工具正常运作和构建社区两方面来看，提供商业支持的开源框架是最恰当的解决方案。
<br />

#### 利与弊

* (-) Lua
* 通常需要自己编写定型代码（即插即用相对较少）
* (+) 大量模块化组件，容易组合
* (+) 很容易编写自己的层类型并在GPU上运行
* (+) Lua.;) （大多数学习库的代码是Lua，容易读取）
* (+) 有很多已预定型的模型！
* (-) 不适合递归神经网络

### <a name="caffe">Caffe</a>

[**Caffe**](http://caffe.berkeleyvision.org/)是一个广为人知、广泛应用的机器视觉库，将Matlab实现的快速卷积网络移植到了C和C++平台上（[参见Steve Yegge关于一个芯片一个芯片地移植C++代码的博客，可以帮助你思考如何在速度和这种特定的技术债务之间进行权衡](https://sites.google.com/site/steveyegge2/google-at-delphi)）。Caffe不适用于文本、声音或时间序列数据等其他类型的深度学习应用。与本文提到的其他一些框架相同，Caffe选择了Python作为其API。

Deeplearning4j和Caffe都可以用卷积网络进行图像分类，这是最先进的技术。与Caffe不同，Deeplearning4j*支持*任意芯片数的GPU并行运行，并且提供许多看似微不足道，却能使深度学习在多个并行GPU集群上运行得更流畅的功能。虽然在论文中被广泛引述，但Caffe主要用于为其Model Zoo网站提供已预定型的模型。Deeplearning4j正在开发将Caffe模型导入Spark的[开发解析器](https://github.com/deeplearning4j/deeplearning4j/pull/480)。
<br />

#### 利与弊：

* (+) 适合前馈网络和图像处理
* (+) 适合微调已有的网络
* (+) 定型模型而无需编写任何代码
* (+) Python界面相当有用
* (-) 需要用C++ / CUDA编写新的GPU层
* (-) 不适合递归网络
* (-) 用于大型网络（GoogLeNet、ResNet）时过于繁琐

### <a name="cntk">CNTK</a>

[**CNTK**](https://github.com/Microsoft/CNTK)是微软的开源深度学习框架。CNTK的全称是“计算网络工具包。”此学习库包括前馈DNN、卷积网络和递归网络。CNTK提供基于C++代码的Python API。虽然CNTK遵循一个[比较宽松的许可协议](https://github.com/Microsoft/CNTK/blob/master/LICENSE.md)，却并未采用ASF 2.0、BSD或MIT等一些较为传统的许可协议。

### <a name="dsstne">DSSTNE</a>

亚马逊的深度可伸缩稀疏张量网络引擎又称[DSSTNE](https://github.com/amznlabs/amazon-dsstne)，是用于机器学习和深度学习建模的学习库。它是众多最新的开源深度学习库之一，在Tensorflow和CNTK之后发布。DSSTNE主要用C++写成，速度较快，不过吸引到的用户群体规模尚不及其他学习库。

### <a name="licensing">许可</a>

上述开源项目的另一区别在于其许可协议：Theano、Torch和Caffe采用BSD许可协议，未能解决专利和专利争端问题。Deeplearning4j和ND4J采用**[Apache 2.0许可协议](http://en.swpat.org/wiki/Patent_clauses_in_software_licences#Apache_License_2.0)**发布。该协议包含专利授权和防止报复性诉讼的条款，也就是说，任何人都可以自由使用遵循Apache 2.0协议的代码创作衍生作品并为其申请专利，但如果对他人提起针对原始代码（此处即DL4J）的专利权诉讼，就会立即丧失对代码的一切专利权。（换言之，这帮助你在诉讼中进行自我防卫，同时阻止你攻击他人。）BSD一般不能解决这个问题。

### <a name="speed">速度</a>

Deeplearning4j依靠ND4J进行基础的线性代数运算，事实表明其处理大矩阵乘法的[速度至少是NumPy的两倍](http://nd4j.org/benchmarking)。这正是DL4J被NASA的喷气推进实验室所采用的原因之一。此外，Deeplearning4j为多芯片运行而优化，支持采用CUDA C的x86和GPU。

虽然Torch7和DL4J都采用并行运行，DL4J的**并行运行是自动化的**。我们实现了从节点（worker nodes）和连接的自动化设置，让用户在[Spark](https://github.com/deeplearning4j/deeplearning4j/tree/master/deeplearning4j/deeplearning4j-scaleout/spark)、[Hadoop](https://github.com/deeplearning4j/deeplearning4j/tree/master/deeplearning4j/deeplearning4j-scaleout/hadoop-yarn)或[Akka和AWS](http://deeplearning4j.org/scaleout.html)环境中建立大型并行网络时可以绕过学习库。Deeplearning4j最适合快速解决具体问题。

Deeplearning4j的所有功能参见[功能介绍](features)。

### <a name="java">为什么用Java？</a>

经常有人问我们，既然有如此之多的深度学习用户都专注于Python，为什么还选择Java来实施开源深度学习项目。的确，Python有着优越的语法要素，可以直接将矩阵相加，而无需像Java那样先创建显式类。Python还有由Theano、NumPy等原生扩展组成的广泛的科学计算环境。

但Java也具备不少优点。首先，Java语言从根本上看要快于Python。如不考虑依赖用Cython加速的情况，任何用Python写成的代码在根本上速度都相对较慢。不可否认，运算量最大的运算都是用C或C++语言编写的。（此处所说的运算也包括高级机器学习流程中涉及的字符和其他任务。）大多数最初用Python编写的深度学习项目在用于生产时都必须重新编写。Deeplearning4j依靠[JavaCPP](https://github.com/bytedeco/javacpp)从Java中调用预编译的本地C++代码，大幅提升定型速度。

其次，大型企业主要使用Java或基于JVM的系统。在企业界，Java依然是应用范围最广的语言。Java是Hadoop、Hive、Lucene和Pig的语言，而它们恰好都是解决机器学习问题的有用工具。也就是说，深度学习本可以帮助许多需要解决现实问题的程序员，但他们却被语言屏障阻碍。我们希望提高深度学习对于这一广大群体的可用性，这些新的用户可以将深度学习直接付诸实用。

第三，为了解决Java缺少强大的科学计算库的问题，我们编写了[ND4J](http://nd4j.org)。ND4J在分布式CPU或GPU上运行，可以通过Java或Scala的API进行对接。

最后，Java是一种安全的网络语言，本质上具有跨平台的特点，可在Linux服务器、Windows和OSX桌面、安卓手机上运行，还可通过嵌入式Java在物联网的低内存传感器上运行。Torch和Pylearn2通过C++进行优化，优化和维护因而存在困难，而Java则是“一次编写，随处运行”的语言，适合需要在多个平台上使用深度学习系统的企业。

### <a name="ecosystem">生态系统</a>

生态系统也是为Java增添人气的优势之一。[Hadoop](https://hadoop.apache.org/)是用Java实施的；[Spark](https://spark.apache.org/)在Hadoop的Yarn运行时中运行；[Akka](https://www.typesafe.com/community/core-projects/akka)等开发库让我们能够为Deeplearning4j开发分布式系统。总之，对几乎所有应用而言，Java的基础架构都经过反复测试，用Java编写的深度学习网络可以靠近数据，方便广大程序员的工作。Deeplearning4j可以作为YARN的应用来运行和预配。

Scala、Clojure、Python和Ruby等其他通行的语言也可以原生支持Java。我们选择Java，也是为了尽可能多地覆盖主要的程序员群体。

虽然Java的速度不及C和C++，但它仍比许多人想象得要快，而我们建立的分布式系统可以通过增加节点来提升速度，节点可以是GPU或者CPU。也就是说，如果要速度快，多加几盒处理器就好了。

最后，我们也在用Java为DL4J打造NumPy的基本应用，其中包括ND-Array。我们相信Java的许多缺点都能很快克服，而其优势则大多会长期保持。

### <a name="scala">Scala</a>

我们在打造Deeplearning4j和ND4J的过程中特别关注[Scala](scala)，因为我们认为Scala具有成为数据科学主导语言的潜力。用[Scala API](http://nd4j.org/scala.html)为JVM编写数值运算、向量化和深度学习库可以帮助整个群体向实现这一目标迈进。

关于DL4J与其他框架的不同之处，也许只需要[尝试一下](quickstart)就能有深入的体会。

### <a name="ml">机器学习框架</a>

上文提到的深度学习框架都是比较专业化的框架，此外还有许多通用型的机器学习框架。这里列举主要的几种：

* [sci-kit learn](http://scikit-learn.org/stable/)－Python的默认开源机器学习框架。
* [Apache Mahout](https://mahout.apache.org/users/basics/quickstart.html)－Apache的主打机器学习框架。Mahout可实现分类、聚类和推荐。
* [SystemML](https://sparktc.github.io/systemml/quick-start-guide.html)－IBM的机器学习框架，可进行描述性统计、分类、聚类、回归、矩阵参数化和生存分析，还包括支持向量机。
* [微软DMTK](http://www.dmtk.io/)－微软的分布式机器学习工具包。分布式词嵌入和LDA。

### <a name="tutorial">Deeplearning4j教程</a>

* [深度神经网络简介](neuralnet-overview)
* [卷积网络教程](convolutionalnets)
* [LSTM和递归网络教程](lstm)
* [通过DL4J使用递归网络](usingrnns)
* [MNIST中的深度置信网络](../deepbeliefnetwork)
* [用Canova定制数据加工管道](image-data-pipeline)
* [受限玻尔兹曼机](restrictedboltzmannmachine)
* [本征向量、PCA和熵](eigenvector)
* [深度学习词汇表](glossary.html)
* [Word2vec、Doc2vec和GloVe](word2vec)
