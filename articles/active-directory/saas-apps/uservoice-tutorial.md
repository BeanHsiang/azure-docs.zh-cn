---
title: 教程：Azure Active Directory 与 UserVoice 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 与 UserVoice 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: dbd7189b1761a9ea88ce32dae3d7b45a88301ff6
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59278116"
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a>教程：Azure Active Directory 与 UserVoice 集成

本教程介绍如何将 UserVoice 与 Azure Active Directory (Azure AD) 集成。
将 UserVoice 与 Azure AD 集成提供以下优势：

* 可以在 Azure AD 中控制谁有权访问 UserVoice
* 可让用户使用其 Azure AD 帐户自动登录到 UserVoice（单一登录）。
* 可在中心位置（即 Azure 门户）管理帐户。

如果要了解有关 SaaS 应用与 Azure AD 集成的更多详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。
如果还没有 Azure 订阅，可以在开始前[创建一个免费帐户](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 UserVoice 的集成，需备齐以下项目：

* 一个 Azure AD 订阅。 如果没有 Azure AD 环境，可以获取一个[免费帐户](https://azure.microsoft.com/free/)
* 已启用 UserVoice 单一登录的订阅

## <a name="scenario-description"></a>方案描述

本教程会在测试环境中配置和测试 Azure AD 单一登录。

* UserVoice 支持 **SP** 发起的 SSO

## <a name="adding-uservoice-from-the-gallery"></a>从库添加 UserVoice

要配置 UserVoice 与 Azure AD 的集成，需要从库中将 UserVoice 添加到托管 SaaS 应用列表。

**若要从库中添加 UserVoice，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。

    ![“Azure Active Directory”按钮](common/select-azuread.png)

2. 转到“企业应用”，并选择“所有应用”选项。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

3. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮](common/add-new-app.png)

4. 在搜索框中键入 **UserVoice**，在结果面板中选择“UserVoice”，并单击“添加”按钮添加该应用程序。

     ![结果列表中的 UserVoice](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分，我们基于名为 **Britta Simon** 的测试用户来配置并测试 UserVoice 的 Azure AD 单一登录。
若要正常使用单一登录，需要在 Azure AD 用户与 UserVoice 相关用户之间建立链接关系。

若要配置和测试 UserVoice 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
2. **[配置 UserVoice 单一登录](#configure-uservoice-single-sign-on)** - 在应用程序端配置单一登录设置。
3. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
4. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
5. **[创建 UserVoice 测试用户](#create-uservoice-test-user)** - 在 UserVoice 中创建 Britta Simon 的对应用户，并将其关联到其在 Azure AD 中的表示形式。
6. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录。

若要配置 UserVoice 的 Azure AD 单一登录，请执行以下步骤：

1. 在 [Azure 门户](https://portal.azure.com/)中的“UserVoice”应用程序集成页上，选择“单一登录”。

    ![配置单一登录链接](common/select-sso.png)

2. 在**选择单一登录方法**对话框中，选择 **SAML/WS-Fed**模式以启用单一登录。

    ![单一登录选择模式](common/select-saml-option.png)

3. 在“使用 SAML 设置单一登录”页上，单击“编辑”图标以打开“基本 SAML 配置”对话框。

    ![编辑基本 SAML 配置](common/edit-urls.png)

4. 在“基本 SAML 配置”部分中，按照以下步骤操作：

    ![UserVoice 域和 URL 单一登录信息](common/sp-identifier.png)

    a. 在“登录 URL”文本框中，使用以下模式键入 URL：`https://<tenantname>.UserVoice.com`

    b. 在“标识符(实体 ID)”文本框中，使用以下模式键入 URL：`https://<tenantname>.UserVoice.com`

    > [!NOTE]
    > 这些不是实际值。 使用实际登录 URL 和标识符更新这些值。 请联系 [UserVoice 客户端支持团队](https://www.uservoice.com/)获取这些值。 还可以参考 Azure 门户中的“基本 SAML 配置”部分中显示的模式。

5. 在“SAML 签名证书”部分中，单击“编辑”按钮以打开“SAML 签名证书”对话框。

    ![编辑 SAML 签名证书](common/edit-certificate.png)

6. 在“SAML 签名证书”部分中，复制**指纹**并将其保存在计算机上。

    ![复制指纹值](common/copy-thumbprint.png)

7. 在“设置 UserVoice”部分，根据要求复制相应的 URL。

    ![复制配置 URL](common/copy-configuration-urls.png)

    a. 登录 URL

    b. Azure AD 标识符

    c. 注销 URL

### <a name="configure-uservoice-single-sign-on"></a>配置 UserVoice 单一登录

1. 在另一个 Web 浏览器窗口中，以管理员身份登录到 UserVoice 公司站点。

2. 在顶部工具栏中，单击“设置”，并从菜单中选择“Web 门户”。
   
    ![应用端上的“设置”部分](./media/uservoice-tutorial/ic777519.png "设置")

3. 在“Web 门户”选项卡的“用户身份验证”部分中，单击“编辑”以打开“编辑用户身份验证”对话框页
   
    ![Web 门户选项卡](./media/uservoice-tutorial/ic777520.png "Web 门户")

4. 在“编辑用户身份验证”对话框页上，执行以下步骤：
   
    ![编辑用户身份验证](./media/uservoice-tutorial/ic777521.png "编辑用户身份验证")
   
    a. 单击“单一登录(SSO)”。
 
    b. 将从 Azure 门户复制的“登录 URL”值粘贴到“SSO 远程登录”文本框中。

    c. 将从 Azure 门户复制的“注销 URL”值粘贴到“SSO 远程注销”文本框中。
 
    d. 在“当前证书 SHA1 指纹”文本框中，粘贴从 Azure 门户复制的“指纹”值。
    
    e. 单击“保存身份验证设置”。

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户 

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”。

    ![“用户和组”以及“所有用户”链接](common/users.png)

2. 选择屏幕顶部的“新建用户”。

    ![“新建用户”按钮](common/new-user.png)

3. 在“用户属性”中，按照以下步骤操作。

    ![“用户”对话框](common/user-properties.png)

    a. 在“名称”字段中，输入 BrittaSimon。
  
    b. 在“用户名”字段中键入 brittasimon@yourcompanydomain.extension。 例如： BrittaSimon@contoso.com

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 UserVoice 的权限，允许她使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”、“UserVoice”。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

2. 在应用程序列表中，选择“UserVoice”。

    ![应用程序列表中的 UserVoice 链接](common/all-applications.png)

3. 在左侧菜单中，选择“用户和组”。

    ![“用户和组”链接](common/users-groups-blade.png)

4. 单击“添加用户”按钮，然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格](common/add-assign-user.png)

5. 在“用户和组”对话框中，选择“用户”列表中的 Britta Simon，然后单击屏幕底部的“选择”按钮。

6. 如果你在 SAML 断言中需要任何角色值，请在“选择角色”对话框中从列表中为用户选择合适的角色，然后单击屏幕底部的“选择”按钮。

7. 在“添加分配”对话框中，单击“分配”按钮。

### <a name="create-uservoice-test-user"></a>创建 UserVoice 测试用户

为了使 Azure AD 用户能够登录到 UserVoice，必须将其预配到 UserVoice 中。 对于 UserVoice，需要手动执行预配。

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>若要预配用户帐户，请执行以下步骤：

1. 登录到 **UserVoice** 租户。

2. 转到“设置”。
   
    ![设置](./media/uservoice-tutorial/ic777811.png "设置")

3. 单击“常规”。

4. 单击“代理和权限”。
   
    ![代理和权限](./media/uservoice-tutorial/ic777812.png "代理和权限")

5. 单击“添加管理员”。
   
    ![添加管理员](./media/uservoice-tutorial/ic777813.png "添加管理员")

6. 在“邀请管理员”对话框中，执行以下步骤：
   
    ![邀请管理员](./media/uservoice-tutorial/ic777814.png "邀请管理员")
   
    a. 在“电子邮件”文本框中，键入要预配帐户的电子邮件地址，并单击“添加”。
   
    b. 单击“邀请”。

> [!NOTE]
> 可以使用 UserVoice 提供的任何其他 UserVoice 用户帐户创建工具或 API 来预配 AAD 用户帐户。

### <a name="test-single-sign-on"></a>测试单一登录 

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

在访问面板中单击“UserVoice”磁贴时，应会自动登录到设置了 SSO 的 UserVoice。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

- [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory 的应用程序访问与单一登录是什么？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什么是 Azure Active Directory 中的条件访问？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

