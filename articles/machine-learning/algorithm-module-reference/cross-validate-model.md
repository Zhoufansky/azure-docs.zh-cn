---
title: 交叉验证模型：模块引用
titleSuffix: Azure Machine Learning
description: 了解如何在 Azure 机器学习中使用交叉验证模型模块，通过对数据进行分区来交叉验证分类或回归模型的参数估算值。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/11/2020
ms.openlocfilehash: 6dd8246d5751609e2f20ee9d5e519529752940f7
ms.sourcegitcommit: b95983c3735233d2163ef2a81d19a67376bfaf15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/11/2020
ms.locfileid: "77137530"
---
# <a name="cross-validate-model"></a>交叉验证模型

本文介绍如何使用 Azure 机器学习设计器（预览版）中的 "交叉验证模型" 模块。 *交叉验证*是一种通常在机器学习中使用的技术，用于评估数据集的可变性以及通过该数据定型的任何模型的可靠性。  

"交叉验证模型" 模块将标记的数据集作为输入，并将其与未定型的分类或回归模型结合使用。 它将数据集分割成一定数量的子集（*折叠*），在每个折页上生成一个模型，然后为每个折页返回一组准确性统计信息。 通过比较所有折叠的准确性统计信息，可以解释数据集的质量。 然后，您可以了解模型是否容易受到数据的变化。  

交叉验证模型还返回数据集的预测结果和概率，以便您可以评估预测的可靠性。  

### <a name="how-cross-validation-works"></a>交叉验证的工作原理

1. 交叉验证会将定型数据随机分成折叠。 

   如果你以前未对数据集进行分区，则该算法将默认为 10 个折叠。 若要将数据集划分为不同数量的折叠，可以使用[分区和示例](partition-and-sample.md)模块，指示要使用的折叠数。  

2.  该模块将留出折叠1中的数据以用于验证。 （这有时称为*维持折叠*。）模块使用剩余的折叠来训练模型。 

    例如，如果创建了五个折叠，模块将在交叉验证过程中生成五个模型。 该模块通过使用 fifths 数据来训练每个模型。 它在剩余的第五个模型中测试每个模型。  

3.  在为每个折叠测试模型期间，该模块将计算多个准确性统计信息。 模块使用哪些统计信息取决于您要评估的模型的类型。 不同的统计信息用于评估分类模型和回归模型。  

4.  完成所有折叠的构建和评估过程后，交叉验证模型将生成一组性能指标，并为所有数据评分结果。 查看这些度量值，以确定是否有任何一个折叠的准确度高或低。 

### <a name="advantages-of-cross-validation"></a>交叉验证的优点

计算模型的另一种常见方法是使用[拆分数据](split-data.md)将数据分成定型集和测试集，然后验证定型数据的模型。 但交叉验证提供了一些优点：  

-   交叉验证使用的测试数据更多。

    交叉验证使用较大的数据空间中的指定参数来测量模型的性能。 也就是说，交叉验证将整个定型数据集用于定型和计算，而不是使用部分。 与此相反，如果您使用通过随机拆分生成的数据来验证模型，则通常只会在30% 或更少的可用数据上评估模型。  

    但是，因为交叉验证会在更大的数据集上定型和验证模型，所以需要更多的计算工作量。 比对随机拆分进行验证要长得多。  

-   交叉验证会计算数据集和模型。

    交叉验证不只是衡量模型的准确性。 它还可让你了解数据集的代表方式以及模型在数据中的变化程度。  

## <a name="how-to-use-cross-validate-model"></a>如何使用交叉验证模型

如果数据集很大，则可能需要较长时间才能运行交叉验证。  因此，您可以在生成和测试模型的初始阶段中使用交叉验证模型。 在该阶段中，您可以评估模型参数的好坏（假定计算时间是可容忍的）。 然后，您可以通过将已建立的参数与[训练模型](train-model.md)和[评估模型](evaluate-model.md)模块结合使用来训练和评估模型。

在此方案中，您将通过使用交叉验证模型来定型和测试模型。

1. 将交叉验证模型模块添加到管道。 在 Azure 机器学习设计器中，可以在**模型计分 & 计算**类别中找到它。 

2. 连接任何分类或回归模型的输出。 

    例如，如果使用**两个类提升决策树**进行分类，请使用所需的参数配置模型。 然后，将连接器从分类器的 "未**训练的模型**" 端口拖到 "交叉验证模型" 的匹配端口。 

    > [!TIP] 
    > 您无需定型模型，因为交叉验证模型会自动将模型作为计算的一部分训练。  
3.  在交叉验证模型的**数据集**端口上，连接任何标记的定型数据集。  

4.  在交叉验证模型的右面板中，单击 "**编辑列**"。 选择包含类标签的单个列或可预测值。 

5. 如果要在同一数据的连续运行之间重复交叉验证的结果，请设置**随机种子**参数的值。  

6. 运行管道。

7. 有关报告的说明，请参阅 "[结果](#results)" 部分。

## <a name="results"></a>结果

所有迭代完成后，交叉验证模型将为整个数据集创建分数。 它还会创建性能指标，可用于评估模型的质量。

### <a name="scored-results"></a>评分的结果

模块的第一个输出为每一行提供源数据以及一些预测值和相关的概率。 

若要查看结果，请在管道中右键单击 "交叉验证模型" 模块。 选择 "**可视化评分结果**"。

| 新列名      | 说明                              |
| -------------------- | ---------------------------------------- |
| 评分标签        | 此列将添加到数据集的末尾。 它包含每行的预测值。 |
| 评分概率 | 此列将添加到数据集的末尾。 它指示**评分标签**中的值的预计概率。 |
| 折叠编号          | 指示在交叉验证过程中每行数据已分配到的折叠的从零开始的索引。 |

 ### <a name="evaluation-results"></a>评估结果

第二个报表按折叠分组。 请记住，在执行过程中，交叉验证模型将定型数据随机拆分为*n*个折叠（默认为10）。 在数据集的每次迭代中，交叉验证模型都使用一个折叠作为验证数据集。 它使用剩余的*第 n-1 个*折叠来定型模型。 对于所有其他折叠中的数据，将对其中的每*个模型进行*测试。

在此报表中，按索引值以升序列出折叠。  若要对其他任何列进行排序，可以将结果保存为数据集。

若要查看结果，请在管道中右键单击 "交叉验证模型" 模块。 选择 "**通过折叠直观显示计算结果**"。


|列名称| 说明|
|----|----|
|折叠编号| 每个折叠的标识符。 如果创建了五个折叠，则会有五个数据子集，编号为0到4。
|折叠中的示例数|分配给每个折叠的行数。 它们应该大致相等。 |


该模块还包括每个折叠的以下度量值，具体取决于您要评估的模型类型： 

+ **分类模型**：精度、回调、F 评分、AUC、准确性  

+ **回归模型**：平均绝对错误、根本平均平方误差、相对绝对错误、相对平方误差和确定系数


## <a name="technical-notes"></a>技术说明  

+ 在将数据集用于交叉验证之前，最佳做法是规范化数据集。 

+ 交叉验证模型的计算工作量要多得多，并且完成时间比使用随机分隔的数据集验证模型的时间更长。 原因是，交叉验证模型训练并验证模型多次。

+ 使用交叉验证来度量模型的准确性时，无需将数据集拆分为定型集和测试集。 


## <a name="next-steps"></a>后续步骤

查看可用于 Azure 机器学习[的模块集](module-reference.md)。 

