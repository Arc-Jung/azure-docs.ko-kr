---
title: Chef를 사용하여 Azure 가상 머신 배포 | Microsoft Docs
description: Chef를 사용하여 자동화된 가상 머신 배포 및 Microsoft Azure에서 구성하는 방법에 알아봅니다.
services: virtual-machines-windows
documentationcenter: ''
author: diegoviso
manager: gwallace
tags: azure-service-management,azure-resource-manager
editor: ''
ms.assetid: 0b82ca70-89ed-496d-bb49-c04ae59b4523
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.topic: article
ms.date: 07/09/2019
ms.author: diviso
ms.openlocfilehash: 5cbf53da5a0af0a511350b9f30153e2fefe72dcf
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70080058"
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Chef를 사용하여 Azure 가상 머신 배포 자동화

Chef는 자동화 및 필요한 상태 구성을 제공하는 유용한 도구입니다.

최신 클라우드 API 릴리스에서 Chef는 Azure와 원활한 통합을 통해 단일 명령으로 구성 상태를 프로비전 및 배포할 수 있는 기능을 제공합니다.

이 문서에서는 Chef 환경을 설정하여 Azure Virtual Machine을 프로비전하고, 정책 또는 “Cookbook”을 만든 다음, 이 Cookbook을 Azure Virtual Machine에 배포하는 과정을 안내합니다.

