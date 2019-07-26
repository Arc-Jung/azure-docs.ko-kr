---
title: Azure Service Fabric에서 구성 파일을 매개 변수화 | Microsoft Docs
description: Service Fabric에서 구성 파일을 매개 변수화하는 방법을 알아보세요.
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/09/2018
ms.author: mikhegn
ms.openlocfilehash: dad497978de7187177998524db3b2f2ee448c717
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68464776"
---
# <a name="how-to-parameterize-configuration-files-in-service-fabric"></a>Service Fabric에서 구성 파일을 매개 변수화하는 방법

이 문서에서는 Service Fabric에서 구성 파일을 매개 변수화하는 방법을 보여 줍니다.  여러 환경에 대한 응용 프로그램 관리의 핵심 개념에 익숙하지 않은 경우 [여러 환경에 대한 응용 프로그램 관리](service-fabric-manage-multiple-environment-app-configuration.md)를 읽어보세요.

## <a name="procedure-for-parameterizing-configuration-files"></a>구성 파일 매개 변수화 절차

이 예제에서는 애플리케이션 배포에서 매개 변수를 사용하여 구성 값을 재정의합니다.

1. 서비스 프로젝트에서  *\<MyService > \PackageRoot\Config\Settings.xml* 파일을 엽니다.
1. 다음 XML을 추가하여 구성 매개 변수 이름 및 값(예: 캐시 크기가 25임)을 설정하세요.

   ```xml
    <Section Name="MyConfigSection">
      <Parameter Name="CacheSize" Value="25" />
    </Section>
   ```

1. 파일을 저장한 후 닫습니다.
1. *\<MyApplication > \ApplicationPackageRoot\ApplicationManifest.xml* 파일을 엽니다.
1. ApplicationManifest.xml 파일에서 `Parameters` 요소에 매개 변수와 기본값을 선언하세요.  매개 변수 이름에 서비스 이름(예: “내 서비스”)이 포함되어 있는 것이 좋습니다.

   ```xml
    <Parameters>
      <Parameter Name="MyService_CacheSize" DefaultValue="80" />
    </Parameters>
   ```
1. Applicationmanifest .xml 파일의 `ConfigOverrides` `ConfigOverride` 섹션에서 및 요소를 추가 하 고 구성 패키지, 섹션 및 매개 변수를 참조 합니다. `ServiceManifestImport`

   ```xml
    <ConfigOverrides>
      <ConfigOverride Name="Config">
          <Settings>
            <Section Name="MyConfigSection">
                <Parameter Name="CacheSize" Value="[MyService_CacheSize]" />
            </Section>
          </Settings>
      </ConfigOverride>
    </ConfigOverrides>
   ```

> [!NOTE]
> ConfigOverride를 추가하는 경우, Service Fabric은 항상 애플리케이션 매니페스트에 지정된 애플리케이션 매개 변수 또는 기본값을 선택합니다.
>
>

## <a name="next-steps"></a>다음 단계
Visual Studio에서 사용할 수 있는 다른 앱 관리 기능에 대한 정보는 [Visual Studio에서 Service Fabric 애플리케이션 관리](service-fabric-manage-application-in-visual-studio.md)를 참조하세요.
