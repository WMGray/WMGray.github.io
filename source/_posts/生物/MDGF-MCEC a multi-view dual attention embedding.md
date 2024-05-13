---
title: MDGF-MCEC a multi-view dual attention embedding
categories:
  - 生物
tags: []
date: 2023-04-17 21:17:17
cover:

---

## 名词解释：

1. **==环状RNA==**（Circular RNA，简称**circRNA**）为生物细胞中的一类[RNA](https://zh.wikipedia.org/wiki/RNA)，由线状RNA[5'端](https://zh.wikipedia.org/wiki/方向性_(分子生物学))与3'端经[共价](https://zh.wikipedia.org/wiki/共價鍵)结合（反向剪接）而形成，是一种广泛存在于真核细胞中的单链RNA。环状RNA在基因表达调控、细胞周期、信号转导等生物学过程中发挥重要作用。最近的研究表明，环状RNA在许多疾病的发生和发展中也扮演着重要的角色，如癌症、心血管疾病、神经系统疾病等。==环状rna可以作为一种可靠的疾病生物标志物。==

## 近期方法：

- Wang et al.：建立相似矩阵，提出了一种基于多源数据的深度卷积神经网络模型。
- Deepthi、Jereesh：自编码器与随机森林相结合的AE-RF模型
- Zheng et al.：ICDA-CGR模型。模型引入了序列信息，利用基于序列位置信息的混沌博弈表示(chaos game representation, CGR)量化了circRNA序列中的非线性关系。
- Wang et al.：提出了一种半监督生成对抗网络(GAN)模型，称为SGANRDA。融合了circRNA序列的自然语言特征，对GAN网络进行预训练，然后使用**标记样本**对网络参数进行**微调**
- Li et al.：提出了基于矩阵补全的SIMCCDA，通过加速感应矩阵补全(SIMC)预测环状rna与疾病之间的关系。
- Wei 、Liu：提出iCircDA-MF模型，在有限的训练数据中引入基因信息，构建circrna -基因-疾病关系网络扩充数据源，并采用矩阵分解和补全技术降低特征噪声。
- Niu et al.：提出了采用图马尔可夫神经网络(GMNN)进行环状rna -疾病关联预测的GMNN2DC方法，将图自编码器与变分推理相结合
- Chen et al.：提出了RGCNCDA方法用于环状rna -疾病关联预测。该方法首先整合多相似网络，构建全局异构网络。然后，采用带重启的随机游走学习低维和高阶信息作为拓扑特征。建立了基于R-GCN编码器和DistMult解码器的预测模型进行预测。

## MDGF-MCEC模型：多视角双注意力

- `多视角`
- `双注意力`：在信道级和空间级对特征的不同视图进行权重调整和平衡，增强了学习到的嵌入特征的识别能力。

从不同角度利用circRNA的相似性和疾病的相似性，构建了不同的疾病关系图和circRNA关系图

<<<<<<< HEAD
### 1. 数据集

=======

### 1. 数据集


>>>>>>> 23255866cbb13b098b79454540dc9f6f1b6f0245
#### 1. CircRNA-疾病关联数据集

- [CircR2Disease](http://bioinfo.snnu.edu.cn/CircR2Disease/)
  [CircR2Disease](http://bioinfo.snnu.edu.cn/CircR2Disease/) 包含了**739**个经实验验证的 circRNA--疾病关联，涉及**661**个 circRNA 和**100**种疾病  
<<<<<<< HEAD
  
  > 去重后，可得到 650 对 circRNA--疾病关联，涉及 585 个 circRNA 和 88 种疾病

- [Circ2Disease](http://bioinformatics.zju.edu.cn/Circ2Disease/browse.html)
  当前版本：5368 associations、237 circRNAs、217 diseases、313 miRNAs sponges、1746 miRNA targets、647 RBPs
- circRNADisease

{% ablock color:blue %}

- 由于CircR2Disease数据库中已知的关联更加充分，因此本研究的实验主要在CircR2Disease数据库中进行
- 利用circRNADisease和Circ2Disease数据集来{% mark 验证  %}模型的预测能力

{% endablock %}
=======

> 去重后，可得到 650 对 circRNA--疾病关联，涉及 585 个 circRNA 和 88 种疾病

- [Circ2Disease](http://bioinformatics.zju.edu.cn/Circ2Disease/browse.html)
  当前版本：5368 associations、237 circRNAs、217 diseases、313 miRNAs sponges、1746 miRNA targets、647 RBPs
>>>>>>> 23255866cbb13b098b79454540dc9f6f1b6f0245

#### 2. 疾病数据集Mesh

- [Mesh](https://www.nlm.nih.gov/mesh/meshhome.html)
  该数据库以基于医学和生物学关联的有向无环图表示疾病。它包含4818种疾病和疾病之间的11648种关联。

#### 3. 数据集构建

- 使用 CircR2Disease 数据库中650对已知的 circRNA-疾病关联作为**阳性集**。
- 在 circrna 与疾病之间随机选取650对不在阳性集中的 circRNA-disease 作为**阴性集**

>错误率：$650 ÷ (585 × 88)≈1.26$

### 2. 框架图

{% image https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230417221947443.png  模型流程图   bg:white %}

{% ablock 模型流程图 color:green child:tabs %}

{% folders %}

<!-- folder Part A -->

`数据源模块`：包含三种数据库：疾病数据库、circRNA 数据库和 circRNA--疾病数据库。



<!-- folder Part B -->

`关系构建模块`：通过不同的相似度度量生成**疾病关系图**和**circRNA关系图**

<!-- folder Part C -->

`图嵌入学习模块`：用于学习基于 `多视图双注意力的GCN` 的两种关系图的嵌入特征

<!-- folder Part D -->

`分类模块`：同时使用多视图和集成学习

{% endfolders %}

{% endablock %}

### 3. 关系图

- 构建 Circ2Disease 关系的临界矩阵，大小为 $585 * 88$，对应585个 circRNA 和88种疾病

- 构建两个疾病关系图和两个 CircRNA 关系图  

  - 基于**语义相似度**的疾病关系图 
  $$
  G_{dss} = (V_{dss}, E_{dss})
  $$

  - 基于**疾病 GIP 核相似性**的疾病关系图
  $$
  G_{dgs} = (V_{dgs}, E_{dgs})
  $$

  - 基于**功能相似性**的关系图
  $$
  G_{cfs} = (V_{cfs}, E_{cfs})
  $$

  - 基于**功能相似性**的 CircRNA 关系图
  $$
  G_{cgs} = (V_{cgs}, E_{cgs})
  $$


{% image https://raw.githubusercontent.com/WMGray/blog.images/master/images/20230418160709.png 四种关系图  %}

### 4. 模型介绍

{% image https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230418162159055.png 四种关系图上的多视角双注意力GCN %}

#### GCN模块

{% quot 以CircRNA的GIP核关系图为例 %}

{% image https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230418162710063.png 双注意力GCN %}

$A_c$是$C_{cgs}$的邻接矩阵

$V_c$是节点集，$N_c$是节点个数，$D_c$是embedding的维度大小，$C_c$为通道数，默认为2。整个图上节点的特征可以表示为矩阵$X_g \in R^{N_c \times D_c}$,其中$X[i] \in R^{D_c} i \in \{1, 2, ......, N_c\}$ 为第 $i$ 个节点的嵌入

$$
X_\mathbf{g}^{(t)}=\hat{S}_\mathbf{c}^{-1/2}\hat{A}_\mathbf{c}\hat{S}_{\mathbf{c}}^{-1/2}X_\mathsf{g}^{(t-1)}\Theta^{(t-1)}
$$
$X_g \in R^{N_c \times D_c}$, $\hat{A}_{c}=A_{c}+I_{c},$ $[\hat{S}_\mathsf{c}]_{ii}~=~\sum_{j}[\hat{A}_\mathbf{c}]_j$, $\Theta^{\left(t-1\right)}\in R^{Dc\times Dc}$是一个可学习的矩阵

为了在卷积后获得更多的信息，在上图所示的模块中，不仅保留了第一层的嵌入，还保留了第二层的嵌入

{%image https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230418223421220.png  %}

经过GCN模块后，可得到一个嵌入 $X_g = \{X_g^{(1)}; X_g^{(2)} \}$$where X_{g} \in R^{2 \times N_c \times D_c}$

#### Attention模块

{% quot Channel Attention Block %}

<<<<<<< HEAD
{% image  https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230420131701354.png  Channel Attention Block %}

​	{% mark 输入 color:red %}  ：$X_g, 2 \times D_c \times N_c$    

​	{% mark 处理: color:blue %}   

1. 对输入分别进行平均池化/最大池化得到$X_g^{avg} \in R^{C_c}, X_g^{max} \in R^{C_c}$, 形状：$R^{2 \times N_c \times D_c} --> R^{2 \times 1 \times 1}$
2. 再输入到MLP中得到两个输出
3. 经过MLP的输出相加再经过激活函数便可得到Channel Attention Block的输出$W_{channel} (X_g )$，大小为$R^{C_c}$,即2

$$
\begin{gathered}
W_{channel} (X_g )=\sigma(MLP{(AvgPool)}(X_{\mathsf{g}}))+{MLP}{(MaxPool}{(X_g)))}) \\
=\sigma(MLP(X_g^{avg})+MLP  {(X_g)}^{\mathrm{max}}))
\end{gathered}
$$

4. 对$W_g$进行维度扩充，得到$M_g, M_g \in R^ {2 \times N_c \times 2}$

   ```python
   .....
   scale = F.sigmoid( channel_att_sum ).unsqueeze(2).unsqueeze(3).expand_as(x)
   return x * scale
   ```

   

​	{% mark 注意力得分: color:green %}  $M_g,  2 \times N_c \times 2$

​	{% mark 输出: color:yellow %}  $X_g^{'}=M_{channel}(X_g)\otimes X_g,$

{% quot Spatial Attention Block %}  

{% image https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230419095934382.png Spatial Attention Block %}

​	{% mark 输入: color:red %} ${X_g}^{'},  2 \times D_c \times N_c$  

​	{% mark 处理: color:blue %} 

1.  对$X_g^{'}$进行平均池化和最大池化，得到$X_g^{'avg}$ 和 $X_g^{'max'}$,形状为$R^{N_c \times D_c} $
1.  对池化后的输出连接后进行卷积操作，获取其空间上的联系,得到$W_{spatial}(X_g^{'}), W_{spatial}(X_g^{'}) \in R^{N_c \times D_c}$

$$
\begin{gathered}
W_{spatial}(X_g^{'})= \sigma (f_{conv}([AvgPool(X_g^{'});{MaxPool}(X_g^{'}])) \\
=\sigma(f_{conv} ({X_g^{'avg};{X_g^{'max}}})
\end{gathered}
$$

​	{% mark 注意力得: color:green %}  $M_{spatial}(X_g^{'}) \in R^{C_c \times N_ c \times D_c}$  

​	{% mark 输出: color:yellow %}  $X_g^{''}=M_{spatial}(X_g^{'})\otimes X_g^{'},$
=======
​	{% mark 输入 color:red %}  ：$X_g, 2 \times N_c \times D_c$    

​	{% mark 处理: color:blue %}   怎么处理

​	{% mark 输出: color:green %}  ${X_g}^{'},  2 \times N_c \times D_c$   

{% quot Spatial Attention Block %}  

​	{% mark 输入: color:red %} ${X_g}^{'},  2 \times N_c \times D_c$  

​	{% mark 处理: color:blue %} 

​	{% mark 输出: color:green %}  ${X_g}^{''},  2 \times N_c \times D_c$  
>>>>>>> 23255866cbb13b098b79454540dc9f6f1b6f0245

#### 多视角协同集成分类模块

{% quot 传统的方法 %}

传统的方法通常将CircRNA和疾病的多视角的特征连接起来，然后训练分类器，{% tag color:warning 对多视图信息的利用不足 %}。

{% quot 本文提出的方法 %}

<<<<<<< HEAD
{% image https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230420104709881.png 多视角协同集成分类模块 %}

1. 从不同的角度获得的进行{% mark 随机组合 color:blue %}，进而获得不同的特征集

2. 针对circRNA特征和疾病特征的不同组合，训练不同的XGBoost (Extreme Gradient Boosting) classifiers

   > 针对circRNA和疾病特征的不同组合构建了9个不同的视图

3. 在这9个分类器的基础上，使用{% mark 投票 %}机制

   

### 5. 实验表现

1. 数据集：CircR2Disease		实验方法：五折交叉验证

   {% image https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230420110054207.png 模型在CircR2Disease上的表现 %}

   {% image https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230420110150313.png ROC曲线 %}

2. 数据集： Circ2Disease、CircRNADisease  实验方法：五折交叉验证

   {% image https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230420111041680.png 模型在Circ2Disease上的表现 %}

   {% image https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230420115749703.png 模型在CircRNADisease上的表现 %}

   {% image https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230420115843418.png  模型在两个数据集上的ROC曲线 %}

3. 不同视角的分类器性能

   {% image https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230420120619021.png 不同组合视角的分类器性能 %}

4. 注意力消融实验

   {% image https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230420120523311.png 注意力消融实验 %}

{% quot 实验验证 %}

{% note color:blue 预测的CircRNA--疾病关联存在文献支持，不存在文献支持的可能会是潜在的关联，待后续生物验证%}

{% image  https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230420122416801.png 模型预测的circRNA-disease关联（Top 20）%

### 6. 局限性

1. 标注的{% mark 数据有限 %}，无法增强和评估所提出方法的泛化能力

2. 在本方法中，{% mark color:warning 将关联未知的样本当作负样本进行训练 %}。

   这是一种可行的策略，但它可能导致CircRNA--疾病对的错误标记，从而导致训练过程的偏差

   {% note color:green 一种可能的解决方案是引入正无标签学习策略，该策略构建的分类器只有正样例和未标记样例，而没有负样例 %}



=======
>>>>>>> 23255866cbb13b098b79454540dc9f6f1b6f0245
## 参考：

1. [环状RNA](https://zh.wikipedia.org/wiki/%E7%92%B0%E7%8B%80RNA)

<<<<<<< HEAD
2. 





































=======
>>>>>>> 23255866cbb13b098b79454540dc9f6f1b6f0245
