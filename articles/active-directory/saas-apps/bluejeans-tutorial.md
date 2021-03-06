---
title: 教程：Azure Active Directory 与 BlueJeans 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 BlueJeans 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/31/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d1c10d9df85b04cf7b8ea7e0020bc8e802a0d5d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59698922"
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>教程：Azure Active Directory 与 BlueJeans 集成

在本教程中，了解如何将 BlueJeans 与 Azure Active Directory (Azure AD) 进行集成。
将 BlueJeans 与 Azure AD 集成可提供以下优势：

* 可以在 Azure AD 中控制谁有权访问 BlueJeans。
* 可以让用户使用其 Azure AD 帐户自动登录到 BlueJeans（单一登录）。
* 可在中心位置（即 Azure 门户）管理帐户。

如果要了解有关 SaaS 应用与 Azure AD 集成的更多详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。
如果还没有 Azure 订阅，可以在开始前[创建一个免费帐户](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 BlueJeans 的集成，需要以下项：

* 一个 Azure AD 订阅。 如果没有 Azure AD 环境，可以获取一个[免费帐户](https://azure.microsoft.com/free/)
* 已启用 BlueJeans 单一登录的订阅

## <a name="scenario-description"></a>方案描述

本教程会在测试环境中配置和测试 Azure AD 单一登录。

* BlueJeans 支持 SP 发起的 SSO

* BlueJeans 支持[自动用户预配](bluejeans-provisioning-tutorial.md)

## <a name="adding-bluejeans-from-the-gallery"></a>从库中添加 BlueJeans

若要配置 BlueJeans 与 Azure AD 的集成，需要从库中将 BlueJeans 添加到托管 SaaS 应用列表。

若要从库中添加 BlueJeans，请执行以下步骤：

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。

    ![“Azure Active Directory”按钮](common/select-azuread.png)

2. 转到“企业应用”，并选择“所有应用”选项。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

3. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮](common/add-new-app.png)

4. 在搜索框中，键入“BlueJeans”，在结果面板中选择“BlueJeans”，然后单击“添加”按钮添加该应用程序。

     ![结果列表中的 BlueJeans](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，将基于名为“Britta Simon”的测试用户配置和测试 BlueJeans 的 Azure AD 单一登录。
若要运行单一登录，需要在 Azure AD 用户与 BlueJeans 相关用户之间建立链接关系。

若要配置并测试 BlueJeans 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
2. **[配置 BlueJeans 单一登录](#configure-bluejeans-single-sign-on)** - 在应用程序端配置单一登录。
3. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
4. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
5. **[创建 BlueJeans 测试用户](#create-bluejeans-test-user)** - 在 BlueJeans 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
6. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录。

若要配置 BlueJeans 的 Azure AD 单一登录，请执行以下步骤：

1. 在 Azure 门户中的“BlueJeans”应用程序集成页上，选择“单一登录” [Azure portal](https://portal.azure.com/) 。

    ![配置单一登录链接](common/select-sso.png)

2. 在**选择单一登录方法**对话框中，选择 **SAML/WS-Fed**模式以启用单一登录。

    ![单一登录选择模式](common/select-saml-option.png)

3. 在“使用 SAML 设置单一登录”页上，单击“编辑”图标以打开“基本 SAML 配置”对话框。

    ![编辑基本 SAML 配置](common/edit-urls.png)

4. 在“基本 SAML 配置”部分中，按照以下步骤操作：

    ![BlueJeans 域和 URL 单一登录信息](common/sp-identifier.png)

    a. 在“登录 URL”文本框中，使用以下模式键入 URL：`https://<companyname>.BlueJeans.com`。

    b. 在“标识符”文本框中键入 URL：`http://samlsp.bluejeans.com`

    > [!NOTE]
    > 登录 URL 值不是实际值。 请使用实际登录 URL 更新此值。 若要获取此值，请与 [BlueJeans 客户端支持团队](https://support.bluejeans.com/contact)联系。 还可以参考 Azure 门户中的“基本 SAML 配置”部分中显示的模式。

5. 在“使用 SAML 设置单一登录”页上，在“SAML 签名证书”部分中，单击“下载”以根据要求从给定的选项下载**证书(Base64)** 并将其保存在计算机上。

    ![证书下载链接](common/certificatebase64.png)

6. 在“设置 BlueJeans”部分中，根据要求复制相应的 URL。

    ![复制配置 URL](common/copy-configuration-urls.png)

    a. 登录 URL

    b. Azure AD 标识符

    c. 注销 URL

### <a name="configure-bluejeans-single-sign-on"></a>配置 BlueJeans 单一登录

1. 在其他 Web 浏览器窗口中，以管理员身份登录 BlueJeans 公司站点。

2. 依次转到“管理”\>“组设置”\>“安全”。

    ![管理员](./media/bluejeans-tutorial/ic785868.png "管理员")

3. 在“安全”部分中，执行以下步骤：

    ![SAML 单一登录](./media/bluejeans-tutorial/ic785869.png "SAML 单一登录")

    a. 选择“SAML 单一登录”。

    b. 选择“启用自动预配”。

4. 使用以下步骤继续：

    ![证书路径](./media/bluejeans-tutorial/ic785870.png "证书路径")

    a. 单击“选择文件”，上传从 Azure 门户下载的 base-64 编码的证书。

    b. 在“登录 URL”文本框中，粘贴从 Azure 门户复制的“登录 URL”值。

    c. 在“密码更改 URL”文本框中，粘贴从 Azure 门户复制的“更改密码 URL”值。

    d. 在“注销 URL”文本框中，粘贴从 Azure 门户复制的“注销 URL”值。

5. 使用以下步骤继续：

    ![保存更改](./media/bluejeans-tutorial/ic785874.png "保存更改")

    a. 在“用户 ID”文本框中，键入 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`。

    b. 在“电子邮件”文本框中，键入 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`。

    c. 单击“保存更改”。

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”。

    ![“用户和组”以及“所有用户”链接](common/users.png)

2. 选择屏幕顶部的“新建用户”。

    ![“新建用户”按钮](common/new-user.png)

3. 在“用户属性”中，按照以下步骤操作。

    ![“用户”对话框](common/user-properties.png)

    a. 在“名称”字段中，输入 BrittaSimon。
  
    b. 在“用户名”字段中键入 `brittasimon\@yourcompanydomain.extension`。 例如，BrittaSimon@contoso.com。

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 BlueJeans 的权限，以支持其使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”和“BlueJeans”。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

2. 在应用程序列表中选择“BlueJeans”。

    ![应用程序列表中的 BlueJeans 链接](common/all-applications.png)

3. 在左侧菜单中，选择“用户和组”。

    ![“用户和组”链接](common/users-groups-blade.png)

4. 单击“添加用户”按钮，然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格](common/add-assign-user.png)

5. 在“用户和组”对话框中，选择“用户”列表中的 Britta Simon，然后单击屏幕底部的“选择”按钮。

6. 如果你在 SAML 断言中需要任何角色值，请在“选择角色”对话框中从列表中为用户选择合适的角色，然后单击屏幕底部的“选择”按钮。

7. 在“添加分配”对话框中，单击“分配”按钮。

### <a name="create-bluejeans-test-user"></a>创建 BlueJeans 测试用户

本部分的目的是在 BlueJeans 中创建名为“Britta Simon”的用户。 BlueJeans 支持在默认情况下启用的自动用户预配。 有关如何配置自动用户预配的更多详细信息，请参见[此处](bluejeans-provisioning-tutorial.md)。

如果需要手动创建用户，请执行以下步骤：

1. 以管理员身份登录到 BlueJeans 公司站点。

2. 转到“管理”\>“管理用户”\>“添加用户”。

    ![管理员](./media/bluejeans-tutorial/ic785877.png "管理员")

    > [!IMPORTANT]
    > “添加用户”选项卡仅在“安全”选项卡中的的“启用自动预配”处于未选中状态时可用。

3. 在“添加用户”部分中，执行以下步骤：

    ![添加用户](./media/bluejeans-tutorial/ic785886.png "添加用户")

    a. 在“名字”文本框中，输入用户的名字，如 Britta。

    b. 在“姓氏”文本框中，输入用户的名字，如 Simon。

    c. 在“选取 BlueJeans 用户名”文本框中，输入用户的用户名，例如 **Brittasimon**

    d. 在“创建密码”文本框中，输入你的密码。

    e. 在“公司”文本框中，输入你的公司。

    f. 在“电子邮件地址”文本框中，输入用户的电子邮件地址，例如 `brittasimon\@contoso.com`。

    g. 在“创建 BlueJeans 会议 ID”文本框中，输入你的会议 ID。

    h. 在“选取审查方密码”文本框中，输入你的密码。

    i. 单击“继续”。

    ![添加用户](./media/bluejeans-tutorial/ic785887.png "添加用户")

    J. 单击“添加用户”。

> [!NOTE]
> 可使用 BlueJeans 提供的任何其他 BlueJeans 用户帐户创建工具或 API 预配 Azure AD 用户帐户。

### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的 BlueJeans 磁贴时，应当会自动登录到为其设置了 SSO 的 BlueJeans。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

- [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory 的应用程序访问与单一登录是什么？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什么是 Azure Active Directory 中的条件访问？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
