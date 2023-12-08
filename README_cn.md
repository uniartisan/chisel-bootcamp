[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/freechipsproject/chisel-bootcamp/master)

** _对于以前的训练营用户，我们已经从Scala 2.11升级到Scala 2.12。如果您遇到错误，请按照安装说明升级到2.12._**


[![Publish Docker image](https://github.com/uniartisan/chisel-bootcamp/actions/workflows/docker.yml/badge.svg)](https://github.com/uniartisan/chisel-bootcamp/actions/workflows/docker.yml)

这是我个人修复 Docker 的版本，原版的 Docker 无法在重启后正常运行。同时我做了一些汉化，可能还有学习工程中的一些笔记。

快速开始：
```
docker run -it --name chisel -p 8888:8888 uniartisan/chisel-bootcamp
```

这将为训练营下载一个Docker映像并运行它。输出将以以下消息结束:

```
    要访问笔记本，请在浏览器中打开这个文件:
        file:///home/bootcamp/.local/share/jupyter/runtime/nbserver-6-open.html
    或者复制粘贴其中一个url:
        http://79b8df8411f2:8888/?token=LONG_RANDOM_TOKEN
    或者 http://127.0.0.1:8888/?token=LONG_RANDOM_TOKEN
```

复制最后一个链接，以 https://127.0.0.1:8888 开头的链接到您的浏览器，并遵循Bootcamp。

# Chisel训练营

将您的硬件设计水平从实例提升到生成器!
这个训练营教你Chisel，一个用Scala编写的伯克利硬件构造DSL。
它一路教你Scala，并在“硬件生成器”的概念上框定了Chisel的学习。

## 多语言版本

There are two versions of this document, one in Chinese and one in English.
本文档提供中文和英文两个版本。

- [English](https://github.com/uniartisan/chisel-bootcamp/blob/master/README.md)
- [中文版](https://github.com/uniartisan/chisel-bootcamp/blob/master/README_cn.md)


## 你会学到什么?

- 为什么硬件设计最好表现为生成器，而不是实例
- 现代编程语言Scala的基础知识和一些高级特性
- Chisel的基础和一些高级特性，这是一种嵌入在Scala中的硬件描述语言
- 如何为Chisel设计编写单元测试
- 基本介绍了一些有用的功能在Chisel库，包括[dsptools](https://github.com/ucb-bar/dsptools/)和[rocketchip](https://github.com/freechipsproject/rocket-chip)。

## 先决条件

熟悉Verilog, VHDL，或至少一些数字硬件设计
- 具有高级语言编程经验，如Python, Java, c++等。
- 求知欲强

## 开始

试试吧[在这里](https://mybinder.org/v2/gh/freechipsproject/chisel-bootcamp/master)！无需本地安装！

如果您想在本地试用它，请查看这里的安装说明(Install.md)。

## 大纲

训练营分为几个模块，这些模块又被进一步细分。
本自述文件作为*模块0*，介绍和激励学习所包含的材料。
模块1*快速介绍Scala。
它教会了您足够的东西来开始编写Chisel，但在此过程中还教授了更多的Scala概念。
在模块2中介绍了Chisel，从一个硬件示例开始并将其分解。
模块2的其余部分涵盖组合逻辑和顺序逻辑，以及软件和硬件控制流。
模块3教你如何用Chisel编写硬件生成器，利用Scala的高级编程语言特性。
到最后，您将能够阅读和理解大部分[Chisel代码库](https://github.com/freechipsproject/chisel3)并开始使用[Rocket Chip](https://github.com/freechipsproject/rocket-chip)。
本教程*尚未涵盖*SBT，构建系统，后端流的FPGA或ASIC进程，或模拟电路。

## 动力
所有硬件描述语言都支持编写单个实例。
然而，编写实例是乏味的。
为什么要犯同样的错误，为别人可能已经设计好的东西写一个稍微修改过的版本呢?
Verilog支持有限的参数化，比如位宽和生成语句，但这只能让您到此为止。
如果不能编写Verilog生成器，则需要编写一个新实例，从而使代码大小加倍。
作为一个更好的选择，我们应该编写一个程序来生成两个硬件实例，这将减少我们的代码大小，并使繁琐的事情变得更容易。
这些程序被称为生成器。

理想情况下，我们希望我们的生成器能够(1)可组合，(2)功能强大，以及(3)支持对生成的设计进行细粒度控制。
错误检查是必要的，以确保作文是合法的;没有它，调试是困难的。
这需要一种生成器语言来理解设计的语义(知道什么是合法的，什么是不合法的)。
此外，生成器不应该过于冗长!
我们希望生成器程序能够简洁地表达许多不同的设计，而不必在每个实例的if语句中重写它。
最后，它应该是零成本的抽象。
硬件设计性能对微小的变化非常敏感，因此，您需要能够准确地指定微体系结构。
生成器与高级合成(high-level synthesis, HLS)非常不同。

Chisel的好处在于你如何使用它，而不是语言本身。
如果您决定编写实例而不是生成器，那么您将看到Chisel相对于Verilog的优势更少。
但是，如果您花时间学习如何编写生成器，那么Chisel的力量将变得明显，您将意识到您永远不会回到编写Verilog。
学习编写生成器很困难，但我们希望本教程将为您成为更好的硬件设计师、程序员和思想家铺平道路!

## 常见问题解答

内核在启动时崩溃

在启动Scala笔记本时，我得到以下错误，Jupyter说内核已经崩溃:

```
Exception in thread "main" java.lang.RuntimeException: java.lang.NullPointerException
	at jupyter.kernel.server.ServerApp$.apply(ServerApp.scala:174)
	at jupyter.scala.JupyterScalaApp.delayedEndpoint$jupyter$scala$JupyterScalaApp$1(JupyterScala.scala:93)
	at jupyter.scala.JupyterScalaApp$delayedInit$body.apply(JupyterScala.scala:13)
  ...

Caused by: java.lang.NullPointerException
	at ammonite.runtime.Classpath$.classpath(Classpath.scala:31)
	at ammonite.interp.Interpreter.init(Interpreter.scala:93)
	at ammonite.interp.Interpreter.processModule(Interpreter.scala:409)
	at ammonite.interp.Interpreter$$anonfun$10.apply(Interpreter.scala:151)
	at ammonite.interp.Interpreter$$anonfun$10.apply(Interpreter.scala:148)
  ...
```

确保为运行Jupyter选择了**Java 8**(参见上面的说明)。

## 贡献者
- Stevo Bailey ([stevo@berkeley.edu](mailto:stevo@berkeley.edu))
- Adam Izraelevitz ([adamiz@berkeley.edu](mailto:azidar@berkeley.edu))
- Richard Lin ([richard.lin@berkeley.edu](mailto:edwardw@berkeley.edu))
- Chick Markley ([chick@berkeley.edu](mailto:chick@berkeley.edu))
- Paul Rigge ([rigge@berkeley.edu](mailto:rigge@berkeley.edu))
- Edward Wang ([edwardw@berkeley.edu](mailto:edwardw@berkeley.edu))
