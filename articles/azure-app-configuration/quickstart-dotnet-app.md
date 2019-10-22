---
title: .NET Framework 지원 Azure App Configuration용 빠른 시작 | Microsoft Docs
description: .NET Framework 앱 지원 Azure App Configuration 사용 빠른 시작
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: .NET
ms.workload: tbd
ms.date: 10/09/2019
ms.author: lcozzens
ms.openlocfilehash: 17b2e7272d499ce99d40d2ee52de1c7a5a1d0d04
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72329805"
---
# <a name="quickstart-create-a-net-framework-app-with-azure-app-configuration"></a>빠른 시작: Azure App Configuration을 사용하여 .NET Framework 앱 만들기

이 빠른 시작에서는 Azure App Configuration을 .NET Framework 기반 콘솔 앱에 통합하여 코드와 별도로 애플리케이션 설정의 스토리지 및 관리를 중앙 집중화합니다.

## <a name="prerequisites"></a>필수 조건

- Azure 구독 - [체험 구독 만들기](https://azure.microsoft.com/free/)
- [Visual Studio 2019](https://visualstudio.microsoft.com/vs)
- [.NET Framework 4.7.2](https://dotnet.microsoft.com/download)

## <a name="create-an-app-configuration-store"></a>앱 구성 저장소 만들기

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. **구성 탐색기** >  **+ 만들기**를 선택하여 다음 키-값 쌍을 추가합니다.

    | 키 | 값 |
    |---|---|
    | TestApp:Settings:Message | Azure App Configuration의 정보 |

    지금은 **레이블**과 **콘텐츠 형식**을 비워 두세요.

## <a name="create-a-net-console-app"></a>.NET 콘솔 앱 만들기

1. Visual Studio를 시작하고 **파일** > **새로 만들기** > **프로젝트**를 차례로 선택합니다.

1. **새 프로젝트 만들기**에서 **콘솔** 프로젝트 형식을 필터링하고 **콘솔 앱(.NET Framework)** 을 클릭합니다. **다음**을 클릭합니다.

1. **새 프로젝트 구성**에서 프로젝트 이름을 입력합니다. **Framework**에서 **.NET Framework 4.7.1** 이상을 선택합니다. **만들기**를 클릭합니다.

## <a name="connect-to-an-app-configuration-store"></a>앱 구성 저장소에 연결

1. 마우스 오른쪽 단추로 프로젝트를 클릭하고, **NuGet 패키지 관리**를 선택합니다. **찾아보기** 탭에서 다음 NuGet 패키지를 검색하여 프로젝트에 추가합니다. 찾을 수 없으면 **시험판 포함** 확인란을 선택합니다.

    ```
    Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration 1.0.0 preview or later
    Microsoft.Configuration.ConfigurationBuilders.Environment 2.0.0 preview or later
    System.Configuration.ConfigurationManager version 4.6.0 or later
    ```

1. 프로젝트의 *App.config* 파일을 다음과 같이 업데이트합니다.

    ```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>

    <configBuilders>
        <builders>
            <add name="MyConfigStore" mode="Greedy" connectionString="${ConnectionString}" type="Microsoft.Configuration.ConfigurationBuilders.AzureAppConfigurationBuilder, Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration" />
            <add name="Environment" mode="Greedy" type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Environment" />
        </builders>
    </configBuilders>

    <appSettings configBuilders="Environment,MyConfigStore">
        <add key="AppName" value="Console App Demo" />
        <add key="ConnectionString" value ="Set via an environment variable - for example, dev, test, staging, or production connection string." />
    </appSettings>
    ```

   앱 구성 저장소의 연결 문자열은 `ConnectionString` 환경 변수에서 읽습니다. `appSettings` 섹션의 `configBuilders` 속성에 있는 `MyConfigStore` 앞에 `Environment` 구성 작성기를 추가합니다.

1. *Program.cs*를 열고, `ConfigurationManager`를 호출하여 App Configuration을 사용하도록 `Main` 메서드를 업데이트합니다.

    ```csharp
    static void Main(string[] args)
    {
        string message = System.Configuration.ConfigurationManager.AppSettings["TestApp:Settings:Message"];

        Console.WriteLine(message);
    }
    ```

## <a name="build-and-run-the-app-locally"></a>로컬로 앱 빌드 및 실행

1. **ConnectionString**이라는 환경 변수를 앱 구성 저장소의 연결 문자열로 설정합니다. Windows 명령 프롬프트를 사용하는 경우 다음 명령을 실행합니다.

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Windows PowerShell을 사용하는 경우 다음 명령을 실행합니다.

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

1. Visual Studio를 다시 시작하여 변경 내용을 적용합니다. Ctrl+F5를 눌러 콘솔 앱을 빌드하고 실행합니다.

## <a name="clean-up-resources"></a>리소스 정리

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 새 앱 구성 저장소를 만들고, .NET Framework 콘솔 앱에 사용했습니다. App Configuration을 사용하는 방법을 자세히 알아보려면 인증에 대해 설명하는 다음 자습서로 계속 진행하세요.

> [!div class="nextstepaction"]
> [관리 ID 통합](./howto-integrate-azure-managed-service-identity.md)
