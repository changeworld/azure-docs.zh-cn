---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/09/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 8b68aa5263a95a7efc15f76289a62a4cbec23ee3
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100093892"
---
## <a name="azure-security-benchmark"></a>Azure 安全基准

[Azure 安全基准](../../../../articles/security/benchmarks/overview.md)提供有关如何在 Azure 上保护云解决方案的建议。 若要查看此服务如何完全映射到 Azure 安全基准，请参阅 [Azure 安全基准映射文件](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)。

若要查看所有 Azure 服务的可用 Azure Policy 内置项如何映射到此合规性标准，请参阅 [Azure Policy 法规遵从性 - Azure 安全基准](../../../../articles/governance/policy/samples/azure-security-benchmark.md)。

|域 |控制 ID |控制标题 |策略<br /><sub>（Azure 门户）</sub> |策略版本<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|日志记录和威胁检测 |LT-4 |为 Azure 资源启用日志记录 |[应启用 Azure Data Lake Store 中的资源日志](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F057ef27e-665e-4328-8ea3-04b3122bd9fb) |[4.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Data%20Lake/DataLakeStore_AuditDiagnosticLog_Audit.json) |

## <a name="azure-security-benchmark-v1"></a>Azure 安全基准 v1

[Azure 安全基准](../../../../articles/security/benchmarks/overview.md)提供有关如何在 Azure 上保护云解决方案的建议。 若要查看此服务如何完全映射到 Azure 安全基准，请参阅 [Azure 安全基准映射文件](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)。

若要查看所有 Azure 服务的可用 Azure Policy 内置项如何映射到此合规性标准，请参阅 [Azure Policy 法规遵从性 - Azure 安全基准](../../../../articles/governance/policy/samples/azure-security-benchmark.md)。

|域 |控制 ID |控制标题 |策略<br /><sub>（Azure 门户）</sub> |策略版本<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|日志记录和监视 |2.3 |为 Azure 资源启用审核日志记录 |[应启用 Azure Data Lake Store 中的资源日志](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F057ef27e-665e-4328-8ea3-04b3122bd9fb) |[4.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Data%20Lake/DataLakeStore_AuditDiagnosticLog_Audit.json) |

## <a name="cmmc-level-3"></a>CMMC 级别 3

若要查看各项 Azure 服务可用的 Azure Policy 内置项如何映射到此合规性标准，请参阅 [Azure Policy 法规合规性 - CMMC 级别 3](../../../../articles/governance/policy/samples/cmmc-l3.md)。
有关此合规性标准的详细信息，请参阅[网络安全成熟度模型认证 (CMMC)](https://www.acq.osd.mil/cmmc/docs/CMMC_Model_Main_20200203.pdf)。

|域 |控制 ID |控制标题 |策略<br /><sub>（Azure 门户）</sub> |策略版本<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|系统和通信保护 |SC.3.177 |采用经 FIPS 验证的加密系统来保护 CUI 的机密性。 |[要求对 Data Lake Store 帐户进行加密](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa7ff3161-0087-490a-9ad9-ad6217f4f43a) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Data%20Lake/DataLakeStoreEncryption_Deny.json) |
|系统和通信保护 |SC.3.191 |保护静态 CUI 的机密性。 |[要求对 Data Lake Store 帐户进行加密](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa7ff3161-0087-490a-9ad9-ad6217f4f43a) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Data%20Lake/DataLakeStoreEncryption_Deny.json) |

## <a name="hipaa-hitrust-92"></a>HIPAA HITRUST 9.2

若要查看所有 Azure 服务的可用 Azure Policy 内置项如何映射到此合规性标准，请参阅 [Azure Policy 法规合规性 - HIPAA HITRUST 9.2](../../../../articles/governance/policy/samples/hipaa-hitrust-9-2.md)。
有关此合规性标准的详细信息，请参阅 [HIPAA HITRUST 9.2](https://www.hhs.gov/hipaa/for-professionals/security/laws-regulations/index.html)。

|域 |控制 ID |控制标题 |策略<br /><sub>（Azure 门户）</sub> |策略版本<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|审核日志 |1202.09aa1System.1 - 09.aa |为系统上的所有活动（创建、读取、更新、删除）创建安全审核记录，涉及涵盖的信息。 |[应启用 Azure Data Lake Store 中的资源日志](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F057ef27e-665e-4328-8ea3-04b3122bd9fb) |[4.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Data%20Lake/DataLakeStore_AuditDiagnosticLog_Audit.json) |
|可移动媒体管理 |0304.09o3Organizational.1 - 09.o |组织限制在组织系统中使用可写可移动媒体和个人拥有的可移动媒体。 |[要求对 Data Lake Store 帐户进行加密](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa7ff3161-0087-490a-9ad9-ad6217f4f43a) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Data%20Lake/DataLakeStoreEncryption_Deny.json) |

