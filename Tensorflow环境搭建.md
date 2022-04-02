## 环境搭建

运行环境：

1. python 3.9.9
2. tensorflow 2.7（默认支持CPU和GPU）
3. GTX-1650
4. vscode+juoyter notebook

首先，需要对系统中的python进行管理，避免多个版本的python共存，选择适合python适合的版本安装，安装时选择将python加入系统环境变量中。使用tensorflow-gpu版本，目前TensorFlow的最新版本（2.7）已经默认支持CPU和GPU，tensorflow-gpu版本需要显卡支持，并且需要额外下载安装其他显卡驱动以及用于深度神经网络的GPU加速库，查看tensorflow官网文档，寻找与当前tensorflow版本匹配的驱动。使用的组合为“CUDnn8.1.1.33+CUDA11.2”。

![image-20220109190152782](https://gitee.com/cpicture/picture-1/raw/master/202201091901944.png)

在深度神经网络学习过程中，我们可能需要多个不同版本的python和库，这里使用了python虚拟环境，使用了python包管理软件poetry搭建虚拟环境，在虚拟环境中选择python、tensorflow、numpy等库的版本。jupyter notebook或jupyter lab具有逐个单元运行的特性，可以在第一时间发现代码错误所在，在编辑器中使用虚拟环境中的python和库，需要在jupyter中添加虚拟环境中的python解释器（内核），内核添加后运行jupyter lab或notebook，更换虚拟环境内核，对检测tensorflow gpu进行检测，到此，环境搭建完成。

## 遇到的问题

1.系统中python版本混乱（可忽略），解决方案：全部卸载重装

2.CUDA和CUDnn版本与当前tensorflow版本不匹配

3.jupyter lab打开后无法运行，安装jupyter notebook的时候，会自动安装ipykernel, 然后自动安装了高版本的pyzmq

解决策略：降低pyzmq版本，如安装pyzmq==19.0.2

4.jupyter lab无法自动识别虚拟环境或总是出现kernel dead情况，解决策略：通过ipykernel为jupyter添加新环境--python -m ipykernel install --name xxx_x

## 结果输出

主要函数：对单张图片进行每个分类的可能性评估，选择可能性最大的标签作为此图片的标签

首先将输入的单张图片进行预处理，对输入图片进行RGB转换，将图片压缩为与训练集同等大小的图片，使用np.array将image数据转化为矩阵array，再使用tf.cast(image_array, tf.float32)前把图像的数据格式转换为float32，对图像进行标准化，加快测试，对标准化后的数据进行变形，与分类器分类器的输入相匹配。从指定路径载入使用已训练好的模型，将处理后的数据送入模型中，返回图像对于每种分类的可能性，使用np.argmax()对最大可能性所在的维度进行返回，返回的维度即为分类的标签，将标签加入该测试图片的label中。

使用os.listdir(file_dir)对测试集进行遍历，将图片送入单张图片评估函数，得到两个列表，一个列表用来存放图片的名称，一个列表用来存放图片的预测结果，最终使用pandas下的DataFrame({'picture':list1,'label':list2})和dataframe.to_csv("test.csv",index=False,sep=',') 将输出结果写入到csv文件中。

