---
title: 教程：Azure Active Directory 与 Zoho One 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Zoho One 之间配置单一登录。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/20/2021
ms.author: jeedes
ms.openlocfilehash: 225d6b13c882566a6b71c5809d67955a27561ed6
ms.sourcegitcommit: 2501fe97400e16f4008449abd1dd6e000973a174
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/08/2021
ms.locfileid: "99821010"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho-one"></a>教程：Azure Active Directory 与 Zoho One 的集成

本教程介绍如何将 Zoho One 与 Azure Active Directory (Azure AD) 进行集成。 将 Zoho One 与 Azure AD 集成后，你可以：

* 在 Azure AD 中控制谁有权访问 Zoho One。
* 让用户能够使用其 Azure AD 帐户自动登录到 Zoho One。
* 在一个中心位置（Azure 门户）管理帐户。

## <a name="prerequisites"></a>必备条件

若要配置 Azure AD 与 Zoho One 的集成，需要准备好以下各项：

* 一个 Azure AD 订阅。 如果没有 Azure AD 环境，可以获取一个[免费帐户](https://azure.microsoft.com/free/)。
* 启用了单一登录的 Zoho One 订阅。

## <a name="scenario-description"></a>方案描述

本教程会在测试环境中配置和测试 Azure AD 单一登录。

* Zoho One 支持 **SP** 和 **IDP** 发起的 SSO

> [!NOTE]
> 此应用程序的标识符是一个固定字符串值，因此只能在一个租户中配置一个实例。

## <a name="add-zoho-one-from-the-gallery"></a>从库中添加 Zoho One

若要配置 Zoho One 与 Azure AD 的集成，需要从库中将 Zoho One 添加到托管 SaaS 应用列表。

1. 使用工作或学校帐户或个人 Microsoft 帐户登录到 Azure 门户。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务  。
1. 导航到“企业应用程序”，选择“所有应用程序”   。
1. 若要添加新的应用程序，请选择“新建应用程序”  。
1. 在“从库中添加”部分的搜索框中，键入“Zoho One” 。
1. 在结果面板中选择“Zoho One”，然后添加该应用。 在该应用添加到租户时等待几秒钟。

## <a name="configure-and-test-azure-ad-sso-for-zoho-one"></a>配置并测试 Zoho One 的 Azure AD SSO

使用名为“B.Simon”的测试用户配置并测试 Zoho One 的 Azure AD SSO。 若要使 SSO 有效，需要在 Azure AD 用户与 Zoho One 相关用户之间建立关联。

若要配置并测试 Zoho One 的 Azure AD SSO，请执行以下步骤：

1. **[配置 Azure AD SSO](#configure-azure-ad-sso)** - 使用户能够使用此功能。
    1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 B. Simon 测试 Azure AD 单一登录。
    1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 B. Simon 能够使用 Azure AD 单一登录。
1. [配置 Zoho One SSO](#configure-zoho-one-sso) - 在应用程序端配置单一登录设置。
    1. **[创建 Zoho One 测试用户](#create-zoho-one-test-user)** - 在 Zoho One 中创建 B.Simon 的对应用户，并将其关联到其在 Azure AD 中的表示形式。
1. **[测试 SSO](#test-sso)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-sso"></a>配置 Azure AD SSO

按照下列步骤在 Azure 门户中启用 Azure AD SSO。

1. 在 Azure 门户中的“Zoho One”应用程序集成页上，找到“管理”部分并选择“单一登录”  。
1. 在“选择单一登录方法”页上选择“SAML” 。
1. 在“设置 SAML 单一登录”页面上，单击“基本 SAML 配置”旁边的铅笔图标以编辑设置 。

   ![编辑基本 SAML 配置](common/edit-urls.png)

4. 如果要在 **IDP** 发起的模式下配置应用程序，请在“基本 SAML 配置”部分执行以下步骤： 

    a. 在“标识符”文本框中键入 URL：`one.zoho.com`

    b. 在“回复 URL”  文本框中，使用以下模式键入 URL：`https://accounts.zoho.com/samlresponse/<saml-identifier>`

    > [!NOTE]
    > 上面的“回复 URL”值不是实际值。  将从“配置 Zoho One 单一登录”部分的步骤 4 获取 `<saml-identifier>` 值，本教程后面部分介绍此内容。

    c. 单击“设置其他 URL”  。

    d. 在“中继状态”文本框中键入 URL：`https://one.zoho.com`

5. 如果要在“SP”发起的模式下配置应用程序，请执行以下步骤  ：

    在“登录 URL”  文本框中，使用以下模式键入 URL：`https://accounts.zoho.com/samlauthrequest/<domain_name>?serviceurl=https://one.zoho.com` 

    > [!NOTE] 
    > 上面的“登录 URL”值不是实际值。  在本教程稍后所述的“配置 Zoho One 单一登录”部分，我们将使用实际“登录 URL”更新该值。  

6. 在“使用 SAML 设置单一登录”  页上，在“SAML 签名证书”  部分中，单击“下载”  以根据要求从给定的选项下载 **证书(Base64)** 并将其保存在计算机上。

    ![证书下载链接](common/certificatebase64.png)

7. 在“设置 Zoho One”部分，根据要求复制相应的 URL。 

    ![复制配置 URL](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户 

在本部分，我们将在 Azure 门户中创建名为 B.Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”  。
1. 选择屏幕顶部的“新建用户”。
1. 在“用户”属性中执行以下步骤：
   1. 在“名称”字段中，输入 `B.Simon`。  
   1. 在“用户名”字段中输入 username@companydomain.extension。 例如，`B.Simon@contoso.com` 。
   1. 选中“显示密码”复选框，然后记下“密码”框中显示的值。
   1. 单击“创建”。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，你将通过授予 B.Simon 访问 Zoho One 的权限，允许该用户使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”。 
1. 在应用程序列表中，选择“Zoho One”  。
1. 在应用的概述页中，找到“管理”部分，选择“用户和组” 。
1. 选择“添加用户”，然后在“添加分配”对话框中选择“用户和组”。
1. 在“用户和组”对话框中，从“用户”列表中选择“B.Simon”，然后单击屏幕底部的“选择”按钮。
1. 如果你希望将某角色分配给用户，可以从“选择角色”下拉列表中选择该角色。 如果尚未为此应用设置任何角色，你将看到选择了“默认访问权限”角色。
1. 在“添加分配”对话框中，单击“分配”按钮。

### <a name="configure-zoho-one-sso"></a>配置 Zoho One SSO

1. 在另一个 Web 浏览器窗口中，以管理员身份登录到 Zoho One 公司站点。

2. 在“组织”  选项卡上，单击“SAML 身份验证”  下的“设置”。 

    ![Zoho One 组织](./media/zoho-one-tutorial/set-up.png)

3. 在弹出页面上执行以下步骤：

    ![Zoho One 登录](./media/zoho-one-tutorial/save.png)

    a. 在“登录 URL”文本框中，粘贴从 Azure 门户复制的“登录 URL”值   。

    b. 在“注销 URL”文本框中，粘贴从 Azure 门户复制的“注销 URL”值   。

    c. 单击“浏览”  来上传从 Azure 门户下载的 **证书 (Base64)** 。

    d. 单击“保存”  。

4. 保存 SAML 身份验证设置后，复制“SAML 标识符”值并在其后追加“回复 URL”（替代 `<saml-identifier>`，例如 `https://accounts.zoho.com/samlresponse/one.zoho.com`），然后在“基本 SAML 配置”部分的“回复 URL”文本框中粘贴生成的值。

    ![Zoho One SAML](./media/zoho-one-tutorial/saml-identifier.png)

5. 转到“域”  选项卡，然后单击“添加域”  。

    ![Zoho One 域](./media/zoho-one-tutorial/add-domain.png)

6. 在“添加域”  页面上，执行以下步骤：

    ![Zoho One 添加域](./media/zoho-one-tutorial/add-domain-name.png)

    a. 在“域名”文本框中键入域，例如 contoso.com。 

    b. 单击“添加”  。

    >[!Note]
    >在添加域后，按照[这些](https://www.zoho.com/one/help/admin-guide/domain-verification.html)步骤验证域。 验证域后，请在 Azure 门户上“基本 SAML 配置”部分的“登录 URL”中使用该域名。  

### <a name="create-zoho-one-test-user"></a>创建 Zoho One 测试用户

若要使 Azure AD 用户能够登录到 Zoho One，必须将其预配到 Zoho One 中。 在 Zoho One 中，需要手动执行预配任务。

**若要预配用户帐户，请执行以下步骤：**

1. 以安全管理员身份登录到 Zoho One。

2. 在“用户”  选项卡上，单击“用户登录”  。

    ![Zoho One 用户](./media/zoho-one-tutorial/user.png)

3. 在“添加用户”页上执行以下步骤： 

    ![Zoho One 添加用户](./media/zoho-one-tutorial/add-user.png)
    
    a. 在“姓名”文本框中，输入用户名，例如 Britta Simon   。
    
    b. 在“电子邮件地址”文本框中，输入用户的电子邮件地址，例如 brittasimon@contoso.com。

    >[!Note]
    >从域列表中选择已验证的域。

    c. 单击“添加”  。

### <a name="test-sso"></a>测试 SSO

在本部分，你将使用以下选项测试 Azure AD 单一登录配置。 

#### <a name="sp-initiated"></a>SP 启动的：

* 在 Azure 门户中单击“测试此应用程序”。 这样将会重定向到 Zoho One 登录 URL，可以从那里启动登录流。  

* 直接转到 Zoho One 登录 URL，从那里启动登录流。

#### <a name="idp-initiated"></a>IDP 启动的：

* 在 Azure 门户中单击“测试此应用程序”，你应该会自动登录到为其设置了 SSO 的 Zoho One。 

还可以使用 Microsoft“我的应用”在任何模式下测试此应用程序。 当你在“我的应用”中单击 Zoho One 磁贴时，如果这是在 SP 模式下配置的，系统会将你重定向到应用程序登录页来启动登录流；如果这是在 IDP 模式下配置的，系统会让你自动登录到为其设置了 SSO 的 Zoho One。 有关“我的应用”的详细信息，请参阅[“我的应用”简介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="next-steps"></a>后续步骤

在配置 Zoho One 后，可以强制实施会话控制，从而实时防止组织的敏感数据外泄和渗透。 会话控制从条件访问扩展而来。 [了解如何通过 Microsoft Cloud App Security 强制实施会话控制](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)。
