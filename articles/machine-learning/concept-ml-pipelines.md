---
title: 什么是 ML 管道
titleSuffix: Azure Machine Learning
description: 本文介绍可通过适用于 Python 的 Azure 机器学习 SDK 生成的机器学习（ML）管道的优点。 数据科学家可以使用机器学习管道来构建、优化和管理其机器学习工作流。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: laobri
author: lobrien
ms.date: 11/06/2019
ms.openlocfilehash: fd10a3e62bcbe438eb17edfc71a5285ad071e29a
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/15/2020
ms.locfileid: "77366217"
---
# <a name="what-are-azure-machine-learning-pipelines"></a>什么是 Azure 机器学习管道？

Azure 机器学习管道使你可以在机器学习项目中创建工作流。 这些工作流有多个优点： 

+ 简易
+ 速度
+ 可重复性
+ 灵活性
+ 版本控制和跟踪
+ 模块化 
+ 质量保证
+ 成本控制

当你的机器学习项目超出了纯粹的探索和迭代后，这些优势就变得非常明显。 即使是简单的单步管道也很有价值。 机器学习项目通常处于一种复杂的状态，这可能会使单个工作流的精确成就成为一个非常简单的过程。

了解如何[创建你的第一个管道](how-to-create-your-first-pipeline.md)。

![Azure 机器学习中的机器学习管道](./media/concept-ml-pipelines/pipeline-flow.png)

<a name="compare"></a>
### <a name="which-azure-pipeline-technology-should-i-use"></a>我应该使用哪种 Azure 管道技术？

Azure 云提供多个其他管道，每个都有不同的用途。 下表列出了不同的管道以及它们的用途：

