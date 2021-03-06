---
title: 教程：Azure Active Directory 单一登录 (SSO) 与 etouches 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 etouches 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 76cccaa8-859c-4c16-9d1d-8a6496fc7520
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e0774873370d6006e0a1108c4b01c60274d69f0d
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/31/2019
ms.locfileid: "75561179"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-etouches"></a>教程：Azure Active Directory 单一登录 (SSO) 与 etouches 集成

本教程介绍如何将 etouches 与 Azure Active Directory (Azure AD) 进行集成。 将 etouches 与 Azure AD 集成后，可以：

* 在 Azure AD 中控制谁有权访问 etouches。
* 让用户使用其 Azure AD 帐户自动登录到 etouches。
* 在一个中心位置（Azure 门户）管理帐户。

若要了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。

## <a name="prerequisites"></a>必备条件

若要开始操作，需备齐以下项目：

* 一个 Azure AD 订阅。 如果没有订阅，可以获取一个[免费帐户](https://azure.microsoft.com/free/)。
* 已启用单一登录 (SSO) 的 etouches 订阅。

## <a name="scenario-description"></a>方案描述

本教程在测试环境中配置并测试 Azure AD SSO。

* etouches 支持 SP 发起的 SSO 





## <a name="adding-etouches-from-the-gallery"></a>从库中添加 etouches

若要配置 etouches 与 Azure AD 的集成，需要从库中将 etouches 添加到托管 SaaS 应用列表。

1. 使用工作或学校帐户或个人 Microsoft 帐户登录到 [Azure 门户](https://portal.azure.com)。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务  。
1. 导航到“企业应用程序”，选择“所有应用程序”   。
1. 若要添加新的应用程序，请选择“新建应用程序”  。
1. 在“从库中添加”部分的搜索框中，键入“etouches”   。
1. 从结果面板中选择“etouches”，然后添加该应用  。 在该应用添加到租户时等待几秒钟。


## <a name="configure-and-test-azure-ad-single-sign-on-for-etouches"></a>配置和测试 etouches 的 Azure AD 单一登录

使用名为 B.Simon 的测试用户配置和测试 etouches 的 Azure AD SSO  。 若要正常使用 SSO，需要在 Azure AD 用户与 etouches 相关用户之间建立链接关系。

若要配置和测试 etouches 的 Azure AD SSO，请完成以下构建基块：

1. **[配置 Azure AD SSO](#configure-azure-ad-sso)** - 使用户能够使用此功能。
    1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 B. Simon 测试 Azure AD 单一登录。
    1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 B. Simon 能够使用 Azure AD 单一登录。
1. **[配置 etouches SSO](#configure-etouches-sso)** - 在应用程序端配置单一登录设置。
    1. **[创建 etouches 测试用户](#create-etouches-test-user)** - 在 etouches 中创建 B.Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[测试 SSO](#test-sso)** - 验证配置是否正常工作。

## <a name="configure-azure-ad-sso"></a>配置 Azure AD SSO

按照下列步骤在 Azure 门户中启用 Azure AD SSO。

1. 在 [Azure 门户](https://portal.azure.com/)的“etouches”应用程序集成页上，找到“管理”部分，选择“单一登录”    。
1. 在“选择单一登录方法”页上选择“SAML”   。
1. 在“使用 SAML 设置单一登录”页上，单击“基本 SAML 配置”的编辑/笔形图标以编辑设置   。

   ![编辑基本 SAML 配置](common/edit-urls.png)

1. 在“基本 SAML 配置”部分，输入以下字段的值  ：

    a. 在“登录 URL”文本框中，使用以下模式键入 URL：`https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>` 

    b. 在“标识符(实体 ID)”文本框中，使用以下模式键入 URL：`https://www.eiseverywhere.com/<instance name>` 

    > [!NOTE] 
    > 这些不是实际值。 本教程稍后将介绍如何使用实际的登录 URL 和标识符来更新该值。

1. etouches 应用程序需要特定格式的 SAML 断言，这要求向 SAML 令牌属性配置添加自定义属性映射。 以下屏幕截图显示了默认属性的列表。

    ![image](common/edit-attribute.png)

1. 除了上述属性，etouches 应用程序还要求在 SAML 响应中传递回更多的属性，如下所示。 这些属性也是预先填充的，但可以根据要求查看它们。

    | 名称 | 源属性|
    | ------------------- | -------------------- |
    | 电子邮件 | user.mail | 

1. 在“使用 SAML 设置单一登录”页的“SAML 签名证书”部分中找到“联合元数据 XML”，选择“下载”以下载该证书并将其保存在计算机上     。

    ![证书下载链接](common/metadataxml.png)

1. 在“设置 etouches”部分中，根据要求复制相应的 URL  。

    ![复制配置 URL](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

在本部分，我们将在 Azure 门户中创建名为 B.Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”    。
1. 选择屏幕顶部的“新建用户”  。
1. 在“用户”属性中执行以下步骤  ：
   1. 在“名称”  字段中，输入 `B.Simon`。  
   1. 在“用户名”字段中输入 username@companydomain.extension  。 例如，`B.Simon@contoso.com` 。
   1. 选中“显示密码”复选框，然后记下“密码”框中显示的值。  
   1. 单击“创建”。 

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，将通过授予 B.Simon 访问 etouches 的权限，允许其使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”。  
1. 在应用程序列表中，选择“etouches”。 
1. 在应用的概述页中，找到“管理”部分，选择“用户和组”   。

   ![“用户和组”链接](common/users-groups-blade.png)

1. 选择“添加用户”，然后在“添加分配”对话框中选择“用户和组”。   

    ![“添加用户”链接](common/add-assign-user.png)

1. 在“用户和组”对话框中，从“用户”列表中选择“B.Simon”，然后单击屏幕底部的“选择”按钮。   
1. 如果在 SAML 断言中需要任何角色值，请在“选择角色”对话框的列表中为用户选择合适的角色，然后单击屏幕底部的“选择”按钮。  
1. 在“添加分配”对话框中，单击“分配”按钮。  

## <a name="configure-etouches-sso"></a>配置 etouches SSO

1. 若要为应用程序配置 SSO，请在 etouches 应用程序中执行以下步骤： 

    ![etouches 配置](./media/etouches-tutorial/tutorial_etouches_06.png) 

    a. 使用管理员权限登录到 **etouches** 应用程序。
   
    b. 转到 **SAML** 配置。

    c. 在“常规设置”部分中，在记事本中打开你从 Azure 门户下载的证书，复制内容，并将其粘贴到 IDP 元数据文本框中。  

    d. 单击“保存并停留”按钮  。
  
    e. 在“SAML 元数据”部分中单击“更新元数据”按钮。  

    f. 这将打开页面并执行 SSO。 在 SSO 开始运行后，便可设置用户名。

    g. 在“用户名”字段中，选择**电子邮件地址**，如下图所示。 

    h. 复制 **SP 实体 ID** 值并将其粘贴到“标识符”文本框中，这是 Azure 门户中“基本的 SAML 配置”部分的值。  

    i. 复制 **SSO URL / ACS** 值并将其粘贴到“登录 URL”文本框中，这是 Azure 门户中“基本的 SAML 配置”部分的值。  

### <a name="create-etouches-test-user"></a>创建 etouches 测试用户

在本部分中，将在 etouches 中创建一个名为 Britta Simon 的用户。 请与 [etouches 客户端支持团队](https://www.etouches.com/event-software/support/customer-support/)协作来在 eTouches 平台中添加用户。

## <a name="test-sso"></a>测试 SSO 

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的 etouches 磁贴时，应当会自动登录到你为其设置了 SSO 的 etouches。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

- [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什么是使用 Azure Active Directory 的应用程序访问和单一登录？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什么是 Azure Active Directory 中的条件访问？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [通过 Azure AD 试用 etouches](https://aad.portal.azure.com/)

