---
title: 教程：使用 Azure Active Directory 为 MindTickle 配置自动用户预配 | Microsoft Docs
description: 了解如何配置 Azure Active Directory 来使其自动将用户帐户预配到 MindTickle 和取消其预配。
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/23/2019
ms.author: Zhchia
ms.openlocfilehash: c158358b1c4fcd72d9189d7a991645cb65a4dc83
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2020
ms.locfileid: "96348120"
---
# <a name="tutorial-configure-mindtickle-for-automatic-user-provisioning"></a>教程：为 MindTickle 配置自动用户预配

本教程的目的是演示需要执行哪些步骤在 MindTickle 和 Azure Active Directory (Azure AD) 中配置 Azure AD 来使其自动将用户和/或组预配到 MindTickle 以及对其取消预配。

> [!NOTE]
> 本教程介绍在 Azure AD 用户预配服务之上构建的连接器。 有关此服务的功能、工作原理以及常见问题的重要详细信息，请参阅[使用 Azure Active Directory 自动将用户预配到 SaaS 应用程序和取消预配](../app-provisioning/user-provisioning.md)。
>
> 此连接器目前以公共预览版提供。 若要详细了解 Microsoft Azure 预览版功能的一般使用条款，请参阅 [Microsoft Azure 预览版补充使用条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

## <a name="prerequisites"></a>先决条件

本教程中概述的方案假定你已具有以下先决条件：

* Azure AD 租户
* [MindTickle 租户](https://www.mindtickle.com/)
* 在 MindTickle 中具有管理员权限的用户帐户。

## <a name="assigning-users-to-mindtickle"></a>将用户分配到 MindTickle

Azure Active Directory 使用称为分配的概念来确定哪些用户应收到对所选应用的访问权限。 在自动用户预配的上下文中，只同步已分配到 Azure AD 中的应用程序的用户和/或组。

在配置和启用自动用户预配之前，应确定 Azure AD 中的哪些用户和/或组需要访问 MindTickle。 确定后，可按照此处的说明将这些用户和/或组分配到 MindTickle：
* [向企业应用分配用户或组](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-mindtickle"></a>有关将用户分配到 MindTickle 的重要提示

* 建议将单个 Azure AD 用户分配到 MindTickle 来测试自动用户预配配置。 其他用户和/或组可以稍后分配。

* 如果将用户分配到 MindTickle，必须在分配对话框中选择应用程序特定的所有有效角色（若可用）。 具有“默认访问权限”  角色的用户排除在预配之外。

## <a name="setup-mindtickle-for-provisioning"></a>设置 MindTickle 以进行预配

使用 Azure AD 为 MindTickle 配置自动用户预配之前，需要在 MindTickle 上启用 SCIM 预配。


1.  请联系 [MindTickle 的支持团队](mailto:help@mindtickle.com) ，获取配置 SCIM 预配所需的 JWT 令牌。


## <a name="add-mindtickle-from-the-gallery"></a>从库中添加 MindTickle

若要使用 Azure AD 为 MindTickle 配置自动用户预配，需要从 Azure AD 应用程序库将 MindTickle 添加到托管的 SaaS 应用程序列表。

**若要从 Azure AD 应用程序库中添加 MindTickle，请执行以下步骤：**

1. 在 [Azure 门户](https://portal.azure.com)的左侧导航面板中，选择“Azure Active Directory” 。

    ![“Azure Active Directory”按钮](common/select-azuread.png)

2. 转到“企业应用程序”，并选择“所有应用程序”。 

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

3. 要添加新应用程序，请选择窗格顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮](common/add-new-app.png)

4. 在搜索框中输入 MindTickle，在结果面板中选择 MindTickle，然后单击“添加”按钮来添加该应用程序  。

    ![结果列表中的 MindTickle](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-mindtickle"></a>配置到 MindTickle 的自动用户预配 

本部分介绍如何配置 Azure AD 预配服务以基于 Azure AD 中的用户和/或组分配在 MindTickle 中创建、更新和禁用用户和/或组。

> [!TIP]
> 还可选择按照 [MindTickle 单一登录教程](mindtickle-tutorial.md)中提供的说明为 MindTickle 启用基于 SAML 的单一登录。 可独立于自动用户预配来配置单一登录，尽管这两个功能互相补充

### <a name="to-configure-automatic-user-provisioning-for-mindtickle-in-azure-ad"></a>若要在 Azure AD 中为 MindTickle 配置自动用户预配：

1. 登录 [Azure 门户](https://portal.azure.com)。 依次选择“企业应用程序”、“所有应用程序” 。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

2. 在“应用程序”列表中，选择“MindTickle”。

    ![“应用程序”列表中的“MindTickle”链接](common/all-applications.png)

3. 选择“预配”  选项卡。

    ![“管理”选项的屏幕截图，其中突出显示了“预配”选项。](common/provisioning.png)

4. 将“预配模式”  设置为“自动”  。

    ![“预配模式”下拉列表的屏幕截图，其中突出显示了“自动”选项。](common/provisioning-automatic.png)

5. 在“管理员凭据”部分下的“租户 URL”中，输入 `https://admin.mindtickle.com/scim` 。 输入之前在“机密令牌”文本框中检索到的 JWT 令牌值，然后输入 MindTickle 支持团队提供的 JWT 令牌值 。 单击“测试连接”，确保 Azure AD 可连接到 myPolicies。 如果连接失败，请确保 MindTickle 帐户具有管理员权限，然后重试。

    ![租户 URL + 令牌](common/provisioning-testconnection-tenanturltoken.png)

6. 在“通知电子邮件”字段中，输入应接收预配错误通知的个人或组的电子邮件地址，并选中复选框“发生故障时发送电子邮件通知”   。

    ![通知电子邮件](common/provisioning-notification-email.png)

7. 单击“ **保存**”。

8. 在“映射”部分下，选中“将 Azure Active Directory 用户同步到 MindTickle” 。

    :::image type="content" source="media/mindtickle-provisioning-tutorial/usermapping.png" alt-text="“映射”部分的屏幕截图。在“名称”下可看到“将 Azure Active Directory 用户同步到 MindTickle”。" border="false":::

9. 在“特性映射”部分中，查看从 Azure AD 同步到 MindTickle 的用户特性。 选为“匹配”属性的特性用于匹配 MindTickle 中的用户帐户以执行更新操作。 选择“保存”按钮以提交任何更改  。

    :::image type="content" source="media/mindtickle-provisioning-tutorial/userattribute.png" alt-text="“属性映射”页的屏幕截图。有一张表列出了 Azure Active Directory 和 MindTickle 特性以及匹配优先级。" border="false":::

12. 若要配置范围筛选器，请参阅[范围筛选器教程](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)中提供的以下说明。

13. 若要为 MindTickle 启用 Azure AD 预配服务，请在“设置”部分中将“预配状态”更改为“开”  。

    ![预配状态已打开](common/provisioning-toggle-on.png)

14. 通过在“设置”部分的“范围”中选择所需的值，定义要预配到 MindTickle 的用户和/或组 。

    ![预配范围](common/provisioning-scope.png)

15. 已准备好预配时，单击“保存”  。

    ![保存预配配置](common/provisioning-configuration-save.png)

此操作会对“设置”部分的“范围”中定义的所有用户和/或组启动初始同步   。 初始同步所需的时间比后续同步要长。 若要详细了解预配用户和/或组所需的时间，请参阅[预配用户所需的时间](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users)。 

可使用“当前状态”部分监视进度并跟踪指向预配活动报告的链接，该报告描述了 Azure AD 预配服务对 MindTickle 执行的所有操作。 有关详细信息，请参阅[检查用户预配状态](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md)。 若要读取 Azure AD 预配日志，请参阅[有关自动用户帐户预配的报告](../app-provisioning/check-status-user-account-provisioning.md)。

## <a name="additional-resources"></a>其他资源

* [管理企业应用的用户帐户预配](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)
