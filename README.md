# CIFAR10_mxnet

### 目录文件描述

文件名 | 描述
--- | ---
log | 一些训练的日志，主要是训练的loss和acc
models | 一些训练的模型
result | 程序forward的最终结果，保留了10个类别的output
submission | 最终提交的结果文件
CIFAR10_train | CIFAR10上训练模型和产生结果的代码，主程序。
plot | 绘制一些模型训练过程的精度和loss曲线
netlib.py | ResNet18, ResNet164_v2, densenet, Focal Loss 的gluon的实现,被调用。
utils.py | 一些工具函数

models、result、log等内容放在网盘上，
链接: https://pan.baidu.com/s/1pLjzQWj 密码: f6p3

### 方法描述
方法大致如下:
使用不同的网络的数据增强的方法，我做了多个实验，得到了多个网络模型(全部放到了models下面)，然后ensemble，发现下面5个网络的效果最好。</br>
这5个网络的训练策略和单独提交的精度分别是：

policy | kaggle 精度
--- | ---
res164_v2 + DA1| 0.9529
res164_v2 + DA2| 0.9527
res164_v2 + focal loss + DA3| 0.9540
res164_v2 + focal loss + DA3 | 只使用train_data训练: 0.9506
[sherlock_densenet](https://discuss.gluon.ai/t/topic/1545/273)| 0.9539

上面的DA是3中不同的数据增强的方法:
DA  | policy
--- | ---
DA1 | 就是最常用的那种padding到40,然后crop的方法，就是sherlock代码里使用的加强
DA2 | 是先resize到一定的大小，然后crop的方法，同时设置了HSI的几个参数为0.3,PCA噪声为0.01
DA3 | 时在DA2后，将图片的颜色clip导（0,1）之间（动机时创建更符合人感官的自然图片数据）

五个网络按照各自的精度加权求和作为最后的结果，就有了0.9688的效果。
