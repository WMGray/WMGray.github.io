---
title: gutMDisorder
categories:
  - 生物
tags:
  - 生物
  - 数据集/肠道菌群
date: 2023-03-18 19:19:56
---

#生物 #数据集/肠道菌群 

##  数据集介绍：

- GutDiscoders 目前版本记录了 579 种肠道微生物与人类 123 种疾病或 77 种干预措施之间的 2263 种策划关联，以及在小鼠中 273 种肠道微生物与 33 种疾病或 151 种干预措施之间的 930 种策划关联

  ![image.png](https://raw.githubusercontent.com/WMGray/blog.images/master/images/20230318131247.png)

- [数据集地址](http://bio-annotation.cn/gutMDisorder/)


- 数据分布:
  ![image.png](https://raw.githubusercontent.com/WMGray/blog.images/master/images/20230318153123.png)
- 微生物和疾病数量/干预措施的直方图
  ![](https://raw.githubusercontent.com/WMGray/blog.images/master/images/20230318153947.png)
  >eg: 大多数人 (54.75%，317/579)和小鼠 (58.97%，161/273)微生物只与一种疾病和干预措施相关
  >某种微生物的数量改变后, 会影响到多种的疾病变化
- 疾病数量/干预措施与微生物的直方图
  ![image.png](https://raw.githubusercontent.com/WMGray/blog.images/master/images/20230318154953.png)

  >eg: 人类 30 种疾病及干预措施仅与一种微生物有关。而大多数疾病和干预措施 (143 种)与 3 种或 3 种以上微生物有关。患某种疾病/使用某种措施之后, 可以影响到的菌群种类的数量

## 数据来源

- 从出版物中收集

  > 1. 从出版物中手动提取
  > 2. 在 `PubMed` 数据库中搜索关键字列表, 获取相关论文  eg: gut, intestinal, microbiota, microbiome, mice, rat, 16 S
  > 3. 检查论文中记录实验验证的关联, 并进行复查

  ## 数据集内容

  ### 1. 整体介绍

1. **Association**: 记录关联类型、肠道微生物、疾病、干预措施 (药物、食物或其他)、疾病或干预下肠道微生物变化的简要描述

2. **Sample**:记录了物种 (人或小鼠)、样本量和来源、性别、年龄、BMI、测序技术和平台等。

3. **Literature**: 记录了文献参考的详细描述。![image.png](https://raw.githubusercontent.com/WMGray/blog.images/master/images/20230318163614.png)

  > - Gut Microbe: 肠道微生物名称
  > - Gut Microbiata ID: 肠道微生物的独特标识符号
  > - Gut Microbiata NCBI ID: 肠道微生物的 NCBI 数据库标识符号
  > - P Value: 统计学中的 P 值，表示观察到的差异在假设检验下的显著性
  > - Statistical Method: 使用的统计方法，如 T 检验、方差分析等
  >   -   Student's t test（学生 t 检验）：用于比较两组样本的均值是否有显著差异。假设样本服从正态分布，且方差相等。
  >   -   Unpaired t-test（无配对 t 检验）：用于比较两组独立样本的均值是否有显著差异。假设样本服从正态分布，且方差相等。
  >   -   Wilcoxon-Signed ranks test（威尔科克森符号秩检验）：用于比较两组样本的中位数是否有显著差异，不需要假设样本服从正态分布。
  >   -   ANOVA（方差分析）：用于比较三个或以上组的样本均值是否有显著差异。如果显著差异存在，需要进行后续事后比较。
  > - Alteration: 肠道微生物在疾病组与健康组之间的变化，可能是上升或下降。

### 2. Association

![image.png](https://raw.githubusercontent.com/WMGray/blog.images/master/images/20230318133204.png)

- Project ID: 对应的 NCBI 的 BioProject ID

- Condition: 实验对象所处的状态
	- HIV Infections: 包括人类免疫缺陷病毒感染的范围，从无症状的血清阳性，到艾滋病相关综合症（ARC），再到获得性免疫缺陷综合症（AIDS）。
	- Metformin: 一种双胍类降糖药，用于治疗对饮食调整没有反应的非胰岛素依赖型糖尿病。
	- Obesity: 体重严重超过建议标准的状态，
	- Weight Loss: 现有体重的减少

- Sequencing Technology: 序列测序技术

- Research Type:
	- Gut microbiota associated with phenotype: 与疾病相关的肠道微生物群
	
	  >当肠道微生物的种类、数量、组成等出现变化时，会影响宿主身体的健康状况，因此肠道微生物与多种疾病之间存在着密切的联系。
	
	- Interventions change the composition of gut microbiota: 干预措施改变肠道菌群的组成

	  >通过一些干预措施，如饮食、药物、植入物等，可以改变肠道微生物的组成，从而对宿主身体产生积极或消极的影响。
	

### 3. 树状数据库
![image.png](https://raw.githubusercontent.com/WMGray/blog.images/master/images/20230318182454.png)
- 根据物种进行划分：人和鼠

- 每一个物种下面都有两个子类别：肠道菌群和条件（Intervention 和 phenotype）
	- 肠道菌群：分支为每一种菌
	- 条件：condition 1 和 condition 2 
		- Phenotype：表现（eg：health 和 aged）
		- Intervention：药物、食物、其他（eg: Smoking Cessation 和 Non-Smokers）
	
- Example：

  ![image.png](https://raw.githubusercontent.com/WMGray/blog.images/master/images/20230318190212.png)

  - Condition：根据上面的进行筛选。1 作为实验组，2 作为对照组

  - Classification：六大类种一个（门、属、种...）

  - Alteration：与 2 相比，1 的某些菌群的数量发生了什么变化（增加、减少、消失）

  - Microbial-Mediated Networks：某一个菌所有实验的图。可以进行筛选（疾病/干预、菌群增加/减少、文献/测序、实验设置）
  	![image.png](https://raw.githubusercontent.com/WMGray/blog.images/master/images/20230318191031.png)

  - Details: 给出了该实验的参考文献、作者、年份、物种类别、实验设置、实验结论等
  	![image.png](https://raw.githubusercontent.com/WMGray/blog.images/master/images/20230318191158.png)