## <a name="chef-basics"></a>Chef 기본 사항
시작하기 전에 [Chef의 기본 개념을 검토](https://www.chef.io/chef)하세요.

다음 다이어그램에 대략적인 Chef 아키텍처가 나와 있습니다.

![][2]

Chef는 세 가지 주요 아키텍처 구성 요소인 Chef Server, Chef Client(노드) 및 Chef Workstation으로 구분됩니다.

Chef Server는 관리 지점이며, 호스트 솔루션과 온-프레미스 솔루션의 두 가지 옵션이 있습니다.

Chef Client(노드)는 관리 중인 서버에 있는 에이전트입니다.

Chef Workstation은 정책을 만들고 Chef 도구의 관리 명령 및 소프트웨어 패키지를 실행하는 관리 워크스테이션의 이름을 나타냅니다.

일반적으로 _워크스테이션_을 작업을 수행하는 위치로 생각하고, _Chef Workstation_은 소프트웨어 패키지라고 생각할 수 있습니다.
예를 들어, knife 명령은 _Chef Workstation_의 일부로 다운로드하지만 _워크스테이션_에서 knife 명령을 실행하여 인프라를 관리합니다.

또한 Chef는 "Cookbook" 및 "Recipe" 개념을 사용합니다. 이러한 개념은 결과적으로 사용자가 정의하여 서버에 적용하는 정책이 됩니다.

## <a name="preparing-your-workstation"></a>워크스테이션 준비

먼저 Chef 구성 파일과 Cookbook을 저장할 디렉터리를 만들어 워크스테이션을 준비합니다.

C:\Chef. 라는 디렉터리를 만듭니다.

워크스테이션에 최신 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 버전을 다운로드 하 여 설치 합니다.

## <a name="configure-azure-service-principal"></a>Azure 서비스 주체 구성

가장 간단한 용어에서 Azure 서비스 주체는 서비스 계정입니다.   Microsoft는 Chef 워크스테이션에서 Azure 리소스를 만드는 데 도움이 되는 서비스 주체를 사용 합니다.  필요한 권한으로 관련 서비스 주체를 만들려면 PowerShell 내에서 다음 명령을 실행 해야 합니다.
 
```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<yourSubscriptionName>"
$myApplication = New-AzureRmADApplication -DisplayName "automation-app" -HomePage "https://chef-automation-test.com" -IdentifierUris "https://chef-automation-test.com" -Password "#1234p$wdchef19"
New-AzureRmADServicePrincipal -ApplicationId $myApplication.ApplicationId
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $myApplication.ApplicationId
```

SubscriptionID, TenantID, ClientID 및 클라이언트 암호 (위에서 설정한 암호)를 기록해 두세요 .이는 나중에 필요 합니다. 

## <a name="setup-chef-server"></a>Chef Server 설치

이 가이드에서는 사용자가 Hosted Chef에 등록한다고 가정합니다.

Chef Server를 아직 사용하고 있지 않으면 다음을 수행할 수 있습니다.

* [Hosted Chef](https://manage.chef.io/signup)에 등록합니다. 이렇게 하는 것이 Chef를 시작하는 가장 빠른 방법입니다.
* [Chef Docs](https://docs.chef.io/)의 [설치 지침](https://docs.chef.io/install_server.html)에 따라 Linux 기반 컴퓨터에 독립 실행형 Chef Server를 설치합니다.

### <a name="creating-a-hosted-chef-account"></a>Hosted Chef 계정 만들기

[여기](https://manage.chef.io/signup)에서 Hosted Chef 계정을 등록합니다.

등록 프로세스 중에 새 조직을 만들지 묻는 메시지가 나타납니다.

![][3]

조직을 만든 후 시작 키트를 다운로드합니다.

![][4]

> [!NOTE]
> 키가 다시 설정된다는 경고 메시지가 나타나는 경우, 아직 구성된 기존 인프라가 없으므로 계속 진행해도 됩니다.
>

이 시작 키트 zip 파일의 `.chef` 디렉터리에는 조직 구성 파일 및 사용자 키가 포함되어 있습니다.

`organization-validator.pem`은 프라이빗 키이며 프라이빗 키는 Chef Server에 저장하지 않아야 하므로 별도로 다운로드해야 합니다. [Chef Manage](https://manage.chef.io/)에서 관리 섹션으로 이동 하 고 "유효성 검사 키 다시 설정"을 선택 하 여 별도로 다운로드할 파일을 제공 합니다. 파일을 c:\chef에 저장합니다.

### <a name="configuring-your-chef-workstation"></a>Chef Workstation 구성

c:\chef에 chef-starter.zip의 압축을 풉니다.

chef-starter\chef-repo\.chef의 모든 파일을 c:\chef 디렉터리에 복사합니다.

`organization-validator.pem` 파일을 c:\chef에 복사합니다(c:\Downloads에 저장된 경우).

이제 디렉터리가 다음 예제와 같이 표시됩니다.

```powershell
    Directory: C:\Users\username\chef

Mode           LastWriteTime    Length Name
----           -------------    ------ ----
d-----    12/6/2018   6:41 PM           .chef
d-----    12/6/2018   5:40 PM           chef-repo
d-----    12/6/2018   5:38 PM           cookbooks
d-----    12/6/2018   5:38 PM           roles
-a----    12/6/2018   5:38 PM       495 .gitignore
-a----    12/6/2018   5:37 PM      1678 azuredocs-validator.pem
-a----    12/6/2018   5:38 PM      1674 user.pem
-a----    12/6/2018   5:53 PM       414 knife.rb
-a----    12/6/2018   5:38 PM      2341 README.md
```

이제 c:\chef의 루트에는 5개의 파일과 4개의 디렉터리(빈 chef-repo 디렉터리 포함)가 있습니다.

### <a name="edit-kniferb"></a>knife.rb 편집

PEM 파일에는 조직 및 관리자의 통신용 프라이빗 키가 들어 있고, knife.rb 파일에는 knife 구성이 들어 있습니다. knife.rb 파일을 편집해야 합니다.

원하는 편집기에서 knife.rb 파일을 엽니다. 변경되지 않은 파일은 다음과 같습니다.

```rb
current_dir = File.dirname(__FILE__)
log_level           :info
log_location        STDOUT
node_name           "mynode"
client_key          "#{current_dir}/user.pem"
chef_server_url     "https://api.chef.io/organizations/myorg"
cookbook_path       ["#{current_dir}/cookbooks"]
```

knife.rb에 다음 정보를 추가합니다.

validation_client_name   "myorg-validator"

validation_key           "#{current_dir}/myorg.pem"

knife[:azure_tenant_id] =         "0000000-1111-aaaa-bbbb-222222222222"

knife[:azure_subscription_id] =   "11111111-bbbbb-cccc-1111-222222222222"

knife[:azure_client_id] =         "11111111-bbbbb-cccc-1111-2222222222222"

knife[:azure_client_secret] =     "#1234p$wdchef19"


이러한 줄은가 c:\chef\cookbooks 아래의 cookbook 디렉터리를 참조 하 고 Azure 작업 중에 만든 Azure 서비스 주체를 사용 하도록 합니다.

이제 knife.rb 파일이 다음 예제와 유사하게 표시됩니다.

![][14]

<!--- Giant problem with this section: Chef 12 uses a config.rb instead of knife.rb
// However, the starter kit hasn't been updated
// So, I don't think this will even work on the modern Chef -->

<!--- update image [6] knife.rb -->

```rb
current_dir = File.dirname(__FILE__)
log_level                :info
log_location             STDOUT
node_name                "myorg"
client_key               "#{current_dir}/myorg.pem"
validation_client_name   "myorg-validator"
validation_key           "#{current_dir}/myorg-validator.pem"
chef_server_url          "https://api.chef.io/organizations/myorg"
cookbook_path            ["#{current_dir}/../cookbooks"]
knife[:azure_tenant_id] = "0000000-1111-aaaa-bbbb-222222222222"
knife[:azure_subscription_id] = "11111111-bbbbb-cccc-1111-222222222222"
knife[:azure_client_id] = "11111111-bbbbb-cccc-1111-2222222222222"
knife[:azure_client_secret] = "#1234p$wdchef19"
```

## <a name="install-chef-workstation"></a>Chef Workstation 설치

다음으로, Chef Workstation을 [다운로드 및 설치](https://downloads.chef.io/chef-workstation/)합니다.
Chef Workstation을 기본 위치에 설치합니다. 설치하는 데 몇 분 정도 걸릴 수 있습니다.

바탕 화면에 "CW PowerShell"이 표시됩니다. 이것은 Chef 제품과 상호 작용하는 데 필요한 도구와 함께 로드되는 환경입니다. CW PowerShell은와 같은 기존 Chef CLI 명령 `chef-run` `chef`뿐만 아니라와 같은 새 임시 명령을 사용할 수 있도록 합니다. `chef -v`를 사용하여 설치된 Chef Workstation 및 Chef 도구 버전을 확인합니다. Chef Workstation 앱에서 "Chef Workstation 정보"를 선택하여 Workstation 버전을 확인할 수도 있습니다.

`chef --version`은 다음과 같은 결과를 반환합니다.

```
Chef Workstation: 0.4.2
  chef-run: 0.3.0
  chef-client: 15.0.300
  delivery-cli: 0.0.52 (9d07501a3b347cc687c902319d23dc32dd5fa621)
  berks: 7.0.8
  test-kitchen: 2.2.5
  inspec: 4.3.2
```

> [!NOTE]
> 경로 순서가 중요합니다! opscode 경로가 올바른 순서가 아니면 문제가 발생합니다.
>

계속하기 전에 워크스테이션을 다시 부팅하세요.

### <a name="install-knife-azure"></a>Knife Azure 설치

이 자습서는 Azure Resource Manager를 사용하여 가상 머신과 상호 작용한다고 가정합니다.

Knife Azure 확장을 설치합니다. 이는 “Azure 플러그 인”이 포함된 Knife를 제공합니다.

다음 명령을 실행합니다.

    chef gem install knife-azure ––pre

> [!NOTE]
> –pre 인수는 최신 API 집합에 대한 액세스를 제공하는 최신 RC 버전의 knife azure 플러그인을 받을 수 있도록 해줍니다.
>
>

이와 동시에 여러 종속성도 설치될 수 있습니다.

![][8]

모든 것이 올바르게 구성되었는지 확인하려면 다음 명령을 실행합니다.

    knife azurerm server list

모든 것이 올바르게 구성되었으면 사용 가능한 Azure 이미지 목록이 표시됩니다.

축하합니다. 워크스테이션이 설정되었습니다.

## <a name="creating-a-cookbook"></a>Cookbook 만들기

Cookbook은 Chef에서 관리되는 클라이언트를 실행할 명령 집합을 정의하는 데 사용됩니다. Cookbook 만들기는 간단하며, **chef generate cookbook** 명령을 사용하여 Cookbook 템플릿을 생성할 수 있습니다. 이 Cookbook은 IIS를 자동으로 배포하는 웹 서버용입니다.

C:\Chef 디렉터리에서 다음 명령을 실행합니다.

    chef generate cookbook webserver

이 명령은 C:\Chef\cookbooks\webserver 디렉터리 아래에 파일 세트를 생성합니다. 다음으로, 관리되는 가상 머신에서 실행할 Chef Client에 대한 명령 세트를 정의합니다.

명령은 default.rb 파일에 저장되어 있습니다. 이 파일에서 IIS를 설치하고, IIS를 시작하며, 템플릿 파일을 wwwroot 폴더에 복사하는 명령 집합을 정의합니다.

C:\chef\cookbooks\webserver\recipes\default.rb를 수정하고 다음 줄을 추가합니다.

    powershell_script 'Install IIS' do
         action :run
         code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
         action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
         source 'Default.htm.erb'
         rights :read, 'Everyone'
    end

작업이 완료되면 파일을 한 번 저장합니다.

## <a name="creating-a-template"></a>템플릿 만들기
이 단계에서는 default.html 페이지로 사용할 템플릿 파일을 생성합니다.

다음 명령을 실행하여 템플릿을 생성합니다.

    chef generate template webserver Default.htm

`C:\chef\cookbooks\webserver\templates\default\Default.htm.erb` 파일로 이동합니다. 간단한 “Hello World” HTML 코드를 추가하여 파일을 편집하고 파일을 저장합니다.

## <a name="upload-the-cookbook-to-the-chef-server"></a>Chef Server에 Cookbook 업로드
이 단계에서는 로컬 컴퓨터에서 만든 쿡북을 복사하여 Chef Hosted Server에 업로드합니다. 업로드가 완료되면 **정책** 탭에 쿡북이 표시됩니다.

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Knife Azure를 사용하여 가상 머신 배포
Azure Virtual Machine을 배포하고 IIS 웹 서비스 및 기본 웹 페이지를 설치할 “Webserver” 쿡북을 적용합니다.

이 작업을 수행 하려면 **azurerm server create** 명령을 사용 합니다.

명령 예제가 다음에 나옵니다.

    knife azurerm server create `
    --azure-resource-group-name rg-chefdeployment `
    --azure-storage-account store `
    --azure-vm-name chefvm `
    --azure-vm-size 'Standard_DS2_v2' `
    --azure-service-location 'westus' `
    --azure-image-reference-offer 'WindowsServer' `
    --azure-image-reference-publisher 'MicrosoftWindowsServer' `
    --azure-image-reference-sku '2016-Datacenter' `
    --azure-image-reference-version 'latest' `
    -x myuser -P myPassword123 `
    --tcp-endpoints '80,3389' `
    --chef-daemon-interval 1 `
    -r "recipe[webserver]"


위의 예제에서는 미국 서 부 지역에 Windows Server 2016가 설치 된 Standard_DS2_v2 가상 컴퓨터를 만듭니다. 특정 변수를 대체하고 실행합니다.

> [!NOTE]
> 명령줄에서 –tcp-endpoints 매개 변수를 사용하여 엔드포인트 네트워크 필터 규칙을 자동화해 보겠습니다. 웹 페이지 및 RDP 세션에 대 한 액세스를 제공 하기 위해 80 및 3389 포트를 열었습니다.
>
>

명령을 실행하고 나면 Azure Portal로 이동하여 프로비전을 시작할 컴퓨터를 봅니다.

![][15]

명령 프롬프트가 다음에 나타납니다.

![][16]

배포가 완료 되 면 배포가 완료 될 때 새 가상 머신의 공용 IP 주소가 표시 됩니다. 그런 다음이를 복사 하 여 웹 브라우저에 붙여 넣고 배포한 웹 사이트를 볼 수 있습니다. 가상 머신을 배포할 때 포트 80을 열어 외부에서 사용할 수 있어야 합니다.   

![][11]

이 예제에서는 creative HTML 코드를 사용합니다.

노드의 상태 [Chef 관리](https://manage.chef.io/)를 볼 수도 있습니다. 

![][17]

포트 3389에서도 Azure Portal에서 RDP 세션을 통해 연결할 수 있다는 점을 기억하세요.

감사합니다. 이제 Azure의 Infrastructure as Code 과정을 시작해 보세요.

<!--Image references-->
[2]: media/chef-automation/2.png
[3]: media/chef-automation/3.png
[4]: media/chef-automation/4.png
[5]: media/chef-automation/5.png
[6]: media/chef-automation/6.png
[7]: media/chef-automation/7.png
[8]: media/chef-automation/8.png
[9]: media/chef-automation/9.png
[10]: media/chef-automation/10.png
[11]: media/chef-automation/11.png
[13]: media/chef-automation/13.png
[14]: media/chef-automation/14.png
[15]: media/chef-automation/15.png
[16]: media/chef-automation/16.png
[17]: media/chef-automation/17.png


<!--Link references-->
