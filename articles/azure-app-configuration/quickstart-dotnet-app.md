---
title: 将 Azure 应用配置与 .NET Framework 结合使用的快速入门| Microsoft Docs
description: 将 Azure 应用配置与 .NET Framework 应用结合使用的快速入门
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: quickstart
ms.date: 12/17/2019
ms.author: lcozzens
ms.openlocfilehash: 8190265753bddb3038c5403411c4be193dd8075c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75433624"
---
# <a name="quickstart-create-a-net-framework-app-with-azure-app-configuration"></a>快速入门：使用 Azure 应用配置创建 .NET Framework 应用

在本快速入门中，会将 Azure 应用程序配置合并到基于 .NET Framework 的控制台应用中，以集中存储和管理与代码分离的应用程序设置。

## <a name="prerequisites"></a>必备条件

- Azure 订阅 - [创建免费帐户](https://azure.microsoft.com/free/)
- [Visual Studio 2019](https://visualstudio.microsoft.com/vs)
- [.NET Framework 4.7.2](https://dotnet.microsoft.com/download)

## <a name="create-an-app-configuration-store"></a>创建应用配置存储区

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. 选择“配置资源管理器” > “创建”来添加以下键值对   ：

    | 密钥 | 值 |
    |---|---|
    | TestApp:Settings:Message | Azure 应用配置的数据 |

    暂时将“标签”和“内容类型”保留为空   。

## <a name="create-a-net-console-app"></a>创建 .NET 控制台应用

1. 启动 Visual Studio，并选择“文件” > “新建” > “项目”    。

1. 在**创建新项目**中，针对“控制台”  项目类型进行筛选，然后单击“控制台应用(.NET Framework)”  。 选择“**下一页**”。

1. 在**配置新项目**中，输入项目名称。 在“框架”  下，选择“.NET Framework 4.7.1”  或更高版本。 选择“创建”  。

## <a name="connect-to-an-app-configuration-store"></a>连接到应用程序配置存储区

1. 右键单击项目，然后选择“管理 NuGet 包”  。 在“浏览”选项卡中，搜索以下 NuGet 包并将其添加到项目中  。 如果无法找到，请选中“包括预发行版”复选框  。

    ```
    Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration 1.0.0 preview or later
    Microsoft.Configuration.ConfigurationBuilders.Environment 2.0.0 preview or later
    System.Configuration.ConfigurationManager version 4.6.0 or later
    ```

1. 如下所述，更新项目的 App.config 文件  ：

    ```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>

    <configBuilders>
        <builders>
            <add name="MyConfigStore" mode="Greedy" connectionString="${ConnectionString}" type="Microsoft.Configuration.ConfigurationBuilders.AzureAppConfigurationBuilder, Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration" />
            <add name="Environment" mode="Greedy" type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Environment" />
        </builders>
    </configBuilders>

    <appSettings configBuilders="Environment,MyConfigStore">
        <add key="AppName" value="Console App Demo" />
        <add key="ConnectionString" value ="Set via an environment variable - for example, dev, test, staging, or production connection string." />
    </appSettings>
    ```

   从环境变量 `ConnectionString` 中读取应用程序配置存储区的连接字符串。 在 `appSettings` 部分的 `configBuilders` 属性中，在 `MyConfigStore` 之前添加 `Environment` 配置生成器。

1. 打开 Program.cs 并更新 `Main` 方法，以通过调用 `ConfigurationManager` 来使用应用程序配置  。

    ```csharp
    static void Main(string[] args)
    {
        string message = System.Configuration.ConfigurationManager.AppSettings["TestApp:Settings:Message"];

        Console.WriteLine(message);
    }
    ```

## <a name="build-and-run-the-app-locally"></a>在本地生成并运行应用

1. 将名为 ConnectionString 的环境变量设置为应用程序配置存储区的连接字符串  。 如果使用 Windows 命令提示符，请运行以下命令：

    ```CLI
        setx ConnectionString "connection-string-of-your-app-configuration-store"
    ```

    如果使用 Windows PowerShell，请运行以下命令：

    ```azurepowershell
        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"
    ```
1. 重启 Visual Studio 以便使所做更改生效。 按 Ctrl+F5 生成并运行控制台应用。

## <a name="clean-up-resources"></a>清理资源

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>后续步骤

在本快速入门中，你已经创建了一个新的应用程序配置存储区，并将其用于 .NET Framework 控制台应用。 在应用程序启动后，`ConfigurationManager` 的值 `AppSettings` 不会更改。 应用程序配置 .NET Standard 配置提供程序库，但也可在 .NET Framework 应用中使用。 若要了解如何启用 .NET Framework 应用以动态刷新配置设置，请继续学习下一个教程。

> [!div class="nextstepaction"]
> [启用动态配置](./enable-dynamic-configuration-dotnet.md)
