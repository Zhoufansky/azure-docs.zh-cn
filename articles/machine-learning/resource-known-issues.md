---
title: 已知问题与故障排除
titleSuffix: Azure Machine Learning
description: 获取有关 Azure 机器学习的已知问题、解决方法和疑难解答的列表。
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 2522b31788df294c37db4326985edd6c85774561
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/29/2020
ms.locfileid: "78191837"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning"></a>已知问题和故障排除 Azure 机器学习

本文可帮助你查找和更正使用 Azure 机器学习时遇到的错误或故障。

## <a name="sdk-installation-issues"></a>SDK 安装问题

**错误消息：无法卸载 'PyYAML'**

用于 Python 的 Azure 机器学习 SDK： PyYAML 是 distutils 安装的项目。 因此，在部分卸载的情况下，我们无法准确确定哪些文件属于它。 若要在忽略此错误的同时继续安装 SDK，请使用：

```Python
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

错误消息： **`ERROR: No matching distribution found for azureml-dataprep-native`**

Anaconda 的 Python 3.7.4 分发有一个破坏 azureml-sdk 安装的缺陷。 此[GitHub 问题](https://github.com/ContinuumIO/anaconda-issues/issues/11195)中讨论了此问题，可通过使用此命令创建新的 Conda 环境来解决此问题：
```bash
conda create -n <env-name> python=3.7.3
```
这将使用 Python 3.7.3 创建 Conda 环境，而3.7.4 中不存在安装问题。

## <a name="training-and-experimentation-issues"></a>培训和试验问题

### <a name="metric-document-is-too-large"></a>指标文档太大
Azure 机器学习对可从定型运行一次记录的指标对象的大小进行内部限制。 如果在记录列表值指标时遇到 "指标文档太大" 错误，请尝试将列表拆分为较小的区块，例如：

```python
run.log_list("my metric name", my_metric[:N])
run.log_list("my metric name", my_metric[N:])
```

在内部，Azure ML 会将具有相同指标名称的块连接到一个连续列表中。

### <a name="moduleerrors-no-module-named"></a>ModuleErrors （没有名为的模块）
如果在 Azure ML 中提交试验时运行到 ModuleErrors 中，则表示训练脚本需要安装一个包，但不会添加它。 提供包名称后，Azure ML 会在用于定型运行的环境中安装包。 

如果使用[估算](concept-azure-machine-learning-architecture.md#estimators)提交试验，则可以通过基于要安装包的源中的估计器 `pip_packages` 或 `conda_packages` 参数指定包名称。 你还可以使用 `conda_dependencies_file`指定包含所有依赖项的 docker-compose.override.yml 文件，或者使用 `pip_requirements_file` 参数列出 txt 文件中的所有 pip 要求。 如果你有自己的 Azure ML 环境对象，而你想要重写估计器使用的默认图像，则可以通过估计器构造函数的 `environment` 参数指定该环境。

Azure ML 还为 Tensorflow、PyTorch、Chainer 和 Spark-sklearn 提供框架特定的估算。 使用这些估算将确保在用于定型的环境中代表您安装核心框架依赖项。 您可以选择指定额外的依赖项，如上所述。 
 
Azure ML 维护的 docker 映像，可以在[AzureML 容器](https://github.com/Azure/AzureML-Containers)中查看其内容。
特定于框架的依赖关系在各自的框架文档中列出- [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py#remarks)、 [PyTorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py#remarks)、 [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py#remarks)、 [spark-sklearn](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py#remarks)。

> [!Note]
> 如果你认为某个特定包的常用空间足以添加到 Azure ML 维护的映像和环境中，请在[AzureML 容器](https://github.com/Azure/AzureML-Containers)中提出 GitHub 问题。 
 
### <a name="nameerror-name-not-defined-attributeerror-object-has-no-attribute"></a>NameError （未定义名称），AttributeError （对象没有属性）
此异常应来自训练脚本。 你可以查看 Azure 门户的日志文件，以获取有关特定名称（未定义）或属性错误的详细信息。 从 SDK，你可以使用 `run.get_details()` 来查看错误消息。 这还将列出为运行生成的所有日志文件。 请确保在重新提交运行之前，查看训练脚本并修复错误。 

### <a name="horovod-has-been-shut-down"></a>Horovod 已关闭
在大多数情况下，如果遇到 "AbortedError： Horovod 已关闭"，则表示某个进程中存在一个导致 Horovod 关闭的基础异常。 MPI 作业中的每个排名都在 Azure ML 中获得专用的日志文件。 这些日志命名为 `70_driver_logs`。 对于分布式培训，日志名称以 `_rank` 为后缀，以便更容易区分日志。 若要查找导致 Horovod 关闭的确切错误，请浏览所有日志文件，并在 driver_log 文件的末尾查找 `Traceback`。 其中一项文件会为你带来实际的基础异常。 

### <a name="sr-iov-availability-on-ncv3-machines-in-amlcompute-for-distributed-training"></a>AmlCompute 中的 NCv3 计算机上的 SR-IOV 可用性，用于分布式培训
Azure 计算已推出 NCv3 计算机的[sr-iov 升级](https://azure.microsoft.com/updates/sriov-availability-on-ncv3-virtual-machines-sku/)，客户可以利用它来利用 Azure ML 的托管计算产品/服务（AmlCompute）。 这些更新将支持整个 MPI 堆栈的支持，并可以使用不使用的 RDMA 网络来改进多节点分布式定型性能，尤其是对于深度学习。

查看[更新计划](https://azure.microsoft.com/updates/sr-iov-availability-schedule-on-ncv3-virtual-machines-sku/)，查看何时为你的区域推出支持。

### <a name="run-or-experiment-deletion"></a>运行或试验删除
可以通过使用[试验. archive](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#archive--)方法或通过 "存档试验" 按钮 Azure 机器学习 studio 客户端中的 "试验" 选项卡视图来存档试验。 此操作将从列表查询和视图中隐藏实验，但不会将其删除。

当前不支持永久删除各个试验或运行。 有关删除工作区资产的详细信息，请参阅[导出或删除机器学习服务工作区数据](how-to-export-delete-data.md)。

## <a name="azure-machine-learning-compute-issues"></a>Azure 机器学习计算问题
使用 Azure 机器学习计算（AmlCompute）的已知问题。

### <a name="trouble-creating-amlcompute"></a>创建 AmlCompute 时遇到问题

在 GA 版本之前，从 Azure 门户创建其 Azure 机器学习工作区的某些用户极有可能无法在该工作区中创建 AmlCompute。 你可以对服务提出支持请求，或者通过门户或 SDK 创建新的工作区以立即取消阻止。

### <a name="outage-sr-iov-upgrade-to-ncv3-machines-in-amlcompute"></a>中断： SR-IOV 升级到 AmlCompute 中的 NCv3 计算机

Azure 计算将更新从11月 2019 11 日年初开始的 NCv3 Sku，以支持所有 MPI 实现和版本，并提供适用于未受支持的虚拟机的 RDMA 谓词。 这将需要较短的停机时间-[详细了解 sr-iov 升级](https://azure.microsoft.com/updates/sriov-availability-on-ncv3-virtual-machines-sku)。

作为 Azure 机器学习托管计算产品（AmlCompute）的客户，此时不需要进行任何更改。 根据[更新计划](https://azure.microsoft.com/updates/sr-iov-availability-schedule-on-ncv3-virtual-machines-sku)，你需要在训练中计划一个短暂的中断。 服务将负责更新群集节点上的 VM 映像，并自动扩展群集。 升级完成后，除了获得更高的带宽、延迟时间和更好的分布式应用程序性能之外，还可以使用其他所有 MPI 分发（如 OpenMPI 和 Pytorch）。

## <a name="azure-machine-learning-designer-issues"></a>Azure 机器学习设计器问题

设计器的已知问题。

### <a name="long-compute-preparation-time"></a>长计算准备时间

创建新的计算或调用它保留计算需要时间，可能需要几分钟甚至更长时间。 团队正在努力优化。


### <a name="cannot-run-an-experiment-only-contains-a-dataset"></a>无法运行只包含数据集的试验 

你可能想要运行一个试验，只包含数据集来可视化数据集。 但是，不允许运行仅包含数据集的试验。 我们正在积极地解决此问题。
 
在修复之前，可以将数据集连接到任何数据转换模块（选择数据集中的列，编辑元数据，拆分数据等）并运行试验。 然后，可以可视化数据集。 

下图显示了如何： ![visulize-data](./media/resource-known-issues/aml-visualize-data.png)

## <a name="image-building-failure"></a>映像生成失败

部署 Web 服务时映像生成失败。 解决方法是将“pynacl==1.2.1”作为 pip 依赖项添加到 Conda 文件以进行映像配置。

## <a name="deployment-failure"></a>部署失败

如果观察到 `['DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>`，请将部署中使用的 VM 的 SKU 更改为具有更多内存的 SKU。

## <a name="fpgas"></a>FPGA

你将无法在 FPGA 上部署模型，直到已请求并获得 FPGA 配额批准为止。 若要请求访问权限，请填写配额请求表单： https://aka.ms/aml-real-time-ai

## <a name="automated-machine-learning"></a>自动化机器学习

Tensor Flow 自动化机器学习当前不支持 Tensor 流版本1.13。 安装此版本将导致包依赖关系停止工作。 我们正在努力解决此问题。 

### <a name="experiment-charts"></a>试验图

自4/12 以来，自动 ML 试验迭代中显示的二进制分类图表（精度召回、ROC、增益曲线等）在用户界面中无法正确呈现。 图表绘图当前显示的是反转结果，在这种情况下，较低的结果显示更好的模型。 正在调查解决方案。

## <a name="datasets-and-data-preparation"></a>数据集和数据准备

这些是 Azure 机器学习数据集的已知问题。

### <a name="typeerror-filenotfound-no-such-file-or-directory"></a>TypeError： FileNotFound：没有此类文件或目录

如果您提供的文件路径不在文件所在位置，则会出现此错误。 你需要确保引用文件的方式与你在计算目标上将数据集装载到的位置一致。 为了确保确定状态，我们建议在将数据集装载到计算目标时使用抽象路径。 例如，在下面的代码中，我们将数据集装载到计算目标的文件系统的根目录下，`/tmp`。 

```python
# Note the leading / in '/tmp/dataset'
script_params = {
    '--data-folder': dset.as_named_input('dogscats_train').as_mount('/tmp/dataset'),
} 
```

如果不包含前导正斜杠 "/"，则需要为工作目录添加前缀，例如，在计算目标上 `/mnt/batch/.../tmp/dataset`，以指示要将数据集装载到的位置。 

### <a name="fail-to-read-parquet-file-from-http-or-adls-gen-2"></a>无法从 HTTP 或 ADLS Gen 2 读取 Parquet 文件

AzureML DataPrep SDK 版本1.1.25 中存在一个已知问题，它会在通过从 HTTP 或 ADLS Gen 2 读取 Parquet 文件创建数据集时导致失败。 它将失败，并 `Cannot seek once reading started.`。 若要解决此问题，请将 `azureml-dataprep` 升级到高于1.1.26 的版本，或降级到低于1.1.24 的版本。

```python
pip install --upgrade azureml-dataprep
```

### <a name="typeerror-mount-got-an-unexpected-keyword-argument-invocation_id"></a>TypeError： mount （）获取了意外的关键字参数 "invocation_id"

如果 `azureml-core` 和 `azureml-dataprep`不兼容，则会出现此错误。 如果看到此错误，请将 `azureml-dataprep` 包升级到较新版本（大于或等于1.1.29）。

```python
pip install --upgrade azureml-dataprep
```

## <a name="databricks"></a>Databricks

Databricks 和 Azure 机器学习问题。

### <a name="failure-when-installing-packages"></a>安装包时失败

安装更多包时，Azure 机器学习 SDK 安装在 Azure Databricks 上失败。 某些包（如 `psutil`）可能会导致冲突。 若要避免安装错误，请通过冻结库版本来安装包。 此问题与 Databricks 相关，不适用于 Azure 机器学习 SDK。 其他库也可能会遇到此问题。 示例：

```python
psutil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
```

或者，如果在 Python 库中保留了饰面安装问题，则可以使用 init 脚本。 此方法并不正式支持。 有关详细信息，请参阅[群集范围的初始化脚本](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts)。

### <a name="cancel-an-automated-machine-learning-run"></a>取消自动机器学习运行

在 Azure Databricks 上使用自动机器学习功能时，若要取消运行并启动新的试验运行，请重新启动 Azure Databricks 群集。

### <a name="10-iterations-for-automated-machine-learning"></a>自动机器学习 > 10 次迭代

在自动机器学习设置中，如果迭代次数超过10个，则在提交运行时，将 `show_output` 设置为 `False`。

### <a name="widget-for-the-azure-machine-learning-sdk-and-automated-machine-learning"></a>用于 Azure 机器学习 SDK 和自动化机器学习的小组件

Databricks 笔记本不支持 Azure 机器学习 SDK 小组件，因为笔记本无法解析 HTML 小组件。 可以通过在 Azure Databricks 笔记本单元中使用此 Python 代码在门户中查看小组件：

```
displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
```

### <a name="import-error-cannot-import-name-timedelta-from-pandas_libstslibs"></a>导入错误：无法从 "pandas _libs. tslibs" 导入名称 "Timedelta"

如果你在使用自动机器学习时看到此错误，请在笔记本中运行以下两行：
```
%sh rm -rf /databricks/python/lib/python3.7/site-packages/pandas-0.23.4.dist-info /databricks/python/lib/python3.7/site-packages/pandas
%sh /databricks/python/bin/pip install pandas==0.23.4
```

### <a name="import-error-no-module-named-pandascoreindexes"></a>导入错误：没有名为 "pandas" 的模块

如果你在使用自动机器学习时看到此错误：

1. 运行以下命令，在 Azure Databricks 群集中安装两个包： 

   ```
   scikit-learn==0.19.1
   pandas==0.22.0
   ```

1. 分离群集，然后将其重新连接到笔记本。 

如果这些步骤不能解决问题，请尝试重新启动群集。

### <a name="failtosendfeather"></a>FailToSendFeather

如果在 Azure Databricks 群集上读取数据时出现 `FailToSendFeather` 错误，请参阅以下解决方案：

* 将 `azureml-sdk[automl]` 包升级到最新版本。
* 添加 `azureml-dataprep` 版本1.1.8 或更高版本。
* 添加 `pyarrow` 版本0.11 或更高版本。

## <a name="azure-portal"></a>Azure 门户

如果直接通过 SDK 或门户的共享链接查看工作区，则将无法在扩展程序中查看包含订阅信息的常规概述页。 也将无法切换到另一个工作区。 如果需要查看其他工作区，则解决方法是直接执行[Azure 机器学习 studio](https://ml.azure.com)并搜索工作区名称。

## <a name="diagnostic-logs"></a>诊断日志

如果在请求帮助时可以提供诊断信息，有时会很有帮助。 若要查看某些日志，请访问[Azure 机器学习 studio](https://ml.azure.com)并访问工作区，并选择 "**工作区" > 试验 > 运行 > 日志**。  

> [!NOTE]
> Azure 机器学习在定型期间记录来自各种来源的信息，如 AutoML 或运行训练作业的 Docker 容器。 其中许多日志没有记录。 如果遇到问题，请与 Microsoft 支持部门联系，他们可以在故障排除过程中使用这些日志。

## <a name="activity-logs"></a>活动日志

Azure 机器学习工作区中的某些操作不会将信息记录到__活动日志__。 例如，启动定型运行或注册模型。

其中的一些操作显示在工作区的 "__活动__" 区域中，但并不表示启动了活动的人员。

## <a name="resource-quotas"></a>资源配额

了解使用 Azure 机器学习时可能遇到的[资源配额](how-to-manage-quotas.md)。

## <a name="authentication-errors"></a>身份验证错误

如果通过远程作业对某个计算目标执行管理操作，会收到以下错误之一：

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

例如，如果尝试通过一个为实施远程执行操作而提交的机器学习管道创建或附加计算目标，会收到错误。

## <a name="overloaded-azurefile-storage"></a>重载的 AzureFile 存储

如果收到 `Unable to upload project files to working directory in AzureFile because the storage is overloaded`错误，请应用以下解决方法。

如果对其他工作负荷（如数据传输）使用文件共享，则建议使用 blob，以便可以自由地使用文件共享来提交运行。 你还可以在两个不同的工作区之间拆分工作负荷。

## <a name="webservices-in-azure-kubernetes-service-failures"></a>Azure Kubernetes 服务中的 Webservices 故障 

Azure Kubernetes 服务中的许多 webservice 故障都可以通过使用 `kubectl`连接到群集来进行调试。 可以通过运行来获取 Azure Kubernetes 服务群集的 `kubeconfig.json`

```bash
az aks get-credentials -g <rg> -n <aks cluster name>
```

## <a name="updating-azure-machine-learning-components-in-aks-cluster"></a>更新 AKS 群集中 Azure 机器学习组件

必须手动应用 Azure Kubernetes Service 群集中安装 Azure 机器学习组件的更新。 

可以通过从 "Azure 机器学习" 工作区分离群集，然后将群集重新附加到工作区来应用这些更新。 如果在群集中启用了 SSL，则需要在重新附加群集时提供 SSL 证书和私钥。 

```python
compute_target = ComputeTarget(workspace=ws, name=clusterWorkspaceName)
compute_target.detach()
compute_target.wait_for_completion(show_output=True)

attach_config = AksCompute.attach_configuration(resource_group=resourceGroup, cluster_name=kubernetesClusterName)

## If SSL is enabled.
attach_config.enable_ssl(
    ssl_cert_pem_file="cert.pem",
    ssl_key_pem_file="key.pem",
    ssl_cname=sslCname)

attach_config.validate_configuration()

compute_target = ComputeTarget.attach(workspace=ws, name=args.clusterWorkspaceName, attach_configuration=attach_config)
compute_target.wait_for_completion(show_output=True)
```

如果不再具有 SSL 证书和私钥，或者使用的是 Azure 机器学习生成的证书，则可以在分离群集之前通过使用 `kubectl` 和 `azuremlfessl`检索机密来检索这些文件。

```bash
kubectl get secret/azuremlfessl -o yaml
```

>[!Note]
>Kubernetes 以64编码格式存储密码。 在向 `attach_config.enable_ssl`提供机密之前，需要对 `cert.pem` 和机密的 `key.pem` 组件进行64解码。 

## <a name="labeling-projects-issues"></a>标记项目问题

标记项目的已知问题。

### <a name="only-datasets-created-on-blob-datastores-can-be-used"></a>只能使用在 blob 数据存储上创建的数据集

这是当前版本的已知限制。 

### <a name="after-creation-the-project-shows-initializing-for-a-long-time"></a>在创建后，项目会长时间显示 "正在初始化"

手动刷新页面。 初始化应大约每秒大约20个数据点。 缺少 autorefresh 是一个已知问题。 

### <a name="when-reviewing-images-newly-labeled-images-are-not-shown"></a>查看图像时，不会显示新标记的图像

若要加载所有标记的图像，请选择**第一个**按钮。 **第一个**按钮将返回到列表的前面，但会加载所有标记的数据。

### <a name="pressing-esc-key-while-labeling-for-object-detection-creates-a-zero-size-label-on-the-top-left-corner-submitting-labels-in-this-state-fails"></a>如果在为对象检测标记时按 Esc 键，则会在左上角创建大小为零的标签。 提交处于此状态的标签失败。

单击标签旁边的交叉标记，删除标签。

## <a name="moving-the-workspace"></a>移动工作区

> [!WARNING]
> 不支持将 Azure 机器学习工作区移动到不同的订阅，或将拥有的订阅移动到新租户。 这样做可能会导致错误。
