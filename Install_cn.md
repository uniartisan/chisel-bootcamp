## 本地安装说明

如果您想在本地运行训练营，请根据您的特定情况运行以下说明。
注意，我们为Jupyter包含了一个自定义javascript文件，所以如果您已经安装了Jupyter，您仍然需要安装custom.js文件。

注意:请确保您使用的是 **Java 8** (不是Java 9)，并安装了JDK8。截至2018年1月，Coursier/jupyter-scala似乎还不兼容Java 9。

如果您有多个版本的Java，请确保在运行`jupyter notebook`之前选择Java 8 (1.8):

* Windows: https://gist.github.com/rwunsch/d157d5fe09e9f7cdc858cec58c8462d6
* 在Mac OS上: https://stackoverflow.com/questions/21964709/how-to-set-or-change-the-default-java-jdk-version-on-os-x

### 使用Docker进行本地安装- Linux/Mac/Windows

确保你的系统上已经安装了Docker (https://docs.docker.com/get-docker/)。

执行如下命令:

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

### 本地安装- Mac/Linux

这个训练营使用Jupyter笔记本。
Jupyter笔记本允许您在浏览器中以交互方式运行代码。
它支持多种编程语言。
对于这个训练营，我们将首先安装jupyter，然后安装特定于scala的jupyter后端(现在称为almond)。


#### Jupyter
首先安装Jupyter。

依赖项:openssh-client, openjdk-8-jre, openjdk-8-jdk (-headless OK for both)， ca-certificates-java

首先，使用pip3来安装jupyter(或python 2的pip): http://jupyter.org/install.html
```
pip3 install --upgrade pip
pip3 install jupyter --ignore-installed
```

如果pip3不能开箱即用(可能是因为您的Python3版本已经过时)，您可以尝试使用`python3 -m pip`代替`pip3`。

(无论出于何种原因，以后要重新安装jupyter，您可以使用`——no-deps`来避免重新安装所有依赖项。)

您可能想尝试一下Jupyter lab，这是由Project Jupyter开发的较新的接口。
如果您希望能够在浏览器中运行终端仿真程序，那么它特别有用。
它可以用`pip3`安装:
```
pip3 install jupyterlab
```

#### Scala的Jupyter后端

如果在此部分遇到错误或问题，请尝试运行 `rm -rf ~/.local/share/jupyter/kernels/scala/`。

接下来，下载coursier并使用它来安装almond(参见[这里](https://almond.sh/docs/quick-start-install)获取这些说明的源代码):
```
curl -L -o coursier https://git.io/coursier-cli && chmod +x coursier
SCALA_VERSION=2.12.10 ALMOND_VERSION=0.9.1
./coursier bootstrap -r jitpack \
    -i user -I user:sh.almond:scala-kernel-api_$SCALA_VERSION:$ALMOND_VERSION \
    sh.almond:scala-kernel_$SCALA_VERSION:$ALMOND_VERSION \
    --sources --default=true \
    -o almond
./almond --install
```

如果你愿意，你可以删除` coursier `和` almond `文件。

#### 可视化

[Graphviz](https://graphviz.org/download/)需要显示Chisel模块的可视化，例如在演示页面中。然而，可视化是可选的，因为其他Chisel和Scala特性没有它也可以工作。

#### 安装训练营
现在克隆bootcamp repo并安装定制脚本。
如果您已经有一个，请将此脚本附加到其中。

```
git clone https://github.com/freechipsproject/chisel-bootcamp.git
cd chisel-bootcamp
mkdir -p ~/.jupyter/custom
cp source/custom.js ~/.jupyter/custom/custom.js
```

要在本地机器上启动训练营:
```
jupyter notebook
```

如果您安装了Jupyter Lab，请运行`jupyter-lab`。


###  本地安装- Windows

我个人更推荐使用 `WSL` 安装 `Ubuntu`，然后按照上面的说明或者使用 Docker 进行安装。