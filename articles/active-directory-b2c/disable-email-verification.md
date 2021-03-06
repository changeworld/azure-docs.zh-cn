---
title: 客户注册期间禁用电子邮件验证
titleSuffix: Azure AD B2C
description: 了解客户在 Azure Active Directory B2C 中注册期间如何禁用电子邮件验证。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/10/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 0f70c8d501a7d56f4bc29e0f2b065760cad625e5
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97585013"
---
# <a name="disable-email-verification-during-customer-sign-up-in-azure-active-directory-b2c"></a>客户在 Azure Active Directory B2C 中注册期间禁用电子邮件验证

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

默认情况下，Azure Active Directory B2C (Azure AD B2C) 将验证客户的本地帐户（使用电子邮件地址或用户名注册的用户的帐户）的电子邮件地址。 Azure AD B2C 通过要求客户在注册过程中验证电子邮件地址来确保电子邮件地址有效。 它还可防止恶意执行组件使用自动过程在您的应用程序中生成欺诈帐户。

某些应用程序开发人员愿意在注册过程中跳过电子邮件验证，而让客户在以后验证电子邮件地址。 要支持此功能，可将 Azure AD B2C 配置为禁用电子邮件验证。 这样做可创建更流畅的注册过程，并使开发人员可以灵活地将已验证电子邮件地址的客户与尚未验证电子邮件地址的客户区分开来。

> [!WARNING]
> 在注册过程中禁用电子邮件验证可能会导致垃圾邮件。 如果禁用默认 Azure AD B2C 提供的电子邮件验证，我们建议你实现替换验证系统。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]
## <a name="disable-email-verification"></a>禁用电子邮件验证

::: zone pivot="b2c-user-flow"

请按照以下步骤禁用电子邮件验证：

1. 登录到 [Azure 门户](https://portal.azure.com)
1. 使用顶部菜单中的“目录 + 订阅”筛选器来选择包含 Azure AD B2C 租户的目录。
1. 在左侧菜单中，选择“Azure AD B2C”。 或者，选择“所有服务”并搜索并选择“Azure AD B2C”。
1. 选择“用户流”。
1. 选择要禁用电子邮件验证的用户流。 例如，*B2C_1_signinsignup*。
1. 选择“页面布局”。
1. 选择“本地帐户注册页”。
1. 在 **用户属性** 下，选择“电子邮件地址”。
1. 在 " **需要验证** " 下拉顺序中，选择 " **否**"。
1. 选择“保存”。 现在已为此用户流禁用电子邮件验证。

::: zone-end

::: zone pivot="b2c-custom-policy"
**LocalAccountSignUpWithLogonEmail** 技术配置文件是 [自行断言](self-asserted-technical-profile.md)的，它是在注册流程中调用的。 若要禁用电子邮件验证，请将 `EnforceEmailVerification` 元数据设置为 false。 覆盖扩展文件中的 LocalAccountSignUpWithLogonEmail 技术配置文件。 

1. 打开策略的扩展文件， 例如，<em>`SocialAndLocalAccounts/``TrustFrameworkExtensions.xml`</em>。
1. 查找 `ClaimsProviders` 元素。 如果该元素不存在，请添加该元素。
1. 将以下声明提供程序添加到 `ClaimsProviders` 元素：

```xml
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
      <Metadata>
        <Item Key="EnforceEmailVerification">false</Item>
      </Metadata>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```
::: zone-end

::: zone pivot="b2c-user-flow"

## <a name="test-your-policy"></a>测试策略 

1. 登录到 [Azure 门户](https://portal.azure.com)
1. 使用顶部菜单中的“目录 + 订阅”筛选器来选择包含 Azure AD B2C 租户的目录。
1. 在左侧菜单中，选择“Azure AD B2C”。 或者，选择“所有服务”并搜索并选择“Azure AD B2C”。
1. 选择“用户流”。
1. 选择要禁用电子邮件验证的用户流。 例如，*B2C_1_signinsignup*。
1. 若要测试策略，请选择 " **运行用户流**"。
1. 对于 " **应用程序**"，请选择前面注册的名为 *testapp1-template.json* 的 web 应用程序。 “回复 URL”应显示为 `https://jwt.ms`。
1. 单击 "**运行用户流**"
1. 你应该能够在不进行验证的情况下使用电子邮件地址进行注册。

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="update-and-test-the-relying-party-file"></a>更新和测试信赖方文件

1. 登录 [Azure 门户](https://portal.azure.com)。
1. 请确保使用包含 Azure AD 租户的目录，方法是选择顶部菜单中的“目录 + 订阅”筛选器，然后选择包含 Azure AD 租户的目录。
1. 选择 Azure 门户左上角的“所有服务”，然后搜索并选择“应用注册” 。
1. 选择“标识体验框架”。
1. 选择“上传自定义策略”，然后上传已更改的两个策略文件。
1. 选择已上传的注册或登录策略，并单击“立即运行”按钮。
1. 你应该能够在不进行验证的情况下使用电子邮件地址进行注册。

::: zone-end


## <a name="next-steps"></a>后续步骤

- 了解如何 [自定义中的用户界面 Azure Active Directory B2C](customize-ui-with-html.md)

