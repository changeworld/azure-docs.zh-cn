---
title: 使用 Azure 数据工厂（预览版）从 QuickBooks Online 复制数据
description: 了解如何通过在 Azure 数据工厂管道中使用复制活动，将数据从 QuickBooks Online 复制到受支持的接收器数据存储。
author: linda33wj
ms.author: jingwang
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/15/2021
ms.openlocfilehash: eecbcb817ad31480f8f6c3c7272328d06b17c081
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/14/2021
ms.locfileid: "100384055"
---
# <a name="copy-data-from-quickbooks-online-using-azure-data-factory-preview"></a>使用 Azure 数据工厂（预览版）从 QuickBooks Online 复制数据
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

本文概述了如何使用 Azure 数据工厂中的复制活动从 QuickBooks Online 复制数据。 它是基于概述复制活动总体的[复制活动概述](copy-activity-overview.md)一文。

> [!IMPORTANT]
> 此连接器目前提供预览版。 欢迎试用并提供反馈。 若要在解决方案中使用预览版连接器的依赖项，请联系 [Azure 客户支持](https://azure.microsoft.com/support/)。

## <a name="supported-capabilities"></a>支持的功能

以下活动支持此 QuickBooks 连接器：

- 带有[支持的源或接收器矩阵](copy-activity-overview.md)的[复制活动](copy-activity-overview.md)
- [Lookup 活动](control-flow-lookup-activity.md)

可以将数据从 QuickBooks Online 复制到任何受支持的接收器数据存储。 有关复制活动支持作为源/接收器的数据存储列表，请参阅[支持的数据存储](copy-activity-overview.md#supported-data-stores-and-formats)表。

此连接器支持 QuickBooks OAuth 2.0 身份验证。

## <a name="getting-started"></a>入门

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

对于特定于 QuickBooks 连接器的数据工厂实体，以下部分提供有关用于定义这些实体的属性的详细信息。

## <a name="linked-service-properties"></a>链接服务属性

QuickBooks 链接的服务支持以下属性：

| 属性 | 说明 | 必需 |
|:--- |:--- |:--- |
| type | type 属性必须设置为：**QuickBooks** | 是 |
| connectionProperties | 一组属性，定义如何连接到 QuickBooks。 | 是 |
| 在 `connectionProperties` 下： | | |
| endpoint | QuickBooks Online 服务器的终结点。 （即 quickbooks.api.intuit.com）  | 是 |
| companyId | QuickBooks 公司授权的公司 ID。 有关如何查找公司 ID 的信息，请参阅[如何查找我的公司 ID](https://quickbooks.intuit.com/community/Getting-Started/How-do-I-find-my-Company-ID/m-p/185551)。 | 是 |
| consumerKey | 用于 OAuth 2.0 身份验证的 QuickBooks Online 应用程序的客户端 ID。 可从[此处](https://developer.intuit.com/app/developer/qbo/docs/develop/authentication-and-authorization/oauth-2.0#obtain-oauth2-credentials-for-your-app)了解详细信息。 | 是 |
| consumerSecret | 用于 OAuth 2.0 身份验证的 QuickBooks Online 应用程序的客户端密码。 将此字段标记为 SecureString 以安全地将其存储在数据工厂中或[引用存储在 Azure Key Vault 中的机密](store-credentials-in-key-vault.md)。 | 是 |
| refreshToken | 与 QuickBooks 应用程序关联的 OAuth 2.0 刷新令牌。 可从[此处](https://developer.intuit.com/app/developer/qbo/docs/develop/authentication-and-authorization/oauth-2.0#obtain-oauth2-credentials-for-your-app)了解详细信息。 请注意，刷新令牌将在 180 天后到期。 客户需要定期更新刷新令牌。 <br/>将此字段标记为 SecureString 以安全地将其存储在数据工厂中或[引用存储在 Azure Key Vault 中的机密](store-credentials-in-key-vault.md)。| 是 |
| useEncryptedEndpoints | 指定是否使用 HTTPS 加密数据源终结点。 默认值为 true。  | 否 |

**示例：**

```json
{
    "name": "QuickBooksLinkedService",
    "properties": {
        "type": "QuickBooks",
        "typeProperties": {
            "connectionProperties": {
                "endpoint": "quickbooks.api.intuit.com",
                "companyId": "<company id>",
                "consumerKey": "<consumer key>", 
                "consumerSecret": {
                     "type": "SecureString",
                     "value": "<clientSecret>"
                },
                "refreshToken": {
                     "type": "SecureString",
                     "value": "<refresh token>"
                },
                "useEncryptedEndpoints": true
            }
        }
    }
}
```

## <a name="dataset-properties"></a>数据集属性

有关可用于定义数据集的各部分和属性的完整列表，请参阅[数据集](concepts-datasets-linked-services.md)一文。 本部分提供 QuickBooks 数据集支持的属性列表。

若要从 QuickBooks Online 复制数据，请将数据集的 type 属性设置为 **QuickBooksObject**。 支持以下属性：

| properties | 说明 | 必需 |
|:--- |:--- |:--- |
| type | 数据集的 type 属性必须设置为：**QuickBooksObject** | 是 |
| tableName | 表的名称。 | 否（如果指定了活动源中的“query”） |

**示例**

```json
{
    "name": "QuickBooksDataset",
    "properties": {
        "type": "QuickBooksObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<QuickBooks linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>复制活动属性

有关可用于定义活动的各部分和属性的完整列表，请参阅[管道](concepts-pipelines-activities.md)一文。 本部分提供 QuickBooks 源支持的属性列表。

### <a name="quickbooks-as-source"></a>QuickBooks 作为源

要从 QuickBooks Online 复制数据，请将复制活动中的源类型设置为 **QuickBooksSource**。 复制活动 **source** 部分支持以下属性：

| properties | 说明 | 必需 |
|:--- |:--- |:--- |
| type | 复制活动 source 的 type 属性必须设置为：**QuickBooksSource** | 是 |
| query | 使用自定义 SQL 查询读取数据。 例如：`"SELECT * FROM "Bill" WHERE Id = '123'"`。 | 否（如果指定了数据集中的“tableName”） |

**示例：**

```json
"activities":[
    {
        "name": "CopyFromQuickBooks",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<QuickBooks input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "QuickBooksSource",
                "query": "SELECT * FROM \"Bill\" WHERE Id = '123' "
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```
## <a name="copy-data-from-quickbooks-desktop"></a>从 Quickbooks Desktop 复制数据

Azure 数据工厂中的复制活动不能直接从 Quickbooks Desktop 复制数据。 若要从 Quickbooks Desktop 复制数据，请将 Quickbooks 数据导出到一个逗号分隔值 (CSV) 文件，然后将该文件上传到 Azure Blob 存储。 从该存储中，可以使用数据工厂将数据复制到所选的接收器。

## <a name="lookup-activity-properties"></a>Lookup 活动属性

若要了解有关属性的详细信息，请查看 [Lookup 活动](control-flow-lookup-activity.md)。


## <a name="next-steps"></a>后续步骤
有关 Azure 数据工厂中复制活动支持作为源和接收器的数据存储的列表，请参阅[支持的数据存储](copy-activity-overview.md#supported-data-stores-and-formats)。
