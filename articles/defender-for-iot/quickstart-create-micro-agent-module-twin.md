---
title: 创建 Defender IoT 微代理模块孪生（预览）
titleSuffix: Azure Defender for IoT
description: 了解如何为新设备创建单独的 DefenderIotMicroAgent 模块孪生。
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 1/20/2021
ms.topic: quickstart
ms.service: azure
ms.openlocfilehash: f14f253960b628f2bf8052e92dbec3c897a205db
ms.sourcegitcommit: 2501fe97400e16f4008449abd1dd6e000973a174
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/08/2021
ms.locfileid: "99820834"
---
# <a name="create-a-defender-iot-micro-agent-module-twin-preview"></a>创建 Defender IoT 微代理模块孪生（预览）

可为新设备创建单独的 DefenderIotMicroAgent 模块孪生 **** 。 还可对 IoT 中心内的所有设备批量创建模块孪生。 

## <a name="device-twins"></a>设备孪生 

对于在 Azure 中生成的 IoT 解决方案，设备孪生在设备管理和流程自动化方面发挥着关键作用。 

Defender for IoT 可与你的现有 IoT 设备管理平台完全集成。 完全集成后，你可管理设备的安全状态，还可使用所有现有的设备控制功能。 集成是使用 IoT 中心孪生机制实现的。 

详细了解 Azure IoT 中心内 [设备孪生](../iot-hub/iot-hub-devguide-device-twins.md) 的概念。 

## <a name="security-module-twins"></a>安全模块孪生 

Defender for IoT 对每台设备使用一个安全模块孪生。 该安全模块孪生保存了解决方案中每台特定设备的与设备安全性相关的信息。 设备安全属性通过专用的安全模块配置来增强通信安全性，以实现需要更少资源的更新和维护。 

## <a name="understanding-defenderiotmicroagent-module-twins"></a>了解 DefenderIotMicroAgent 模块孪生 

对于 Azure 中内置的 IoT 解决方案，设备孪生在设备管理和流程自动化方面发挥着关键作用。

Defender for IoT 提供与现有 IoT 设备管理平台完全集成的功能，让你能够管理设备的安全状态，并利用现有的设备控制功能。 你可使用 IoT 中心孪生机制来集成 Defender for IoT。  

若要详细了解 Azure IoT 中心内模块孪生的一般概念，请参阅  [IoT 中心模块孪生](../iot-hub/iot-hub-devguide-module-twins.md)。

Defender for IoT 使用模块孪生机制，并为每台设备维护一个名为 `DefenderIotMicroAgent` 的安全模块孪生。 

若要充分利用各项 Defender for IoT 功能，需要对服务中的每台设备创建、配置和使用这些安全模块孪生。 

## <a name="create-defenderiotmicroagent-module-twin"></a>创建 DefenderIotMicroAgent 模块孪生 

可通过手动编辑每个模块孪生来包含每台设备的特定配置，创建 DefenderIotMicroAgent 模块孪生。 

若要手动为设备创建新的 DefenderIotMicroAgent 模块孪生 **** ： 

1. 在 IoT 中心，找到并选择要在其上创建安全模块孪生的设备。 

1. 选择“添加模块标识” **** 。 

1. 在“模块标识名称”字段中，输入 `DefenderIotMicroAgent`。 ****   

1. 选择“保存”。 **** 

## <a name="verify-the-creation-of-a-module-twin"></a>验证模块孪生的创建情况 

验证特定的设备是否存在安全模块孪生： 

1. 在 Azure IoT 中心，从“资源管理器”菜单中选择“IoT”。 ****   ****   

1. 输入设备 ID，或者在“查询设备字段”中选择一个选项，然后选择“查询设备”。 ****  ****  

    :::image type="content" source="media/quickstart-create-micro-agent-module-twin/iot-devices.png" alt-text="选择“查询设备”以获取设备列表。":::

1. 选择设备，打开“设备详细信息”页面。 

1. 选择“模块标识”菜单，在与设备关联的模块标识列表中，确认是否存在 DefenderIotMicroAgent 模块。 ****   ****  

    :::image type="content" source="media/quickstart-create-micro-agent-module-twin/device-details-module.png" alt-text="从选项卡中选择模块标识。":::

## <a name="next-steps"></a>后续步骤 

转到下一篇文章，了解如何[调查安全建议](quickstart-investigate-security-recommendations.md)。
