---
title: 管理 Azure Service Fabric 应用程序机密
description: 了解如何保护 Service Fabric 应用程序中的机密值（与平台无关）。
ms.topic: conceptual
ms.date: 01/04/2019
ms.custom: devx-track-csharp
ms.openlocfilehash: a11869c3b606ed9e74ce4f598109139fa1bb4164
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "89012817"
---
# <a name="manage-encrypted-secrets-in-service-fabric-applications"></a>管理 Service Fabric 应用程序中的已加密机密
本指南逐步讲解管理 Service Fabric 应用程序中的机密的步骤。 机密可以是任何敏感信息，例如存储连接字符串、密码或其他不应以明文形式处理的值。

在 Service Fabric 应用程序中使用加密的机密涉及三个步骤：
* 设置加密证书并对机密进行加密。
* 在应用程序中指定加密的机密。
* 对服务代码中的已加密机密进行解密。

## <a name="set-up-an-encryption-certificate-and-encrypt-secrets"></a>设置加密证书并对机密进行加密
设置加密证书和使用它来加密机密在 Windows 与 Linux 之间所有不同。
* [在 Windows 群集上设置加密证书并对机密进行加密。][secret-management-windows-specific-link]
* [在 Linux 群集上设置加密证书并对机密进行加密。][secret-management-linux-specific-link]

## <a name="specify-encrypted-secrets-in-an-application"></a>在应用程序中指定加密的机密
上一步骤介绍了如何使用证书来加密机密，并生成要在应用程序中使用的 base-64 编码的字符串。 可以在服务的 Settings.xml 中将此 base-64 编码的字符串指定为加密的[参数][parameters-link]，也可以在服务的 ServiceManifest.xml 中将其指定为加密的[环境变量][environment-variables-link]。

通过在服务的 Settings.xml 配置文件中将 `IsEncrypted` 属性设置为 `true` 来指定加密的[参数][parameters-link]：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```
通过在服务的 ServiceManifest.xml 文件中将 `Type` 属性设置为 `Encrypted` 来指定加密的[环境变量][environment-variables-link]：
```xml
<CodePackage Name="Code" Version="1.0.0">
  <EnvironmentVariables>
    <EnvironmentVariable Name="MyEnvVariable" Type="Encrypted" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </EnvironmentVariables>
</CodePackage>
```

机密也应包括在 Service Fabric 应用程序中，只需在应用程序清单中指定证书即可。 将 **SecretsCertificate** 元素添加到 **ApplicationManifest.xml**，并包括所需证书的指纹。

```xml
<ApplicationManifest … >
  ...
  <Certificates>
    <SecretsCertificate Name="MyCert" X509FindType="FindByThumbprint" X509FindValue="[YourCertThumbrint]"/>
  </Certificates>
</ApplicationManifest>
```
> [!NOTE]
> 激活可指定 SecretsCertificate 的应用程序后，Service Fabric 将查找匹配的的证书，并向该证书的私钥授予应用程序在完全权限下运行的标识。 Service Fabric 还会监视证书的更改，并重新应用相应的权限。 若要检测由公用名称声明的证书更改，Service Fabric 会运行定期任务，该任务查找所有匹配的证书，并将其与缓存的指纹列表进行对比。 如果检测到新指纹，表示该主题的证书已续订。 该任务每分钟在群集的每个节点上运行一次。
>
> 尽管 SecretsCertificate 确实允许使用基于主题的声明，但请注意，加密的设置会绑定到用于对客户端上的设置进行加密的密钥对。 需要确保原始加密证书（或等效证书）与基于主题的声明相匹配，并确保在可承载应用程序的群集的每个节点上安装该证书（包括其相应的私钥）。 与基于主题的声明匹配的且是通过与原始加密证书相同的密钥对生成的所有时间有效的证书均视为等效证书。
>

### <a name="inject-application-secrets-into-application-instances"></a>将应用程序机密注入应用程序实例
理想情况下，部署到不同环境的过程应尽可能自动化。 这可以通过在生成环境中执行机密加密，并在创建应用程序实例时提供加密机密作为参数来实现。

#### <a name="use-overridable-parameters-in-settingsxml"></a>在 Settings.xml 中使用可重写参数
Settings.xml 配置文件允许使用可在创建应用程序时提供的可重写参数。 使用 `MustOverride` 属性而不要提供参数值：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

若要重写 Settings.xml 中的值，可在 ApplicationManifest.xml 中声明服务的 override 参数：

```xml
<ApplicationManifest ... >
  <Parameters>
    <Parameter Name="MySecret" DefaultValue="" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides>
      <ConfigOverride Name="Config">
        <Settings>
          <Section Name="MySettings">
            <Parameter Name="MySecret" Value="[MySecret]" IsEncrypted="true" />
          </Section>
        </Settings>
      </ConfigOverride>
    </ConfigOverrides>
  </ServiceManifestImport>
 ```

现在，可以在创建应用程序实例时将值指定为*应用程序参数* 。 可以使用 PowerShell 或 C# 编写用于创建应用程序实例的脚本，方便在生成过程中轻松集成。

使用 PowerShell 时，参数将以[哈希表](/previous-versions/windows/it-pro/windows-powershell-1.0/ee692803(v=technet.10))的形式提供给 `New-ServiceFabricApplication`：

```powershell
New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

使用 C# 时，应用程序参数将以 `NameValueCollection` 的形式在 `ApplicationDescription` 中指定：

```csharp
FabricClient fabricClient = new FabricClient();

NameValueCollection applicationParameters = new NameValueCollection();
applicationParameters["MySecret"] = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=";

ApplicationDescription applicationDescription = new ApplicationDescription(
    applicationName: new Uri("fabric:/MyApp"),
    applicationTypeName: "MyAppType",
    applicationTypeVersion: "1.0.0",
    applicationParameters: applicationParameters)
);

await fabricClient.ApplicationManager.CreateApplicationAsync(applicationDescription);
```

## <a name="decrypt-encrypted-secrets-from-service-code"></a>对服务代码中的已加密机密进行解密
用于访问[参数][parameters-link]和[环境变量][environment-variables-link]的 API 使已加密的值可以轻松解密。 由于加密的字符串包含用于加密的证书的相关信息，因此不需要手动指定证书。 只需在运行服务的节点上安装该证书。

```csharp
// Access decrypted parameters from Settings.xml
ConfigurationPackage configPackage = FabricRuntime.GetActivationContext().GetConfigurationPackageObject("Config");
bool MySecretIsEncrypted = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].IsEncrypted;
if (MySecretIsEncrypted)
{
    SecureString MySecretDecryptedValue = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].DecryptValue();
}

// Access decrypted environment variables from ServiceManifest.xml
// Note: you do not have to call any explicit API to decrypt the environment variable.
string MyEnvVariable = Environment.GetEnvironmentVariable("MyEnvVariable");
```

## <a name="next-steps"></a>后续步骤
* Service Fabric [机密存储](service-fabric-application-secret-store.md) 
* 深入了解[应用程序和服务安全性](service-fabric-application-and-service-security.md)

<!-- Links -->
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-windows-specific-link]: service-fabric-application-secret-management-windows.md
[secret-management-linux-specific-link]: service-fabric-application-secret-management-linux.md
[service fabric secrets store]: service-fabric-application-secret-store.md
