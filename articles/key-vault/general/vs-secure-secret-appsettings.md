---
title: 웹 애플리케이션의 비밀 애플리케이션 설정을 안전하게 저장 - Azure Key Vault | Microsoft Docs
description: ASP.NET Core Key Vault Provider, 사용자 비밀 또는 .NET 4.7.1을 사용하여 Azure 자격 증명 또는 타사 API와 같은 비밀 애플리케이션 설정을 안전하게 저장하는 방법
services: visualstudio
author: cawaMS
manager: paulyuk
editor: ''
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: cawa
ms.openlocfilehash: bcacd5d2ed9e325383ec7ae75002ae0a6213111c
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81429760"
---
# <a name="securely-save-secret-application-settings-for-a-web-application"></a>웹 애플리케이션의 비밀 애플리케이션 설정을 안전하게 저장

## <a name="overview"></a>개요
이 문서에서는 Azure 애플리케이션의 보안 애플리케이션 구성 설정을 안전하게 저장하는 방법에 대해 설명합니다.

일반적으로 모든 웹 응용 프로그램 구성 설정은 Web.config와 같은 구성 파일에 저장됩니다. 이 방법을 사용하면 클라우드 자격 증명과 같은 비밀 설정을 GitHub와 같은 공용 소스 제어 시스템에 체크 인할 수 있습니다. 한편 소스 코드를 변경하고 개발 설정을 재구성하는 데 필요한 오버헤드로 인해 보안 모범 사례를 따르기 어려울 수 있습니다.

개발 프로세스가 안전한지 확인하기 위해 도구 및 프레임워크 라이브러리를 만들어 소스 코드 변경을 최소화하거나 변경하지 않고 응용 프로그램 보안 설정을 안전하게 저장합니다.

## <a name="aspnet-and-net-core-applications"></a>ASP.NET 및 .NET 코어 애플리케이션

