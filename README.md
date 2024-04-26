# YghDataMining
# 过程
## 一、注册并登录github

进入GitHub官网，点击Sign up进行注册，如果已有帐号，点击Sign in进行登录。

## 二、创建一个仓库
![image](https://github.com/CoderGhowe/YghDataMining/assets/45193360/35d87cc9-63f4-4e51-9466-9b4d75c84066)

1、点击右上方的‘+’标志，选择New repository创建仓库；
![image](https://github.com/CoderGhowe/YghDataMining/assets/45193360/5ba84219-fb71-424a-83e9-e94e9c40e2ac)

2、进入创建仓库页面，“Repository name *”处填写创建仓库的名称，“Description”为仓库添加简要描述，写不写都可以，有助于其他人了解项目的目的和特点；选择默认的仓库类型Public；“Initialize this repository with：”初始化仓库的信息文件，为仓库初始化一份长描述，建议勾选。

3、点击绿色Create repository按钮即可成功创建仓库。

## 三、上传自己的PPT到创建的仓库中
![image](https://github.com/CoderGhowe/YghDataMining/assets/45193360/d379961b-8845-4aeb-a463-c78893c78757)

在创建的仓库中，点击“Add file”,选择“Upload files”可以把本地已有的文件上传至仓库中。

## 四、编辑README.md文件
![image](https://github.com/CoderGhowe/YghDataMining/assets/45193360/ccf10603-f8c7-4daa-9108-92ec1a870761)

点击README右侧的编辑标志可以对README.md文件进行编辑。

# 获得的技能
## vit
Vit的思想是把图片分割成小块，然后将这些小块作为一个线性的embedding作为transformer的输入，处理方式与NLP中的token相同，用监督训练的方式进行图像分类。

## 模型结构
![image](https://github.com/CoderGhowe/YghDataMining/assets/45193360/3f574801-deeb-4d20-bf4c-f7d6ab6ecbdb)

首先，需要把图片输入进网络，和传统的卷积神经网络输入图片不同的是，这里的图片需要分为一个个patch，如图中就是分成了9个patch。每个patch的大小是可以指定的。然后把每个patch输入到embedding层，也就是Linear Projection of Flattened Patches，通过该层以后，可以得到一系列向量（token），9个patch都会得到它们对应的向量，然后在所有的向量之前加入一个用于分类的向量*，它的维度和其他9个向量一致。此外，还需要加入位置信息，也就是图中所示的0~9。然后把所有的token输入Transformer Encoder中，然后把TransFormer Encoder重复堆叠L次，再将用于分类的token的输出输入MLP Head，然后得到最终分类的结果。

## Embedding层

Embedding层如模型结构图中绿色框所示，标准的Transformer模块需要输入token序列，它是一个二维矩阵，也就是[token的数量，token的维度]。在实际实现中，直接用一个卷积层来实现。以步距为16，卷积核的大小也为16，卷积核的个数设置为768为例。维度的变换是这样的：输入224*224*3的图片，通过卷积层变成14*14*768，然后打平高宽所对应的维度，变成196*768。当然，在输入Embedding层之前，需要加入类别和位置这两个参数。假设该图片属于A类别，就将1*768的向量与196*768的向量进行拼接，得到197*768的向量。最后再与位置编码进行数值上的相加，最后的向量是197*768。

## Transformer Encoder层

通过Embedding层的patches输入，经过Layer Norm层以后，输入Multi-Head Attention层，然后在进行相加操作（可以理解为ResNet的残差块）。相加之后，再次输入Layer Norm层，然后经过MLP Block模块，经过DropOut层以后再进行相加操作，得到Encoder的输出。

### Self-attention机制
![image](https://github.com/CoderGhowe/YghDataMining/assets/45193360/559fb22c-a5e1-4573-a4b7-d91328c1a459)

如图给定一个input矩阵，转成相同维度的key和value。query经过tanspose后和key相乘，对相乘后结果的最后一个维度做了softmax，可以组成L*L的attention map。map里面每一行的元素总和为1（L×L的方块中，色块越深值越大，色块越浅值越小）。

得到Query，Key和Value都是普通的1x1卷积，差别只在于输出通道大小不同；将Query的输出转置，并和Key的输出相乘，再经过softmax归一化得到一个attention map； 将得到的attention map和Value逐像素点相乘，得到自适应注意力的特征图。

### 多头自注意力机制
![image](https://github.com/CoderGhowe/YghDataMining/assets/45193360/13e0a6a8-a183-4b3c-b1e5-d89f7793c5c7)

自注意层随后进一步完善，增加了一种叫多头注意力（Multi-head Self-attention）的机制以提高最朴素版本的自注意层的性能。值得注意的是对于一个给定的词，我们往往希望在通读句子是将注意力放在其他几个有关联的词上。因此，一个单头的自注意层限制了模型关注与某一特定位置（或几个特定位置）的同时能不影响关注其他同样重要位置的能力。这是通过赋予注意力层不同的表示子空间来实现的。具体来说，不同的查询（query）、键（key）和值（value）矩阵被用在不同的头上。由于随机初始化，在经过训练后这些输入向量可以被投影在不同的表征子空间上。

### 多层感知机（MLP）

多层感知机由输入层、输出层和至少一层的隐藏层构成。网络中各个隐藏层中神经元可接收相邻前序隐藏层中所有神经元传递而来的信息，经过加工处理后将信息输出给相邻后续隐藏层中所有神经元。在多层感知机中，相邻层所包含的神经元之间通常使用“全连接”方式进行连接。多层感知机可以模拟复杂非线性函数功能，所模拟函数的复杂性取决于网络隐藏层数目和各层中神经元数目。

## 总结

ViT的整体思想主要是将图片分类问题转换成了序列问题。即将图片patch转换成 token，以便使用 Transformer 来处理。ViT模型的提出，打破了以往CNN在图像识别领域的统治地位，展示了Transformer模型在计算机视觉任务中的潜力，为后续的研究提供了新的方向。

网址：https://github.com/CoderGhowe/YghDataMining
