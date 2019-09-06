---
title: Windows에서 Java 웹앱 만들기 - Azure App Service
description: 이 빠른 시작에서는 몇 분 안에 Windows의 Azure App Service에서 첫 번째 Java Hello World를 배포합니다.
keywords: azure, 앱 서비스, 웹앱, windows, java, maven, 빠른 시작
services: app-service\web
documentationcenter: ''
author: msangapu-msft
manager: jeconnoc
editor: ''
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: quickstart
ms.date: 05/29/2019
ms.author: jafreebe
ms.custom: mvc, seo-java-july2019
ms.openlocfilehash: 2af33976a3c1d1458136a5d91d51c656ede2d343
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68514094"
---
# <a name="quickstart-create-a-java-app-in-app-service"></a>빠른 시작: App Service에서 Java 앱 만들기

> [!NOTE]
> 이 문서에서는 Windows의 App Service에 앱을 배포합니다. _Linux_의 App Service에 배포하려면 [Linux에서 Java 웹앱 만들기](./containers/quickstart-java.md)를 참조하세요.
>

[Azure App Service](overview.md)는 확장성 높은 자체 패치 웹 호스팅 서비스를 제공합니다.  이 빠른 시작에서는 [Azure App Service용 Maven 플러그 인](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin)에서 [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)를 사용하여 Java 웹 보관(WAR) 파일을 배포하는 방법을 보여줍니다.

> [!NOTE]
> IntelliJ 및 Eclipse와 같은 인기 있는 IDE를 사용하여 동일한 작업을 수행할 수도 있습니다. [Azure Toolkit for IntelliJ 빠른 시작](/java/azure/intellij/azure-toolkit-for-intellij-create-hello-world-web-app) 또는 [Azure Toolkit for Eclipse 빠른 시작](/java/azure/eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app)에서 유사한 문서를 확인하세요.
>
![Azure에서 실행되는 샘플 앱](./media/app-service-web-get-started-java/java-hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Java 앱 만들기

Cloud Shell 프롬프트에서 다음 Maven 명령을 실행하여 `helloworld`라는 새 앱을 만듭니다.

```bash
mvn archetype:generate -DgroupId=example.demo -DartifactId=helloworld -DarchetypeArtifactId=maven-archetype-webapp
```

## <a name="configure-the-maven-plugin"></a>Maven 플러그 인 구성

Maven에서 배포하려면 Cloud Shell의 코드 편집기를 사용하여 `helloworld` 디렉터리의 `pom.xml` 프로젝트 파일을 엽니다. 

```bash
code pom.xml
```

그런 다음, `pom.xml` 파일의 `<build>` 요소 내에 다음 플러그 인 정의를 추가합니다.

```xml
<plugins>
    <!--*************************************************-->
    <!-- Deploy to Tomcat in App Service Windows         -->
    <!--*************************************************-->
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.7.0</version>
        <configuration>
            <!-- Specify v2 schema -->
            <schemaVersion>v2</schemaVersion>
            <!-- App information -->
            <subscriptionId>${SUBSCRIPTION_ID}</subscriptionId>
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
            <!-- Java Runtime Stack for App Service on Windows-->
            <runtime>
                <os>windows</os>
                <javaVersion>1.8</javaVersion>
                <webContainer>tomcat 9.0</webContainer>
            </runtime>
            <deployment>
                <resources>
                    <resource>
                        <directory>${project.basedir}/target</directory>
                        <includes>
                            <include>*.war</include>
                        </includes>
                    </resource>
                </resources>
            </deployment>
        </configuration>
    </plugin>
</plugins>
```

> [!NOTE]
> 이 문서에서는 WAR 파일에 패키지된 Java 앱만 사용합니다. 플러그 인에서 JAR 웹 애플리케이션도 지원됩니다. [Linux 기반 App Service에 Java SE JAR 파일 배포](https://docs.microsoft.com/java/azure/spring-framework/deploy-spring-boot-java-app-with-maven-plugin?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)를 방문하여 직접 확인해 보세요.


플러그인 구성에서 다음 자리 표시자를 수정합니다.

| Placeholder | 설명 |
| ----------- | ----------- |
| `SUBSCRIPTION_ID` | 앱을 배포하려는 구독의 고유 ID입니다. 기본 구독의 ID는 `az account show` 명령을 사용하여 Cloud Shell 또는 CLI에서 찾을 수 있습니다. 사용 가능한 모든 구독에 대해 `az account list` 명령을 사용합니다.|
| `RESOURCEGROUP_NAME` | 앱을 만들 새 리소스 그룹의 이름입니다. 앱의 모든 리소스를 한 그룹에 배치하여 다 함께 관리할 수 있습니다. 예를 들어 리소스 그룹을 삭제하면 앱과 연결된 모든 리소스가 삭제됩니다. 이 값을 고유한 새 리소스 그룹(예: *myResourceGroup*)으로 수정합니다. 이 리소스 그룹 이름을 사용하여 이후 섹션에서 모든 Azure 리소스를 정리합니다. |
| `WEBAPP_NAME` | 앱 이름은 Azure(WEBAPP_NAME.azurewebsites.net)에 배포할 때 앱에 대한 호스트 이름의 일부가 됩니다. 이 값을 Java 앱을 호스팅할 새 App Service 앱의 고유 이름(예: *contoso*)으로 수정합니다. |
| `REGION` | 앱이 호스팅되는 Azure 지역입니다(예: *westus2*). `az account list-locations` 명령을 사용하여 Cloud Shell 또는 CLI에서 지역 목록을 가져올 수 있습니다. |

## <a name="deploy-the-app"></a>앱 배포

다음 명령을 사용하여 Java 앱을 Azure에 배포합니다.

```bash
mvn package azure-webapp:deploy
```

배포가 완료되면 웹 브라우저에서 다음 URL을 사용하여 배포된 애플리케이션으로 이동합니다(예: `http://<webapp>.azurewebsites.net/`).

![Azure에서 실행되는 샘플 앱](./media/app-service-web-get-started-java/java-hello-world-in-browser.png)

**축하합니다.** Windows의 App Service에 첫 번째 Java 앱이 배포되었습니다.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Java 개발자 리소스용 Azure](/java/azure/)

> [!div class="nextstepaction"]
> [사용자 지정 도메인 매핑](app-service-web-tutorial-custom-domain.md)
