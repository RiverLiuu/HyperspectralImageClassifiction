## Research on Hyperspectral Image Classification Based on Unsupervised Regularization and Capsule Network Part I
###一.思想
充分利用在无标签数据中学习到的无监督知识可以对有监督的分类模型进行正则化，可以有效地缓解小样本现象带来的过拟合问题。<br/>

基于聚类假设，即在同一个流形结构上，相邻的样本拥有相似的输出值。<br/>

这一部分的代码实现共享无监督信息的HSI分类框架，<br/>

即在有监督的训练过程中引入来自样本总体分布的无监督信息，引导模型正则，从而实现在少量标签样本的情况下对数据有效分类。

这一部分实现的主要框架如图：

![](./fig/SUI网络结构.png )

首先通过K-means算法将无标签样本聚成K类，每个样本可以通过聚类算法得到聚类类别（为标签信息），而后通过监督学习的方式进行特征提取。<br/>

每个训练的batch都包含一部分真实带标签样本和有聚类伪标签的无标签样本。<br/>

损失函数通过交叉熵计算，使用直接相加的方式得到总损失。<br/>

###二.环境
算法需要在ubuntu16.04系统，python3.5的环境下运行，需要安装的包如下:<br/>

```
numpy 1.16.2
tensorflow-gpu  1.10.0
scipy 1.3.1
sklearn 0.20.2
matplotlib 2.0.3
```
运行所需虚拟环境的路径：
```
/home/b104/anaconda3/envs/river
```
  ###三.实现

  本文的第一个创新点主要是定义了共享无监督信息的方法：
  
  1.通过K均值聚类算法为所有数据赋予来自聚类的伪标签;[K-means聚类算法](/media/b104/0009C6090001341C/RiverLiu/code/PartI_SUI/k-means.py)

  2.将两种损失都引入分类器中进行反向传播[引入损失](/media/b104/0009C6090001341C/RiverLiu/code/PartI_SUI/model.py)

  3.设置堆叠式自编码器，对输入数据集整体进行无监督预训练[无监督预训练](/media/b104/0009C6090001341C/RiverLiu/code/PartI_SUI/data_loader.py)

  4.将SAE的权值保存在info.mat文件夹下[权值](/media/b104/0009C6090001341C/RiverLiu/code/PartI_SUI/result/0/info.mat)

  5.运行main.py文件进行训练和预测[main](/media/b104/0009C6090001341C/RiverLiu/code/PartI_SUI/main.py)

  ###四.结果
 
  1.数据集在上级目录下的dataset文件夹中，本章在论文中使用的数据集分别是indian_pines、KSC以及PaviaU，<br/>
  
  2.运行的结果图保存在本上级目录的result/0文件夹中。[paviaU](/media/b104/0009C6090001341C/RiverLiu/code/PartI_SUI/result/0/decode_map_PaviaU.png)
    [Salinas](/media/b104/0009C6090001341C/RiverLiu/code/PartI_SUI/result/0/groundtrouth_Salinas.png)
  <br/>

### 五.其他
  可以在本目录中的main.py文件中parser.add_argument所在行更改数据集以及其他参数；<br/>

  
  主要的评价指标是AA、OA以及Kappa系数三种，运行的时候会输出，也可以查看result/0目录下的result_list.mat文件。<br/>
