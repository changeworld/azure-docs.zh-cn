---
title: 教程：为 CybSafe 配置自动用户预配 Azure Active Directory |Microsoft Docs
description: 了解如何自动将用户 Azure AD 帐户预配到 CybSafe 以及取消其预配。
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: 7255fe44-1662-4ae4-9ff3-9492911b7ce0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2020
ms.author: Zhchia
ms.openlocfilehash: b79a63d16229c761b29776667c771e940d8f69d7
ms.sourcegitcommit: e3151d9b352d4b69c4438c12b3b55413b4565e2f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/15/2021
ms.locfileid: "100527116"
---
# <a name="tutorial-configure-cybsafe-for-automatic-user-provisioning"></a>教程：为 CybSafe 配置自动用户预配

本教程介绍了需要在 CybSafe 和 Azure Active Directory (Azure AD) 中执行的步骤，以配置自动用户预配。 配置后，Azure AD 使用 Azure AD 预配服务自动设置用户和组并取消其预配到 [CybSafe](https://app.cybsafe.com/login) 。 有关此服务的功能、工作原理以及常见问题的重要详细信息，请参阅[使用 Azure Active Directory 自动将用户预配到 SaaS 应用程序和取消预配](../app-provisioning/user-provisioning.md)。 


## <a name="capabilities-supported"></a>支持的功能
> [!div class="checklist"]
> * 在 CybSafe 中创建用户
> * 当用户不再需要访问权限时，删除 CybSafe 中的用户
> * 使用户属性在 Azure AD 和 CybSafe 之间保持同步
> * 在 CybSafe 中预配组和组成员身份

## <a name="prerequisites"></a>先决条件

本教程中概述的方案假定你已具有以下先决条件：

* [Azure AD 租户](../develop/quickstart-create-new-tenant.md) 
* Azure AD 中[有权](../roles/permissions-reference.md)配置预配的用户帐户（例如应用管理员、云应用管理员、应用所有者或全局管理员）。 
* 具有企业订阅的 [CybSafe](https://app.cybsafe.com/login) 管理员帐户。

## <a name="step-1-plan-your-provisioning-deployment"></a>步骤 1。 计划预配部署
1. 了解[预配服务的工作原理](../app-provisioning/user-provisioning.md)。
2. 确定谁在[预配范围](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)中。
3. 确定要 [在 Azure AD 与 CybSafe 之间映射](../app-provisioning/customize-application-attributes.md)的数据。 

## <a name="step-2-configure-cybsafe-to-support-provisioning-with-azure-ad"></a>步骤 2. 配置 CybSafe 以支持 Azure AD 的预配

1. CybSafe 的 **租户 URL** 为： `https://app.cybsafe.com/scim/v2/` 。 此值将在 Azure 门户中的 CybSafe 应用程序的 "预配" 选项卡中输入。
2. 用管理员帐户登录到 [CybSafe](https://app.cybsafe.com/login) 。
3. 导航到 " [设置->身份管理](https://app.cybsafe.com/settings/identity-management)"。 
4. 在 "用户预配" 部分中单击 " **生成令牌** "。 此值将在 Azure 门户中的 CybSafe 应用程序的 "预配" 选项卡中输入。

## <a name="step-3-add-cybsafe-from-the-azure-ad-application-gallery"></a>步骤 3. 从 Azure AD 应用程序库添加 CybSafe

从 Azure AD 应用程序库中添加 CybSafe，开始管理预配到 CybSafe。 如果以前为 SSO 设置了 CybSafe，则可以使用相同的应用程序。 不过，建议在最初测试集成时单独创建一个应用。 若要详细了解如何从库中添加应用，可以单击[此处](../manage-apps/add-application-portal.md)。 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>步骤 4. 定义谁在预配范围中 

使用 Azure AD 预配服务，可以根据对应用的分配或用户/组的特性来限定谁在预配范围内。 如果选择根据分配来限定要将谁预配到应用，可以按照下面的[步骤](../manage-apps/assign-user-or-group-access-portal.md)操作，将用户和组分配到应用。 如果选择只根据用户或组的特性来限定谁在预配范围内，可以使用范围筛选器，如[此处](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)所述。 

* 将用户和组分配到 CybSafe 时，必须选择 " **默认" 访问权限** 以外的其他角色。 具有“默认访问”角色的用户被排除在预配范围之外，并在预配日志中被标记为无有效资格。 如果应用上唯一可用的角色是“默认访问”角色，可以[更新应用清单](../develop/howto-add-app-roles-in-azure-ad-apps.md)来添加其他角色。 

* 先小部分测试。 在向全员推出之前，请先使用少量的用户和组进行测试。 如果预配范围设置为分配的用户和组，则可以先尝试将一两个用户或组分配到应用。 当预配范围设置为所有用户和组时，可以指定[基于属性的范围筛选器](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)。 


## <a name="step-5-configure-automatic-user-provisioning-to-cybsafe"></a>步骤 5。 配置 CybSafe 的自动用户预配 

此部分逐步介绍了如何将 Azure AD 预配服务配置为根据 Azure AD 中的用户和/或组分配在 TestApp 中创建、更新和禁用用户和/或组。

### <a name="to-configure-automatic-user-provisioning-for-cybsafe-in-azure-ad"></a>若要在 Azure AD 中配置 CybSafe 的自动用户预配：

1. 登录 [Azure 门户](https://portal.azure.com)。 依次选择“企业应用”和“所有应用”。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

2. 在应用程序列表中，选择 " **CybSafe**"。

    ![应用程序列表中的 CybSafe 链接](common/all-applications.png)

3. 选择“预配”  选项卡。

    ![“预配”选项卡](common/provisioning.png)

4. 将“预配模式”设置为“自动”。

    ![“预配”选项卡“自动”](common/provisioning-automatic.png)

5. 在 " **管理员凭据** " 部分中，输入你的 CYBSAFE 租户 URL 和机密令牌。 单击 " **测试连接** " 以确保 Azure AD 可以连接到 CybSafe。 如果连接失败，请确保 CybSafe 帐户具有管理员权限，然后重试。

    ![令牌](common/provisioning-testconnection-tenanturltoken.png)

6. 在“通知电子邮件”字段中，输入应接收预配错误通知的个人或组的电子邮件地址，并选中“发生故障时发送电子邮件通知”复选框 。

    ![通知电子邮件](common/provisioning-notification-email.png)

7. 选择“保存”。

8. 在 " **映射** " 部分下，选择 " **将 Azure Active Directory 用户同步到 CybSafe**"。

9. 在 " **属性映射** " 部分中，查看从 Azure AD 同步到 CybSafe 的用户属性。 选为 " **匹配** " 属性的特性用于匹配 CybSafe 中的用户帐户以执行更新操作。 如果选择更改 [匹配的目标属性](../app-provisioning/customize-application-attributes.md)，将需要确保 CybSafe API 支持基于该属性筛选用户。 选择“保存”按钮以提交任何更改  。

   |Attribute|类型|支持筛选|
   |---|---|---|
   |userName|字符串|&check;|
   |活动|Boolean|
   |displayName|字符串|
   |title|字符串|
   |preferredLanguage|字符串|
   |name.givenName|字符串|
   |name.familyName|字符串|
   |name.formatted|字符串|
   |name.honorificPrefix|String|
   |name.honorificSuffix|字符串|
   |nickName|字符串|
   |phoneNumbers[type eq "mobile"].value|字符串|
   |emails[type eq "work"].value|字符串|
   |区域设置|字符串|
   |timezone|字符串|
   |userType|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:division|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:organization|字符串|
   

10. 在 " **映射** " 部分下，选择 " **将 Azure Active Directory 组同步到 CybSafe**"。

11. 在 " **属性映射** " 部分中，查看从 Azure AD 同步到 CybSafe 的组属性。 选为 " **匹配** " 属性的特性用于匹配 CybSafe 中的组以执行更新操作。 选择“保存”按钮以提交任何更改  。

      |Attribute|类型|支持筛选|
      |---|---|---|
      |displayName|字符串|&check;|
      |members|参考|

12. 若要配置范围筛选器，请参阅[范围筛选器教程](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)中提供的以下说明。

13. 若要为 CybSafe 启用 Azure AD 预配服务，请在 "**设置**" 部分中将 "**预配状态**" 更改为 **"打开**"。

      ![预配状态已打开](common/provisioning-toggle-on.png)

14. 通过在 "**设置**" 部分的 "**范围**" 中选择所需的值，定义要预配到 CybSafe 的用户和/或组。

      ![预配范围](common/provisioning-scope.png)

15. 已准备好预配时，单击“保存”  。

      ![保存预配配置](common/provisioning-configuration-save.png)

此操作会对“设置”部分的“范围”中定义的所有用户和组启动初始同步周期 。 初始周期执行的时间比后续周期长，只要 Azure AD 预配服务正在运行，后续周期大约每隔 40 分钟就会进行一次。 

## <a name="step-6-monitor-your-deployment"></a>步骤 6. 监视部署
配置预配后，请使用以下资源来监视部署：

1. 通过[预配日志](../reports-monitoring/concept-provisioning-logs.md)来确定哪些用户已预配成功或失败
2. 检查[进度栏](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md)来查看预配周期的状态以及完成进度
3. 如果怀疑预配配置处于非正常状态，则应用程序将进入隔离状态。 有关隔离状态的详细信息，请访问[此处](../app-provisioning/application-provisioning-quarantine-status.md)。  

## <a name="change-log"></a>更改日志

* 02/15/2021-已添加用户企业扩展属性 " **部门**"、" **部门** " 和 " **组织** "。

## <a name="additional-resources"></a>其他资源

* [管理企业应用的用户帐户预配](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>后续步骤

* [了解如何查看日志并获取有关预配活动的报告](../app-provisioning/check-status-user-account-provisioning.md)