# YghDataMining
# 一、注册并登录github

进入GitHub官网，点击Sign up进行注册，如果已有帐号，点击Sign in进行登录。

# 二、创建一个仓库
![image](https://github.com/CoderGhowe/YghDataMining/assets/45193360/35d87cc9-63f4-4e51-9466-9b4d75c84066)

1、点击右上方的‘+’标志，选择New repository创建仓库；
![image](https://github.com/CoderGhowe/YghDataMining/assets/45193360/5ba84219-fb71-424a-83e9-e94e9c40e2ac)

2、进入创建仓库页面，“Repository name *”处填写创建仓库的名称，“Description”为仓库添加简要描述，写不写都可以，有助于其他人了解项目的目的和特点；选择默认的仓库类型Public；“Initialize this repository with：”初始化仓库的信息文件，为仓库初始化一份长描述，建议勾选。

3、点击绿色Create repository按钮即可成功创建仓库。

# 三、上传自己的PPT到创建的仓库中
![image](https://github.com/CoderGhowe/YghDataMining/assets/45193360/d379961b-8845-4aeb-a463-c78893c78757)

在创建的仓库中，点击“Add file”,选择“Upload files”可以把本地已有的文件上传至仓库中。

# 四、编辑README.md文件
![image](https://github.com/CoderGhowe/YghDataMining/assets/45193360/ccf10603-f8c7-4daa-9108-92ec1a870761)

点击README右侧的编辑标志可以对README.md文件进行编辑。

# 获得的技能
## vit
Vit的思想是把图片分割成小块，然后将这些小块作为一个线性的embedding作为transformer的输入，处理方式与NLP中的token相同，用监督训练的方式进行图像分类。

##模型结构
![image](https://github.com/CoderGhowe/YghDataMining/assets/45193360/3f574801-deeb-4d20-bf4c-f7d6ab6ecbdb)

首先，需要把图片输入进网络，和传统的卷积神经网络输入图片不同的是，这里的图片需要分为一个个patch，如图中就是分成了9个patch。每个patch的大小是可以指定的。然后把每个patch输入到embedding层，也就是Linear Projection of Flattened Patches，通过该层以后，可以得到一系列向量（token），9个patch都会得到它们对应的向量，然后在所有的向量之前加入一个用于分类的向量*，它的维度和其他9个向量一致。此外，还需要加入位置信息，也就是图中所示的0~9。然后把所有的token输入Transformer Encoder中，然后把TransFormer Encoder重复堆叠L次，再将用于分类的token的输出输入MLP Head，然后得到最终分类的结果。




网址：https://github.com/CoderGhowe/YghDataMining