| 场景 | 主要角色 | Azure 产品 | OSS 产品 | 规范管道 | 优势 | 
| -------- | --------------- | -------------- | ------------ | -------------- | --------- | 
| 模型业务流程（机器学习） | 数据科学家 | Azure 机器学习管道 | Kubeflow 管道 | 数据 > 模型 | 分布，缓存，代码优先，重用 | 
| 数据业务流程（数据准备） | 数据工程 | [Azure 数据工厂管道](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) | Apache 气流 | 数据 > 数据 | 强类型移动。 以数据为中心的活动。 |
| 代码 & 应用程序业务流程（CI/CD） | 应用开发人员/Ops | [Azure DevOps 管道](https://azure.microsoft.com/services/devops/pipelines/) | Jenkins | 代码 + 模型-> 应用/服务 | 最开放和 flexibile 的活动支持，批准队列，具有门的阶段 | 


## <a name="what-can-azure-ml-pipelines-do"></a>Azure ML 管道有哪些用途？

Azure 机器学习管道是完整的机器学习任务的可独立执行的工作流。 子任务封装为管道中的一系列步骤。 Azure 机器学习管道可以像调用 Python 脚本一样简单，因此_可以执行任何操作。_ 管道_应_将精力集中在机器学习任务上，例如：

+ 数据准备，包括导入、验证、清理、修整、转换、规范化和暂存
+ 训练配置，包括参数化自变量、文件路径和日志记录/报告配置
+ 高效重复地进行培训和验证。 效率可能来自指定特定的数据子集、不同的硬件计算资源、分布式处理和进度监视
+ 部署，包括版本控制、缩放、预配和访问控制 

独立步骤允许多个数据科学家同时在同一管道上处理，而无需过多的计算资源。 单独的步骤还可以简化每个步骤使用不同的计算类型/大小。

管道设计好以后，通常会有更多围绕着管道的训练循环进行的优化。 重新运行管道时，运行将跳转到需要重新运行的步骤，如更新的训练脚本。 将跳过不需要重新运行的步骤。 相同的分析适用于用于步骤成就的未更改脚本。 如果基础数据未更改，这种重复使用功能可帮助避免运行成本高昂且耗时较高的步骤，如数据引入和转换。

使用 Azure 机器学习，可以对管道中的每个步骤使用各种工具包和框架，如 PyTorch 或 TensorFlow。 Azure 会协调你使用的各种[计算目标](concept-azure-machine-learning-architecture.md)，因此你的中间数据可以与下游计算目标共享。

你可以直接在 Azure 门户或[工作区登陆页面（预览版）](https://ml.azure.com)中[跟踪管道试验的指标](https://docs.microsoft.com/azure/machine-learning/how-to-track-experiments)。 在发布管道之后，可以配置 REST 终结点，使你能够从任何平台或堆栈重新运行管道。

简而言之，机器学习生命周期的所有复杂任务都可以帮助您处理管道。 其他 Azure 管道技术有其自身的优势，例如用于处理数据的[Azure 数据工厂管道](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities)和用于持续集成和部署的[Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) 。 但如果您的关注点是机器学习，则 Azure 机器学习管道可能是您的工作流需求的最佳选择。 

## <a name="what-are-azure-ml-pipelines"></a>什么是 Azure ML 管道？

Azure ML 管道使用排序的一系列步骤来执行完整的逻辑工作流。 每个步骤都是一个离散的处理操作。 管道在 Azure 机器学习[试验](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py)的上下文中运行。

在 ML 项目的早期阶段，可以创建一个 Jupyter 笔记本或 Python 脚本，用于完成 Azure 工作区和资源配置、数据准备、运行配置、培训和验证的所有工作。 但正如函数和类很快就会成为代码的单个命令块，ML 工作流很快就会变得更适合单一笔记本或脚本。 

通过模块化 ML 任务，管道支持计算机科学，组件应 "执行（仅适用于）。" 在团队中进行编程时，模块化非常重要，但即使是单独工作，即使是小型 ML 项目也涉及单独的任务，每个任务都有很大的复杂性。 任务包括：工作区配置和数据访问、数据准备、模型定义和配置，以及部署。 尽管一个或多个任务的输出构成了其他任务的输出，但在下一任务中，任何一项任务的具体实现细节都是最重要的。 在最糟糕的情况下，一个任务的计算状态可能导致另一个任务中的 bug。 

### <a name="analyzing-dependencies"></a>分析依赖关系

许多编程生态系统都具有用于协调资源、库或编译依赖项的工具。 通常，这些工具使用文件时间戳来计算依赖关系。 当文件更改时，只更新（下载、重新编译或打包）文件及其依赖项。 Azure ML 管道大大扩展了此概念。 与传统的生成工具一样，管道计算步骤间的依赖关系，只执行必要的重新计算。 

不过，Azure ML 管道中的依赖关系分析比简单的时间戳更复杂。 每个步骤都可以在不同的硬件和软件环境中运行。 数据准备可能是一个耗时的过程，但不需要在具有强大 Gpu 的硬件上运行，某些步骤可能需要特定于 OS 的软件，你可能需要使用分布式培训等。 虽然优化资源的成本节约可能会很大，但对于手动调整硬件和软件资源中的所有不同变化而言，这可能是个巨大的。 这样做的难度更难，无需对步骤之间传输的数据进行错误处理。 

管道解决了这一问题。 Azure 机器学习会自动协调管道步骤之间的所有依赖关系。 此业务流程可能包括旋转和缩小 Docker 映像，附加和分离计算资源，以及以一致且自动的方式在各步骤之间移动数据。

### <a name="reusing-results"></a>重复使用结果

此外，如果您选择，步骤的输出可能会重复使用。 如果将重新使用指定为可能性，并且没有上游依赖关系触发重新计算，则管道服务将使用该步骤结果的缓存版本。 此类重复使用会大幅缩短开发时间。 如果你有一个复杂的数据准备任务，你可能会更频繁地重新运行它，而不是严格必要的。 管道缓解了这一点：如有必要，将会运行该步骤，否则将不会。

所有这些依赖关系分析、业务流程和激活都是在实例化[管道](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline(class)?view=azure-ml-py)对象、将其传递到 `Experiment`并调用 `submit()`时 Azure 机器学习处理的。 

### <a name="coordinating-the-steps-involved"></a>协调所涉及的步骤

创建和运行 `Pipeline` 对象时，会执行以下高级步骤：

+ 对于每个步骤，该服务将计算以下各项的要求：
    + 硬件计算资源
    + OS 资源（Docker 映像）
    + 软件资源（Conda/virtualenv 依赖项）
    + 数据输入 
+ 服务确定步骤间的依赖关系，从而生成动态执行图
+ 当执行关系图中的每个节点运行时：
    + 此服务配置必要的硬件和软件环境（可能重复使用现有资源）
    + 该步骤将运行，向其包含的 `Experiment` 对象提供日志记录和监视信息
    + 步骤完成后，会将其输出准备为下一步和/或写入存储的输入
    + 不再需要的资源将完成并分离

![管道步骤](./media/concept-ml-pipelines/run_an_experiment_as_a_pipeline.png)

## <a name="building-pipelines-with-the-python-sdk"></a>通过 Python SDK 生成管道

在[Azure 机器学习 PYTHON SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)中，管道是在 `azureml.pipeline.core` 模块中定义的 Python 对象。 [管道](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline%28class%29?view=azure-ml-py)对象包含一个或多个[PipelineStep](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.builder.pipelinestep?view=azure-ml-py)对象的有序序列。 `PipelineStep` 类是抽象的，而实际步骤则为子类，如[EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimatorstep?view=azure-ml-py)、 [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.pythonscriptstep?view=azure-ml-py)或[DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.datatransferstep?view=azure-ml-py)。 [ModuleStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.modulestep?view=azure-ml-py)类保存可在管道间共享的可重复使用的步骤序列。 `Pipeline` 作为 `Experiment`的一部分运行。

Azure ML 管道与 Azure 机器学习工作区关联，管道步骤与该工作区中可用的计算目标相关联。 有关详细信息，请参阅[创建和管理 Azure 门户中的 Azure 机器学习工作区](https://docs.microsoft.com/azure/machine-learning/how-to-manage-workspace)或[Azure 机器学习中的计算目标？](https://docs.microsoft.com/azure/machine-learning/concept-compute-target)。

在 Azure 机器学习中，计算目标是发生 ML 阶段的环境。 软件环境可以是远程 VM、Azure 机器学习计算、Azure Databricks、Azure Batch 等。 硬件环境也可能会有很大差异，具体取决于 GPU 支持、内存、存储等。 你可以为每个步骤指定计算目标，这使你可以精细控制成本。 你可以使用更多或不太强大的资源来满足你的项目的特定操作、数据量和性能需求。 

## <a name="building-pipelines-with-the-designer"></a>用设计器生成管道

更喜欢可视化设计图面的开发人员可以使用 Azure 机器学习设计器创建管道。 你可以从工作区主页上的**设计器**选择访问此工具。  设计器允许您将步骤拖放到设计图面上。 若要进行快速开发，可以在 ML 任务范围内使用现有模块;现有模块涵盖了从数据转换到算法选择、定型到部署的所有内容。 或者，你可以通过组合你自己在 Python 脚本中定义的步骤来创建完全自定义管道。

在直观地设计管道时，步骤的输入和输出显示为可见。 您可以拖放数据连接，使您能够快速理解和修改管道的数据流。
 
![Azure 机器学习设计器示例](./media/concept-designer/designer-drag-and-drop.gif)

### <a name="understanding-the-execution-graph"></a>了解执行图

管道内的步骤可能依赖于其他步骤。 Azure ML 管道服务执行分析和协调这些依赖项的工作。 生成的 "执行关系图" 中的节点是处理步骤。 每个步骤都可能涉及创建或重复使用硬件和软件的特定组合、重复使用缓存的结果等。 服务的此执行关系图的业务流程和优化可显著提高 ML 阶段并降低成本。 

由于步骤独立运行，因此必须在外部定义用于保存在步骤之间流动的输入和输出数据的对象。 这是[DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py)、 [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py)和关联类的角色。 这些数据对象与封装其存储配置的数据[存储](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore%28class%29?view=azure-ml-py)对象相关联。 始终使用 `name` 字符串、`inputs`列表和 `outputs`列表创建 `PipelineStep` 的基类。 通常情况下，它还具有 `arguments` 的列表，并且通常会有一个 `resource_inputs`列表。 子类通常还会有其他参数（例如，`PythonScriptStep` 需要要运行的脚本的文件名和路径）。 

执行图是非循环的，但管道可以按重复计划运行，并且可以运行 Python 脚本，这些脚本可以将状态信息写入文件系统，从而能够创建复杂的配置文件。 如果设计管道，使某些步骤可以并行或异步运行，Azure 机器学习会以透明方式处理扇出和扇入的依赖项分析和协调。 通常，您不必关心执行关系图的详细信息，但可通过 "[管道](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline.pipeline?view=azure-ml-py#attributes)" 属性获得。 


### <a name="a-simple-python-pipeline"></a>简单的 Python 管道

此代码段显示了创建和运行基本 `Pipeline`所需的对象和调用：

```python
ws = Workspace.from_config() 
blob_store = Datastore(ws, "workspaceblobstore")
compute_target = ws.compute_targets["STANDARD_NC6"]
experiment = Experiment(ws, 'MyExperiment') 

input_data = DataReference(
    datastore=Datastore(ws, blob_store),
    data_reference_name="test_data",
    path_on_datastore="20newsgroups/20news.pkl")

output_data = PipelineData(
    "output_data",
    datastore=blob_store,
    output_name="output_data1")

steps = [ PythonScriptStep(
    script_name="train.py",
    arguments=["--input", input_data, "--output", output_data],
    inputs=[input_data],
    outputs=[output_data],
    compute_target=compute_target,
    source_directory="myfolder"
) ]

pipeline = Pipeline(workspace=ws, steps=steps)

pipeline_run = experiment.submit(pipeline)
pipeline_run.wait_for_completion()
```

代码段开头为常见的 Azure 机器学习对象、`Workspace`、`Datastore`、 [ComputeTarget](https://docs.microsoft.com/python/api/azureml-core/azureml.core.computetarget?view=azure-ml-py)和 `Experiment`。 然后，该代码将创建用于保存 `input_data` 和 `output_data`的对象。 数组 `steps` 保存单个元素、将使用数据对象并在 `compute_target`上运行的 `PythonScriptStep`。 然后，该代码将实例化 `Pipeline` 对象本身，并传入工作区和步骤数组。 调用 `experiment.submit(pipeline)` 会开始 Azure ML 管道的运行。 在管道完成之前，对 `wait_for_completion()` 的调用会被阻止。 

若要详细了解如何将管道连接到数据，请参阅文章[如何访问数据](how-to-access-data.md)和[如何注册数据集](how-to-create-register-datasets.md)。 

## <a name="best-practices-when-using-pipelines"></a>使用管道的最佳做法

正如您所看到的，创建 Azure ML 管道比启动脚本稍微复杂一些。 管道需要配置和创建一些 Python 对象。 

建议使用管道的一些情况：

* 在团队环境中：将 ML 任务划分为多个独立的步骤，使开发人员能够单独工作和演化其程序。 

* 在或接近部署时：钉子配置并使用计划的操作和事件驱动的操作，使数据保持最高。

* 在 ML 项目的早期阶段中或单独工作：使用管道自动执行生成。 如果已开始担心在实现新概念之前重新创建配置和计算状态，这是一个可以考虑使用管道自动执行工作流的信号。 

可以轻松地热情重复使用缓存的结果、对计算成本进行精细控制和处理隔离，但管道确实会产生费用。 某些反模式包括：

* 使用管道作为单独的方法来分隔问题。 Python 的内置函数、对象和模块会非常长，以避免混淆编程状态！ 与函数调用相比，管道步骤要昂贵得多。

* 管道步骤间的繁重耦合。 如果重构依赖步骤通常需要修改上一个步骤的输出，则可能是单独的步骤的成本要低于权益。 步骤过于耦合的另一个提示是不是数据的步骤的参数，而是用于控制处理的标志。 

* 提前优化计算资源。 例如，有几个阶段用于数据准备，一阶段通常会看到 "哦，这里就是可以使用 `MpiStep` 进行并行编程的位置，但在这里，我可以将 `PythonScriptStep` 用于不太强大的计算目标，等等。 而且，在很长一段时间内，创建如这样的细化步骤可能会很有价值，尤其是在有可能使用缓存的结果而不是始终重新计算时。 但管道并不旨在代替 `multiprocessing` 模块。 

在项目很大或更接近部署之前，您的管道应该更粗糙，而不是细化。 如果你将 ML 项目视为涉及_阶段_和管道，以提供完整的工作流来使你逐步完成某个特定阶段，则会出现正确的路径。 

## <a name="key-advantages"></a>主要优点

为机器学习工作流使用管道的主要优点是：

|主要优点|说明|
|:-------:|-----------|
|**无人参与&nbsp;运行**|计划以可靠且无人参与的方式并行运行或按顺序运行的步骤。 数据准备和建模可以过去几天或几周，而管道可用于在进程运行时将精力集中在其他任务上。 |
|**异类计算**|使用跨异类和可缩放计算资源和存储位置可靠协调的多个管道。 通过在不同的计算目标（例如 HDInsight、GPU 数据科学 Vm 和 Databricks）上运行单个管道步骤来有效地使用可用计算资源。|
|**可重用性**|为特定方案（如重新训练和批处理计分）创建管道模板。 通过简单的 REST 调用从外部系统触发已发布的管道。|
|**跟踪和版本控制**|可以使用管道 SDK 显式对数据源、输入和输出进行命名和版本控制，而不是在循环访问时手动跟踪数据和结果路径。 还可以将脚本和数据分开管理以提高工作效率。|
| **模块化** | 分离关注区和隔离更改可让软件以更快的速度以更高的质量发展。 | 
|**协作**|管道使数据科学家能够在机器学习设计过程的所有方面进行协作，同时能够同时处理管道步骤。|

## <a name="modules"></a>模块

虽然管道步骤允许重复使用以前运行的结果，但在许多情况下，该步骤的构造假定所需的脚本和依赖文件必须在本地可用。 如果数据科研人员希望在现有代码的基础上进行构建，则通常必须从单独的存储库克隆脚本和依赖项。

模块在使用管道步骤时相似，但提供通过工作区加速的版本控制，可实现大规模协作和可重用性。 模块设计为在多个管道中重复使用，并且可以发展以适应不同的用例。 用户可以通过工作区执行以下任务，而无需使用外部存储库：

* 创建新模块并发布现有模块的新版本
* 弃用现有版本
* 标记禁用的版本以防止使用者使用该版本
* 指定默认版本
* 从工作区按版本检索模块，以确保团队使用相同的代码

有关如何在 Azure 机器学习管道中创建、连接和使用模块的代码示例，请参阅[笔记本](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-how-to-use-modulestep.ipynb)。

## <a name="next-steps"></a>后续步骤

Azure ML 管道是一项功能强大的工具，可在早期开发阶段开始交付价值。 随着团队和项目的增长，该值也随之增加。 本文介绍了如何通过 Azure 机器学习 Python SDK 指定管道，并在 Azure 上安排了管道。 您已经了解了一些基本的源代码，并介绍了一些可用的 `PipelineStep` 类。 你应了解何时使用 Azure ML 管道以及 Azure 如何运行它们。 


+ 了解如何[创建你的第一个管道](how-to-create-your-first-pipeline.md)。

+ 了解如何[对大型数据运行批预测](tutorial-pipeline-batch-scoring-classification.md )。

+ 有关[管道核心](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py)和[管道的步骤](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py)，请参阅 SDK 参考文档。

+ 试用 Jupyter 笔记本展示[Azure 机器学习管道](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines)的示例。 了解如何[运行笔记本来浏览此服务](samples-notebooks.md)。
