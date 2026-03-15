# 第一步：安装“实验室管理员”（Miniforge）
Miniforge 是管理 Python 虚拟环境的神器，它原生支持苹果芯片，能避免很多底层的 C++ 编译报错。

- 如果你电脑上已经装了 Homebrew（Mac 常用的包管理器），直接在终端输入这行代码并回车：

 `brew install miniforge.`
- 如果没有 Homebrew，可以先运行：`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

安装完成后，输入以下命令让管理员“上岗”（初始化）：
`conda init zsh`
⚠️ 重要提示： 运行完这句后，请完全关闭你的终端窗口，然后重新打开一个新的终端。你会发现命令行最前面多了一个 (base)，说明 Miniforge 已经接管环境了。

# 第二步：创建专属“隔离房间”（虚拟环境）
现在我们为你的神经网络项目建一个独立的房间，起名叫 tf_env，并给它分配一个极其稳定的 Python 3.10 版本：

在终端输入：

`conda create --name tf_env python=3.10`
中途如果问你 Proceed ([y]/n)?，输入 y 并回车。

建好之后，我们进入这个房间：

`conda activate tf_env`
此时，你终端前面的 (base) 会变成 (tf_env)。这就相当于你进入了这个隔离区！接下来的所有安装，都只会存在于这里，完全不影响系统。

# 第三步：在房间里安装 TensorFlow（苹果加速版）
苹果对自家的芯片做过专门的优化插件，能让 TensorFlow 调用 Mac 的 GPU 进行加速。

在依然显示着 (tf_env) 的终端里，依次运行以下两行命令（这就是你熟悉的 npm install 的 Python 版本——pip install）：

安装基础的 TensorFlow：

`pip install tensorflow`

安装苹果专属的 GPU 硬件加速插件：

`pip install tensorflow-metal`

# 第四步：把 VS Code 连接到这个“房间”
环境建好了，现在我们要告诉 VS Code 去哪里找它。

打开 VS Code。

在左侧的插件市场（四个方块的图标），搜索并安装两个官方插件：

Python (微软官方出品)

Jupyter (微软官方出品，用于跑交互式数据分析)

随便新建一个文件夹（比如叫 DL_Project），用 VS Code 打开它。

新建一个文件，命名为 test.ipynb（这是一个 Jupyter Notebook 文件）。

最关键的一步： 在 VS Code 的右上角（或者右下角），找到 Select Kernel（选择内核）或 Python 解释器版本号 的地方，点击它。

在弹出的菜单中，选择 Python Environments -> Conda Env -> 选择你的 tf_env。

第五步：见证奇迹的时刻（测试 GPU）
在你的 test.ipynb 文件的第一个代码块里，复制粘贴以下测试代码：


```python
import tensorflow as tf
print("TensorFlow 版本:", tf.__version__)
print("可用的 GPU 数量:", len(tf.config.list_physical_devices('GPU')))
```

点击代码块左侧的 “播放（运行）” 按钮。如果输出结果类似如下（能看到 GPU 数量大于 0）：
TensorFlow 版本: 2.15.0  (或其他数字)
可用的 GPU 数量: 1
恭喜你！ 你的纯净、隔离且支持硬件加速的深度学习环境已经搭建完毕。