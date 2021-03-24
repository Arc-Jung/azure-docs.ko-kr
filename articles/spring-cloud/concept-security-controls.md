---
title: Azure Spring Cloud Service의 보안 컨트롤
description: Azure 스프링 클라우드 서비스에 기본 제공 되는 보안 제어를 사용 합니다.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 04/23/2020
ms.custom: devx-track-java
ms.openlocfilehash: 62a0bd19f6b10bbe6561f5587ed85d4d1e5880b3
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/23/2021
ms.locfileid: "104878627"
---
# <a name="security-controls-for-azure-spring-cloud-service"></a>Azure Spring Cloud Service의 보안 컨트롤

**이 문서는 다음에 적용됩니다.** ✔️ Java ✔️ C#

보안 제어는 Azure 스프링 클라우드 서비스에 기본 제공 됩니다.

보안 제어는 보안 취약성을 방지, 탐지 및 대응할 수 있는 서비스의 기능에 기여하는 Azure 서비스의 품질 또는 기능입니다.  각 컨트롤에 대해 *예* 또는 *아니요* 를 사용 하 여 서비스에 대 한 현재 준비 여부를 나타냅니다.  서비스에 적용 되지 않는 컨트롤에는 *N/A* 를 사용 합니다. 

**데이터 보호 보안 제어**

| 보안 컨트롤 | 예/아니요 | 메모 | 문서화 |
|:-------------|:-------|:-------------------------------|:----------------------|
| 미사용 서버 쪽 암호화: Microsoft 관리형 키 | 예 | 사용자가 업로드 한 원본 및 아티팩트, 구성 서버 설정, 앱 설정 및 영구 저장소의 데이터는 Azure Storage에 저장 되며, 미사용 콘텐츠를 자동으로 암호화 합니다.<br><br>구성 서버 캐시, 업로드 된 원본에서 빌드된 런타임 이진 파일 및 응용 프로그램 수명 중에 응용 프로그램 로그는 미사용 콘텐츠를 자동으로 암호화 하는 Azure 관리 디스크에 저장 됩니다.<br><br>사용자가 업로드 한 원본에서 빌드된 컨테이너 이미지는 미사용 이미지 콘텐츠를 자동으로 암호화 하는 Azure Container Registry에 저장 됩니다. | [미사용 데이터에 대한 Azure Storage 암호화](../storage/common/storage-service-encryption.md)<br><br>[Azure Managed Disks의 서버 쪽 암호화](../virtual-machines/disk-encryption.md)<br><br>[Azure Container Registry의 컨테이너 이미지 스토리지](../container-registry/container-registry-storage.md) |
| 일시적인 암호화 | 예 | 사용자 앱 공용 끝점은 기본적으로 인바운드 트래픽에 대해 HTTPS를 사용 합니다. |  |
| API 호출 암호화 | 예 | Azure 스프링 클라우드 서비스를 구성 하기 위한 관리 호출은 HTTPS를 통한 Azure Resource Manager 호출을 통해 수행 됩니다. | [Azure Resource Manager](../azure-resource-manager/index.yml) |

**네트워크 액세스 보안 제어**

| 보안 컨트롤 | 예/아니요 | 메모 | 문서화 |
|:-------------|:-------|:-------------------------------|:----------------------|
| 서비스 태그 | 예 | **AzureSpringCloud** service 태그를 사용 하 여 Azure 스프링 클라우드 응용 프로그램에 대 한 트래픽을 허용 하는 [네트워크 보안 그룹](../virtual-network/network-security-groups-overview.md#security-rules) 또는 [azure 방화벽](../firewall/service-tags.md)에서 아웃 바운드 네트워크 액세스 제어를 정의 합니다.<br><br>*참고:* 현재 2020/07/14 이후 생성 된 새 Azure 스프링 클라우드 서비스 인스턴스만 **AzureSpringCloud** service 태그를 지원 합니다. | [서비스 태그](../virtual-network/service-tags-overview.md) |

## <a name="next-steps"></a>다음 단계

* [빠른 시작: 첫 번째 Azure Spring Cloud 애플리케이션 배포](spring-cloud-quickstart.md)