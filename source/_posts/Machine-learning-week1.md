---
title: 斯坦福大学 机器学习 吴恩达 第一周
date: 2019-04-01 21:00:21
categories:

- Machine Learning
tags:
- Machine Learning
- Notes
mathjax: true
---

* 本文采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh) 协议，转载请注明出处。
* 自学笔记。课程地址[机器学习_创建者 Stanford University](https://www.coursera.org/learn/machine-learning/home/welcome)

---
# 定义
“A computer program is said to learn from experience E with respect to
some task T and some performance measure P, if its performance on T,
as measured by P, improves with experience E.”
从已知的经验P中学习，解决任务T，并可以用P来度量完成情况。然后计算机根据经验E来提升在任务T上的表现P，就是机器学习。

# 分类
目前存在几种不同类型的学习算法。主要的两种类型被我们称之为监督学习和无监督学习。监督学习这个想法是指，我们将教计算机如何去完成任务，而在无监督学习中，我们打算让它自己进行学习。

## 监督学习（Supervised Learning）
给出一个算法，需要部分数据集已经有正确答案。根据这些已知的数据，来预测其他数据的输出。
### 回归问题（Regression Problem）
比如像根据房屋面积、楼层等变量预测房屋价格。
### 分类问题（Classification Problem）
比如像根据大小，位置等预测肿瘤良性或者恶性。

## 无监督学习（Unsupervised Learning）
在无监督学习中，所给数据没有属性或标签这一概念，也就是说所有的数据，都是一样的，没有区别。所以在无监督学习中，我们只有一个数据集，没人告诉我们该怎么做。 我们也不知道，每个数据点究竟是什么意思，相反，它只告诉我们，现在有一个数据集，你能在其中找到某种结构吗？对于给定的数据集，无监督学习算法可能判定，该数据集包含几个不同的聚类。比如对所有新闻进行无监督学习，把有关NBA的消息放在一起推送。除此以外，无监督学习还应用于社交网络分析（自动识别好友圈，可能认识的朋友）、在很嘈杂的声音中分辨声音

# 代价函数（Cost Function）
$$
J(\theta_0, \theta_1) = \dfrac {1}{2m} \sum _{i=1}^m \left ( \hat{y}_{i}- y_{i} \right)^2 = \dfrac {1}{2m} \sum _{i=1}^m \left (h_\theta (x_{i}) - y_{i} \right)^2
$$
（线性回归）
这个又可称为均方误差（Mean Squared Error）

机器学习就是要不断调整$\theta$使得代价函数最小。

# 梯度下降法（Gradient Descental Gorithm）
梯度下降法的算法是对于每一个参数$\theta$：
$$
\theta_j := \theta_j - \alpha \frac{\partial}{\partial \theta_j} J(\theta)
$$
$\alpha$是自己选的一个值，如果选择太小，就会耗费过多的时间才到达代价函数最低点，如果选择太大，则这个算法最后不会收敛。
经过推导，
$$
\frac{\partial}{\partial \theta_j} J(\theta)= \dfrac {1}{m} \displaystyle \sum _{i=1}^m \left (h_\theta (x_{i}) - y_{i} \right)x_i
$$
（线性回归，其中$x_1=1$）