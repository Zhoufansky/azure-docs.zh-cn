---
title: 连接下游设备 - Azure IoT Edge | Microsoft Docs
description: 如何配置下游或叶设备以连接到 Azure IoT Edge 网关设备。
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/08/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 6ddda38d887cdfe30b449847e2f625ba17f33898
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2020
ms.locfileid: "76510798"
---
# <a name="connect-a-downstream-device-to-an-azure-iot-edge-gateway"></a>将下游设备连接到 Azure IoT Edge 网关

本文介绍如何在下游设备与 IoT Edge 透明网关之间建立受信任的连接。 在透明网关方案中，一个或多个设备可以通过与 IoT 中心连接的单个网关设备传递其消息。 下游设备可以是包含通过 [Azure IoT 中心](https://docs.microsoft.com/azure/iot-hub)云服务创建的标识的任何应用程序或平台。 在许多情况下，这些应用程序使用 [Azure IoT 设备 SDK](../iot-hub/iot-hub-devguide-sdks.md)。 下游设备甚至可以是在 IoT Edge 网关设备上运行的应用程序。

设置成功透明的网关连接有三个常规步骤。 本文介绍第三个步骤：

1. 网关设备需要安全连接到下游设备，接收来自下游设备的通信，并将消息路由到适当的目标。 有关详细信息，请参阅[将 IoT Edge 设备配置为用作透明网关](how-to-create-transparent-gateway.md)。
2. 下游设备需要设备标识才能通过 IoT 中心进行身份验证，并且知道要通过其网关设备进行通信。 有关详细信息，请参阅[对 Azure IoT 中心的下游设备进行身份验证](how-to-authenticate-downstream-device.md)。
3. **下游设备需要安全连接到其网关设备。**

本文列出了下游设备的常见连接问题，并引导你设置下游设备。具体内容包括：

* 介绍传输层安全性 (TLS) 和证书基础知识。
* 介绍 TLS 库在不同操作系统中的工作原理，以及每个操作系统如何处理证书。
* 演练不同语言的 Azure IoT 示例以帮助你入门。

在本文中，术语“网关”和“IoT Edge 网关”是指配置为透明网关的 IoT Edge 设备。

## <a name="prerequisites"></a>必备组件

* 将配置 IoT Edge 设备中生成的**azure-iot-test-only**证书文件配置为可用作下游设备上可用[的透明网关](how-to-create-transparent-gateway.md)。 下游设备使用此证书来验证网关设备的标识。
* 具有指向网关设备的已修改连接字符串，如[对 Azure IoT 中心的下游设备进行身份验证](how-to-authenticate-downstream-device.md)中所述。

## <a name="prepare-a-downstream-device"></a>准备下游设备

下游设备可以是包含通过 [Azure IoT 中心](https://docs.microsoft.com/azure/iot-hub)云服务创建的标识的任何应用程序或平台。 在许多情况下，这些应用程序使用 [Azure IoT 设备 SDK](../iot-hub/iot-hub-devguide-sdks.md)。 下游设备甚至可以是在 IoT Edge 网关设备上运行的应用程序。 但是，另一个 IoT Edge 设备不能是 IoT Edge 网关的下游。

>[!NOTE]
>具有在 IoT 中心内注册的标识的 IoT 设备可以使用[module 孪生](../iot-hub/iot-hub-devguide-module-twins.md)在单个设备上隔离不同的进程、硬件或功能。 IoT Edge 的网关支持使用对称密钥身份验证的下游模块连接，但不支持 x.509 证书身份验证。

若要将下游设备连接到 IoT Edge 网关，需要准备好以下两项：

* 配置了 IoT 中心设备连接字符串的设备或应用程序，该字符串中追加了用于将该设备或应用程序连接到网关的信息。

    [向 Azure IoT 中心验证下游设备的身份](how-to-authenticate-downstream-device.md)中介绍了此步骤。

* 设备或应用程序必须信任网关的**根 CA**证书，才能验证连接到网关设备的 TLS 连接。

    本文的其余部分详细介绍了此步骤。 可以通过以下两种方式之一执行此步骤：通过在操作系统的证书存储中安装 CA 证书，或通过在应用程序中使用 Azure IoT Sdk 引用证书来引用（适用于某些语言）。

## <a name="tls-and-certificate-fundamentals"></a>TLS 和证书基础知识

将下游设备安全连接到 IoT Edge 所存在的难题就如同通过 Internet 进行其他任何客户端/服务器安全通信。 客户端和服务器使用[传输层安全性 (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) 通过 Internet 安全通信。 TLS 是使用称作“证书”的标准[公钥基础结构 (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure) 构造生成的。 TLS 是一种相当简单的规范，它解决了与确保两个端点相关的各种主题。 本部分概述了与你安全地将设备连接到 IoT Edge 网关相关的概念。

当客户端连接到某个服务器时，该服务器将出示称作“服务器证书链”的证书链。 证书链通常包含根证书颁发机构 (CA) 证书、一个或多个中间 CA 证书，以及服务器证书本身。 客户端通过以加密方式验证整个服务器证书链来与服务器建立信任。 服务器证书链的此客户端验证称为*服务器链验证*。 客户端对服务进行加密后，就会在一种称为 "*所有权证明*" 的进程中证明与服务器证书相关联的私钥。 服务器链验证和所有权证明的组合称为*服务器身份验证*。 若要验证服务器证书链，客户端需要使用创建（或发出）服务器证书时所用的根 CA 证书的副本。 一般情况下，在连接到网站时，浏览器中会预配置常用的 CA 证书，使客户端能够顺利完成验证过程。

当某个设备连接到 Azure IoT 中心时，该设备为客户端，IoT 中心云服务为服务器。 IoT 中心云服务由公开且广泛使用的名为“Baltimore CyberTrust 根”的根 CA 证书提供支持。 由于大多数设备上已安装 IoT 中心 CA 证书，因此，许多 TLS 实现（OpenSSL、Schannel、LibreSSL）在服务器证书验证期间会自动使用该证书。 可成功连接到 IoT 中心的设备在尝试连接到 IoT Edge 网关时可能会出现问题。

当某个设备连接到 IoT Edge 网关时，下游设备为客户端，网关设备为服务器。 Azure IoT Edge 允许操作员（或用户）在适当的情况下生成网关证书链。 操作员可以选择使用公共 CA 证书（例如 Baltimore），或使用自签名的（或内部）根 CA 证书。 公共 CA 证书往往会产生相关的费用，因此通常在生产方案中使用。 最好是使用自签名的 CA 证书进行开发和测试。 简介中列出的透明网关设置项目使用自签名根 CA 证书。

对 IoT Edge 网关使用自签名的根 CA 证书时，需要在尝试连接到该网关的所有下游设备上安装或提供该证书。

![网关证书设置](./media/how-to-create-transparent-gateway/gateway-setup.png)

若要详细了解 IoT Edge 证书和对生产造成的某些影响，请参阅 [IoT Edge 证书用法详细信息](iot-edge-certs.md)。

## <a name="provide-the-root-ca-certificate"></a>提供根 CA 证书

若要验证网关设备的证书，下游设备需要其自己的根 CA 证书副本。 如果使用 IoT Edge git 存储库中提供的脚本来创建测试证书，则根 CA 证书称为**azure-iot-test-only**。 如果尚未执行其他下游设备准备步骤的一部分，请将此证书文件移动到下游设备上的任何目录中。 你可以使用诸如[Azure Key Vault](https://docs.microsoft.com/azure/key-vault)之类的服务或类似于[安全复制协议](https://www.ssh.com/ssh/scp/)的函数来移动证书文件。

## <a name="install-certificates-in-the-os"></a>在 OS 中安装证书

在操作系统的证书存储中安装根 CA 证书通常允许大多数应用程序使用根 CA 证书。 有些例外，如 NodeJS 应用程序不使用 OS 证书存储，而是使用节点运行时的内部证书存储。 如果无法在操作系统级别安装证书，请直接跳到[将证书用于 Azure IoT sdk](#use-certificates-with-azure-iot-sdks)。

### <a name="ubuntu"></a>Ubuntu

以下示例命令演示如何在 Ubuntu 主机上安装 CA 证书。 此示例假设你使用的是必备项文章中的**azure-iot-test-only**证书，并已将证书复制到下游设备上的某个位置。

```bash
sudo cp <path>/azure-iot-test-only.root.ca.cert.pem /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
sudo update-ca-certificates
```

应该会看到一条消息，指出 "在/etc/ssl/certs... 中更新证书。已添加1个，0已删除;完成。 "

### <a name="windows"></a>Windows

以下示例步骤演示如何在 Windows 主机上安装 CA 证书。 此示例假设你使用的是必备项文章中的**azure-iot-test-only**证书，并已将证书复制到下游设备上的某个位置。

你可以使用 PowerShell 的[导入证书](https://docs.microsoft.com/powershell/module/pkiclient/import-certificate?view=win10-ps)作为管理员安装证书：

```powershell
import-certificate  <file path>\azure-iot-test-only.root.ca.cert.pem -certstorelocation cert:\LocalMachine\root
```

还可以使用**certlm.msc**实用工具安装证书：

1. 在“开始”菜单中，搜索并选择“管理计算机证书”。 此时会打开一个名为 **certlm** 的实用工具。
2. 导航到“证书 - 本地计算机” > “受信任的根证书颁发机构”。
3. 右键单击“证书”，并选择“所有任务” > “导入”。 此时应会启动证书导入向导。
4. 按指导执行步骤，导入证书文件 `<path>/azure-iot-test-only.root.ca.cert.pem`。 完成后，应看到“已成功导入”消息。

还可以按本文稍后的 .NET 示例中所示，使用 .NET API 以编程方式安装证书。

通常，应用程序使用 Windows 提供的名为 [Schannel](https://docs.microsoft.com/windows/desktop/com/schannel) 的 TLS 堆栈来通过 TLS 进行安全连接。 在尝试建立 TLS 连接之前，Schannel 要求所有证书已安装在 Windows 证书存储中。

## <a name="use-certificates-with-azure-iot-sdks"></a>配合 Azure IoT SDK 使用证书

本部分介绍 Azure IoT SDK 如何使用简单的示例应用程序连接到 IoT Edge 设备。 所有示例的目标是连接设备客户端并将遥测消息发送到网关，然后关闭连接并退出。

在使用应用程序级示例之前，请做好两项准备：

* 你的下游设备的 IoT 中心连接字符串已修改为指向网关设备，以及对你的下游设备进行身份验证到 IoT 中心所需的任何证书。 有关详细信息，请参阅[对 Azure IoT 中心的下游设备进行身份验证](how-to-authenticate-downstream-device.md)。

* 已复制并保存在下游设备上某个位置的根 CA 证书的完整路径。

    例如，`<path>/azure-iot-test-only.root.ca.cert.pem` 。

### <a name="nodejs"></a>NodeJS

本部分提供用于将 Azure IoT NodeJS 设备客户端连接到 IoT Edge 网关的示例应用程序。 对于 NodeJS 应用程序，必须在应用程序级别安装根 CA 证书，如下所示。 NodeJS 应用程序不使用系统的证书存储区。

1. 从[适用于 Node.js 的 Azure IoT 设备 SDK 示例存储库](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples)获取 **edge_downstream_device.js** 的示例。
2. 查看 **readme.md** 文件，确保满足运行该示例的所有先决条件。
3. 在 edge_downstream_device.js 文件中，更新 **connectionString** 和 **edge_ca_cert_path** 变量。
4. 参阅 SDK 文档，获取有关如何在设备上运行该示例的说明。

若要了解所运行的示例，请参阅以下代码片段，其中演示了客户端 SDK 如何读取证书文件，并使用它来建立安全的 TLS 连接：

```javascript
// Provide the Azure IoT device client via setOptions with the X509
// Edge root CA certificate that was used to setup the Edge runtime
var options = {
    ca : fs.readFileSync(edge_ca_cert_path, 'utf-8'),
};
```

### <a name="net"></a>.NET

本部分介绍用于将 Azure IoT .NET 设备客户端连接到 IoT Edge 网关的示例应用程序。 但是，.NET 应用程序可自动使用 Linux 和 Windows 主机上的系统证书存储中安装的任何证书。

1. 从 [IoT Edge .NET 示例文件夹](https://github.com/Azure/iotedge/tree/master/samples/dotnet/EdgeDownstreamDevice)获取 **EdgeDownstreamDevice** 的示例。
2. 查看 **readme.md** 文件，确保满足运行该示例的所有先决条件。
3. 在 **Properties / launchSettings.json** 文件中，更新 **DEVICE_CONNECTION_STRING** 和 **CA_CERTIFICATE_PATH** 变量。 若要使用主机系统上受信任证书存储中安装的证书，请将此变量留空。
4. 参阅 SDK 文档，获取有关如何在设备上运行该示例的说明。

若要通过 .NET 应用程序以编程方式在证书存储中安装受信任的证书，请参阅 **EdgeDownstreamDevice / Program.cs** 文件中的 **InstallCACert()** 函数。 此操作是幂等的，因此可以使用相同的值运行多次，而不会造成其他影响。

### <a name="c"></a>C

本部分介绍用于将 Azure IoT C 设备客户端连接到 IoT Edge 网关的示例应用程序。 C SDK 可以配合许多 TLS 库（包括 OpenSSL、WolfSSL 和 Schannel）运行。 有关详细信息，请参阅 [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c)。

1. 从[适用于 C 的 Azure IoT 设备 SDK 示例](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples)获取 **iotedge_downstream_device_sample** 应用程序。
2. 查看 **readme.md** 文件，确保满足运行该示例的所有先决条件。
3. 在 iotedge_downstream_device_sample.c 文件中，更新 **connectionString** 和 **edge_ca_cert_path** 变量。
4. 参阅 SDK 文档，获取有关如何在设备上运行该示例的说明。

适用于 C 的 Azure IoT 设备 SDK 提供一个用于在设置客户端时注册 CA 证书的选项。 此操作不会在任何位置安装证书，而是使用内存中证书的字符串格式。 建立连接时，将向底层 TLS 堆栈提供已保存的证书。

```C
(void)IoTHubDeviceClient_SetOption(device_handle, OPTION_TRUSTED_CERT, cert_string);
```

在 Windows 主机上，如果你不使用 OpenSSL 或其他 TLS 库，则 SDK 默认使用 Schannel。 要使 Schannel 正常工作，应在 Windows 证书存储中安装 IoT Edge 根 CA 证书，而不要使用 `IoTHubDeviceClient_SetOption` 操作进行设置。

### <a name="java"></a>Java

本部分介绍用于将 Azure IoT Java 设备客户端连接到 IoT Edge 网关的示例应用程序。

1. 从[适用于 Java 的 Azure IoT 设备 SDK 示例](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples)获取 **Send-event** 的示例。
2. 查看 **readme.md** 文件，确保满足运行该示例的所有先决条件。
3. 参阅 SDK 文档，获取有关如何在设备上运行该示例的说明。

### <a name="python"></a>Python

本部分介绍用于将 Azure IoT Python 设备客户端连接到 IoT Edge 网关的示例应用程序。

1. 从[适用于 Python 的 Azure IoT 设备 SDK](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/advanced-edge-scenarios)示例获取**send_message**的示例。
2. 确保你正在 IoT Edge 容器中运行，或者在调试方案中，设置了 `EdgeHubConnectionString` 和 `EdgeModuleCACertificateFile` 环境变量。
3. 参阅 SDK 文档，获取有关如何在设备上运行该示例的说明。

## <a name="test-the-gateway-connection"></a>测试网关连接

使用此示例命令测试下游设备是否可以连接到网关设备：

```cmd/sh
openssl s_client -connect mygateway.contoso.com:8883 -CAfile <CERTDIR>/certs/azure-iot-test-only.root.ca.cert.pem -showcerts
```

此命令测试 MQTTS 上的连接（端口8883）。 如果使用其他协议，请根据需要为 AMQPS （5671）或 HTTPS （433）调整该命令

此命令的输出可能很长，其中包括有关链中所有证书的信息。 如果连接成功，则会看到一条类似于 `Verification: OK` 或 `Verify return code: 0 (ok)`的行。

![验证网关连接](./media/how-to-connect-downstream-device/verification-ok.png)

## <a name="troubleshoot-the-gateway-connection"></a>排查网关连接问题

如果你的叶设备与它的网关设备有间歇性的连接，请尝试执行以下步骤来解决问题。

1. 连接字符串中的网关主机名与网关设备上 IoT Edge yaml 文件中的主机名值相同吗？
2. 网关主机名是否可解析为 IP 地址？ 可以通过使用 DNS 或在叶设备上添加主机文件条目来解决间歇连接。
3. 通信端口是否在防火墙中打开？ 基于所使用的协议的通信（MQTTS： 8883/AMQPS： 5671/HTTPS：433）在下游设备和透明 IoT Edge 之间必须是可能的。

## <a name="next-steps"></a>后续步骤

了解 IoT Edge 如何将[脱机功能](offline-capabilities.md)扩展到下游设备。