### <a name="save-secret-settings-in-user-secret-store-that-is-outside-of-source-control-folder"></a>소스 제어 폴더 외부의 사용자 비밀 저장소에 비밀 설정 저장
빠른 프로토타입을 수행 중이거나 인터넷에 액세스할 수 없는 경우 소스 제어 폴더 외부의 비밀 설정을 User Secret 저장소로 이동하는 것부터 시작합니다. 사용자 비밀 저장소는 사용자 프로필 폴더 아래에 저장된 파일이므로 비밀이 소스 제어로 체크 인되지 않습니다. 다음 다이어그램은 [사용자 비밀](https://docs.microsoft.com/aspnet/core/security/app-secrets?tabs=visual-studio)의 작동 원리를 보여줍니다.

![사용자 비밀은 비밀 설정을 소스 컨트롤 외부에 유지합니다.](../media/vs-secure-secret-appsettings/aspnetcore-usersecret.PNG)

.NET Core 콘솔 애플리케이션을 실행하는 경우 Key Vault를 사용하여 비밀을 안전하게 저장합니다.

### <a name="save-secret-settings-in-azure-key-vault"></a>Azure Key Vault에 비밀 설정 저장
프로젝트를 개발하면서 소스 코드를 안전하게 공유해야 하는 경우 [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)를 사용합니다.

1. Azure 구독에 Key Vault를 만듭니다. UI에서 필수 필드를 모두 채우고 블레이드 아래쪽에서 *만들기*를 클릭합니다.

    ![Azure Key Vault 만들기](../media/vs-secure-secret-appsettings/create-keyvault.PNG)

2. 나와 나의 팀 멤버에게 Key Vault에 대한 액세스 권한을 부여합니다. 팀이 큰 경우 [Azure Active Directory 그룹](../../active-directory/active-directory-groups-create-azure-portal.md)을 만들어서 해당 보안 그룹 액세스를 Key Vault에 추가할 수 있습니다. *비밀 권한* 드롭다운에서 *비밀 관리 작업* 아래의 *가져오기*와 *목록*을 선택합니다.
웹 앱을 이미 만든 경우 앱 설정 또는 파일에 비밀 구성을 저장하지 않고 키 자격 증명 모음에 액세스할 수 있도록 키 자격 증명 모음에 대한 웹 앱 액세스 권한을 부여합니다. 이름으로 웹 앱을 검색하고 사용자에게 액세스 권한을 부여하는 것과 동일한 방식으로 추가합니다.

    ![Key Vault 액세스 정책 추가](../media/vs-secure-secret-appsettings/add-keyvault-access-policy.png)

3. Azure 포털의 키 자격 증명 모음에 비밀을 추가합니다. 중첩된 구성 설정의 경우 Key Vault 비밀 이름이 유효하도록 ':'을 '--'로 바꿉니다. ':'은 Key Vault 비밀의 이름에 사용할 수 없습니다.

    ![Key Vault 비밀 추가](../media/vs-secure-secret-appsettings/add-keyvault-secret.png)

    > [!NOTE]
    > Visual Studio 2017 V15.6 이전에는 Visual Studio용 Azure 서비스 인증 확장을 설치하는 것이 좋습니다. 그러나 기능이 Visual Studio 내에 통합되어 있으므로 이제 더 이상 사용되지 않습니다. 따라서 Visual Studio 2017의 이전 버전에 있는 경우 이 기능을 기본적으로 사용하고 Visual Studio 로그인 ID 자체를 사용하지 못하도록 최소 VS 2017 15.6 또는 위로 업데이트하는 것이 좋습니다.
    >

4. 다음 NuGet 패키지를 프로젝트에 추가합니다.

    ```
    Microsoft.Azure.KeyVault
    Microsoft.Azure.Services.AppAuthentication
    Microsoft.Extensions.Configuration.AzureKeyVault
    ```
5. 다음 코드를 Program.cs 파일에 추가합니다.

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
             Host.CreateDefaultBuilder(args)
                .ConfigureAppConfiguration((ctx, builder) =>
                {
                    var keyVaultEndpoint = GetKeyVaultEndpoint();
                    if (!string.IsNullOrEmpty(keyVaultEndpoint))
                    {
                        var azureServiceTokenProvider = new AzureServiceTokenProvider();
                        var keyVaultClient = new KeyVaultClient(
                            new KeyVaultClient.AuthenticationCallback(
                                azureServiceTokenProvider.KeyVaultTokenCallback));
                        builder.AddAzureKeyVault(
                        keyVaultEndpoint, keyVaultClient, new DefaultKeyVaultSecretManager());
                    }
                })
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                });

        private static string GetKeyVaultEndpoint() => Environment.GetEnvironmentVariable("KEYVAULT_ENDPOINT");
    ```
6. Key Vault URL을 launchsettings.json 파일에 추가합니다. 환경 변수 이름인 *KEYVAULT_ENDPOINT*는 6단계에서 추가한 코드에서 정의됩니다.

    ![Key Vault URL을 프로젝트 환경 변수로 추가](../media/vs-secure-secret-appsettings/add-keyvault-url.png)

7. 프로젝트 디버그를 시작합니다. 성공적으로 실행되어야 합니다.

## <a name="aspnet-and-net-applications"></a>ASP.NET 및 .NET 애플리케이션

.NET 4.7.1은 비밀이 코드 변경 없이 소스 제어 폴더 외부로 이동하도록 보장하는 Key Vault 및 비밀 구성 빌더를 지원합니다.
계속하려면 [.NET 4.7.1을 다운로드](https://www.microsoft.com/download/details.aspx?id=56115)하고 이전 버전의 .NET Framework를 사용하는 경우 애플리케이션을 마이그레이션합니다.

### <a name="save-secret-settings-in-a-secret-file-that-is-outside-of-source-control-folder"></a>소스 제어 폴더 외부에 있는 비밀 파일에 비밀 설정 저장
빠른 프로토타입을 작성하면서 Azure 리소스를 프로비전하지 않으려면 이 옵션을 사용하세요.

1. 다음 NuGet 패키지를 프로젝트에 설치합니다.
    ```
    Microsoft.Configuration.ConfigurationBuilders.Base
    ```

2. 다음과 유사한 파일을 만듭니다. 프로젝트 폴더 외부의 위치에 저장합니다.

    ```xml
    <root>
        <secrets ver="1.0">
            <secret name="secret1" value="foo_one" />
            <secret name="secret2" value="foo_two" />
        </secrets>
    </root>
    ```

3. 비밀 파일을 Web.config 파일에 구성 빌더로 정의합니다. 이 섹션을 *appSettings* 섹션 앞에 배치합니다.

    ```xml
    <configBuilders>
        <builders>
            <add name="Secrets"
                 secretsFile="C:\Users\AppData\MyWebApplication1\secret.xml" type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
                    Microsoft.Configuration.ConfigurationBuilders, Version=1.0.0.0, Culture=neutral" />
        </builders>
    </configBuilders>
    ```

4. appSettings 섹션에서 비밀 구성 빌더를 사용하는 것으로 지정합니다. 더미 값이 있는 비밀 설정에 대한 항목이 있는지 확인합니다.

    ```xml
        <appSettings configBuilders="Secrets">
            <add key="webpages:Version" value="3.0.0.0" />
            <add key="webpages:Enabled" value="false" />
            <add key="ClientValidationEnabled" value="true" />
            <add key="UnobtrusiveJavaScriptEnabled" value="true" />
            <add key="secret" value="" />
        </appSettings>
    ```

5. 앱을 디버그합니다. 성공적으로 실행되어야 합니다.

### <a name="save-secret-settings-in-an-azure-key-vault"></a>Azure Key Vault에 비밀 설정 저장
ASP.NET Core 섹션의 지침에 따라 프로젝트의 Key Vault를 구성합니다.

1. 다음 NuGet 패키지를 프로젝트에 설치합니다.
   ```
   Microsoft.Configuration.ConfigurationBuilders.UserSecrets
   ```

2. Web.config에서 키 볼트 구성 빌더를 정의합니다. *appSettings* 섹션 앞에 이 섹션을 배치합니다. Key Vault가 공용 Azure에 있는 경우 *vaultName*이 Key Vault 이름이 되도록 바꾸거나 독립적 클라우드를 사용하는 경우 전체 URI를 대체합니다.

    ```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>
    <configBuilders>
        <builders>
            <add name="AzureKeyVault" vaultName="Test911" type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder, ConfigurationBuilders, Version=1.0.0.0, Culture=neutral" />
        </builders>
    </configBuilders>
    ```
3. appSettings 섹션에서 Key Vault 구성 빌더를 사용하는 것으로 지정합니다. 더미 값을 사용하여 비밀 설정에 대한 항목이 있도록 합니다.

   ```xml
   <appSettings configBuilders="AzureKeyVault">
       <add key="webpages:Version" value="3.0.0.0" />
       <add key="webpages:Enabled" value="false" />
       <add key="ClientValidationEnabled" value="true" />
       <add key="UnobtrusiveJavaScriptEnabled" value="true" />
       <add key="secret" value="" />
   </appSettings>
   ```

4. 프로젝트 디버그를 시작합니다. 성공적으로 실행되어야 합니다.
