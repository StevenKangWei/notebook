# 框架：机器学习术语

什么是（监督）机器学习？简单说，就是这样：

- 机器学习系统学习怎样组合输入来生成有用的预测对于从未见过的数据

## 标签(Labels)

标签是我们需要预测的 - 简单线性回归中的 y 变量，标签可以是小麦未来的价格，图片中的动物类型，语音片段的意思，或者任何的东西。

## 特征(Features)

特征是输入变量 - 简单线性回归中的 x 变量，简单的机器学习项目可能用只一个特征，复杂的机器学习项目可能使用数以万计的特征，定义如下：

$$\{x_1, x_2, ... x_N\}$$

在垃圾邮件检测器的例子中，特征可以包括一下内容：

- 邮件内容的字符
- 发送者地址
- 邮件发送时间
- 邮件包含词组“一个奇怪的花招(one wired trick)”

## 参考资料

- [1] [Framing: Key ML Terminology ](https://developers.google.com/machine-learning/crash-course/framing/ml-terminology)