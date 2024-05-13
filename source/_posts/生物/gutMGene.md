---
title: gutMGene
categories:
  - 生物
tags: [生物, 数据集/肠道菌群]
date: 2023-03-19 12:56:32
---

## 数据集介绍：

### 1. 背景

- 哺乳动物血液中发现的代谢物中有10%来自肠道菌群，它们可以通过激活或抑制基因表达对宿主产生系统性影响

- 肠道微生物与宿主之间的关系是高度互利的，宿主为微生物提供营养物质和庇护所，而微生物则帮助宿主从不可消化的食物中获取能量并合成必需的代谢物

  > 代谢物分为三类：
  >
  > 1. 由微生物从外源消耗的化合物中产生
  >
  >    肠道微生物可以代谢宿主摄入的外源性物质，例如食物中的碳水化合物、蛋白质和脂类等，产生代谢产物
  >
  > 2. 由宿主产生，由肠道微生物进行生物化学修饰
  >
  >    宿主细胞可以合成一些化合物，例如胆固醇、荷尔蒙和黄酮类化合物等，这些化合物在肠道中经过微生物的代谢和修饰后，产生了一些具有生物活性的代谢产物
  >
  > 3. 由肠道微生物从头合成（synthesized de novo by gut microbes）
  >
  >    肠道微生物可以从头开始合成一些化合物，例如维生素、氨基酸、短链脂肪酸和多糖等

### 2. 数据集

- `gutMGene`：肠道微生物和微生物代谢产物靶基因的综合数据库（论文中经过实验验证）
- [数据集地址](http://bio-annotation.cn/gutmgene)    [论文地址](https://academic.oup.com/nar/article/50/D1/D795/6368055)
- 简介：
  - 目的：提供人类和小鼠肠道微生物和微生物代谢产物的目标基因的资源
  - 数据构成（当前版本）：
    1. 人类中332种肠道微生物、207种微生物代谢物和223种基因之间的1331种关系
    2. 小鼠中209种肠道微生物、149种微生物代谢物和544种基因之间的2349种关系
  - 数据内容：关系的详细信息
    - 肠道菌群
    - 微生物代谢物
    - 靶基因

### 3. 其他数据集：

- GMrepo和gutMEGA策划并注释了人类肠道宏基因组
- gutMDisorder收集了经实验验证的肠道菌群与疾病或干预措施之间的关联



## 数据来源

1. 用关键词列表搜索PubMed数据库，获取可能相关的论文，如“gut”，“bowel”，“microbe”，“metabolite”等
   （>3000篇论文）

2. 筛选出了记录实验验证的论文

3. 从360多篇论文中手动提取了人和小鼠中的微生物-代谢物、微生物-靶点和代谢-靶点关系



## 数据内容

### 1. 数据构成

- 人类中332种肠道微生物、207种微生物代谢物和223种基因之间的1331种关系

- 小鼠中209种肠道微生物、149种微生物代谢物和544种基因之间的2349种关系

  ![image-20230320112847125](https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230320112847125.png)

### 2. 数据分布

1. 六大类的分布（左--Human  右--Mice）

   ![a](https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230320113803332.png)

### 3. 整体介绍

- gutMGene中的每个条目都包含两个用于记录关联的部分

  - Relationships between gut microbes, metabolites, and genes

    记录了肠道微生物、样品类型、底物、代谢物、基因、肠道微生物或代谢物下基因变化的描述、物种、实验方法、测量技术和基因表达实验技术的通量描述

  - Literature

    记录了对参考文献的详细描述

1. 单个微生物相关的代谢物数量（对于一种微生物，能够产生代谢物的数量）

   ![image-20230320122110450](https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230320122110450.png)

   > eg: 小鼠中有20种微生物(y)，这20种微生物中的任一种与3种代谢物有关

2. 单个代谢物相关的微生物数量（对于一种代谢物，有多少种微生物与之有关）

   ![image-20230320122456463](https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230320122456463.png)

   > eg：人类中有18种代谢物，这18种代谢物中的任一种代谢物与两种微生物有关

3. 微生物和基因（一种微生物受多少种基因的调控）

   ![image-20230320124855958](https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230320124855958.png)

   > eg：有1种微生物与34种基因有关；有4种微生物，其中的任一种与7种基因有关 

4. 基因和微生物（一种基因能够调控的微生物数量）

   ![image-20230320124908515](https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230320124908515.png)

   > eg：有两种基因，其中的任一种与8种微生物有关

### 4. 树状数据库

- 按照物种分为两类：**Human** 和 **Mouse**

- 每一类下细分为三个小类：GutMicrobiota, Metabolite和Gene

  ![image-20230320143739760](https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230320143739760.png)

- example：

  ![image-20230320154005003](https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230320154005003.png)

  包含以下信息：

  1. 物种

  2. Gut Microbiota (ID)：肠道菌群，可以使用ID来指代特定的菌群

  3. Substrate (ID): 底物，指代代谢产物的前体物质

  4. Metabolite (ID): 代谢产物，指代菌群代谢底物产生的化合物

  5. Gene (ID): 基因，可以使用ID来指代特定的基因

  6. Throughput: 数据处理的规模和复杂度，高通量或低通量

  7. PMID: PubMed ID，文章的唯一标识符，可用于检索该文章

  8. Details：给出了这个菌所能进行的一个代谢的总结（上图中的detail相同，且囊括了四种代谢反应）

  9. Network：针对某一个反应

     结点包括：当前微生物的**所有代谢产物**/基因 + 与**当前反应的代谢产物**/基因有关的所有微生物/基因

     eg：上面第二条数据的network：包含了Bd这个微生物有关的所有代谢产物/基因 + 与Bile acid这个产物有关的所有微生物/基因

     ![image-20230320154041323](https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230320154041323.png)

     



## 补充知识

  1. 高通量技术 与 低通量技术

     - 高通量技术：

       ✨能够同时处理大量生物样本、产生大量数据的技术

       🎈优点在于其速度、高效性和大规模性

       🥚大幅提高实验的产出量和数据量，并且可以从全面的角度来研究复杂的生物系统

     - 低通量技术：

       ✨能够处理少量生物样本、产生较少数据的技术

       🎈优点在于其精度、可重复性和可控性

       🥚主要用于深入了解某个生物系统的特定方面，如基因表达、蛋白质功能和代谢组学等

  
