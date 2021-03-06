---
title: 在 IBM 大型机上连接到3270应用
description: 使用 azure 逻辑应用和 IBM 3270 连接器在 Azure 中集成和自动执行3270屏幕驱动的应用
services: logic-apps
ms.suite: integration
author: ChristopherHouser
ms.author: chrishou
ms.reviewer: estfan, valthom
ms.topic: article
ms.date: 03/06/2019
tags: connectors
ms.openlocfilehash: a9d3d0287e7839d6396553d532ba6f293fb19b68
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2020
ms.locfileid: "77647659"
---
# <a name="integrate-3270-screen-driven-apps-on-ibm-mainframes-with-azure-by-using-azure-logic-apps-and-ibm-3270-connector"></a>使用 azure 逻辑应用和 IBM 3270 连接器将 IBM 大型机上的3270屏幕驱动的应用集成到 Azure 中

> [!NOTE]
> 此连接器为[*公共预览版*](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。 

使用 Azure 逻辑应用和 IBM 3270 连接器，你可以通过在3270仿真器屏幕上导航来访问和运行通常驱动的 IBM 大型机应用。 通过这种方式，你可以通过使用 Azure 逻辑应用创建自动化工作流，将 IBM 大型机应用与 Azure、Microsoft 及其他应用、服务和系统集成。 连接器通过使用 TN3270 协议与 IBM 大型机通信，并在除 Azure 政府版和 Azure 中国世纪以外的所有 Azure 逻辑应用区域中提供。 如果你不熟悉逻辑应用，请查看[什么是 Azure 逻辑应用？](../logic-apps/logic-apps-overview.md)

本文介绍使用3270连接器的以下方面： 

* 为什么使用 Azure 逻辑应用中的 IBM 3270 连接器以及连接器如何运行3270屏幕驱动的应用

* 使用3270连接器的先决条件和设置

* 向逻辑应用添加3270连接器操作的步骤

## <a name="why-use-this-connector"></a>为什么要使用此连接器？

若要访问 IBM 大型机上的应用，通常使用3270终端模拟器，通常称为 "绿屏"。 此方法是一种时间测试方法，但有一些限制。 尽管 Host Integration Server （他）可以帮助你直接使用这些应用程序，但有时，可能无法将屏幕和业务逻辑分离。 或者，也许你不再具有主机应用程序工作原理的信息。

为了扩展这些方案，Azure 逻辑应用中的 IBM 3270 连接器适用于3270设计工具，可用于记录或 "捕获"、用于特定任务的主机屏幕、通过大型机应用定义该任务的导航流，以及定义具有该任务的输入和输出参数的方法。 设计工具将该信息转换为3270连接器在从逻辑应用中调用表示该任务的操作时使用的元数据。

从设计工具生成元数据文件后，可以将该文件添加到 Azure 中的集成帐户。 这样一来，逻辑应用就可以在添加3270连接器操作时访问应用的元数据。 连接器从集成帐户中读取元数据文件，处理通过3270屏幕的导航，并动态显示3270连接器操作的参数。 然后，可以向宿主应用程序提供数据，连接器会将结果返回给逻辑应用。 通过这种方式，你可以将旧版应用与 azure 逻辑应用支持的 Azure、Microsoft 和其他应用、服务和系统集成。

## <a name="prerequisites"></a>必备条件

* Azure 订阅。 如果没有 Azure 订阅，请[注册一个免费 Azure 帐户](https://azure.microsoft.com/free/)。

* 有关[如何创建逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)的基本知识

* 建议： [integration service 环境（ISE）](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) 

  你可以选择此环境作为创建和运行逻辑应用的位置。 ISE 提供从逻辑应用到在 Azure 虚拟网络中受保护的资源的访问权限。

* 用于自动执行和运行3270屏幕驱动应用的逻辑应用

  IBM 3270 连接器没有触发器，因此使用另一个触发器来启动逻辑应用，如**重复**触发器。 然后，可以添加3270连接器操作。 若要开始，请[创建一个空白逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)。 
  如果使用 ISE，请选择该 ISE 作为逻辑应用的位置。

* [下载并安装3270设计工具](https://aka.ms/3270-design-tool-download)。
唯一的先决条件是[Microsoft .NET Framework 4.6.1](https://aka.ms/net-framework-download)。

  此工具可帮助你记录应用中添加并作为3270连接器操作运行的任务的屏幕、导航路径、方法和参数。 此工具将生成一个主机集成设计器 XML （HIDX）文件，该文件为连接器提供必要的元数据以用于驱动您的大型机应用。
  
  下载并安装此工具后，请按照以下步骤连接到你的主机：

  1. 打开3270设计工具。 从**会话**菜单中，选择 "**主机会话**"。
  
  1. 提供 TN3270 主机服务器信息。

* [集成帐户](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)，可将 HIDX 文件存储为地图，使逻辑应用能够访问该文件中的元数据和方法定义。 

  请确保集成帐户已链接到所使用的逻辑应用。 此外，如果使用 ISE，请确保集成帐户的位置与逻辑应用使用的 ISE 相同。

* 访问托管大型机应用的 TN3270 服务器

<a name="define-app-metadata"></a>

## <a name="create-metadata-overview"></a>创建元数据概述

在3270屏幕驱动的应用程序中，屏幕和数据字段对于你的方案来说是唯一的，因此3270连接器需要此有关你的应用程序的信息，你可以将这些信息作为元数据提供。 此元数据描述有助于逻辑应用识别和识别屏幕的信息，描述如何在屏幕之间导航、在何处输入数据以及从何处获得结果。 若要指定和生成此元数据，请使用3270设计工具，该工具会指导你完成这些特定*模式*或阶段，如稍后更详细的介绍：

* **捕获**：在此模式下，您将记录使用您的大型机应用完成特定任务所需的屏幕，例如，获得银行余额。

* **导航**：在此模式中，指定如何在大型机应用程序的屏幕中导航特定任务的计划或路径。

* **方法**：在此模式下，可定义用于描述屏幕导航路径的方法，例如 `GetBalance`。 您还可以在每个屏幕上选择将成为该方法的输入和输出参数的字段。

### <a name="unsupported-elements"></a>不支持的元素

设计工具不支持这些元素：

* 部分 IBM 基本映射支持（BMS）映射：如果导入 BMS 映射，设计工具将忽略部分屏幕定义。
* In/Out 参数：不能定义 In/Out 参数。
* 菜单处理：预览期间不支持
* 数组处理：预览期间不支持

<a name="capture-screens"></a>

## <a name="capture-screens"></a>捕获屏幕

在此模式下，将在每个用于唯一标识该屏幕的3270屏幕上标记一项。 例如，您可以指定一行文本或更复杂的一组条件，如特定文本和非空字段。 您可以通过与主机服务器的实时连接记录这些屏幕，或从 IBM 基本映射支持（BMS）映射中导入此信息。 实时连接使用 TN3270 模拟器连接到主机。 每个连接器操作都必须映射到一个任务，该任务从连接到会话开始，到断开与会话的连接。

1. 打开3270设计工具（如果尚未这样做）。 在工具栏上，选择 "**捕获**" 以便进入捕获模式。

1. 若要开始录制，请按 F5 键，或从 "**录制**" 菜单中选择 "**开始记录**"。 

1. 在**会话**菜单中，选择 "**连接**"。

1. 从应用中的第一个屏幕开始，在 "**捕获**" 窗格中，逐步执行你要录制的特定任务的应用。

1. 完成任务后，请按通常的方式从应用注销。

1. 在**会话**菜单中，选择 "**断开连接**"。

1. 若要停止录制，请按 Shift + F5 键，或从 "**录制**" 菜单中选择 "**停止录制**"。

   捕获任务的屏幕后，设计器工具会显示表示这些屏幕的缩略图。 有关这些缩略图的一些注意事项：

   * 包含在捕获的屏幕中，有一个名为 "Empty" 的屏幕。

     首次连接到[CICS](https://www.ibm.com/it-infrastructure/z/cics)时，必须发送 "Clear" 键，然后才能输入要运行的事务的名称。 发送 "Clear" 键的屏幕没有任何*识别属性*（如屏幕标题），可以使用屏幕识别编辑器添加这些属性。 为表示此屏幕，缩略图包含一个名为 "Empty" 的屏幕。 稍后可以使用此屏幕来表示输入交易名称的屏幕。

   * 默认情况下，捕获的屏幕的名称使用屏幕上的第一个词。 如果该名称已存在，则设计工具会在该名称的后面追加一个下划线和一个数字，例如 "WBGB" 和 "WBGB_1"。

1. 若要为捕获的屏幕指定更有意义的名称，请执行以下步骤：

   1. 在 "**主机屏幕**" 窗格中，选择要重命名的屏幕。

   1. 在同一窗格中，在相同窗格的底部附近，查找 "**屏幕名称**" 属性。

   1. 将当前屏幕名称更改为更具描述性的名称。

1. 现在，指定用于标识每个屏幕的字段。

   使用3270数据流时，屏幕没有默认标识符，因此需要在每个屏幕上选择唯一文本。 对于复杂方案，可以指定多个条件，例如，唯一文本和具有特定条件的字段。

选择完识别字段后，转到下一个模式。

### <a name="conditions-for-identifying-repeated-screens"></a>用于标识重复屏幕的条件

为了使连接器在屏幕之间导航和区分，通常在屏幕上查找唯一文本，您可以在捕获的屏幕中将其用作标识符。 对于重复屏幕，可能需要更多的标识方法。 例如，假设您有两个外观相同的屏幕，但一个屏幕返回一个有效值，而另一个屏幕返回一条错误消息。

在设计工具中，可以通过使用屏幕识别编辑器来添加*识别属性*（例如，"获取帐户余额" 等屏幕标题）。 如果有一个分叉路径，并且两个分支返回相同的屏幕但具有不同的结果，则需要其他识别属性。 在运行时，连接器使用这些属性来确定当前分支和分叉。 下面是可以使用的条件：

* 特定值：此值与指定位置的指定字符串相匹配。
* 不是特定值：此值与指定位置处的指定字符串不匹配。
* 空：此字段为空。
* NOT 空：此字段不为空。

若要了解详细信息，请参阅本主题后面的[示例导航计划](#example-plan)。

<a name="define-navigation"></a>

## <a name="define-navigation-plans"></a>定义导航计划

在此模式下，可定义用于在大型机应用程序的屏幕中导航特定任务的流或步骤。 例如，有时你可能有多个路径，你的应用程序可能会在一个路径生成正确的结果时采用多个路径，而另一个路径则会产生错误。 对于每个屏幕，指定移动到下一屏幕所需的击键，如 `CICSPROD <enter>`。

> [!TIP]
> 如果你使用相同的 "连接" 和 "断开连接" 屏幕自动执行多个任务，则设计工具将提供特殊的 Connect 和 Disconnect 计划类型。 定义这些计划时，可以将它们添加到导航计划的开始和结束位置。

### <a name="guidelines-for-plan-definitions"></a>计划定义准则

* 包括所有屏幕，从连接到断开连接结束。

* 你可以创建独立计划或使用连接和断开连接计划，这使你可以重复使用一系列通用于所有事务的屏幕。

  * 连接计划中的最后一个屏幕必须与导航计划中第一个屏幕的屏幕相同。

  * 断开连接计划中的第一个屏幕必须与导航计划中的最后一个屏幕相同。

* 捕获的屏幕可能包含多个重复的屏幕，因此，请在计划中选择并仅使用任何重复屏幕的一个实例。 下面是重复屏幕的一些示例：

  * 登录屏幕，例如， **MSG-10**屏幕
  * CICS 的欢迎屏幕
  * "清除" 或**空白**屏幕

<a name="create-plans"></a>

### <a name="create-plans"></a>创建计划

1. 在3270设计工具的工具栏上，选择 "**导航**" 以便进入导航模式。

1. 若要启动计划，请在**导航**窗格中选择 "**新建计划**"。

1. 在 "**选择新计划名称**" 下，输入计划的名称。 从 "**类型**" 列表中，选择计划类型：

   | 计划类型 | 说明 |
   |-----------|-------------|
   | **处理** | 对于独立或合并计划 |
   | **“连接”** | 对于连接计划 |
   | **断开连接** | 对于断开连接计划 |
   |||

1. 从 "**主机屏幕**" 窗格中，将捕获的缩略图拖动到**导航**窗格中的导航计划图面。

   若要表示输入交易名称的空白屏幕，请使用 "空" 屏幕。

1. 按照描述正在定义的任务的顺序排列这些屏幕。

1. 若要定义屏幕之间的流路径，包括分叉和联接，请在设计工具的工具栏上选择 "**流**"。

1. 选择流中的第一个屏幕。 将连接拖放到流中的下一屏幕。

1. 对于每个屏幕，提供**辅助键**属性的值（注意标识符）和 "**固定文本**" 属性的值，该属性会将流移动到下一屏幕。

   您可能只需要辅助密钥，或助手键和固定文本。

完成导航计划后，可以[在下一模式下定义方法](#define-method)。

<a name="example-plan"></a>

### <a name="example"></a>示例

在此示例中，假设你运行了具有以下步骤的名为 "WBGB" 的 CICS 事务： 

* 在第一个屏幕上，输入 "名称" 和 "帐户"。
* 在第二个屏幕上，可以获得帐户余额。
* 退出到 "空" 屏幕。
* 从 CICS 注销到 "MSG-10" 屏幕。

同时假设您重复这些步骤，但您输入了不正确的数据，以便您可以捕获显示错误的屏幕。 下面是您捕获的屏幕：

* MSG-10
* CICS 欢迎
* 空
* WBGB_1 （输入）
* WBGB_2 （错误）
* Empty_1
* MSG-10_1

尽管此处的许多屏幕都有唯一的名称，但有些屏幕是相同的屏幕，例如，"MSG-10" 和 "Empty"。 对于重复屏幕，你的计划中只使用该屏幕的一个实例。 以下示例演示了独立计划、连接计划、断开连接计划和组合计划的外观：

* 独立计划

  ![独立导航计划](./media/connectors-create-api-3270/standalone-plan.png)

* 连接计划

  ![连接计划](./media/connectors-create-api-3270/connect-plan.png)

* 断开连接计划

  ![断开连接计划](./media/connectors-create-api-3270/disconnect-plan.png)

* 合并计划

  ![合并计划](./media/connectors-create-api-3270/combined-plan.png)

#### <a name="example-identify-repeated-screens"></a>示例：标识重复屏幕

为了使连接器导航和区分屏幕，通常会在屏幕上查找唯一文本，可以将其用作捕获屏幕上的标识符。 对于重复屏幕，可能需要更多的标识方法。 示例计划有一个分叉，你可以在其中获取类似的屏幕。 一个屏幕返回帐户余额，而另一个屏幕返回一条错误消息。

使用 "设计" 工具，可以通过使用屏幕识别编辑器来添加识别属性，例如名为 "获取帐户余额" 的屏幕标题。 对于类似的屏幕，需要其他属性。 在运行时，连接器使用这些属性来确定分支和分叉。

* 在返回有效输入的分支中，这是具有帐户余额的屏幕，你可以添加具有 "非空" 条件的字段。

* 在返回错误的分支中，可以添加一个具有 "空" 条件的字段。

<a name="define-method"></a>

## <a name="define-methods"></a>定义方法

在此模式下，您定义一个与您的导航计划相关联的方法。 对于每个方法参数，指定数据类型，如字符串、整数、日期或时间，等等。 完成后，可以在实时主机上测试方法，并确认方法按预期方式工作。 然后生成元数据文件或主机集成设计器 XML （HIDX）文件，该文件现在具有用于创建和运行 IBM 3270 连接器的操作的方法定义。

1. 在3270设计工具的工具栏上，选择 "**方法**"，以便输入方法模式。 

1. 在**导航**窗格中，选择包含所需输入字段的屏幕。

1. 若要添加方法的第一个输入参数，请执行以下步骤：

   1. 在 "**捕获**" 窗格的 "3270 仿真程序" 屏幕上，选择要作为第一个输入的整个字段，而不只是字段中的文本。

      > [!TIP]
      > 若要显示所有字段并确保选择 "完成" 字段，请在 "**视图**" 菜单上选择 "**所有字段**"。

   1. 在设计工具的工具栏上，选择 "**输入字段**"。 

   若要添加更多输入参数，请为每个参数重复前面的步骤。

1. 若要添加方法的第一个输出参数，请执行以下步骤：

   1. 在 "**捕获**" 窗格的 "3270 仿真程序" 屏幕上，选择要作为第一个输出的整个字段，而不只是字段中的文本。

      > [!TIP]
      > 若要显示所有字段并确保选择 "完成" 字段，请在 "**视图**" 菜单上选择 "**所有字段**"。

   1. 在设计工具的工具栏上，选择 "**输出字段**"。

   若要添加更多输出参数，请为每个参数重复前面的步骤。

1. 添加所有方法的参数后，为每个参数定义以下属性：

   | 属性名称 | 可能值 | 
   |---------------|-----------------|
   | **数据类型** | Byte、Date Time、Decimal、Int、Long、Short、String |
   | **字段填充方法** | 参数支持这些填充类型，并在必要时填充空白： <p><p>- **类型**：按顺序在字段中输入字符。 <p>- **填充**：将字段内容替换为字符，如有必要，用空格填充。 <p>- **EraseEofType**：清除字段，然后在字段中按顺序输入字符。 |
   | **格式字符串** | 某些参数数据类型使用格式字符串，该字符串通知3270连接器如何将文本从屏幕转换为 .NET 数据类型： <p><p>- **datetime**： datetime 格式字符串遵循[.net 自定义日期和时间格式字符串](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings)。 例如，`06/30/2019` 日期使用 `MM/dd/yyyy`格式字符串。 <p>- **decimal**： decimal 格式字符串使用[COBOL Picture 子句](https://www.ibm.com/support/knowledgecenter/SS6SG3_5.2.0/com.ibm.cobol52.ent.doc/PGandLR/ref/rlddepic.html)。 例如，`100.35` 的数字使用 `999V99`格式字符串。 |
   |||

## <a name="save-and-view-metadata"></a>保存并查看元数据

在定义方法后，但在测试方法之前，将已定义的所有信息都保存到 RAP （rap）文件。
在任何模式下，你都可以随时保存到此 RAP 文件。 该设计工具还包括一个示例 RAP 文件，您可以通过浏览到该位置的设计工具的安装文件夹并打开 "Woodgrovebank.jpg" 文件来打开和查看该文件：

`..\Program Files\Microsoft Host Integration Server - 3270 Design Tool\SDK\WoodgroveBank.rap`

但是，如果尝试保存对示例 RAP 文件所做的更改，或在文件保留在设计工具的安装文件夹中时从示例 RAP 文件生成 HIDX 文件，则可能会出现 "拒绝访问" 错误。 默认情况下，设计工具将安装在 "Program Files" 文件夹中，而无需提升权限。 如果遇到错误，请尝试以下解决方案之一：

* 将示例文件复制到其他位置。
* 以管理员身份运行设计工具。
* 使自己成为 SDK 文件夹的所有者。

## <a name="test-your-method"></a>测试方法

1. 若要针对实时宿主运行方法，同时仍处于方法模式，请按 F5 键，或从设计工具的工具栏中，选择 "**运行**"。

   > [!TIP]
   > 随时可以更改模式。 在 "**文件**" 菜单上，选择 "**模式**"，然后选择所需的模式。

1. 输入参数的值，然后选择 **"确定**"。

1. 若要继续进入下一屏幕，请选择 "**下一步**"。

1. 完成后，请选择 "**完成**"，其中显示了输出参数的值。

<a name="add-metadata-integration-account"></a>

## <a name="generate-and-upload-hidx-file"></a>生成和上传 HIDX 文件

准备就绪后，生成 HIDX 文件，以便你可以上传到集成帐户。 3270设计工具在保存 RAP 文件的新子文件夹中创建 HIDX 文件。

1. 在3270设计工具的工具栏上，选择 "**生成代码**"。

1. 前往包含 RAP 文件的文件夹，然后打开该工具在生成 HIDX 文件后创建的子文件夹。 确认该工具创建了 HIDX 文件。

1. 登录到[Azure 门户](https://portal.azure.com)，找到你的集成帐户。

1. [按照添加地图的类似步骤](../logic-apps/logic-apps-enterprise-integration-liquid-transform.md)，将 HIDX 文件作为地图添加到集成帐户，但在选择地图类型时，请选择 " **HIDX**"。

稍后在本主题中，当你首次将 IBM 3270 操作添加到逻辑应用时，系统将提示你通过提供连接信息（例如，你的集成帐户和主机服务器的名称）来创建逻辑应用与主机服务器之间的连接. 创建连接后，可以选择之前添加的 HIDX 文件、要运行的方法以及要使用的参数。

完成所有这些步骤后，可以使用逻辑应用中创建的操作连接到 IBM 大型机，为应用驱动屏幕，输入数据，返回结果，等等。 还可以继续在逻辑应用中添加其他操作，以便与其他应用、服务和系统集成。

<a name="run-action"></a>

## <a name="run-ibm-3270-actions"></a>运行 IBM 3270 操作

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. 登录到 [Azure 门户](https://portal.azure.com)，在逻辑应用设计器中打开逻辑应用（如果尚未打开）。

1. 在要添加操作的最后一个步骤下，选择 "**新建步骤**"，并选择 "**添加操作**"。 

1. 在搜索框中，选择 "**企业**"。 在搜索框中，输入 "3270" 作为筛选器。 从 "操作" 列表中，选择此操作：**通过 TN3270 连接运行大型机程序**

   ![选择3270操作](./media/connectors-create-api-3270/select-3270-action.png)

   若要在步骤之间添加操作，请将鼠标指针移到步骤之间的箭头上。 
   选择出现的加号 ( **+** )，然后选择“添加操作”。

1. 如果尚未建立连接，请提供连接所需的信息，然后选择 "**创建**"。

   | properties | 必选 | 值 | 说明 |
   |----------|----------|-------|-------------|
   | **连接名称** | 是 | <connection-name> | 连接名称 |
   | **集成帐户 ID** | 是 | <integration-account-name> | 集成帐户的名称 |
   | **集成帐户 SAS URL** | 是 | <*集成--SAS-URL*> | 集成帐户的共享访问签名（SAS） URL，可从 Azure 门户中的集成帐户的设置生成。 <p>1. 在集成帐户菜单的 "**设置**" 下，选择 "**回调 URL**"。 <br>2. 在右侧窗格中，复制 "生成的**回调 URL** " 值。 |
   | **Server** | 是 | <*TN3270*> | TN3270 服务的服务器名称 |
   | 端口 | 否 | <*TN3270-端口*> | TN3270 服务器使用的端口。 如果留空，连接器将使用 `23` 作为默认值。 |
   | **设备类型** | 否 | <*IBM 终端模型*> | 要模拟的 IBM 终端的型号名称或编号。 如果留空，连接器将使用默认值。 |
   | **代码页** | 否 | <*代码-页码*> | 宿主的代码页号。 如果留空，连接器将使用 `37` 作为默认值。 |
   | **逻辑单元名称** | 否 | <*逻辑单元名称*> | 要从主机请求的特定逻辑单元名称 |
   | **启用 SSL?** | 否 | 打开或关闭 | 启用或禁用 SSL 加密。 |
   | **验证主机 ssl 证书？** | 否 | 打开或关闭 | 打开或关闭服务器证书验证。 |
   ||||

   例如：

   ![连接属性](./media/connectors-create-api-3270/connection-properties.png)

1. 提供操作所需的信息：

   | properties | 必选 | 值 | 说明 |
   |----------|----------|-------|-------------|
   | **Hidx 名称** | 是 | <*HIDX*> | 选择要使用的 3270 HIDX 文件。 |
   | **方法名称** | 是 | <*方法-name*> | 选择要使用的 HIDX 文件中的方法。 选择方法后，将显示 "**添加新参数**" 列表，以便您可以选择要用于该方法的参数。 |
   ||||

   例如：

   **选择 HIDX 文件**

   ![选择 HIDX 文件](./media/connectors-create-api-3270/select-hidx-file.png)

   **选择方法**

   ![Select 方法](./media/connectors-create-api-3270/select-method.png)

   **选择参数**

   ![选择参数](./media/connectors-create-api-3270/add-parameters.png)

1. 完成后，保存并运行逻辑应用。

   逻辑应用运行完毕后，将显示运行中的步骤。 
   成功的步骤显示复选标记，而不成功的步骤显示字母 "X"。

1. 若要查看每个步骤的输入和输出，请展开该步骤。

1. 若要查看输出，请选择 "**查看原始输出**"。

## <a name="connector-reference"></a>连接器参考

有关此连接器的更多技术详细信息，如连接器的 Swagger 文件所述的触发器、操作和限制，请参阅[连接器的参考页](https://docs.microsoft.com/connectors/si3270/)。

> [!NOTE]
> 对于[integration service 环境（ISE）](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)中的逻辑应用，此连接器的 ise 标记版本会改用[ise 消息限制](../logic-apps/logic-apps-limits-and-config.md#message-size-limits)。

## <a name="next-steps"></a>后续步骤

* 了解其他[逻辑应用连接器](../connectors/apis-list.md)
